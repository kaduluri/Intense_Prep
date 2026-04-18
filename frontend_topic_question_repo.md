# Frontend Interview Question Repo
Target: service-to-product switch, mid-tier and upper-mid product companies.

This repo is a topic-wise practice bank built from current high-signal frontend interview resources, especially:
- Front End Interview Handbook / GreatFrontEnd
- GreatFrontEnd JavaScript and React question collections
- Frontend machine-coding question repositories
- Company-style interview guides that highlight common UI/component builds

## How to use this repo
Use it as a question bank, not a reading list.

For every topic:
1. Solve from scratch without IntelliSense.
2. Write the simplest correct version first.
3. Add edge cases and follow-ups.
4. Explain trade-offs out loud.
5. Rewrite once from memory.

## Difficulty tiers
- **Tier 1: Service / easy-mid**
  - Basic implementation
  - Few edge cases
  - Low architectural depth
- **Tier 2: Mid product**
  - Realistic interview depth
  - Async, state, and UI edge cases
  - Moderate refactoring and reuse
- **Tier 3: Upper-mid product**
  - Extensibility, performance, accessibility, testing, architecture
  - Multiple follow-ups
  - Strong explanation required

---

# 1) JavaScript core utilities

## Tier 1
- Implement `debounce(fn, delay)`.
- Implement `throttle(fn, delay)`.
- Implement `flattenArray(arr)`.
- Implement `curry(fn)`.
- Implement `deepClone(obj)` for plain objects and arrays.
- Implement `groupBy(arr, keyFn)`.
- Implement `get(obj, path, defaultValue)`.
- Implement `shallowEqual(a, b)`.
- Implement `once(fn)`.
- Implement `memoize(fn)` for primitive args.

## Tier 2
- Implement `Promise.all`.
- Implement `Promise.race`.
- Implement `Promise.allSettled`.
- Implement `deepEqual(a, b)` for nested structures.
- Implement `flattenObject(obj)` to dot-path keys.
- Implement `call`, `apply`, `bind`.
- Implement `eventEmitter` with `on/off/emit/once`.
- Implement `retry(fn, attempts, delay)`.
- Implement `limitConcurrency(tasks, limit)`.
- Implement `LRUCache`.

## Tier 3
- Implement `deepClone` with circular references.
- Implement `promiseAll` with cancellation-aware discussion.
- Implement `memoize` with custom key resolver.
- Implement `mergeDeep` with explicit array strategy.
- Implement `compareObjects` with stable sort / custom comparator.
- Implement `pipe` and `compose` with async support.
- Implement `undoRedo(reducer)`.
- Implement `path update` helpers: `set`, `updateIn`, `removeIn`.
- Implement `diff` between two objects/arrays.
- Implement `cloneWithDescriptors` discussion.

## Interview-style follow-ups
- Why does `JSON.parse(JSON.stringify())` fail?
- Why do closures matter in debounce and throttle?
- How do microtasks differ from macrotasks?
- What is the cost of deep equality in UI code?
- When would you prefer a map over an object?

---

# 2) Output-based JavaScript snippets

## Tier 1
- `[] == ![]`
- `{} + []`
- `[] + {}`
- `typeof null`
- `NaN === NaN`
- `Object.is(NaN, NaN)`
- `console.log([1,2,3].map(parseInt))`
- `var` hoisting examples
- `let` temporal dead zone examples
- `this` inside regular vs arrow functions

## Tier 2
- Microtask vs macrotask ordering.
- `async/await` with `setTimeout`.
- Promise chaining output.
- `Promise.resolve().then(...)` vs `queueMicrotask(...)`.
- Nested closures and variable shadowing.
- `bind` with `new`.
- `call/apply` with null/undefined.
- `setTimeout` inside a loop with `var` and `let`.

## Tier 3
- Interleaving `await`, `then`, `setTimeout`, `queueMicrotask`.
- Output of getter/setter side effects.
- `Object.freeze` vs nested mutation.
- Prototype chain output.
- Class vs function constructor behavior.
- Order of execution in mixed sync/async snippets.

---

# 3) React fundamentals and hooks

## Tier 1
- Counter with increment/decrement/reset.
- Controlled input with validation.
- Toggle / accordion.
- Conditional render with loading and error.
- Lift state up between parent and child.
- List rendering with proper keys.
- Simple modal.
- Simple tabs.
- Basic useEffect fetch.

