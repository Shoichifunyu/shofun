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

### STEP2
- 他の人の回答を確認
- https://github.com/h1rosaka/arai60/pull/20/files
- https://github.com/hayashi-ay/leetcode/pull/31/files
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        cumulative_sum_to_count = defaultdict(int)
        cumulative_sum_to_count[0] = 1
        num_subarrays_total_k = 0
        cumulative_sum = 0
        # numsを一つずつ見ていく
        for num in nums:
            # ここまでの累積和を計算
            cumulative_sum += num
            # ここで終了するsubarrayを考え、今まで観測した地点の内、合計がkになりうる(都合の良い)開始位置が何個あるか数えて足す
            complement = cumulative_sum - k
            num_subarrays_total_k += cumulative_sum_to_count[complement]
            # 次回以降のために、今回観測した地点の累積和も登録
            cumulative_sum_to_count[cumulative_sum] += 1
        return num_subarrays_total_k
```
- 違いとしては、defaultdictを使用している点
  - keyに累積和、valueに累積和がkになりうる場合の数(合計何通りか)を設定する
    - まず、numsのfor文内で探索する各要素の累積和とkの間の差分(補完関係にある数)がdict内に(0より大きな数として)登録されているかを確認し、`num_subarrays_total_k`に代入する
    - その後、累積和をkeyに、1インクリメントした値を代入する
    - これにより、次のループで、
      - k = 6
        - cumulative_num(1回目): 4 -> complement: -2
        - cumulative_num(2回目): 10 -> complement: 4
        - cumulative_num(3回目): 16 -> complement: 10
      - このような場合は、2回目と3回目のnumが6であることが想定できるから、結果的にkとの補完関係にある数が`num_subarrays_total_k`で0より大きな数として登録されているかは、numがkと等しいかということになる。
      - 上記の例では、各回数ごとにkの差のnumとなっているが、いくつかの回数を累積した結果がkになることもあり得るので、それを考慮したのが上記の実装という理解
      - ある直近のループの処理について、1つ前のループからの`cumulative_sum_to_count`の動きを確認すると、
        1. 1つ前のループでの配列の累積和をkeyに+1した値(subarrayのcount)を登録
        2. 補完数(直近のループまでの配列の累積和から求めたい累積和(固定)を減じたもの)を参照 -> 1以上であれば、結果(subarrayのcount)に加算される
        3. 直近のループまでの配列の累積和をkeyに+1した値(subarrayのcount)を登録
        
        となるので、`2.`の補完数というのが、`3.`の値から求めたい累積和(固定)を減じたもの、ということもできて、`cumulative_sum_to_count`に未登録の数に関する差分が、すでに`cumulative_sum_to_count`内の数として現れているか、を`2.`で確認している。回りくどくなったが、つまり、求めたい累積和(固定)が1つ前のループまでで累積された数の和(各ループで累積和を登録している)と直近の累積和で表現できるか、ということなのだと思う
### STEP3
- 間違えずに3回解く
  - 1回目: 約2分
  - 2回目: 約2分
  - 3回目: 約2分
