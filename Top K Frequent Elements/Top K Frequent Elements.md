### STEP1
- まずは答えを見ずに解いてみる
```python
from collections import defaultdict

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        nums_by_uniq = list(set(nums))
        num_and_cnt = defaultdict(int)
        for uniq_num in nums_by_uniq:
            for num in nums:
                if uniq_num == num:
                    num_and_cnt[num] += 1
        
        sorted_num_list = sorted(num_and_cnt.items(), key=lambda x:x[0])

        return [sorted_num[0] for sorted_num in sorted_num_list[:k]]
```

- 下記の通り、Wrong Answer
input
```
nums = [4,1,-1,2,-1,2,3]
k = 2
```

output
```
[-1,1]
```

expected
```
[2,-1]
```

### STEP2
- 他人の回答を確認
  - https://github.com/maeken4/Arai60/pull/9/files
  - https://github.com/katataku/leetcode/pull/9/files#r1860305454
- 気づいたこと
  - numsのlistをuniqにして、元のlistと比較する処理が不要
    - 単純にdictにnumsの要素をkeyにカウント数をvalueにすればよいだけ
  - heapqが使える
    - (cnt, num) のようなsetでも格納できる。その際は、先頭の要素でsortされる
- わからないこと
  - なぜ、エラーのような出力となったのか
  - 先頭の要素でsortされるというのはpythonのリファレンスのどの部分に書いている？

- 修正コード
```python
from collections import defaultdict

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        num_and_cnt = defaultdict(int)
        for num in nums:
            num_and_cnt[num] += 1
        
        top_k_frequent = []

        for num, cnt in num_and_cnt.items():
            heapq.heappush(top_k_frequent, (cnt, num))

        while len(top_k_frequent) > k:
            heapq.heappop(top_k_frequent)
        
        return [num for cnt, num in top_k_frequent]
```
- (2025/7/21追記) レビューコメントから下記のようなソースコードでも良いのではないか、という意見をいただいた。
```python
import heapq
from collections import defaultdict

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        num_and_count = defaultdict(int)
        for num in nums:
            num_and_count[num] += 1

        top_k_freqent = []

        for num, cnt in num_and_count.items():
            heapq.heappush(top_k_freqent, (-cnt, num))

        return [num[:k] for cnt, num in top_k_freqent]
```


```
### STEP3
- 修正したコードで3回ミスすることがなくなるまで書く
- 1回目
```python
from collections import defaultdict

import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        nums_dict = defaultdict(int)
        for num in nums:
            nums_dict[num] += 1

        top_k_freqent_list = []
        for num, cnt in nums_dict.items():
            heapq.heappush(top_k_freqent_list, (cnt, num))
        
        while len(top_k_freqent_list) > k:
            heapq.heappop(top_k_freqent_list)
        
        return [num for cnt, num in top_k_freqent_list]
```
- 所要時間: 約7分
- 2回目
```python
from collections import defaultdict

import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        num_and_cnt = defaultdict(int)
        for num in nums:
            num_and_cnt[num] += 1

        top_k_freqent_list = []

        for num, cnt in num_and_cnt.items():
            heapq.heappush(top_k_freqent_list, (cnt, num))

        while len(top_k_freqent_list) > k:
            heapq.heappop(top_k_freqent_list)

        return [num for cnt, num in top_k_freqent_list]
```

- 所要時間: 約3分

- 3回目
```python
from collections import defaultdict

import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        num_and_cnt = defaultdict(int)
        for num in nums:
            num_and_cnt[num] += 1

        top_k_freqent_list = []

        for num, cnt in num_and_cnt.items():
            heapq.heappush(top_k_freqent_list, (cnt, num))

        while len(top_k_freqent_list) > k:
            heapq.heappop(top_k_freqent_list)

        return [num for cnt, num in top_k_freqent_list]
```

- 所要時間: 約3分
