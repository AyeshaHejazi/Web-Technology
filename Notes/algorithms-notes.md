# Algorithms Notes

## 1. Big O Basics

**Concept:** A way to describe how an algorithm's time/space needs grow as input size grows.

**Common complexities (fastest → slowest):**
```
O(1)        constant     — array index access
O(log n)    logarithmic  — binary search
O(n)        linear       — single loop through array
O(n log n)  linearithmic — efficient sorts (merge, quick avg case)
O(n²)       quadratic    — nested loops (bubble sort, selection sort)
```

**Gotchas:**
- Big O describes worst-case growth trend, not exact speed — an O(n) algorithm can still be slower than O(n²) for small n.
- Nested loops over the same input are almost always O(n²) — a common thing to look for when optimizing.

---

## 2. Sorting Algorithms

**Concept:** Arranging data in order — different algorithms trade off simplicity vs performance.

**Bubble Sort (simple, O(n²)):**
```javascript
function bubbleSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
    return arr;
}
```

**Merge Sort (efficient, O(n log n)):**
```javascript
function mergeSort(arr) {
    if (arr.length <= 1) return arr;
    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));
    return merge(left, right);
}

function merge(left, right) {
    const result = [];
    while (left.length && right.length) {
        result.push(left[0] <= right[0] ? left.shift() : right.shift());
    }
    return [...result, ...left, ...right];
}
```

**Gotchas:**
- Bubble/selection/insertion sort are easy to write and explain but impractical for large datasets — good for learning, not production.
- Built-in `Array.prototype.sort()` in modern JS engines is efficient (Timsort-like) — no need to hand-roll for real projects.

---

## 3. Searching Algorithms

**Concept:** Finding a value in a collection.

**Linear Search — O(n):**
```javascript
function linearSearch(arr, target) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === target) return i;
    }
    return -1;
}
```

**Binary Search — O(log n), requires sorted array:**
```javascript
function binarySearch(arr, target) {
    let low = 0, high = arr.length - 1;
    while (low <= high) {
        const mid = Math.floor((low + high) / 2);
        if (arr[mid] === target) return mid;
        if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

**Gotchas:**
- Binary search only works on sorted data — running it on an unsorted array gives wrong results silently.
- Off-by-one errors around `low`/`high`/`mid` are the most common bug — trace through a small example by hand when debugging.

---

## 4. Recursion Basics

**Concept:** A function that calls itself, breaking a problem into smaller subproblems, with a base case to stop.

**Syntax:**
```javascript
function factorial(n) {
    if (n <= 1) return 1;       // base case
    return n * factorial(n - 1); // recursive case
}
```

**Gotchas:**
- Missing or unreachable base case causes infinite recursion → stack overflow.
- Deep recursion (large n) can hit call stack limits — an iterative approach may be needed for large inputs.

---

## 5. Common Problem Patterns

| Problem | Idea |
|---|---|
| FizzBuzz | Loop 1–n, check divisibility by 3/5 |
| Palindrome check | Compare string to its reverse, or two-pointer from both ends |
| Prime number check | Test divisibility up to `sqrt(n)` only |
| Two Sum | Use a hash map to track complements in one pass — O(n) instead of O(n²) |
| Fibonacci | Recursion (simple but slow) vs iterative/memoized (fast) |

**Gotcha (Fibonacci especially):** naive recursive Fibonacci is O(2ⁿ) — exponential — because it recalculates the same subproblems repeatedly. Memoization (caching results) brings it down to O(n).

---

## Quick Reference: Common Mistakes
- Not identifying the base case before writing a recursive function.
- Reaching for nested loops (O(n²)) when a hash map could solve it in O(n).
- Running binary search on unsorted data.
- Optimizing prematurely — for small n, a simple O(n²) solution is often fine; only chase O(n log n) when data size actually demands it.
