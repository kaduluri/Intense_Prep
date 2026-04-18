# 30-Day Senior Frontend Interview Plan (React + JavaScript)
Target: service-to-product switch, mid/upper-mid product companies, 7+ years frontend roles.
Primary goal: remove IntelliSense dependency, build from-scratch coding muscle, and raise interview performance for JS, React, machine coding, frontend LLD, and TypeScript.

## How to use this plan
- Keep one plain text editor profile with all auto-suggestions off.
- Write every solution from scratch before checking references.
- Maintain a mistake log every day.
- Speak answers out loud for senior-level reasoning.
- Every day includes React + JavaScript in parallel.
- Light revision days are for consolidation, not rest.

## Daily time split
- Weekdays: ~2 hours practice + up to 45 minutes commute revision.
- Weekends: 6 to 8 hours.
- Output drill should be done as a timed 15-minute sprint.

## What senior interviewers usually care about
- Can you write correct code from scratch.
- Can you reason about state, rendering, and side effects.
- Can you design maintainable frontend codebases.
- Can you handle edge cases and trade-offs clearly.
- Can you build interview-style machine coding solutions under pressure.

## Learning prompts to use every day
### Prompt 1: Interview mode
Act as a senior frontend interviewer hiring for a 7+ year React role in a product-based company. Give me a problem on [topic]. Do not give hints initially. After I respond, evaluate correctness, edge cases, performance, code quality, readability, and whether the solution feels senior-level. Then show an improved solution and ask 2 follow-up questions.

### Prompt 2: Concept-through-building mode
Teach me [concept] using a hands-on approach. Start with a small problem, let me attempt it, then increase difficulty. Include interview-style variations, real React usage, edge cases, and a short summary of the patterns at the end. Avoid long theory and focus on doing and building.

## 30-day schedule
### Day 1 - Intense
**Focus:** JavaScript execution model, scope, hoisting, closures + React state basics and render cycle

**React build:** Build a tiny counter with derived state, reset button, and conditional rendering. Explain why state resets or is preserved.

**JS coding questions:**
1. Implement `var`/`let`/`const` behavior examples and explain output.
2. Write `isEmptyObject(obj)` without using JSON.stringify.
3. Write `once(fn)` decorator.
4. Write `curry(fn)` for fixed arity.
5. Write `debounce(fn, delay)`.
6. Write `throttle(fn, delay)`.
7. Write `compose(...fns)`.
8. Write `pipe(...fns)`.
9. Write `pick(obj, keys)`.
10. Write `omit(obj, keys)`.

**Output / snippet drills:**
1. `console.log(a); var a = 5;`
2. `let x = 1; { console.log(x); let x = 2; }`
3. `function f(){ return arguments.length }`
4. `console.log(typeof null)`
5. `(() => { console.log(this) })()`

**Senior-level explanation checks:**
1. Explain why React state should be modeled with the minimal shape.
2. When would you lift state up vs keep it local?
3. What problems happen with duplicate state?

### Day 2 - Intense
**Focus:** Functions as values, this-binding, callbacks, and React props/state flow

**React build:** Build a parent-child form with controlled input, submit, reset, and prop drilling; refactor to lift state up.

**JS coding questions:**
1. Write `call`, `apply`, and `bind` polyfills.
2. Implement `partial(fn, ...presetArgs)`.
3. Implement `memoize(fn)` with primitive arguments.
4. Implement `reArg(fn, order)`.
5. Write `isFunction(value)` robustly.
6. Implement `maybe(fn)` or null-safe callback wrapper.
7. Write a curried adder that supports chained invocation.
8. Write `deepEqual(a, b)` for plain objects.
9. Write `shallowEqual(a, b)`.
10. Write `cloneShallow(value)`.

**Output / snippet drills:**
1. `const obj = { x: 1, f(){ return this.x } }; const g = obj.f; console.log(g())`
2. `function a(){ return () => this }`
3. `console.log((function(){ return this }).call(null))`
4. `console.log([1,2,3].map(parseInt))`
5. `console.log(Boolean([]), Boolean({}))`

