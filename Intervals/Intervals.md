**252 Meeting Rooms**

**Python**
 ```
 class Solution:
    def canAttendMeetings(self, intervals) -> bool:
        intervals.sort(key = lambda i: i[0])
        num = len(intervals)  
        for i in range(num-1):
            if intervals[i][1] > intervals[i+1][0]:
                return False
        return True
 ```
 Time Complexity: O(nlogn)  
 Space Complexity: O(1)


 **253 Meeting Rooms II**

 **Python**
 ```
 class Solution:
     def minMeetingRooms(self, intervals):
         if not intervals:
             return 0
         #initialize the min_heap
         free_rooms = []
         #sort intervals
         intervals.sort(key = lambda x:x[0])
         heapq.heappush(free_rooms,intervals[0][1])
         for i in intervals[1:]:
             if free_rooms[0] <= i[0]:
                 heapq.heappop(free_rooms)
             heapq.heappush(free_rooms,i[1])
         return len(free_rooms)
```

 **Algorithm**  
 Priority Queue
 1. Sort the given meetings by their start time.  
 2. Initialize a new min-heap and add the first meeting's ending time to the heap.  
 3. For every meeting room check if the minimum element of the heap (at the top of the min_heap) is free or not by comparing the value with the start time of the room.  
 4. If the room is free, pop the current topmost element.
 5. Push the end time of the room to the min_heap.
 6. The length of the final min_heap is the number of minimum required rooms.

 **56 Merge Intervals**

 **Python**
 ```
 class Solution:
    def merge(self, intervals):
        intervals = sorted(intervals,key = lambda x:x[0])
        merged = []
        for interval in intervals:
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            else:
                merged[-1][1] = max(merged[-1][1],interval[1])
        return merged
 ```

**Algorithm**
1. Sort intervals by its start
2. If current start <= last end:
       Merge intervals
   else:
       Insert a new interval

Time complexity: O(nlogn)
Space complexity: O(n)


**57 Insert Interval**

**Python**
Solution 1:
```
class Solution:
    def insert(self, intervals, newInterval):
        intervals.append(newInterval)
        intervals = sorted(intervals,key = lambda x: x[0])
        
        ans = []
        for i in range(len(intervals)):
            if not ans or ans[-1][1] < intervals[i][0]:
                ans.append(intervals[i])
            else:
                ans[-1][1] = max(intervals[i][1],ans[-1][1])

        return ans
```
Time complexity: O(nlogn)

Solution 2:
```
class Solution:
    def insert(self, intervals, newInterval):
        new_start,new_end = newInterval
        idx,n = 0,len(intervals)
        output = []
        while idx < n and intervals[idx][0] < new_start:
            output.append(intervals[idx])
            idx += 1

        if not output or output[-1][1] < new_start:
            output.append(newInterval)
        else:
            output[-1][1] = max(output[-1][1],new_end)

        while idx < n:
            interval = intervals[idx]
            start,end = interval
            idx += 1
            if output[-1][1] < start:
                output.append(interval)
            else:
                output[-1][1] = max(output[-1][1],end)

        return output
```
Time complexity: O(n)
