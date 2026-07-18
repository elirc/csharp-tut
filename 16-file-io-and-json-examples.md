# 16 - File I/O & JSON Examples

These snippets reinforce file reads and writes, paths, streams, JSON serialization, casing, attributes, async I/O, and simple persistence.

## Example 1 - Read all text

```csharp
string text = File.ReadAllText("notes.txt");
```

`ReadAllText` is simple when the file is small enough to load into memory.

## Example 2 - Read all lines

```csharp
string[] lines = File.ReadAllLines("notes.txt");
```

`ReadAllLines` gives you an array where each element is one line from the file.

## Example 3 - Write all text

```csharp
File.WriteAllText("notes.txt", "Hello file");
```

`WriteAllText` creates the file or replaces its existing contents.

## Example 4 - Append text

```csharp
File.AppendAllText("log.txt", "Started\n");
```

Appending preserves existing content and adds new text at the end.

## Example 5 - Check existence

```csharp
if (File.Exists("settings.json"))
{
    Console.WriteLine("Settings found");
}
```

Existence checks help you choose between loading and creating a file.

## Example 6 - Delete a file

```csharp
if (File.Exists("old.txt"))
{
    File.Delete("old.txt");
}
```

Checking first avoids treating a missing file as a surprise during simple cleanup.

## Example 7 - File info

```csharp
var info = new FileInfo("notes.txt");
Console.WriteLine(info.Length);
```

`FileInfo` exposes metadata such as size, name, and timestamps.

## Example 8 - Combine paths

```csharp
string path = Path.Combine("data", "settings.json");
```

`Path.Combine` handles directory separators correctly for the current platform.

## Example 9 - Get extension

```csharp
string extension = Path.GetExtension("report.pdf");
```

The `Path` class contains helpers for common path operations.

## Example 10 - Create directory

```csharp
Directory.CreateDirectory("data");
```

Creating an existing directory is safe, so this works well before writing files.

## Example 11 - Stream read

```csharp
using var stream = File.OpenRead("data.bin");
byte[] buffer = new byte[stream.Length];
stream.Read(buffer);
```

Streams are useful for binary data or large files.

## Example 12 - Serialize object

```csharp
var user = new User("Ada", 36);
string json = JsonSerializer.Serialize(user);
```

Serialization converts an object into JSON text.

## Example 13 - Pretty JSON

```csharp
var options = new JsonSerializerOptions { WriteIndented = true };
string json = JsonSerializer.Serialize(user, options);
```

Indented JSON is easier for humans to read.

## Example 14 - Deserialize object

```csharp
User? user = JsonSerializer.Deserialize<User>(json);
```

Deserialization converts JSON text back into .NET objects.

## Example 15 - Case-insensitive deserialize

```csharp
var options = new JsonSerializerOptions
{
    PropertyNameCaseInsensitive = true
};
```

This option accepts JSON names that differ by casing from C# property names.

## Example 16 - Camel case output

```csharp
var options = new JsonSerializerOptions
{
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase
};
```

Camel case is common in JSON APIs even when C# properties use PascalCase.

## Example 17 - JSON property name

```csharp
public record Product([property: JsonPropertyName("product_name")] string Name);
```

`JsonPropertyName` maps a C# member to a specific JSON property name.

## Example 18 - Async file read

```csharp
string text = await File.ReadAllTextAsync("notes.txt");
```

Async file APIs avoid blocking the current thread while I/O is in progress.

## Example 19 - Deserialize collection

```csharp
List<User>? users = JsonSerializer.Deserialize<List<User>>(json);
```

Generic collection types deserialize naturally when the JSON contains an array.

## Example 20 - Tiny JSON store save

```csharp
Directory.CreateDirectory("data");
string json = JsonSerializer.Serialize(users, new JsonSerializerOptions { WriteIndented = true });
await File.WriteAllTextAsync(Path.Combine("data", "users.json"), json);
```

A simple JSON-backed store is often just serialization plus careful path handling.