**Senior-level explanation checks:**
1. Why does `React.memo` depend on prop stability?
2. Why can inline object props trigger re-renders?
3. How would you make callbacks stable?

### Day 3 - Intense
**Focus:** Arrays and objects deeply, iteration patterns, immutable updates in React

**React build:** Build editable list items with add/edit/delete; ensure updates are immutable and explain why.

**JS coding questions:**
1. Write `flattenArray(arr)` iteratively and recursively.
2. Write `flattenObject(obj, prefix)` to dot-path keys.
3. Write `groupBy(arr, keyFn)`.
4. Write `countBy(arr, keyFn)`.
5. Write `uniqueBy(arr, keyFn)`.
6. Write `get(obj, path, defaultValue)`.
7. Write `set(obj, path, value)` immutably.
8. Write `updateIn(obj, path, updater)`.
9. Write `sortBy(arr, iteratees)`.
10. Write `mergeDeep(a, b)` for plain objects.

**Output / snippet drills:**
1. `console.log({} + [])`
2. `console.log([] + {})`
3. `console.log([1,2] + [3,4])`
4. `console.log(typeof NaN)`
5. `console.log(Object.is(NaN, NaN))`

**Senior-level explanation checks:**
1. How do you avoid mutation bugs in reducers and state updates?
2. Why is deep equality expensive in UI state?
3. When is cloning unnecessary?

### Day 4 - Intense
**Focus:** Closures, event loop, microtasks/macrotasks, async JavaScript

**React build:** Build a debounced search box with loading state and stale-request handling.

**JS coding questions:**
1. Write `sleep(ms)` returning a Promise.
2. Write `timeoutPromise(promise, ms)`.
3. Write `retry(fn, attempts, delay)`.
4. Implement `promiseAll(promises)`.
5. Implement `promiseRace(promises)`.
6. Implement `promiseAllSettled(promises)`.
7. Write `sequence(tasks)` to run async functions in order.
8. Write `parallelLimit(tasks, limit)`.
9. Write `abortableFetchLike` using an AbortController style pattern.
10. Write `eventEmitter()` with on/off/emit/once.

**Output / snippet drills:**
1. `console.log('A'); setTimeout(()=>console.log('B')); Promise.resolve().then(()=>console.log('C'));`
2. `async function f(){ console.log(1); await 0; console.log(2) }`
3. `Promise.resolve().then(()=>console.log(1)); queueMicrotask(()=>console.log(2))`
4. `setTimeout(()=>console.log(1),0); setTimeout(()=>console.log(2),0)`
5. `console.log('start'); setTimeout(()=>console.log('timeout')); console.log('end')`

**Senior-level explanation checks:**
1. Why can stale network responses break UX?
2. How would you cancel or ignore old requests?
3. How do microtasks affect UI updates?

### Day 5 - Intense
**Focus:** Prototypes, classes under the hood, inheritance, and React composition patterns

**React build:** Build a reusable modal system with compound components and portal behavior.

**JS coding questions:**
1. Write `createObject(proto)`.
2. Write `instanceOf(left, right)` polyfill.
3. Write `inherits(child, parent)` helper.
4. Write `new` operator polyfill.
5. Write a simple `EventEmitter` with listener priorities.
6. Write `Object.freeze`-like shallow freeze explanation and wrapper.
7. Write `isPrototypeOf`-style helper.
8. Write `deepClone` for objects, arrays, Date, RegExp.
9. Write `cloneWithCircularRefs` using WeakMap.
10. Write `equalsIgnoringOrder` for arrays of primitives.

**Output / snippet drills:**
1. `function A(){}; A.prototype.x = 1; const a = new A(); console.log(a.__proto__ === A.prototype)`
2. `console.log(Object.create(null))`
3. `console.log(new Boolean(false) ? 'yes' : 'no')`
4. `console.log(({}).toString.call([]))`
5. `console.log(typeof class X {})`

