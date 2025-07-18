### STEP1
- まずは答えを見ずに解く

```python
class KthLargest:
    # 引数のkとnumsの内容に沿って、k番目に大きな数を出す(initではNoneを返す(つまり返り値はなし))
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.nums = sorted(nums, reverse=True)

    def add(self, val: int) -> int:
        self.nums.append(val)
        self.nums.sort(reverse=True)
        return self.nums[self.k-1]


# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```
- 所要時間約10分

### STEP2
- 他の人の解答を見て、書き直せるところはないか、検討する
- https://github.com/ryosuketc/leetcode_arai60/pull/8/files
- https://github.com/TORUS0818/leetcode/pull/10/files
- https://github.com/Mike0121/LeetCode/pull/19/files
- どうやら`heapq`なるものを使用して優先度付きキューを生成しているよう
- これにより、優先度付きキューに値を取り出すときと挿入するときの時間計算量がNからlogNになることが分かった
  - https://qiita.com/ell/items/fe52a9eb9499b7060ed6#%E5%84%AA%E5%85%88%E5%BA%A6%E4%BB%98%E3%81%8D%E3%82%AD%E3%83%A5%E3%83%BC%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6
```python
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.largest_nums_up_to_kth = []
        for num in nums:
            self.add(num)

    def add(self, val: int) -> int:
        heapq.heappush(self.largest_nums_up_to_kth, val)
        if len(self.largest_nums_up_to_kth) > self.k:
            heapq.heappop(self.largest_nums_up_to_kth)
        return self.largest_nums_up_to_kth[0]



# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```

- 上記内容を参考にして再度書き直した結果

```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self._k = k
        self._heap_ordered_nums = nums
        heapq.heapify(self._heap_ordered_nums)

        while len(self._heap_ordered_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)

    def add(self, val: int) -> int:
        heapq.heappush(self._heap_ordered_nums, val)
        if len(self._heap_sorted_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)
        return self._heap_ordered_nums[0]
```

- 一応、インスタンス変数には先頭にアンダーバーを付けておく

### STEP3
- 3回連続でミスせずコーディングするまでコーディング
#### 1回目
```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self._k = k
        self._heap_sorted_nums = nums
        heapq.heapify(self._heap_ordered_nums)

        while len(self._heap_ordered_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)


    def add(self, val: int) -> int:
        heapq.heappush(self._heap_sorted_nums, val)
        if len(self._heap_ordered_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)
        return self._heap_ordered_nums[0]
```
- 実装時間: 03:08
#### 2回目
```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self._k = k
        self._heap_sorted_nums = nums
        heapq.heapify(self._heap_ordered_nums)

        while len(self._heap_sorted_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)

    def add(self, val: int) -> int:
        heapq.heappush(self._heap_ordered_nums, val)

        if len(self._heap_ordered_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)
        return self._heap_ordered_nums[0]
```
- 実装時間: 03:59

#### 3回目
```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self._k = k
        self._heap_sorted_nums = nums

        heapq.heapify(self._heap_ordered_nums)

        while len(self._heap_sorted_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)

    def add(self, val: int) -> int:
        heapq.heappush(self._heap_ordered_nums, val)

        if len(self._heap_sorted_nums) > self._k:
            heapq.heappop(self._heap_ordered_nums)
            
        return self._heap_ordered_nums[0]
```
- 実装時間: 04:30