## Tier 2
- Search with debounce.
- Filtered list with derived state.
- Custom hook: `useDebounce`.
- Custom hook: `useLocalStorage`.
- Custom hook: `useClickOutside`.
- Custom hook: `useFetch`.
- Memoized list rows with `React.memo`.
- Stable callbacks with `useCallback`.
- Derived computations with `useMemo`.
- Cleanup in effects.

## Tier 3
- Compound components with context.
- Portal-based modal system.
- Accessible dropdown with keyboard nav.
- Optimized table with sort/filter/pagination.
- Virtualized list.
- Infinite scroll.
- Proper state partitioning for large components.
- Refactor from prop drilling to context carefully.
- Avoiding stale closures in hooks.
- Designing reusable hook APIs.

## Interview-style follow-ups
- Why does React re-render?
- What triggers `useEffect` again?
- When does memoization help or hurt?
- Why can inline objects/functions cause re-renders?
- Context vs Redux vs local state?

---

# 4) Machine coding / UI component builds

These are the most common style of questions for the level you are targeting.

## Tier 1
- Accordion.
- Todo list.
- Signup form.
- Modal dialog.
- Star rating.
- Tabs.
- Progress bar.
- Counter with reset and conditional rendering.
- Digital clock.
- Traffic light.

## Tier 2
- Data table with search and sort.
- File explorer from nested JSON.
- Image carousel.
- Transfer list.
- Todo list with filters and persistence.
- Autocomplete with debounce.
- Pagination widget.
- Search bar with query state.
- Form with validation and error display.
- Toast notifications.

## Tier 3
- Kanban board.
- Infinite scroll feed.
- Accessible dropdown with keyboard navigation.
- Rich data table with selection, pagination, filtering, and memoization.
- Nested tree editor.
- Multi-step form builder.
- Dashboard shell with route guards and data cache.
- Chip input with autosuggest.
- File explorer with lazy expansion and renaming.
- Time-range picker or calendar-style UI.

## Common follow-ups
- Support multiple open accordion sections.
- Add persistence.
- Add undo.
- Add keyboard shortcuts.
- Add loading/empty/error states.
- Make it accessible.
- Support large data efficiently.
- Add filtering/sorting/pagination.
- Refactor for reuse.
- Add tests.

---

# 5) DOM, browser, and event handling

## Tier 1
- Event bubbling and capturing examples.
- Event delegation for lists.
- `preventDefault` vs `stopPropagation`.
- Click outside detection.
- Focus/blur handling.
- Keyboard event handling.
- `closest()` usage.
- `contains()` usage.

## Tier 2
- Accessible custom dropdown.
- Modal focus handling.
- Scroll listener optimization.
- `requestAnimationFrame` throttle.
- Resize listener debouncing.
- IntersectionObserver-based lazy load.
- LocalStorage persistence.
- SessionStorage vs LocalStorage.
- Browser history state sync.
- URL query params sync.

## Tier 3
- Focus trap in modal.
- Accessibility tree and roles.
- Performance costs of layout thrashing.
- Critical rendering path discussion.
- XSS-safe rendering strategy.
- CORS / CSRF basics for frontend interviews.
- Avoiding memory leaks in event listeners.
- Managing global listeners in large apps.
- Browser caching implications.
- Network retry / backoff strategy.

---

# 6) State management and architecture

## Tier 1
- Local state vs lifted state.
- Context for theme / auth-like data.
- Redux Toolkit slice basics.
- Reducer purity.
- Derived state from store.
- Form state vs server state.

## Tier 2
- Normalized state shape.
- Memoized selectors.
- Redux vs Context trade-offs.
- Async loading / error / success state pattern.
- Pagination state architecture.
- Search/filter state separation.
- Undo/redo state wrapper.
- Reducer composition.
- Entity update helpers.
- Cache invalidation strategy.

## Tier 3
- Folder structure for a growing frontend app.
- Feature-first vs type-first structure.
- Shared components vs feature components.
- Where to place API code.
- Where to place utilities.
- State partitioning for large teams.
- Designing reusable UI libraries.
- Boundaries between hooks, services, and components.
- Architecture for dashboard/admin apps.
- Architecture for search-heavy apps.

---

# 7) TypeScript frontend questions