**Senior-level explanation checks:**
1. Why prefer composition over inheritance in React?
2. What are compound components and why use them?
3. How do portals help in UI architecture?

### Day 6 - Intense
**Focus:** DOM events, bubbling/capture, uncontrolled vs controlled, accessibility basics

**React build:** Build a custom dropdown with keyboard navigation, click-outside, and accessible roles.

**JS coding questions:**
1. Write `delegate(root, eventType, selector, handler)`.
2. Write `addEventOnce(element, type, handler)`.
3. Write `closestPolyfill(el, selector)`.
4. Write `containsPolyfill(parent, child)`.
5. Write a simplified `querySelectorAllByClass` idea explanation.
6. Write `isElementVisible(el)` heuristic.
7. Write `getEventPath(event)` wrapper.
8. Write `stopPropagationDemo()` example.
9. Write `preventDefaultDemo()` example.
10. Write `rafThrottle(fn)`.

**Output / snippet drills:**
1. `button.addEventListener('click', fn); button.click();`
2. `event.currentTarget` vs `event.target` explanation drill
3. `console.log('a'); Promise.resolve().then(()=>console.log('b'));`
4. `document.body.contains(document.body)`
5. `console.log('1'); setTimeout(()=>console.log('2'), 0);`

**Senior-level explanation checks:**
1. How do you make custom widgets accessible?
2. How do event delegation and controlled inputs improve scalability?
3. What are common sources of unnecessary event handlers?

### Day 7 - Light Revision
**Focus:** Review week 1 mistakes, rewrite core utilities without help, and speak answers aloud

**React build:** Rewrite the dropdown, counter, and form exercises from memory. No new concept today.

**JS coding questions:**
1. Redrive from memory: debounce, throttle, curry, deepClone.
2. Redrive from memory: promiseAll, eventEmitter, flattenArray.
3. Redrive from memory: call/apply/bind.
4. Redrive from memory: deepEqual and get/set path helpers.
5. Write 5 mini snippets from your mistake log.

**Output / snippet drills:**
1. Review your top 10 wrong outputs from this week.
2. Explain one bug you hit and how you fixed it.

**Senior-level explanation checks:**
1. Summarize 3 lessons from the week.
2. Explain your decision tree for choosing local state vs context vs Redux.

### Day 8 - Intense
**Focus:** React hooks internals, useEffect pitfalls, dependency arrays, state management

**React build:** Build a search + filter list with effects, cleanup, loading, and error handling.

**JS coding questions:**
1. Write `usePrevious` style logic outside React as a closure exercise.
2. Write `compareDeps(prev, next)` shallow compare.
3. Write `debounce` again with leading/trailing options.
4. Write `throttle` again with leading/trailing options.
5. Write `createStore(reducer)` minimal Redux.
6. Write `combineReducers` simple version.
7. Write immutable `updateNestedState` helper.
8. Write `selectorMemoize` basic version.
9. Write `normalizeData` helper.
10. Write `diffArrays` helper.

**Output / snippet drills:**
1. Why does `useEffect` run twice in dev Strict Mode?
2. Why are dependencies compared by reference?
3. `console.log(useEffect)` is invalid in render — why?

**Senior-level explanation checks:**
1. When should state be lifted up?
2. When should Context be avoided for fast-changing data?
3. What are the costs of overusing effects?

### Day 9 - Intense
**Focus:** useMemo/useCallback/React.memo, render optimization, reference stability

**React build:** Optimize a table/list UI with memoized rows and stable callbacks.

**JS coding questions:**
1. Write `memoizeOne(fn)`.
2. Write `cacheLastCall(fn)`.
3. Write `areShallowEqualProps(prev, next)`.
4. Write `stableSort` helper.
5. Write `chunkArray(arr, size)`.
6. Write `windowedSlice(arr, start, size)`.
7. Write `findIndexBy(arr, predicate)`.
8. Write `sumBy(arr, selector)`.
9. Write `maxBy(arr, selector)`.
10. Write `minBy(arr, selector)`.

