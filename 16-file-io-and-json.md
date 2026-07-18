# 16 — File I/O & JSON

Reading/writing files and converting objects to/from JSON are everyday tasks: config files, data import/export, talking to APIs, logging. This chapter covers the practical 90%.

## Files and paths

### Reading files

```csharp
using System.IO;

// Whole file as one string:
string content = File.ReadAllText("data.txt");

// All lines as an array:
string[] lines = File.ReadAllLines("data.txt");

// Lazily, line by line (good for big files — doesn't load it all):
foreach (string line in File.ReadLines("big.txt"))
    Process(line);

// Async versions (prefer these in web apps):
string text = await File.ReadAllTextAsync("data.txt");
string[] allLines = await File.ReadAllLinesAsync("data.txt");
byte[] bytes = await File.ReadAllBytesAsync("image.png");
```

### Writing files

```csharp
File.WriteAllText("out.txt", "Hello");                 // overwrites
File.WriteAllLines("out.txt", new[] { "a", "b" });
File.AppendAllText("log.txt", "new line\n");           // append
await File.WriteAllTextAsync("out.txt", content);      // async
```

### Existence, deletion, info

```csharp
File.Exists("data.txt");           // bool
File.Delete("old.txt");
File.Copy("a.txt", "b.txt", overwrite: true);
File.Move("a.txt", "b.txt");

Directory.Exists("logs");
Directory.CreateDirectory("logs");                     // creates nested dirs; no-op if exists
string[] files = Directory.GetFiles("logs", "*.txt");
string[] dirs = Directory.GetDirectories(".");
foreach (var f in Directory.EnumerateFiles("logs"))    // lazy version
    Console.WriteLine(f);
```

### Paths — use `Path`, never string concatenation