## Tier 1
- Type a simple component prop interface.
- Type a form object.
- Type array of records.
- Narrow union types.
- Use `Partial`, `Pick`, `Omit`, `Record`.
- Type a callback prop.

## Tier 2
- Generic `Table<T>`.
- Generic `groupBy<T, K>`.
- Generic `fetcher<T>`.
- Typed `useLocalStorage<T>`.
- Discriminated union for UI state.
- Typing `useReducer`.
- Typing event handlers.
- Typing refs.
- Typing reusable modal props.
- Typing async results.

## Tier 3
- Deep generic utility types.
- Conditional types for keys of object.
- Inferring types from API responses.
- Typing an event emitter.
- Typing a custom hook with overloads.
- Typing component composition APIs.
- Typing a form builder.
- Typing a reusable data table.
- Typing state machine-like UI state.
- Reducing overuse of `any`.

---

# 8) Testing with Jest / RTL

## Tier 1
- Test a counter.
- Test input change.
- Test conditional rendering.
- Test button click.
- Test a modal open/close.
- Test a list render.

## Tier 2
- Test async loading state.
- Test debounce behavior with fake timers.
- Test form validation.
- Test search filtering.
- Test keyboard navigation.
- Test callback invocation.
- Test route guards.
- Test custom hooks using hook testing approach.

## Tier 3
- Test virtualized list behavior.
- Test complex table behavior.
- Test accessible interactions.
- Test error recovery.
- Test optimistic UI flow.
- Decide what not to test.
- Avoid brittle snapshots.
- Mock network responsibly.
- Separate unit vs integration tests.
- Design testable component boundaries.

---

# 9) Frontend system design / LLD prompts

## Tier 1
- Design a modal system.
- Design a dropdown system.
- Design a basic todo app.
- Design a form with validation.
- Design a tab system.

## Tier 2
- Design autocomplete.
- Design data table.
- Design file explorer.
- Design pagination/search page.
- Design dashboard with filters.
- Design infinite scroll feed.
- Design notification/toast system.
- Design search suggestions.
- Design reusable form components.
- Design an image carousel.

## Tier 3
- Design a frontend architecture for a large enterprise dashboard.
- Design a component library strategy.
- Design a reusable table platform.
- Design a large search application.
- Design tree-based editors.
- Design multi-step workflows.
- Design state and URL synchronization.
- Design offline-friendly UI.
- Design a high-performance list rendering system.
- Design an app shell with route-based code splitting.

## What interviewers usually probe
- state boundaries
- data flow
- render performance
- accessibility
- error handling
- testing
- extensibility
- trade-offs

---

# 10) Company-style question mapping

These are the recurring shapes seen in frontend interview prep resources and company guides:

- Accordion
- Signup form
- Data table
- Todo list
- Tabs
- Modal dialog
- Image carousel
- Star rating
- Progress bar
- Traffic light
- Digital clock
- Stopwatch
- Transfer list
- File explorer
- Kanban board
- Infinite scroll
- Search/autocomplete
- Directory tree
- Chip input with autosuggest
- Relative time widget

---

# 11) Practical drill order for you

Use this order first:

1. debounce
2. throttle
3. curry
4. deepClone
5. deepEqual
6. Promise.all
7. eventEmitter
8. flattenArray
9. flattenObject
10. call/apply/bind
11. autocomplete
12. data table
13. file explorer
14. virtualized list
15. custom hooks
16. frontend system design
17. TypeScript types
18. RTL tests

---

# 12) What to say in interviews
For every solution, be ready to explain:
- why this data shape
- why this state location
- why this boundary
- what breaks at scale
- what trade-off was chosen
- how you would test it
- how you would make it accessible
- how you would make it reusable

---

# 13) Mistake log format
For each mistake, capture:
- topic
- exact bug
- why it happened
- corrected version
- one-line rule to remember

Example:
- Topic: debounce
- Bug: lost `this` binding
- Why: used arrow function incorrectly
- Rule: preserve call-site context when wrapping a method

---

# 14) Minimum pass bar for your target
You should be able to solve, explain, and refactor:
- 10 JS utilities
- 10 output snippets
- 8 React/UI components
- 3 machine coding problems
- 2 frontend system design prompts
- 1 RTL testing scenario
- 1 TypeScript typing exercise

without looking at syntax references.