**Output / snippet drills:**
1. `const a = {x:1}; const b = {x:1}; console.log(a===b)`
2. `useCallback(() => {}, [])` vs inline function
3. `memo` with children prop
4. `useMemo(() => ({a:1}), [])`
5. `console.log([NaN].indexOf(NaN))`

**Senior-level explanation checks:**
1. When does memoization hurt instead of help?
2. How do stable references affect child renders?
3. What would you measure before optimizing?

### Day 10 - Intense
**Focus:** Custom hooks design and reusable logic

**React build:** Build `useDebounce`, `useFetch`, `useLocalStorage`, and `useClickOutside` in a mini hooks library.

**JS coding questions:**
1. Write a generic state container function.
2. Write observer-subscription style utility.
3. Write `onceAsync(fn)`.
4. Write `limitConcurrency(tasks, n)`.
5. Write `cacheByKey(fn)`.
6. Write `extractErrorMessage(err)`.
7. Write `safeParseJSON(str, fallback)`.
8. Write `deepMerge` with array handling choice documented.
9. Write `composeHooks` conceptually as function composition.
10. Write `createCancelablePromise`.

**Output / snippet drills:**
1. Why must custom hooks start with `use`?
2. Why should hooks not be called conditionally?
3. What is the benefit of returning stable APIs from hooks?

**Senior-level explanation checks:**
1. How do you design a hook for reuse across teams?
2. How do you document hook contracts?
3. What belongs in a hook vs a component?

### Day 11 - Intense
**Focus:** Redux Toolkit mental model, reducer design, state normalization, side effects

**React build:** Build a small task manager with Redux Toolkit-like slices or a minimal store; include async loading and error states.

**JS coding questions:**
1. Write a minimal `createSlice` idea in plain JS.
2. Write `dispatchQueue` for ordered actions.
3. Write `undoRedo(reducer)` wrapper.
4. Write `selector` helpers for normalized data.
5. Write `memoizedSelector` with last args cache.
6. Write `entityAdapter` style add/update/remove helpers.
7. Write `serializeState`/`hydrateState` considerations.
8. Write `isPlainObject`.
9. Write `updateArrayItemById` immutably.
10. Write `removeArrayItemById` immutably.

**Output / snippet drills:**
1. Why can reducers not mutate state?
2. What is the difference between action and reducer?
3. Why does normalized state help in large apps?

**Senior-level explanation checks:**
1. Redux vs Context vs local state: defend the choice.
2. How would you structure app state for scale?
3. Where do you put server cache vs UI state?

### Day 12 - Intense
**Focus:** Forms: controlled/uncontrolled, validation, dynamic fields, UX edge cases

**React build:** Build a multi-step form with validation, touched state, conditional fields, and reset.

**JS coding questions:**
1. Write `validateRequired` / `validateEmail` helpers.
2. Write `validateForm(schema, values)`.
3. Write `getDirtyFields(initial, current)`.
4. Write `toFormData` style serializer concept.
5. Write `parseNumberInput` safely.
6. Write `fieldArrayAdd/remove/update` helpers.
7. Write `isValidDateString`.
8. Write `normalizePhoneInput` helper.
9. Write `formatCurrencyInput` helper.
10. Write `debouncedValidator`.

**Output / snippet drills:**
1. Why is `value={undefined}` dangerous in controlled inputs?
2. Controlled vs uncontrolled input trade-off.
3. Why can form state get out of sync?

**Senior-level explanation checks:**
1. How would you structure form logic in a reusable way?
2. How do you prevent validation from becoming spaghetti?
3. What accessibility attributes belong in forms?

### Day 13 - Intense
**Focus:** TypeScript for frontend interviews: generics, utility types, narrowing, typing hooks

**React build:** Type a reusable table component, a custom hook, and a modal API with TS.

