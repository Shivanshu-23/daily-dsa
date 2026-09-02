# DSA Notes

# P01 - Two Pointers: Opposite Ends

> **Pattern:** Sorted array/string + pair/triplet condition.
>
> Use two pointers from opposite ends to reduce the search space.

---

# 1. Valid Palindrome

## String Cleaning

```java
s = s.toLowerCase().replaceAll("[^a-z0-9]", "");
```

### Remember

```text
toLowerCase() → convert to lowercase

[^a-z0-9] → NOT a-z or 0-9

replaceAll(..., "") → remove
```

### Regex

```text
[a-z]   → lowercase letters
[0-9]   → digits
^       → NOT when inside [ ]
""      → replace with nothing
```

### Example

```text
"A man, a plan!"
        ↓
"a man, a plan!"
        ↓
"amanaplan"
```

### Two Pointer

```text
l → left
r → right
```

Compare:

```text
s.charAt(l)
s.charAt(r)
```

Then:

```text
same       → l++, r--
different  → not palindrome
```

### Core Pattern

```text
Clean string
     ↓
l = 0
r = n - 1
     ↓
Compare both ends
     ↓
Move inward
```

---

# 2. Squares of a Sorted Array

## Key Observation

The array is sorted, but negative numbers can become large after squaring.

Example:

```text
[-7, -3, 2, 3, 11]
 ↑             ↑
 49            121
```

Therefore, the largest square can only come from:

```text
leftmost element
        OR
rightmost element
```

---

## Two Pointers

```java
int ls = nums[l] * nums[l];
int rs = nums[r] * nums[r];
```

Compare:

```text
ls > rs
   ↓
put ls at the end
   ↓
l++
```

Otherwise:

```text
rs >= ls
   ↓
put rs at the end
   ↓
r--
```

---

## Why Fill From the End?

We find the **largest square first**.

Therefore:

```text
largest      → last position
2nd largest  → second-last
3rd largest  → third-last
```

Use:

```java
p = n - 1;
```

Then:

```java
p--;
```

---

# Why `l <= r`?

Ask:

> **Do I need to process the case where `l == r`?**

YES.

When:

```text
l == r
```

there is still **one element remaining**.

That element must be processed.

Therefore:

```java
while (l <= r)
```

---

## `l < r` vs `l <= r`

### `l <= r`

Use when the last remaining element needs to be processed.

```text
l < r   → multiple elements
l == r  → one element remaining
```

Example:

```text
Sorted Squares
```

Therefore:

```java
while (l <= r)
```

---

### `l < r`

Use when the meeting point does NOT need to be processed.

Usually pair problems.

Example:

```text
Two Sum II
```

When:

```text
l == r
```

there are not two different elements left to form a pair.

Therefore:

```java
while (l < r)
```

---

## Quick Decision Rule

```text
Do I need to process l == r?

YES → l <= r

NO  → l < r
```

### Memory Trick

```text
l <= r → process last element
l < r  → stop when pointers meet
```

---

# In-Place vs Extra Array

When solving an array problem, ask:

> **What am I reading and what am I writing?**

If reading:

```java
nums[l]
nums[r]
```

and writing:

```java
nums[p]
```

ask:

> **Can my write overwrite a value that I still need to read?**

If YES:

```text
Use a separate array
```

Example:

```java
int[] ans = new int[nums.length];
```

---

## Memory Trick

```text
READ  → What information do I still need?

WRITE → What information am I destroying?
```

### Problem-Solving Order

```text
Correct solution
      ↓
Analyze complexity
      ↓
Optimize
```

Don't try to force `O(1)` space before proving that the in-place solution is safe.

---

# 3. Two Sum II - Input Array Is Sorted

## Key Observation

The array is already sorted.

Use:

```text
l → beginning
r → end
```

Calculate:

```java
sum = nums[l] + nums[r];
```

---

## Pointer Movement

### `sum == target`

Found the answer:

```java
if (sum == target) {
    return new int[]{l + 1, r + 1};
}
```

### Why `l + 1` and `r + 1`?

Java uses:

```text
0-based indexing
```

But the problem may ask for:

```text
1-based indexing
```

Therefore:

```text
Java index → Problem position

l → l + 1
r → r + 1
```

---

## `sum < target`

Need a **larger sum**.

Since the array is sorted:

```text
move l right
```

```java
l++;
```

---

## `sum > target`

Need a **smaller sum**.

Move `r` left:

```java
r--;
```

---

## Memory Trick

