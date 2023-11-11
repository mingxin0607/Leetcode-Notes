
### [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

- sorted_str = ''.join(sorted(s))
- time O(nklog(k))
- space O(n)

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans = []
        tbl = {}
        for s in strs:
            ts = ''.join(sorted(s))
            if ts in tbl:
                tbl[ts].append(s)
            else:
                tbl[ts] = [s]
        for x in tbl.values():
            ans.append(x)
        return ans
```

- map 
	- Map<String,List< String>> tbl = new HashMap< > ();
	- tbl.get(key);
	- tbl.put(key, value)
	- for (-- value : tbl.values()) { ... }
	- map.remove(key);
	- for (-- value : tbl.keySet()) { ... }
- sort string
	- char[ ] charArr = s.toCharArray();
	- Arrays.sort(chArr); for int[ ] char [ ]
	- Collections.sort(); for new ArrayList()
	- String str = new String(chArr);
- list
	- new ArrayList<>();
	- lst.add(item);
	- lst.remove(index);

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> ans = new ArrayList<>();
        Map<String,List<String>> tbl = new HashMap<>();
        for (String s : strs){
            char[] chArr = s.toCharArray();
            Arrays.sort(chArr);
            String key = new String(chArr);
            if (tbl.containsKey(key)){
                tbl.get(key).add(s);
            }
            else{
                List<String> tmp = new ArrayList<>();
                tmp.add(s);
                tbl.put(key, tmp);
            }
        }
        for (List<String> lst : tbl.values()){
            ans.add(lst);
        }
        return ans;
    }
}
```

### [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

- set.remove(value)
- lst.remove(value)
- both O(v+e)

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        tbl = [[] for _ in range(numCourses)]

        for a, b in prerequisites:
            tbl[a].append(b)
        
        ans = []
        st = set(range(numCourses))
        while True:
            tmp = []
            for i in st:
                if len(tbl[i]) == 0:
                    tmp.append(i)

            for i in range(numCourses):
                if i in tmp:
                    st.remove(i)
                else:
                    arr = []
                    for j in tbl[i]:
                        if j not in tmp:
                            arr.append(j)
                    tbl[i] = arr
            ans.extend(tmp)
            # print(tmp)
            # print(tbl)
            if (len(tmp) == 0):
                break
        # print(ans)
        return [] if len(ans) != numCourses else ans
```



```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> tbl = new ArrayList<>(numCourses);
        // !!init
        for (int i = 0; i < numCourses; i++){
            tbl.add(new ArrayList<Integer>());
        }
        // System.out.println(tbl);
        for (int i = 0; i < prerequisites.length; i++) {
            int a = prerequisites[i][0];
            int b = prerequisites[i][1];
            tbl.get(a).add(b);
        }
        // setttttttt
        Set<Integer> st = new HashSet<>();
        for (int i = 0; i < numCourses; i++){
            st.add(i);
        }
        List<Integer> ans = new ArrayList<>();
        while (true){
            List<Integer> tmp = new ArrayList<>();
            for (Integer i : st){
            // arr.get().size();
                if (tbl.get(i).size() == 0){
                    tmp.add(i);
                }
            }
            for (int i = 0; i < numCourses;i++){
            // arr contains
                if (tmp.contains(i)){
                    st.remove(i);
                }
                else {
                    List<Integer> arr = new ArrayList<>();
                    for (Integer j : tbl.get(i)){
                        if (tmp.contains(j) == false) {
                            arr.add(j);
                        }
                    }
                    // arr.set(index, value);
                    tbl.set(i, arr);
                }
            }
            ans.addAll(tmp);
            if (tmp.size() == 0){
                break;
            }
        }
        if (ans.size() != numCourses){
            return new int[]{};
        }
        // transfer List<Integer> to int[] 
        int[] res = new int[numCourses];
        for (int i = 0; i < numCourses; i++)
            res[i] = ans.get(i);
        return res;
    }
}
```

```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>> &prerequisites){
        vector<int> adj[numCourses];

        // Creating Adjacency List
        for (auto it : prerequisites){
            adj[it[1]].push_back(it[0]);
        }

        return using_kahnAlgo(numCourses, adj);
    }

    vector<int> using_kahnAlgo(int V, vector<int> adj[]){

        vector<int> indegree(V, 0);

        for (int i = 0; i < V; i++){
            for (int adjNode : adj[i]){
                indegree[adjNode]++; // calculating indegree of every node
            }
        }

        queue<int> q;
        for (int i = 0; i < V; i++){
            if (indegree[i] == 0){
                q.push(i); // pushing starting sources
            }
        }

        vector<int> topo;
        while (!q.empty()){
            int node = q.front();
            q.pop();

            topo.push_back(node);

            for (int adjNode : adj[node]){
                indegree[adjNode]--;

                // new start points
                if (indegree[adjNode] == 0){
                    q.push(adjNode);
                }
            }
        }

        if (topo.size() == V){
            return topo;
        }

        return {};
    }
};
```


### [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

- PriorityQueue
- add
- poll
- time nlogk
- space k

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int num : nums){
            pq.add(num);
            if (pq.size() > k){
                pq.poll();
            }
        }
        return pq.poll();
    }
}
```

- heapq.heappush(lst, value)
- heapq.heappop(lst)

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heap = []

        for num in nums:
            heapq.heappush(heap, num)
            if (len(heap) > k):
                heapq.heappop(heap)
        return heapq.heappop(heap)
```

implement heap

```Python
class MinHeap:
    def __init__(self):
        self.heap = []

    def insert(self, value):
        self.heap.append(value)
        self._heapify_up(len(self.heap) - 1)

    def extract_min(self):
        if len(self.heap) == 0:
            return None

        if len(self.heap) == 1:
            return self.heap.pop()

        min_value = self.heap[0]
        self.heap[0] = self.heap.pop()
        self._heapify_down(0)
        return min_value

    def _heapify_up(self, index):
        parent_index = (index - 1) // 2
        while index > 0 and self.heap[index] < self.heap[parent_index]:
            self.heap[index], self.heap[parent_index] = self.heap[parent_index], self.heap[index]
            index = parent_index
            parent_index = (index - 1) // 2

    def _heapify_down(self, index):
        left_child_index = 2 * index + 1
        right_child_index = 2 * index + 2

        smallest = index
        if (
            left_child_index < len(self.heap)
            and self.heap[left_child_index] < self.heap[smallest]
        ):
            smallest = left_child_index
        if (
            right_child_index < len(self.heap)
            and self.heap[right_child_index] < self.heap[smallest]
        ):
            smallest = right_child_index

        if smallest != index:
            self.heap[index], self.heap[smallest] = self.heap[smallest], self.heap[index]
            self._heapify_down(smallest)

```


