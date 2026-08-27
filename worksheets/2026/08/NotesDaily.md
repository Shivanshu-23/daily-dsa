
# Example 

## Problem 1
Given an array and a target, find two numbers whose sum equals target.

## Approach

Use a `HashMap`.

For every number:
1. Calculate `target - nums[i]`
2. Check if it exists in the map
3. If yes, return the indices
4. Otherwise store the current number

## Code

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int need = target - nums[i];

            if (map.containsKey(need)) {
                return new int[]{map.get(need), i};
            }

            map.put(nums[i], i);
        }

        return new int[]{};
    }
}

# 1 String Cleaning

```java
s = s.toLowerCase().replaceAll("[^a-z0-9]", "");
Explanation
toLowerCase() → Converts all characters to lowercase.
replaceAll("[^a-z0-9]", "") → Removes all characters except lowercase letters and numbers.
"" → Replaces the matched characters with nothing.
Example
String s = "A man, a plan! 123";
s = s.toLowerCase().replaceAll("[^a-z0-9]", "");

System.out.println(s);

Output:

amanaplan123
Regex

[^a-z0-9]

a-z → lowercase letters
0-9 → digits
^ → NOT
Therefore, it matches anything that is not a letter or digit.