```text
sum < target → l++

sum > target → r--

sum == target → answer
```

---

# 4. 3Sum

## Key Idea

Sort the array first.

Then:

```text
Fix one element
      ↓
Use two pointers for the remaining elements
```

Pattern:

```text
Sort
 ↓
Fix i
 ↓
l = i + 1
r = n - 1
 ↓
Check nums[i] + nums[l] + nums[r]
```

---

# Why Sort First?

Example:

```text
[-1, 0, 1, 2, -1, -4]
```

After sorting:

```text
[-4, -1, -1, 0, 1, 2]
```

Sorting gives us two important benefits:

```text
1. Two-pointer movement becomes possible
2. Duplicate values become adjacent
```

---

# 3Sum Pointer Movement

Calculate:

```java
sum = nums[i] + nums[l] + nums[r];
```

### `sum < 0`

Need a larger sum:

```java
l++;
```

### `sum > 0`

Need a smaller sum:

```java
r--;
```

### `sum == 0`

Found a valid triplet:

```java
ls.add(Arrays.asList(nums[i], nums[l], nums[r]));
```

Then:

```java
l++;
r--;
```

---

# Why Move Both `l` and `r`?

After:

```text
nums[i] + nums[l] + nums[r] == 0
```

the current `l` and `r` values have already been used.

So move both:

```text
l → next
r → previous
```

```java
l++;
r--;
```

Then skip duplicates.

---

# Skip Duplicate `i`

```java
if (i > 0 && nums[i - 1] == nums[i])
    continue;
```

### Why?

After sorting, duplicate values are next to each other.

Example:

```text
[-4, -1, -1, 0, 1, 2]
      ↑   ↑
      same value
```

Using both `-1`s as the starting value can generate the same triplet.

Therefore:

```text
duplicate nums[i]
       ↓
     skip
```

### Memory Trick

```java
if (i > 0 && nums[i] == nums[i - 1])
    continue;
```

---

# Add Triplet

```java
ls.add(Arrays.asList(nums[i], nums[l], nums[r]));
```

### Remember

```text
Arrays.asList(a, b, c)
        ↓
Creates a List containing a, b, c
```

So:

```java
ls.add(Arrays.asList(nums[i], nums[l], nums[r]));
```

means:

```text
Add the current triplet to the result.
```

---

# Skip Duplicate `l`

```java
while (l < r && nums[l] == nums[l - 1])
    l++;
```

### Why?

After finding a valid triplet, the next `l` value may be the same.

Example:

```text
[-2, 0, 0, 0, 2]
     ↑  ↑
     l  duplicate
```

We don't want to generate the same triplet again.

Therefore:

```text
duplicate nums[l]
       ↓
     l++
```

---

# Skip Duplicate `r`

```java
while (l < r && nums[r] == nums[r + 1])
    r--;
```

Same idea for the right pointer.

```text
duplicate nums[r]
       ↓
     r--
```

---

# 3Sum Duplicate Rule

There are **3 places** where duplicates matter:

```text
1. i → skip duplicate starting values

if (i > 0 && nums[i] == nums[i - 1])
    continue;


2. l → skip duplicate left values

while (l < r && nums[l] == nums[l - 1])
    l++;


3. r → skip duplicate right values

while (l < r && nums[r] == nums[r + 1])
    r--;
```

### Easy Memory

```text
i → duplicate? SKIP

l → duplicate? MOVE RIGHT

r → duplicate? MOVE LEFT
```

---

# 3Sum Core Pattern

```text
SORT
 ↓
FIX i
 ↓
l = i + 1
r = n - 1
 ↓
Calculate sum
 ↓
sum < 0 → l++
sum > 0 → r--
sum = 0 → save + l++ + r--
 ↓
Skip duplicates
```
# 5. Container With Most Water

### Formula

```java
int area = Math.min(height[l], height[r]) * (r - l);
maxArea = Math.max(maxArea, area);
```

### Two Pointer Rule

```text
l = 0
r = n - 1
```

```text
height[l] < height[r] → l++

height[l] >= height[r] → r--
```

### Key Learning

```text
Area = shorter height × width
```

**Always move the shorter pointer** because the shorter wall limits the water.

### Loop

```java
while (l < r)
```

Why?

```text
l == r → only one wall remains → no container
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

### Memory Trick

```text
Maximum width
     ↓
Calculate area
     ↓
Move shorter wall
     ↓