**JS coding questions:**
1. Type `deepClone` function signature with generics.
2. Write `flattenArray<T>` signature.
3. Write `groupBy<T, K>` signature.
4. Write `KeysOfType<T, V>` utility.
5. Write `NonNullableDeep<T>` concept.
6. Write `EventMap` typing for event emitter.
7. Write typed `promiseAll` signature.
8. Write typed `createStore` signatures.
9. Write discriminated union examples.
10. Write generic `apiResponse<T>` types.

**Output / snippet drills:**
1. Narrowing with `typeof`, `in`, and custom type guards.
2. Why does `any` hide bugs?
3. When is `unknown` better than `any`?

**Senior-level explanation checks:**
1. Where does TypeScript improve architecture and where does it not?
2. How do you type reusable components without overengineering?
3. What are the risks of overly complex generics?

### Day 14 - Light Revision
**Focus:** Review hooks, state management, and TypeScript mistakes; rebuild 2 utilities from scratch

**React build:** Refactor one hook and one form from memory. Explain every dependency choice.

**JS coding questions:**
1. Rebuild debounce, promiseAll, eventEmitter.
2. Rebuild flattenObject, deepEqual, deepClone.
3. Rebuild one TS generic exercise from memory.

**Output / snippet drills:**
1. Review the 10 hardest output snippets from weeks 1-2.

**Senior-level explanation checks:**
1. State your top 5 bugs so far and how you are eliminating them.

### Day 15 - Intense
**Focus:** Machine coding round 1: searchable autocomplete with keyboard navigation and caching

**React build:** Build autocomplete end-to-end: debounce, fetch simulation, keyboard nav, highlight, empty/error states.

**JS coding questions:**
1. Write `prefixSearch` helper.
2. Write `fuzzyMatch` simple implementation.
3. Write `LRUCache` basic class/function.
4. Write `debouncePromise`.
5. Write `dedupeByKey`.
6. Write `paginate` helper.
7. Write `highlightMatches(text, query)`.
8. Write `sortSuggestions`.
9. Write `stableIdGenerator`.
10. Write `normalizeQuery`.

**Output / snippet drills:**
1. Why does keyboard navigation require careful state handling?
2. Why can debounce improve both performance and UX?
3. When should you cache search results?

**Senior-level explanation checks:**
1. Talk through component boundaries for autocomplete.
2. Where do you keep query, selected item, and cache?
3. How do you make it accessible?

### Day 16 - Intense
**Focus:** Machine coding round 2: data table with sorting, filtering, pagination, selection

**React build:** Build a reusable table with memoized rows, selection, sort controls, and empty state.

**JS coding questions:**
1. Write `sortComparatorFactory`.
2. Write `multiSort` helper.
3. Write `filterRows` by multiple criteria.
4. Write `paginateRows`.
5. Write `getSelectedIds`/`toggleSelectedId`.
6. Write `selectAllVisible`.
7. Write `extractColumns` from data.
8. Write `rowKey` helper.
9. Write `computeTableStats`.
10. Write `tableStateReducer`.

**Output / snippet drills:**
1. Why can unstable row keys break selection state?
2. Why does derived state in tables often get duplicated accidentally?
3. What is the cost of sorting on every render?

**Senior-level explanation checks:**
1. How would you design the table API for reuse?
2. What would you memoize and what would you not?
3. How would you test the table behavior?

### Day 17 - Intense
**Focus:** Machine coding round 3: file explorer / tree UI / nested data

**React build:** Build a file explorer with expand/collapse, lazy nested rendering, and node actions.

**JS coding questions:**
1. Write tree traversal DFS helpers.
2. Write tree traversal BFS helpers.
3. Write `findNodeById`.
4. Write `updateNodeById` immutably.
5. Write `removeNodeById` immutably.
6. Write `insertNodeUnderParent`.
7. Write `flattenTree`.
8. Write `getPathToNode`.
9. Write `reorderSiblings`.
10. Write `treeToMap` normalization.

