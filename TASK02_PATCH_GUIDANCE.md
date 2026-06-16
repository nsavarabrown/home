# Phase 3 Task 02 Patch Guidance

This is a manual patch guide, not an automatic diff.

## 1. SheetAccess.gs

Inside `CPS.SheetAccess = (function () {`, replace:

```javascript
const C = CPS.CONSTANTS;
```

with:

```javascript
function constants() {
  return CPS.getConstants ? CPS.getConstants() : CPS.CONSTANTS;
}
```

Then replace uses of:

```javascript
C.HEADER_ROWS.DEFAULT
C.MASTER_SPREADSHEET_ID
```

with either:

```javascript
constants().HEADER_ROWS.DEFAULT
constants().MASTER_SPREADSHEET_ID
```

or define inside the function:

```javascript
const C = constants();
```

## 2. Logger.gs

Inside `CPS.Logger = (function () {`, replace:

```javascript
const C = CPS.CONSTANTS;
```

with:

```javascript
function constants() {
  return CPS.getConstants ? CPS.getConstants() : CPS.CONSTANTS;
}
```

Then add this near the start of each function that uses constants:

```javascript
const C = constants();
```

Functions that need it:

- `createRunContext`
- `logRunStart`
- `logRunComplete`
- `logFinding`
- `logRuntimeProblem`
- `withRun`

## 3. RegistryService.gs

Inside `CPS.RegistryService = (function () {`, replace:

```javascript
const C = CPS.CONSTANTS;
```

with:

```javascript
function constants() {
  return CPS.getConstants ? CPS.getConstants() : CPS.CONSTANTS;
}
```

Then resolve constants inside functions.

Also replace `smokeTestRegistryRead()` with this read-only version:

```javascript
function smokeTestRegistryRead() {
  const snapshot = getRegistrySnapshot();
  return {
    counts: snapshot.counts,
    totalRowsRead:
      snapshot.counts.employees +
      snapshot.counts.projects +
      snapshot.counts.tasks +
      snapshot.counts.trackers +
      snapshot.counts.templates +
      snapshot.counts.updateQueue
  };
}
```

## 4. README.md

Add Phase 3 Task 02 to the current implementation scope:

```markdown
Phase 3 Task 02 reviews and hardens the skeleton utilities before business modules are added.
```

Add the new doc:

```markdown
- `docs/PHASE3_IMPLEMENTATION_TASK_02.md`
```
