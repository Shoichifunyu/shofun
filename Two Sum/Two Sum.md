### STEP1
- 回答を見ずに、問題を解く
```python
import heapq

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        ordered = []
        if target > 0:
            for index, num in enumerate(nums):
                heapq.heappush(ordered, (num, index))
        else:
            target = target*-1
            for index, num in enumerate(nums):
                heapq.heappush(ordered, (num*-1, index))
        output= []
        for ele in ordered:
            if target == 0:
                if len(output) == 0:
                    first = heapq.heappop(ordered)
                    second = heapq.heappop(ordered)
                    return self.return_output([first[1], second[1]], target)
                return self.return_output(output, target)
            output.append(ele[1])
            target -= ele[0]
            if target < 0:
                popped_index_candidate = output[0]
                for num, index in ordered:
                    if abs(target) == num:
                        output.remove(index)
                        target += num
                        return self.return_output(output, target)
                    elif popped_index_candidate == index:
                        modified_num_candidate = num 
                output.pop(0)
                target += modified_num_candidate
        return self.return_output(output, target)

    def return_output(self, outputs, target):
        return_outputs = []
        if target < 0:
            for output in outputs:
                return_outputs.append(output*-1)
            return return_outputs
        return outputs
```
- まずheapqに、引数numsの要素を大きさ順に入れていく。この際、元のindexも併せてsetとして入れる
- このソートされたsetのリストをfor文で1要素ずつ取り出していく
  - あとで気づいたのですが、きちんとheapq.heappopしないと数の小さい順に取得できませんかね。。。
- 取り出した要素(setの中の元のindexの値)を結果出力用リストにappend
- 引数targetから(setの中の)numの値を減じる
- これでtargetが0になったら、結果出力用リストを返す
- 仮に0未満になってしまったら、結果出力用リストから要素(index)を削除し、対応するnumの値を加算する
- このような作りでうまくいくことを期待したが、結果としてはWrong Answer

### STEP2
- 他の人の解いた解答を確認する
```python
import heapq

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_to_index = {}
        for index, num in enumerate(nums):
            complement = target - num
            if complement in num_to_index:
                return (index, num_to_index[complement])
            num_to_index[num] = index
```
- heapqは使わない
- numsからfor文で要素を1ずつ取得する。その際、enumerateでindexも取得できるようにする
- targetから、取得した要素を減じた値を変数complementに保持
- 辞書型変数num_to_index中にcomplementが含まれていれば、取得した要素のindexとcomplementのindexの2要素を結果として返す
- 含まれていなければ、num_to_indexに取得した要素のnumをkey、要素のindexをvalueとして保存
- 以前に保存しておいた値とnumsの各要素numの足した結果がtargetになりうるかを検証している
- 作りとしては非常にシンプル

### STEP3
- 1回目
```python
import heapq

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_to_index = {}
        for index, num in enumerate(nums):
            complement = target - num
            if complement in num_to_index:
                return [index, num_to_index[complement]]
            num_to_index[num] = index
```
- 所要時間: 約2分

- 2回目
```python
import heapq

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_to_index = {}
        for index, num in enumerate(nums):
            complement = target - num
            if complement in num_to_index:
                return [index, num_to_index[complement]]
            num_to_index[num] = index
```
- 所要時間: 約2分
- 3回目
```
import heapq

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_to_index = {}
        for index, num in enumerate(nums):
            complement = target - num
            if complement in num_to_index:
                return [index, num_to_index[complement]]
            num_to_index[num] = index
```
- 所要時間: 約2分
