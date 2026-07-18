# 17 - How to Learn React This Way

React is not mainly about memorizing APIs. It is about learning to think in components, data flow, state ownership, and user-visible behavior. This folder is designed around that progression: each lesson introduces a concept, each examples file gives repeated exposure, and your job is to turn those examples into working instinct.

## The learning loop

For each concept, use this loop:

1. Read the lesson file once without trying to memorize it.
2. Type 10 examples from the matching examples file.
3. Change 5 examples and predict what will happen.
4. Build one tiny component using the concept.
5. Add one edge state: loading, error, empty, disabled, invalid, or selected.
6. Explain where the state lives and why.
7. Rebuild the component from memory two days later.

React becomes much easier when you repeatedly ask: what data do I have, where should it live, what events can change it, and what UI should appear for each state?

## What to focus on

Focus first on components and props. If you cannot split UI into clear components and pass data down cleanly, hooks will only add confusion.

Then focus on state. Most React bugs come from state living in the wrong place, duplicated state, mutated state, or state that should have been derived during render. Practice moving state down, lifting state up, and deleting unnecessary state.

Then focus on effects. Effects are not a general-purpose way to "do something after render." They are for synchronizing with systems outside React: APIs, timers, subscriptions, browser APIs, and third-party libraries. If you are using an effect just to calculate a value from props or state, you probably do not need the effect.

Then focus on user behavior. A component is not finished because it renders the happy path. Real UI needs loading states, error states, empty states, disabled states, keyboard access, focus behavior, and readable feedback.

## What this method is good at

The paired-file method is good for building fluency. JSX, props, state, events, lists, forms, effects, context, reducers, and tests all become less intimidating after you have seen many small examples.

It is also good for building pattern recognition. You will start to recognize that form state has a shape, list rendering has a shape, data fetching has a shape, and route-driven screens have a shape. That recognition is useful in real code reviews because you can spot when something feels off.

## What this method is not enough for

Small examples cannot fully teach product judgment. Real React work includes design constraints, accessibility, API failures, browser quirks, stale data, inconsistent requirements, and existing code that was not written for a tutorial.

Do not stop at examples. Use them as reps, then build screens. A screen forces decisions that snippets avoid: where data loads, who owns form state, how navigation works, whether a child should receive data or fetch it, and what happens when something fails.

## Gaps to expand next

After this tutorial, expand into:

- Accessibility: semantic HTML, keyboard navigation, focus management, labels, ARIA, and screen reader behavior.
- CSS: layout, responsive design, design systems, and component states.
- TypeScript: most professional React teams expect at least reading fluency.
- Routing frameworks: React Router, Next.js, Remix, or other framework-level routing.
- Server state: TanStack Query, SWR, framework loaders, caching, invalidation, mutations, and optimistic updates.
- Forms: React Hook Form, schema validation, field-level errors, and complex form UX.
- Testing: React Testing Library, user-event, MSW, component tests, and end-to-end tests with Playwright or Cypress.
- Performance: profiling, large lists, memoization tradeoffs, bundle size, and network waterfalls.
- Security: XSS, unsafe HTML, auth flows, token storage tradeoffs, and dependency updates.
- Team workflow: Git, pull requests, code review, issue scoping, and communicating UI tradeoffs.

## What contribution readiness looks like

You are ready to contribute when you can take a small UI request and move it through the app without being lost. For example: add a filter to a task page, store the filter in the URL, call the API with the filter, show loading and empty states, test the user flow, and explain the tradeoffs.

You do not need to know every React feature. You do need to know how to find the right component, understand the state flow, make a small change, avoid unnecessary effects, preserve accessibility, and test the behavior.

The examples build comfort. The capstone builds coordination. Real projects build judgment. Use all three.

---

Back to the [README index](./README.md).
