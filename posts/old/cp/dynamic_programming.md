# Dynamic Programming

There are two approaches:

## 1. Tabulation (bottom-top)
- array that builds up one element at a time
- starts at 0, 1, or whatever the base case is

```python
// Tabulated version to find factorial x.
int dp[MAXN];

// base case
int dp[0] = 1;
for (int i = 1; i< =n; i++)
{
    dp[i] = dp[i-1] * i;
}
```

## 2. Memoization (top-bottom)
- recursion
- starts at value to find, recursively calls down to base case
- still uses array to keep values, only fills up with needed values instead of one by one

```python
// Memoized version to find factorial x.
// To speed up we store the values
// of calculated states

// initialized to -1
int dp[MAXN]

// return fact x!
int solve(int x)
{
    if (x==0)
        return 1;
    if (dp[x]!=-1)
        return dp[x];
    return (dp[x] = x * solve(x-1));
}
```

