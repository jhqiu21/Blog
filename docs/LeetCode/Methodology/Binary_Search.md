# Binary Search and Edge Case

!!! Quote
    Easy algorithms are *hard* to write correctly

## Basic Template of Binary Search
``` java linenums="1"
int binarySearch(int[] nums, int target) {
    int left = 0; 
    int right = nums.length - 1; 

    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target)
            return mid; 
        else if (nums[mid] < target)
            left = mid + 1; 
        else if (nums[mid] > target)
            right = mid - 1;
    }
    return -1;
}
```
## Analysis

### Loop Invariant

Note that loop invariant is `left <= right`, it means when loop terminate 

- We find the `target` number
- `left > right`: this happens when `left = mid = right` and `nums[mid] != target`, thus the loop ends.

!!! Question
    Why we use `<=` instead of `<` in our loop invarinat

