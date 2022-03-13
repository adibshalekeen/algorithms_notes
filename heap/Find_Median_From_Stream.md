Create a class that has some function findMedian that will return the median of all of the values observed in some data stream so far. The data stream is provided element-by-element.

ex.

Input: [5, 15, 1, 3] (Assume you recieve these values one at a time, but in this order)

Output: [5, 10, 5, 4] (Median after each call to addNum)

Solution:

The bruteforce approach is to maintain a sorted list as values are added to the MedianFinder, then return the median by indexing based on the length of the sorted list. This results in $O(n)$ complexity for the insertion, since you have to traverse the sorted list to figure out where to insert the new value.

We dont really need the entire data stream to be sorted. If you think about the data stream as a large array that youre obsering one index at a time, we just need to keep track of where the center point should be if the array *was* sorted. For any array, there are two cases for finding the median.

$[1,2,3]$
The array is odd length, so the median is at index 1, the lower half of the array contains values less than $2$, and the higher half of the array contains values greater than $2$. If we were to segment the array into two contiguous subarrays, we could have $[1], [2, 3]$ or $[1, 2], [3]$ The median is either the bottom of the subarray containing the higher values, or the top of hte subarray containing the lower values.

$[1,2,3,4]$
The array is even length, the median is the average of the values at index $len(arr) // 2$ and $len(arr) // 2 - 1$. If we wanted to segment the array into two contiguous subarrays we could have $[1,2], [3,4]$. The median is the average of the top of the subarray containing lower values, and the bottom of the subarray containing the higher values.

The key here is that the subarrays that we generate need to be approximately the same size, other wise our assumption about the location of the median falls apart (since the boundaries of the lower / higher half no longer correspond to the center of the data stream)

We can keep track of the center of the data stream by using two heaps. The lower half $low$ can be tracked using a max heap (so we can quickly access the maximum value) and the higher half $high$ can be tracked using a min heap (so we can quickly access the lower value). Whenever we call $addNum(num)$, we need to add the value in such a way that the lengths of $high$ and $low$ are within one element of one another. If we need to add to $high$ and $len(high) > len(low)$ we can pop the lowest value from high and append it to low, then add $num$ to $high$. Likewise if we need to add to $low$ and $len(low) > len(high)$ we can pop from $low$ and add to $high$ before adding the new value. There are 3 cases we need to consider to figure out which segment to add $num$ to.

Case 1:
$low[0] <= num <= high[0]$
We can add to either low or high, add so that the sizes are maintained

Case 2:
$low[0] <= high[0] <= num$
We need to add to high

Case 3:
$num <= low[0] <= high[0]$
We need to add to low



```
import heapq
class MedianFinder:

    def __init__(self):
        # lower end of the data stream so far
        # represented as a max heap
        self._low = []
        
        # higher end of the data stream so far
        # represented as a min heap
        self._high = []
        
        self._count = 0

    def addNum(self, num: int) -> None:
        if not self._low:
            # add to low first
            heapq.heappush(self._low, (-1 * num, self._count))
        elif not self._high:
            # low is initialized but high isn't, check if we should add the second number to
            # low or high
            if num < -1 * self._low[0][0]:
                low_pop = heapq.heappop(self._low)
                heapq.heappush(self._high, (-1 * low_pop[0], low_pop[1]))
                heapq.heappush(self._low, (-1 * num, self._count))
            else:
                heapq.heappush(self._high, (num, self._count))
        else:
            # both high and low are initialized
            low_top = -1 * self._low[0][0]
            high_bottom = self._high[0][0]
            
            len_low = len(self._low)
            len_high = len(self._high)
            if low_top <= num and num <= high_bottom:
                # low <= num <= high
                # We can add to either low or high here since the value
                # is between the bottom of the high half and the top
                # of the low half of the data stream observed so far
                # add it so that the size of high and low are within 1
                # element of each other
                if len_low > len_high:
                    heapq.heappush(self._high, (num, self._count))
                else:
                    heapq.heappush(self._low, (-1 * num, self._count))
            elif num >= high_bottom:
                # low <= high <= num
                # We need to add the value to the high half, but if the high
                # half is already larger then we need to move the lowst
                # value in the high half to the lower half then add
                if len_low < len_high:
                    high_pop = heapq.heappop(self._high)
                    heapq.heappush(self._low, (-1 * high_pop[0], high_pop[1]))
                heapq.heappush(self._high, (num, self._count))
            else:
                # num <= low <= high
                # Similar to case 2 but we need to add to low half
                if len_low > len_high:
                    low_pop = heapq.heappop(self._low)
                    heapq.heappush(self._high, (-1 * low_pop[0], low_pop[1]))
                heapq.heappush(self._low, (-1 * num, self._count))
        self._count += 1
                

    def findMedian(self) -> float:
        len_high = len(self._high)
        len_low = len(self._low)
        if self._count % 2 == 0:
            return (-1 * self._low[0][0] + self._high[0][0]) / 2
        else:
            # median is held in the bigger half when length is odd
            if len_high > len_low:
                return self._high[0][0]
            else:
                return -1 * self._low[0][0]

```

