Given an array of points where $points[i] = [x_1, y_1]$ represents a point on the X-Y plane, and an integer k, return the k closest points to the origin (0, 0).

ex.

points = $[[1, 3], [-2, 2]]$ k = 1

output: $[[-2, 2]]$ 

Solution:

We need to sort all the points based on their distance to the origin, and then return the first $k$ elements in that sorted list. We could build up a sorted list, but that would result in an $O(n^2)$ time complexity. Instead if we generate a heap using the entire set of points, and then pop the first $k$ elements, we will achieve the same result in $O(log(n))$ time.

```
import heapq
def kClosest(points: List[List[int]], k: int) -> List[List[int]]:
	h = []
	for point in points:
		dist = sqrt(point[0] ** 2 + point[1] ** 2)
		heapq.heappush(h, (dist, point))
	result = []
	for _ in range(k):
		result.append(heapq.heappop(h)[1])
	return result
```
