### STEP1
- まずは回答を見ずに解いてみる
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        word_sets = []
        for word in strs:
            alphabets = []
            for alphabet in word:
                alphabets.append(alphabet)
            word_sets.append(set(alphabets))

        print(f"word_sets={word_sets}")

        mappings = {}
        for target_index, target_word_set in enumerate(word_sets):
            add = []
            for compare_index, compare_word_set in enumerate(word_sets):
                if target_word_set == compare_word_set and target_index < compare_index:
                    add.append(compare_index)
                    mappings[target_index] = add
                    print(f"add={add}")
                    print(f"target_index={target_index}, compare_index={compare_index}")

        result = []
        for index, word in enumerate(strs):
            pair = []
            pair.append(word)
            print(f"mappings={mappings}")
            for target_index, compare_index in mappings.items():
                if target_index in mappings.keys():
                    for compare_index in mappings[target_index]:
                        if index == target_index and target_index < compare_index:
                            pair.append(strs[compare_index])
                            print(f"pair={pair}")
            result.append(pair)
        
        seen = []
        convert_1 = [inner for inner in result for x in inner if x not in seen and not seen.append(x)]
        seen = []
        convert_2 = [x for x in convert_1 if x not in seen and not seen.append(x)]
        return convert_2
```
- 以下の場合、Wrong Answer
```
input = ["ddddddddddg","dgggggggggg"]
```
- output
```
[["ddddddddddg","dgggggggggg"]]
```
- expected
```
[["dgggggggggg"],["ddddddddddg"]]
```
- やったこと
  - setで文字の重複を削除した
  - その後、それらのsetのlistを二重for文で比較し、一致した場合、key、value構成で保持するようにした
    - この際、当然indexが同じものや同じ組み合わせのものは省くようにした
    - valueについては複数indexが対応することがあり得ることから、list構造にした
  - Wrong Answerが示している通り、set型で比較すると、同じアルファベットで構成されていて、その数が違う場合に対応できないことが分かった

### STEP2
- 他の人の回答を見てみる
- https://github.com/hayashi-ay/leetcode/pull/19/files
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)
        for s in strs:
            groups[tuple(sorted(s))].append(s)
        return list(groups.values())
```
- 分かったこと
- tupleを使用している
  - tuple
    - `緯度 35.686321 と、経度 139.782211 をカンマ , で区切って記述し、一つのタプルオブジェクトとして 変数 ningyocho に代入しています。`
      - https://www.python.jp/train/tuple/index.html#%E3%82%BF%E3%83%97%E3%83%AB%E3%81%AE%E6%9B%B8%E3%81%8D%E6%96%B9
    - 複数の要素を一つの内容として表現するのに適した型である
### STEP3
- 間違えずに3回連続で記述する
- 1回目
  - 所要時間: 約2分
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        sort_str_to_anagrams = defaultdict(list)
        for word in strs:
            sort_str_to_anagrams[tuple(sorted(word))].append(word)
        return [sort_str_to_anagram for sort_str_to_anagram in sort_str_to_anagrams.values()]
```
- 2回目
  - 約1分
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        sort_str_to_anagrams = defaultdict(list)
        for word in strs:
            sort_str_to_anagrams[tuple(sorted(word))].append(word)
        return [sort_str_to_anagram for sort_str_to_anagram in sort_str_to_anagrams.values()]
```
- 3回目
  - 約2分
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        sort_str_to_anagrams = defaultdict(list)
        for word in strs:
            sort_str_to_anagrams[tuple(sorted(word))].append(word)
        return [sort_str_to_anagram for sort_str_to_anagram in sort_str_to_anagrams.values()]
```
