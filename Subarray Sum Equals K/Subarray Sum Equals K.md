### Step1
- 何も見ずに解く
  - 配列numsのサブ配列のそれぞれがkの値と等しいような配列の総数を求める問題
    - サブ配列は、配列内の連続的な要素からなる
  - 最も単純な解き方は、先頭の要素からkの値になるまで加算(calculator)していき、kと等しくなった時点で、subarray_cntを1インクリメントし、calculatorを0に戻す
    - 先頭の要素からインデックスを1ずらした点から、再びkの値になるまで加算(尺取り法)
  - 繰り返して末尾まで達したら、サブ配列の総数を返却

- 困った事
  - 単純に加算だけなら簡単なのだが、numsの要素が負の値の場合があり、これをどう対処するかで悩んだ。
  - 愚直に要素が正の場合と負の場合で分けたとしても、当該要素に続く要素が逆の符号だった場合のことを考えると、kの値を超えた(下回った)から処理を切り上げるとかそういったことができない
  - 故に、処理を切り上げるタイミングは、以下の~~2~~1通りしかなくなる。
    - ~~caluculatorの値がkと等しくなった場合~~
      - 1点目の`caluculatorの値がkと等しくなった場合`について、正負の値が混在している配列では、後続で結果としてプラマイ0になって、subarray_cntを1インクリメントできる可能性があるから、これも切り上げてはいけない
    - 配列の最後の要素まで処理が進んだ場合
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        total_cnt = 0
        subarray_cnt = 0
        pos = -1
        while True:
            total_cnt = 0
            pos += 1
            if pos == len(nums):
                return subarray_cnt
            for index, num in enumerate(nums[pos:]):
                total_cnt += num
                if total_cnt == k:
                    subarray_cnt += 1
```
- 結果としては、TLEで処理失敗
