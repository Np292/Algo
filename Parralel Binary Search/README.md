
---

## Parallel Binary Search

**Parallel Binary Search** is a technique used to compute the result of multiple Binary Search queries more efficiently by processing them together, instead of one at a time.

---

## Meteors Problem

In the [Meteors problem on SPOJ](https://www.spoj.com/problems/METEORS/), a naive solution involves performing binary search combined with a segment tree, resulting in a time complexity of \( O(nq \log^2 n) \).

Here is a code snippet of this naive approach:

```cpp
for (int i = 1; i <= n; i++) {
    int l = 1, r = q;
    while (l <= r) {
        int mid = (l + r) / 2;
        for (int j = 1; j <= mid; j++) {
            upd(j);
        }
        if (get(i) >= p[i]) {
            r = mid - 1;
        } else {
            l = mid + 1;
        }
    }
    r++;
    if (r > q) {
        cout << "NIE\n";  // "No" in Polish, indicating failure
    } else {
        cout << r << '\n';  // Output the result
    }
}
```

### Optimized Solution with Parallel Binary Search

We can improve this solution by noticing that if \( mid_1 \leq mid_2 \leq \dots \leq mid_n \) holds, we can apply the **two-pointer method**. To ensure this, we sort the Binary Search queries by their mid-values, repeating the process \( \log_2(q) \) times. This leads to an improved time complexity of \( O((n + q) \log^2 q) \).

Here is the optimized approach:

```cpp
for (int cycle = 0; cycle <= log2(q); cycle++) {
    sort(pbs);  // pbs is a vector containing l, r, mid, sorted by mid-value
    int j = 1;
    for (auto i : pbs) {
        int mid = (i.l + i.r) / 2;
        if (i.l > i.r) continue;  // Skip if the solution is already found
        while (j <= mid) {
            upd(j);
            j++;
        }
        if (get(i) >= p[i]) {
            i.r = mid - 1;
        } else {
            i.l = mid + 1;
        }
    }
}
```

---

## Almaty and Haziks Problem

In [Problem A](https://codeforces.com/contestInvitation/19d60a2ccc01ea29c4bb5a8c9cc24f7809853b0b), the solution is similar to the Meteors problem.

A **naive solution** involves Binary Search with **Disjoint Set Union (DSU)**, resulting in a time complexity of \( O(qm \log^2 m) \).

To improve this, you can use **Parallel Binary Search with DSU**, reducing the time complexity to \( O((q + m) \log^2 m) \).

Additionally, you can further optimize the solution using the inverse Ackermann function, which helps with DSU operations.

---