**Output / snippet drills:**
1. Why do nested UIs often benefit from normalization?
2. Why can recursive rendering be elegant but risky?
3. What happens to state when tree structure changes?

**Senior-level explanation checks:**
1. How would you prevent unnecessary re-renders in deep trees?
2. How would you represent expansion state?
3. What are the trade-offs of recursion vs iterative rendering?

### Day 18 - Intense
**Focus:** Performance and virtualization, windowing, large lists, and React profiling mindset

**React build:** Build a virtualized list or table slice with fixed row height assumptions and explain limitations.

**JS coding questions:**
1. Write `range(start, end)` generator-like helper.
2. Write `virtualWindow(scrollTop, rowHeight, viewportHeight, total)`.
3. Write `binarySearchByOffset` for virtual list index lookup.
4. Write `overscanRange` helper.
5. Write `throttleWithRaf`.
6. Write `measureVisibleItems` conceptually.
7. Write `calcScrollToIndex`.
8. Write `stickyHeader` math helper.
9. Write `clamp(value, min, max)`.
10. Write `debouncedResizeHandler`.

**Output / snippet drills:**
1. Why does virtualization improve performance?
2. Why does memoization alone not fix large-list performance?
3. Why can frequent state updates still hurt?

**Senior-level explanation checks:**
1. How would you explain trade-offs of fixed vs variable row height?
2. What profiling evidence would you gather before optimizing?
3. How would you test virtualized behavior?

### Day 19 - Intense
**Focus:** Design patterns and architecture for frontend codebases

**React build:** Refactor one earlier machine-coding project into a clean folder structure with components/hooks/utils/services/state.

**JS coding questions:**
1. Write a `factory` pattern example.
2. Write a `strategy` pattern example.
3. Write a `observer` pattern example.
4. Write a `command` pattern example.
5. Write a `adapter` pattern example.
6. Write a `facade` pattern example.
7. Write a `singleton` caveat discussion.
8. Write a `service layer` interface example.
9. Write a `repository`-like abstraction for API calls.
10. Write a `plugin` architecture sketch.

**Output / snippet drills:**
1. Why is composition a strong default in UI code?
2. When should logic move out of components?
3. What does separation of concerns mean in practice?

**Senior-level explanation checks:**
1. How would you structure a new frontend repo from day one?
2. When do you create shared utilities vs keep things local?
3. How do you avoid a giant utils folder?

### Day 20 - Intense
**Focus:** Testing React with RTL/Jest, behavior-first tests, and avoiding brittle tests

**React build:** Test one previous component thoroughly: loading, success, error, keyboard interaction, and callbacks.

**JS coding questions:**
1. Write test cases for `debounce` and `throttle` behavior.
2. Write test cases for `deepClone` edge cases.
3. Write test cases for `promiseAll` rejection behavior.
4. Write test cases for table sorting and pagination logic.
5. Write test cases for form validation helpers.
6. Write test cases for tree update helpers.
7. Write `mockFetch` thinking exercise.
8. Write `fakeTimers` usage notes.
9. Write `spyOn` vs `mockFn` usage notes.
10. Write `assertDeepEqual` helper idea.

**Output / snippet drills:**
1. Why should tests focus on user behavior rather than implementation details?
2. Why are snapshots often overused?
3. What makes a test flaky?

**Senior-level explanation checks:**
1. How do you decide what to test at component level vs utility level?
2. How do you keep tests maintainable in a large app?
3. How do you test async UI safely?

### Day 21 - Light Revision
**Focus:** Review machine coding mistakes, redesign one component from scratch, revisit JS weak spots

**React build:** Pick the weakest of autocomplete/table/file explorer and rebuild only the architecture and state model.

**JS coding questions:**
1. Rebuild one hard utility from memory.
2. Re-solve 5 output snippets from your mistake log.

**Output / snippet drills:**
1. Explain 3 render bugs you encountered in React.

**Senior-level explanation checks:**
1. What design habit improved your code most this week?

