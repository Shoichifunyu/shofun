### Step1
- 何も見ずに解く
  - 問題から考えたこと
    - 概要としては、文字列の中から、1度しか出現しないアルファベット(小文字)の出現箇所をindexで出力する
      - 出力するのは一番先頭(左から数えた)の要素
      - 2度以上存在するアルファベットしかない場合は、-1で返す
    - つまり、文字列の末尾まで探索を行わないと分からない
      - 文字列をNとしたときの計算量は最低でもO(N)？
      - 二分探索は使えるか？
        - 二分探索の場合、「ある値に対してそれ以上かそれ以下かで判定していく」ので、適切ではない
      - アルファベット総体のset(string.ascii_letters)を用意し、対象の文字列をfor文で単語ごとに処理する
        - 処理内容は、setに含まれていたら、その単語をsetから取り除く
        - 取り除けない場合は、それまでに削除済みなので、2度以上存在する
        - 文字列の最後まで探索し、取り除けた単語のindexの最小値を出力する
```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        valid_letters = set(string.ascii_letters)
        pos = {}
        max_value = 10**5
        for index, word in enumerate(s):
            if word in valid_letters:
                pos[word] = index
                valid_letters.remove(word)
            else:
                pos[word] = max_value

        min_value = min(pos.values())
        if min_value == max_value:
            min_value = -1
        return min_value
```
- 制約に`1 <= s.length <= 10**5`とあったので最大でもindexが10の5乗になることはないと考えた。この 10の5乗の値を、-1の代わりに使い、min関数で最小のindexを出すことができた(最小が10の5乗の場合は、重複した文字しかないということで-1を最後に代入して返すようにした)

### Step2
- 他の人の回答を確認する
- https://github.com/yakataN/Arai60/blob/6acdaf7c8b721bb07aa5c2e401178541b24445a6/First%20Unique%20Character%20in%20a%20String/First%20Unique%20Character%20in%20a%20String.md
```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        import collections

        char_to_count_dict = collections.Counter(s)

        for index, char in enumerate(s):
            if char_to_count_dict[char] == 1:
                return index

        return -1
```
- collections.Counterを使うことで、dict部分の実装をシンプルにすることができている
- よりシンプルな実装(https://qiita.com/SaitoTsutomu/items/8eb33adbcde79aaf519f#387-first-unique-character-in-a-string)
```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        for k, v in collections.Counter(s).items():
            if v == 1:
                return s.index(k)
        return -1
```
- ただし、上記の場合、`index`の分の計算量が余計にかかってしまうため、前者の方がよいと考えた。

### Step3
- 間違えずに3回解く
- 1回目 1:11
- 2回目 1:17
- 3回目 1:08
