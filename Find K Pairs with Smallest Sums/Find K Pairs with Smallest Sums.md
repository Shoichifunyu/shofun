### STEP1
- 何も回答を見ずに解いてみる
```python
import heapq

class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        nums1_sort = []
        for num1 in nums1:
            heapq.heappush(nums1_sort, num1)
        
        nums2_sort = []
        for num2 in nums2:
            heapq.heappush(nums2_sort, num2)

        result_set_list = []

        num1_candidate = heapq.heappop(nums1_sort)
        num2_candidate = heapq.heappop(nums2_sort)

        result_set_list.append((num1_candidate, num2_candidate))
        
        while len(result_set_list) < k:
            if num1_candidate < num2_candidate:
                if self.is_popped_value_minimum(nums1_sort, num2_candidate):
                    num1_candidate = heapq.heappop(nums1_sort)
                else:
                    num2_candidate = heapq.heappop(nums2_sort)
            else:
                if self.is_popped_value_minimum(nums2_sort, num1_candidate):
                    num2_candidate = heapq.heappop(nums2_sort)
                else:
                    num1_candidate = heapq.heappop(nums1_sort)

            result_set_list.append((num1_candidate, num2_candidate))

        return result_set_list

    def is_popped_value_minimum(self, target_heapq, compared_value):
        target_value = heapq.heappop(target_heapq)
        heapq.heappush(target_heapq, target_value)
        if target_value < compared_value:
            return True
        else:
            return False
```
- ポイント
  - まず、num1(セットの第一要素)とnum2(セットの第二要素)をheapqを使って取得。リストの先頭の要素(セット型)としてappend
    - 続いてリストの要素（セット）に追加(append)するにあたり、num1(セットの第一要素)とnum2(セットの第二要素)の比較を実施。それぞれの大小に応じて、個別関数`is_popped_value_minimum`を呼び出し
    - 例えば、num1がnum2よりも小さな場合は、num1のリストから現状のnum1に続いて小さな値を取り出す。これが、num2の値より小さいかを確認
    - 仮に小さい場合は、num1は取り出した値に決まり。num2は、比較対象となった値に決まり
    - num2がnum1よりも小さな場合も同様
- 下記の場合、Wrong Answerとなる
input
```
nums1 =
[1,2,4,5,6]
nums2 =
[3,5,7,9]
k =
3
```
output
```
[[1,3],[2,3],[2,5]]
```
expected
```
[[1,3],[2,3],[1,5]]
```

### STEP2
他の人の答えを確認

```python
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:

        candidates = [(nums1[0] + nums2[0], 0, 0)]
        seen = set([(0,0)])
        smallest_pairs = []

        def need_to_be_added(index1, index2):
            if len(nums1) <= index1 or len(nums2) <= index2:
                return False
            if index1 == 0 or index2 == 0:
                return True
            return (index1 - 1, index2) in seen and (index1, index2 - 1) in seen


        def add_if_necessary(index1, index2):
            if need_to_be_added(index1, index2):
                heapq.heappush(candidates, (nums1[index1] + nums2[index2], index1, index2))
               

        while len(smallest_pairs) < k:
            _, index1, index2 = heapq.heappop(candidates)
            smallest_pairs.append([nums1[index1], nums2[index2]])
            seen.add((index1, index2))
            add_if_necessary(index1 + 1, index2)
            add_if_necessary(index1, index2 + 1)
        return smallest_pairs
```
- 気づいたポイント
  - リストの第一要素として、比較対象値(num1とnum2の合計値)を入れることで、heapqでソートされることを狙う
    - 後続の処理でheapqから要素をpopし、結果出力用リストに格納。合わせて、出力済みindexセットにも追加(add)
  - `add_if_nessesary`で、対象としているnum1とnum2のindexの1つ前のindexの組み合わせがそれぞれ、出力済みindexセットに存在するかを確認、一方でも存在していなければ、まだそのindexが追加される余地があるということで、対象のindexに対応する値はheapqにpushしない
    - ~~この処理は、前方から順繰りに実施していくので、`add_if_necessary(index1+1, index2)`と`add_if_nesessary(index1, index2+1)`でともにheapq.pushされる要素がないことはないはず~~
      - `if len(nums1) <= index1 or len(nums2) <= index2`←のルートに入ればheapq.pushされない可能性があるか。その場合、ループ処理の先頭の`_, index1, index2 = heapq.heappop(candidates)`はどうなるか？
        - IndexError: index out of range になりそう、つまりlen(nums1)とlen(nums2)に比べて同じ(かそれ以上の)index番号になった時には、途中でエラーが発生する可能性がある、ということ？
        - Constraintsには`k <= nums1.length * nums2.length`と書かれている(`k <= nums1.length`かつ`k <= nums2.length`なら直感的にこの制約はIndexErrorを回避できると分かるが、それ以上にkは大きくなりうるのでこの実装ではIndexErrorを引き起こす懸念があるのではないか)

