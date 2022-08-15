# [1] Prefix increments are cheaper than postfix increments
Use prefix increments (`++x`) instead of postfix increments (`x++`).

### Recommendation
Change all postfix increments to prefix increments.

# [2] Unnecessary checked arithmetic in for loops
There is no risk of overflow caused by increments to the iteration index in for loops (the `i++` in `for (uint256 i = 0; i < numIterations; i++)`). Increments perform overflow checks that are not necessary in this case.

### Recommendation
Surround the increment expressions with an `unchecked { ... }` block to avoid the default overflow checks. For example, change the loop
```
for (uint256 i = 0; i < numIterations; i++) {
  // ...
}
```
to
```
for (uint256 i = 0; i < numIterations;) {
  // ...
  unchecked { i++; }
}
```
It is a little less readable but it saves a significant amount of gas.