`Path` handles OS differences (`\` vs `/`) and edge cases. **Don't** build paths with `+`:

```csharp
using System.IO;

string path = Path.Combine("data", "users", "sam.json");  // "data/users/sam.json" or "data\..."
string full = Path.GetFullPath(path);
string name = Path.GetFileName(path);                     // "sam.json"
string stem = Path.GetFileNameWithoutExtension(path);     // "sam"
string ext  = Path.GetExtension(path);                    // ".json"
string dir  = Path.GetDirectoryName(full);
string temp = Path.GetTempFileName();                     // a unique temp file

// Special folders:
string appData = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
```

### Streams (for large or binary data)

When a file is too big to load whole, or you're working with binary, use streams (and `using` to dispose them — Chapter 11):

```csharp
using var reader = new StreamReader("big.csv");
string? line;
while ((line = await reader.ReadLineAsync()) is not null)
    Process(line);

using var writer = new StreamWriter("out.txt", append: false);
await writer.WriteLineAsync("first line");
```

`File.ReadAllText` etc. are convenient wrappers over streams; reach for streams directly when memory or streaming matters.

## JSON with `System.Text.Json`

JSON is the lingua franca of web APIs and config. .NET's built-in serializer is `System.Text.Json` (namespace `System.Text.Json`). (The older third-party `Newtonsoft.Json` is still everywhere in legacy code and more feature-rich; know it exists, but default to `System.Text.Json` for new work.)

### Serializing (object → JSON)

```csharp
using System.Text.Json;

public record Person(string Name, int Age, string[] Hobbies);

var person = new Person("Sam", 30, new[] { "chess", "hiking" });

string json = JsonSerializer.Serialize(person);
// {"Name":"Sam","Age":30,"Hobbies":["chess","hiking"]}

// Pretty-printed:
var options = new JsonSerializerOptions { WriteIndented = true };
string pretty = JsonSerializer.Serialize(person, options);
```

### Deserializing (JSON → object)

```csharp
string json = """{ "Name": "Ann", "Age": 25, "Hobbies": ["art"] }""";

Person? p = JsonSerializer.Deserialize<Person>(json);
Console.WriteLine(p?.Name);     // Ann
```

### The big gotcha: casing

JSON conventionally uses `camelCase`; C# uses `PascalCase`. By default `System.Text.Json` matches property names **case-insensitively** on read but writes them exactly as named. To produce/consume camelCase reliably, set the naming policy:

```csharp
var options = new JsonSerializerOptions
{
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,   // Name -> "name"
    PropertyNameCaseInsensitive = true,
    WriteIndented = true,
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull,  // omit null props
};

string json = JsonSerializer.Serialize(person, options);  // {"name":"Sam",...}
var back = JsonSerializer.Deserialize<Person>(json, options);
```

In ASP.NET Core, this camelCase behavior is configured for you by default — your API will emit camelCase JSON automatically.

### Customizing with attributes

```csharp
using System.Text.Json.Serialization;

public class Product
{
    [JsonPropertyName("product_id")]      // map to a specific JSON name
    public int Id { get; set; }

    public string Name { get; set; } = "";

    [JsonIgnore]                          // never serialize this
    public string InternalNotes { get; set; } = "";

    [JsonPropertyName("price_usd")]
    public decimal Price { get; set; }
}
```

### Async JSON over streams

For files/HTTP, serialize/deserialize directly to/from a stream (more efficient than building a big string):

```csharp
// Write JSON to a file:
await using var fs = File.Create("person.json");
await JsonSerializer.SerializeAsync(fs, person, options);

// Read JSON from a file:
await using var input = File.OpenRead("person.json");
var loaded = await JsonSerializer.DeserializeAsync<Person>(input, options);
```

When calling HTTP APIs you'll use the `HttpClient` JSON extensions (`GetFromJsonAsync`, `ReadFromJsonAsync`) — see Chapter 17 — which wrap this for you.

### Deserializing into collections and dynamic shapes

```csharp
var people = JsonSerializer.Deserialize<List<Person>>(jsonArray);
var dict = JsonSerializer.Deserialize<Dictionary<string, int>>(jsonObject);

// When you don't have a type, parse a document and walk it:
using JsonDocument doc = JsonDocument.Parse(json);
JsonElement root = doc.RootElement;
string name = root.GetProperty("name").GetString()!;
int age = root.GetProperty("age").GetInt32();
```

Prefer deserializing into a concrete type/record. Use `JsonDocument`/`JsonElement` only for genuinely dynamic or partial JSON.

## CSV (quick note)

There's no built-in CSV parser worth using. For simple, trusted data you can `Split(',')`, but real CSV (quoted fields, embedded commas/newlines) needs a library — **CsvHelper** is the standard NuGet choice. Don't hand-roll CSV parsing for production data.

## Practical example: a tiny JSON-backed store

```csharp
public class JsonStore<T>
{
    private readonly string _path;
    private static readonly JsonSerializerOptions _opts =
        new() { WriteIndented = true, PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    public JsonStore(string path) => _path = path;

    public async Task<List<T>> LoadAsync()
    {
        if (!File.Exists(_path)) return new List<T>();
        await using var fs = File.OpenRead(_path);
        return await JsonSerializer.DeserializeAsync<List<T>>(fs, _opts) ?? new List<T>();
    }

    public async Task SaveAsync(List<T> items)
    {
        await using var fs = File.Create(_path);
        await JsonSerializer.SerializeAsync(fs, items, _opts);
    }
}
```

## Exercises

1. Write a program that loads a `List<Person>` from `people.json` (returning an empty list if the file is missing), adds a new person, and saves it back — all async.
2. Serialize an object with `WriteIndented` and camelCase, write it to a file, read it back, and confirm equality.
3. Use `[JsonPropertyName]` and `[JsonIgnore]` to map a C# class to an external API's snake_case JSON shape.
4. Given a directory, list all `.json` files and print each filename without its extension, using only `Path`/`Directory` methods.

---

**Next:** [17 — HTTP & Calling APIs](./17-http-and-apis.md)
