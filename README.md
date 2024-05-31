# LEETCODE-BIT-MANIPULATION-260
Sure, let's go through the code step-by-step to understand how it works.

The goal of this function is to find two unique numbers in an array where every other number appears exactly twice.

### Code Analysis
```java
class Solution {
  public int[] singleNumber(int[] nums) {
    final int xors = Arrays.stream(nums).reduce((a, b) -> a ^ b).getAsInt();
    final int lowbit = xors & -xors;
    int[] ans = new int[2];

    // Separate `nums` into two groups by `lowbit`.
    for (final int num : nums)
      if ((num & lowbit) > 0)
        ans[0] ^= num;
      else
        ans[1] ^= num;

    return ans;
  }
}
```

### Steps Explained
1. **Compute XOR of all numbers:**
   ```java
   final int xors = Arrays.stream(nums).reduce((a, b) -> a ^ b).getAsInt();
   ```
   The `xors` variable will hold the XOR of all the numbers in the array. Since every number except the two unique ones appears twice, their XOR will cancel out to 0, leaving the XOR of the two unique numbers.

2. **Find the rightmost set bit (lowbit):**
   ```java
   final int lowbit = xors & -xors;
   ```
   This line finds the rightmost set bit in the `xors` result. This bit is different between the two unique numbers.

3. **Initialize result array:**
   ```java
   int[] ans = new int[2];
   ```

4. **Separate numbers into two groups and XOR within each group:**
   ```java
   for (final int num : nums)
     if ((num & lowbit) > 0)
       ans[0] ^= num;
     else
       ans[1] ^= num;
   ```
   This loop separates the numbers into two groups based on the rightmost set bit found in the previous step. Each group contains one of the unique numbers and the numbers that appear twice. XOR-ing all numbers in each group will cancel out the numbers appearing twice, leaving only the unique number in each group.

5. **Return the result:**
   ```java
   return ans;
   ```

### Dry Run Example
Let's take an example array: `[1, 2, 1, 3, 2, 5]`

1. Compute XOR of all numbers:
   ```
   1 ^ 2 ^ 1 ^ 3 ^ 2 ^ 5 = 6  (Binary: 0110)
   ```

2. Find the rightmost set bit (lowbit):
   ```
   lowbit = 6 & -6 = 2  (Binary: 0010)
   ```

3. Initialize result array:
   ```
   ans = [0, 0]
   ```

4. Separate numbers into two groups and XOR within each group:
   - Group 1 (lowbit set): Numbers `2`, `3`, `2`
     ```
     ans[0] = 2 ^ 3 ^ 2 = 3
     ```
   - Group 2 (lowbit not set): Numbers `1`, `1`, `5`
     ```
     ans[1] = 1 ^ 1 ^ 5 = 5
     ```

5. Return the result:
   ```
   [3, 5]
   ```

So, the two unique numbers are `[3, 5]`.