### Day 22 - Intense
**Focus:** Frontend system design 1: design a dashboard / admin app with filters, tables, auth, and caching

**React build:** Draw architecture and implement one slice: routing, protected routes, data fetching, loading skeletons, and cache invalidation.

**JS coding questions:**
1. Write `routeGuard` helper logic.
2. Write `accessControl` mapping helper.
3. Write `cacheKey` generator.
4. Write `ttlCache` basic implementation.
5. Write `invalidateByPrefix` helper.
6. Write `serializeQueryParams`.
7. Write `parseQueryParams`.
8. Write `skeletonState` decision helper.
9. Write `retryWithBackoff`.
10. Write `requestDeduplication` helper.

**Output / snippet drills:**
1. Why is auth state different from UI state?
2. Why do query params matter for dashboards?
3. What is the trade-off of caching client-side data?

**Senior-level explanation checks:**
1. How would you structure the dashboard codebase?
2. How do you split reusable pieces from feature code?
3. How do you keep route changes from causing global chaos?

### Day 23 - Intense
**Focus:** Frontend system design 2: design a search-heavy product UI with pagination, debouncing, and analytics

**React build:** Add analytics events, search history, and URL state synchronization to a previous search component.

**JS coding questions:**
1. Write `analyticsEvent` wrapper.
2. Write `debounceWithFlush`.
3. Write `historyStateSync` helper.
4. Write `urlStateSerializer`.
5. Write `urlStateParser`.
6. Write `trackOncePerSession`.
7. Write `eventBatcher`.
8. Write `visibilityTracker`.
9. Write `filterChipsReducer`.
10. Write `facetStateManager`.

**Output / snippet drills:**
1. Why should UI state sometimes live in the URL?
2. Why are analytics calls often batched?
3. What can go wrong when syncing state and URL?

**Senior-level explanation checks:**
1. How do you make search fast and debuggable?
2. How do you observe UX without polluting feature code?
3. How would you test URL-state synchronization?

### Day 24 - Intense
**Focus:** Interview-style JS coding round: mixed utilities, edge cases, and code clarity under time pressure

**React build:** Take one utility and expose it in a small demo UI with controls and live output.

**JS coding questions:**
1. Implement `deepClone` with circular refs.
2. Implement `deepEqual` with arrays and nested objects.
3. Implement `flattenArray` with depth parameter.
4. Implement `flattenObject`.
5. Implement `promiseAll`.
6. Implement `promiseRace`.
7. Implement `eventEmitter`.
8. Implement `curry`.
9. Implement `memoize`.
10. Implement `groupBy`.

**Output / snippet drills:**
1. `console.log(0.1 + 0.2 === 0.3)`
2. `console.log([] == ![])`
3. `console.log('' == 0)`
4. `console.log(null == undefined)`
5. `console.log(Number.isNaN(NaN), isNaN('foo'))`

**Senior-level explanation checks:**
1. Which bugs are logic bugs vs JavaScript quirks?
2. How do you explain a solution cleanly under pressure?
3. How do you know your edge cases are enough?

### Day 25 - Intense
**Focus:** Mock interview 1: JS + React mixed round

**React build:** Refactor an older component or build a small feature from scratch within a timed 90-minute slot.

**JS coding questions:**
1. Solve 5 unseen coding utilities from scratch.
2. Solve 5 output snippets from scratch.
3. Explain performance implications of your solutions.

**Output / snippet drills:**
1. Timed output prediction set of 10 mixed snippets.

**Senior-level explanation checks:**
1. Defend one design choice and one trade-off for every solution.

### Day 26 - Intense
**Focus:** Mock interview 2: machine coding round with constraints and follow-up requirements

**React build:** Choose one of: autocomplete, table, file explorer, or form builder and build it as a timed round.

**JS coding questions:**
1. Implement the state model first before UI.
2. Implement one utility needed by the feature.
3. Implement a reusable subcomponent.
4. Implement error/empty/loading states.
5. Implement one optimization.

