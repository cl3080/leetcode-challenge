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

 