Repeat
```
---

# 5. Big-O: `log₂(n)`

## What Does `log₂(n)` Mean?

Think:

```text
log₂(n) = How many times can I divide n by 2?
```

More precisely:

```text
log₂(n) = x
```

means:

```text
2ˣ = n
```

---

## Example: `10⁵`

```text
n = 10⁵
  = 100,000
```

We want:

```text
log₂(100,000)
```

Using change of base:

```text
log₂(100,000)
= log₁₀(100,000) / log₁₀(2)
```

We know:

```text
log₁₀(100,000) = 5

log₁₀(2) ≈ 0.30103
```

Therefore:

```text
5 / 0.30103
≈ 16.61
```

So:

```text
log₂(10⁵) ≈ 16.61
```

---

# `n log n`

For:

```text
n = 10⁵
```

```text
n log₂(n)
= 100,000 × 16.61
≈ 1,660,964
```

Therefore:

```text
O(n log n) ≈ 1.66 million operations
```

---

# Important `n log n` Values

| n | log₂(n) | n log₂(n) |
|---:|---:|---:|
| 10² = 100 | ~6.64 | ~664 |
| 10³ = 1,000 | ~9.97 | ~9,966 |
| 10⁴ = 10,000 | ~13.29 | ~132,877 |
| 10⁵ = 100,000 | ~16.61 | ~1,660,964 |
| 10⁶ = 1,000,000 | ~19.93 | ~19,931,569 |

### Memorize These

```text
10³ → ~10K
10⁴ → ~133K
10⁵ → ~1.66M
10⁶ → ~20M
```

---

# Constraint → Complexity

Useful interview rule of thumb:

| n | Usually Consider |
|---:|---|
| n ≤ 10 | O(n!), O(2ⁿ), O(n³) |
| n ≤ 20 | O(2ⁿ) |
| n ≤ 100 | O(n³) |
| n ≤ 1,000 | O(n²) |
| n ≤ 10,000 | O(n²) may work |
| n ≤ 100,000 | O(n log n) / O(n) |
| n ≤ 1,000,000 | O(n log n) / O(n) |
| n ≥ 10⁷ | Usually O(n) or better |

> These are guidelines, not strict rules. Actual performance depends on the language, time limit, and operations.

---

# Quick Complexity Thinking

If:

```text
n = 10⁵
```

Think:

```text
O(1)       ✅
O(log n)   ✅
O(n)       ✅
O(n log n) ✅
O(n²)      ❌ Usually too slow
O(2ⁿ)      💀
```

If:

```text
n = 10³
```

Think:

```text
O(n)       ✅
O(n log n) ✅
O(n²)      ✅ ~1,000,000
O(n³)      ⚠️ ~1,000,000,000
```

---

# 6. General Two Pointer Decision Rules

When you see a problem, ask:

```text
1. Is the array/string sorted?
        ↓
   Think Two Pointers

2. Can I start from opposite ends?
        ↓
   l = 0
   r = n - 1

3. What does the condition tell me?
        ↓
   Decide which pointer to move

4. Do I need to process l == r?
        ↓
   YES → l <= r
   NO  → l < r

5. Am I modifying the input?
        ↓
   Could I overwrite unread data?
        ↓
   YES → consider a separate array
```

---

# 7. General DSA Problem-Solving Mindset

When stuck, ask:

```text
1. What is the input?

2. What is the output?

3. Is the input sorted?

4. Can I use two pointers?

5. Can I use HashMap / HashSet?

6. Can I avoid nested loops?

7. What information do I need to remember?

8. Am I overwriting information I still need?

9. What is the Time Complexity?

10. What is the Space Complexity?
```

---

# Most Important Rules to Remember

## Two Pointers

```text
Sorted array → Think Two Pointers
```

## Pointer Movement

```text
sum < target → l++

sum > target → r--

sum == target → answer
```

## Loop Condition

```text
Need to process l == r → l <= r

Don't need l == r → l < r
```

## Duplicates in 3Sum

```text
i → skip duplicate

l → skip duplicate by l++

r → skip duplicate by r--
```

## In-Place Modification

```text
READ → What do I still need?

WRITE → What am I destroying?
```

## Complexity

```text
10³ → O(n²) may work

10⁵ → O(n) / O(n log n)

10⁶ → O(n) preferred
```

## Core Principle

```text
Correctness first
       ↓
Analyze
       ↓
Optimize
```

> A simple correct `O(n)` + `O(n)` space solution is better than a complicated `O(n)` + `O(1)` solution that you cannot prove is correct.