Helper record used above:

```csharp
public record User(string Name, int Age);
```

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `File.ReadAllBytes(path)` | Reads a whole binary file into memory. |
| 22 | `File.WriteAllBytes(path, bytes)` | Writes binary data to a file. |
| 23 | `File.Copy(source, target, overwrite: true)` | Copies a file and optionally replaces an existing target. |
| 24 | `File.Move(source, target)` | Moves or renames a file. |
| 25 | `Directory.Exists(path)` | Checks whether a directory exists. |
| 26 | `Directory.GetFiles(path)` | Lists files directly inside a directory. |
| 27 | `Directory.EnumerateFiles(path)` | Streams file names lazily instead of allocating an array. |
| 28 | `Path.GetFileName(path)` | Extracts the file name from a path. |
| 29 | `Path.GetDirectoryName(path)` | Extracts the containing directory from a path. |
| 30 | `Path.ChangeExtension(path, ".bak")` | Creates a path with a different extension. |
| 31 | `Path.GetTempPath()` | Gets the current user's temporary file directory. |
| 32 | `Path.GetRandomFileName()` | Creates a safe random file name. |
| 33 | `Path.Join(folder, fileName)` | Joins path segments without extra validation. |
| 34 | `File.Open(path, FileMode.OpenOrCreate)` | Opens a file with explicit creation behavior. |
| 35 | `FileStream stream = File.Create(path);` | Creates or overwrites a file and returns a stream. |
| 36 | `using var writer = new StreamWriter(path);` | Writes text through a stream abstraction. |
| 37 | `using var reader = new StreamReader(path);` | Reads text through a stream abstraction. |
| 38 | `await reader.ReadLineAsync();` | Reads one line asynchronously. |
| 39 | `await writer.WriteLineAsync(line);` | Writes one line asynchronously. |
| 40 | `JsonSerializer.Serialize(value)` | Converts an object to JSON text. |
| 41 | `JsonSerializer.Deserialize<T>(json)` | Converts JSON text to a typed object. |
| 42 | `JsonSerializer.SerializeToUtf8Bytes(value)` | Serializes JSON directly to UTF-8 bytes. |
| 43 | `JsonSerializer.DeserializeAsync<T>(stream)` | Deserializes JSON from a stream asynchronously. |
| 44 | `JsonSerializer.SerializeAsync(stream, value)` | Serializes an object as JSON into a stream. |
| 45 | `new JsonSerializerOptions(JsonSerializerDefaults.Web)` | Uses web-friendly JSON defaults. |
| 46 | `DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull` | Omits null properties during serialization. |
| 47 | `[JsonIgnore]` | Prevents a property from being serialized. |
| 48 | `[JsonPropertyName("id")]` | Maps a C# property to a JSON property name. |
| 49 | `[JsonConstructor]` | Marks which constructor should be used during deserialization. |
| 50 | `JsonDocument.Parse(json)` | Reads JSON dynamically without a custom type. |
| 51 | `document.RootElement.GetProperty("name")` | Reads a property from a JSON document. |
| 52 | `JsonNode.Parse(json)` | Uses a mutable DOM-like JSON API. |
| 53 | `JsonArray array = new();` | Builds JSON arrays dynamically. |
| 54 | `JsonObject obj = new();` | Builds JSON objects dynamically. |
| 55 | `await File.WriteAllTextAsync(path, json);` | Writes text without blocking the current thread. |
| 56 | `await File.ReadAllBytesAsync(path);` | Reads binary content asynchronously. |
| 57 | `File.AppendAllLines(path, lines)` | Appends multiple lines to a text file. |
| 58 | `File.ReadLines(path)` | Lazily reads lines from a file. |
| 59 | `Directory.CreateDirectory(Path.GetDirectoryName(path)!);` | Ensures a target folder exists before writing. |
| 60 | `var backup = Path.ChangeExtension(path, ".json.bak");` | Creates a predictable backup file path. |