### STEP3
- 間違えずに3回連続でコーディングできるか
- 1回目(所要時間：約8分)
```python
import heapq

class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        k_smallset_pairs = [(nums1[0]+nums2[0], 0, 0)]
        seen = set((0, 0))
        output_list = []

        def _needs_add(index1, index2):
            if len(nums1) <= index1 or len(nums2) <= index2:
                return False
            if index1 == 0 or index2 == 0:
                return True
            return (index1-1, index2) in seen and (index1, index2-1) in seen

        def _need_if_nesessary(index1, index2):
            if _needs_add(index1, index2):
                heapq.heappush(k_smallset_pairs, (nums1[index1]+nums2[index2], index1, index2))

        while len(output_list) < k:
            _, index1, index2 = heapq.heappop(k_smallset_pairs)
            output_list.append([nums1[index1], nums2[index2]])
            seen.add((index1, index2))
            _need_if_nesessary(index1+1, index2)
            _need_if_nesessary(index1, index2+1)

        return output_list
```
- 2回目(所要時間：約7分)
```python
import heapq

class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        k_smallest_pairs = [(nums1[0]+nums2[0], 0, 0)]
        seen = set((0, 0))
        output_list = []

        def _needs_add(index1, index2):
            if len(nums1) <= index1 or len(nums2) <= index2:
                return False
            if index1 == 0 or index2 == 0:
                return True
            return (index1-1, index2) in seen and (index1, index2-1) in seen

        def _need_if_nesessary(index1, index2):
            if _needs_add(index1, index2):
                heapq.heappush(k_smallest_pairs, (nums1[index1]+nums2[index2], index1, index2))

        while len(output_list) < k:
            _, index1, index2 = heapq.heappop(k_smallest_pairs)
            output_list.append([nums1[index1], nums2[index2]])
            seen.add((index1, index2))
            _need_if_nesessary(index1+1, index2)
            _need_if_nesessary(index1, index2+1)

        return output_list
```
- 3回目(所要時間：約9分)
```python
import heapq

class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        k_smallest_list = [(nums1[0]+nums2[0], 0, 0)]
        seen = set((0, 0))
        output_list = []

        def _needs_add(index1, index2):
            if len(nums1) <= index1 or len(nums2) <= index2:
                return False
            if index1 == 0 or index2 == 0:
                return True
            return (index1-1, index2) in seen and (index1, index2-1) in seen

        def _need_if_nesessary(index1, index2):
            if _needs_add(index1, index2):
                heapq.heappush(k_smallest_list, (nums1[index1]+nums2[index2], index1, index2))

        while len(output_list) < k:
            _, index1, index2 = heapq.heappop(k_smallest_list)
            output_list.append([nums1[index1], nums2[index2]])
            seen.add((index1, index2))
            _need_if_nesessary(index1+1, index2)
            _need_if_nesessary(index1, index2+1)

        return output_list
```
