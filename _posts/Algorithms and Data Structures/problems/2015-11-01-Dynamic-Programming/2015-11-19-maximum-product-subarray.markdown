---
layout:     post
title:      "Problem: Maximum Product Subarray"
date:       2015-02-18 00:00:00
author:     "Marcy"
header-img: "img/post-bg-2015.jpg"
catalog: true
category: algnote
algnote-tags:
    - Coding Interview
    - Coding Practice
    - Algorithms and Data Structures
    - Dynamic Programming
    - Medium
---

## Question

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array `[2,3,-2,4]`,
the contiguous subarray `[2,3]` has the largest `product = 6`.

## Solution
TODO

#### Code
```java
class Solution {
    public int maxProduct(int[] nums) {
        if(nums.length == 0) return 0;
        if(nums.length == 1) return nums[0];

        int[] maxProd = new int[nums.length+1];
        int[] maxNegtiveProd = new int[nums.length+1];

        int max = nums[0];
        for(int i=0; i<nums.length; i++) {
            if(nums[i]>=0) {
                maxProd[i+1] = (maxProd[i] > 0 ? maxProd[i] : 1) * nums[i];
                maxNegtiveProd[i+1] = maxNegtiveProd[i] * nums[i];
            }
            else {
                maxProd[i+1] = maxNegtiveProd[i] * nums[i];
                maxNegtiveProd[i+1] = (maxProd[i] > 0 ? maxProd[i] : 1) * nums[i];
            }

            // maxProd must have a valid value after two numbers.
            // so we could just compare max and maxProd[i+1]
            max = Math.max(max, maxProd[i+1]);
        }

        return max;
    }
}
```

#### Optimization

```java
class Solution {
    public int maxProduct(int[] nums) {
        if(nums.length == 1) return nums[0];
        int[] pos = new int[nums.length];
        int[] neg = new int[nums.length];
        int max = nums[0];
        if(nums[0] >= 0) pos[0] = nums[0];
        else neg[0] = nums[0];
        for(int i=1; i<nums.length; i++) {
            if(nums[i] == 0) neg[i] = pos[i] = 0;
            else if(nums[i] > 0) {
                if(pos[i-1] > 0) pos[i] = nums[i] * pos[i-1];
                else if(pos[i-1] == 0) pos[i] = nums[i];
                if(neg[i-1] < 0) neg[i] = nums[i] * neg[i-1];
            }
            else if(nums[i] < 0) {
                if(pos[i-1] > 0) neg[i] = nums[i] * pos[i-1];
                else if(pos[i-1] == 0) neg[i] = nums[i];
                if(neg[i-1] < 0) pos[i] = nums[i] * neg[i-1];
            }
            max = Math.max(pos[i], max);
        }
        return max;
    }
}
```

## Performance
TODO