**Output / snippet drills:**
1. Explain why your architecture supports extension.
2. Explain one thing you would not do in production.

**Senior-level explanation checks:**
1. How did you keep the codebase maintainable?
2. What would you change if requirements doubled?

### Day 27 - Intense
**Focus:** Frontend interview theory through practice: browser basics, performance, security, network, accessibility

**React build:** Audit one component for accessibility, performance, and resilience.

**JS coding questions:**
1. Write a `safeHtmlEscape` utility idea.
2. Write `sanitizeExternalLinkProps` helper.
3. Write `retryOnTransientError` strategy.
4. Write `isSameOrigin` helper.
5. Write `parseJWT` considerations discussion.
6. Write `perfMark` / `perfMeasure` helper usage notes.
7. Write `intersectionObserver` usage scenario notes.
8. Write `lazyLoadImage` helper.
9. Write `focusTrap` outline.
10. Write `ariaLabelFactory` helper.

**Output / snippet drills:**
1. Why can index keys be dangerous?
2. Why is accessibility not just for compliance?
3. How do you reduce layout thrashing?

**Senior-level explanation checks:**
1. What browser behaviors have you had to debug in real work?
2. How do you balance safety and usability?
3. How do you design for keyboard users?

### Day 28 - Light Revision
**Focus:** Revise architecture, TypeScript, testing, and the hardest 20 snippets

**React build:** Summarize your best component architecture choices and rewrite one feature as pseudocode only.

**JS coding questions:**
1. Re-solve the 10 hardest utilities you struggled with.
2. Re-answer 10 output snippets from memory.

**Output / snippet drills:**
1. Do one self-grading pass on output questions.

**Senior-level explanation checks:**
1. Write your personal checklist for 'senior-level code'.

### Day 29 - Intense
**Focus:** Full mock loop 1: coding + React + explanation + refactor

**React build:** Build one timed feature end-to-end and then refactor it for readability and reuse.

**JS coding questions:**
1. One timed JS round: 3 utilities, 3 snippets, 1 async problem.
2. One timed React round: state, effects, derived data, and cleanup.

**Output / snippet drills:**
1. Explain the main bug you nearly introduced and how you avoided it.

**Senior-level explanation checks:**
1. Explain how a strong reviewer would critique your code.

### Day 30 - Intense
**Focus:** Final mock interview day and resume/project storytelling

**React build:** Run one final timed build and narrate decisions like an interview candidate.

**JS coding questions:**
1. Final from-memory run: debounce, promiseAll, deepClone, eventEmitter.
2. Final from-memory run: flattenObject, deepEqual, curry, call/apply/bind.

**Output / snippet drills:**
1. Final 15 output predictions from the mistake log.

**Senior-level explanation checks:**
1. Explain your strongest project as a product-story, not a feature list.
2. Explain why you are now safer to hire than on day 1.

## Repeated core utilities to master by the end
- debounce
- throttle
- curry
- deepClone
- deepEqual
- flattenArray
- flattenObject
- promiseAll
- promiseRace
- eventEmitter
- call/apply/bind
- memoize
- get/set by path
- groupBy
- mergeDeep

## Repeated React patterns to master by the end
- controlled and uncontrolled inputs
- lifting state up
- derived state
- memoization and stable references
- custom hooks
- compound components
- portals
- accessibility
- virtualization
- Redux Toolkit / Context / local state trade-offs
- async loading and error handling
- testing with RTL

## Project / machine-coding themes to rotate later
- autocomplete
- data table
- file explorer
- modal system
- form builder
- infinite list
- dashboard shell
- search page with filters and URL state

## Final interview checklist
- Can write core utilities without seeing a reference.
- Can explain render behavior, effects, memoization, and state flow.
- Can design a maintainable frontend codebase.
- Can build a medium machine-coding solution under time pressure.
- Can discuss testing, performance, accessibility, and trade-offs.
- Can explain one project deeply and honestly.
