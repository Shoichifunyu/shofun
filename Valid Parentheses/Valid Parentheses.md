# 20. Valid Parentheses

- https://leetcode.com/problems/valid-parentheses/description/
## Comments
### Step1
* 20分くらいで下記のコードを書いた
```python
class Solution:
    def isValid(self, s: str) -> bool:
        round_left = '('
        round_right = ')'
        wave_left = '{'
        wave_right = '}'
        sqar_left = '['
        sqar_right = ']'
        split_list = list(s)
        round_left_cnt = 0
        round_right_cnt = 0
        wave_left_cnt = 0
        wave_right_cnt = 0
        sqar_left_cnt = 0
        sqar_right_cnt = 0
        for elem in split_list:
            if elem == round_left:
                round_left_cnt += 1
            elif elem == round_right:
                round_right_cnt += 1
            elif elem == wave_left:
                wave_left_cnt += 1
            elif elem == wave_right:
                wave_right_cnt += 1
            elif elem == sqar_left:
                sqar_left_cnt += 1
            elif elem == sqar_right:
                sqar_right_cnt += 1
        if (round_left_cnt == round_right_cnt and wave_left_cnt == wave_right_cnt and sqar_left_cnt == sqar_right_cnt):
            return True
        else:
            return False
```
* 結果は`Wrong Answer`
  * 例えば入力が`{[}]`のような「括弧の入れ違い」が発生した場合、本来はfalseだがtrueと判定されてしまう

### Step2
* https://leetcode.com/problems/valid-parentheses/solutions/2411675/very-easy-100-fully-explained-c-java-python-js-python3/ から下記のソースコードを確認
```python
class Solution(object):
    def isValid(self, s):
        # Create a pair of opening and closing parrenthesis...
        opcl = dict(('()', '[]', '{}'))
        # Create stack data structure...
        stack = []
        # Traverse each charater in input string...
        for idx in s:
            # If open parentheses are present, append it to stack...
            if idx in '([{':
                stack.append(idx)
            # If the character is closing parentheses, check that the same type opening parentheses is being pushed to the stack or not...
            # If not, we need to return false...
            elif len(stack) == 0 or idx != opcl[stack.pop()]:
                return False
        # At last, we check if the stack is empty or not...
        # If the stack is empty it means every opened parenthesis is being closed and we can return true, otherwise we return false...
        return len(stack) == 0
```
* 一見、複雑に見えたが、実際は実にシンプルなつくりとなっていたことが分かり、この回答を採用して作り直すことにした


### Step3

#### 1回目
```python
class Solution:
    def isValid(self, s: str) -> bool:
        opcl = {"(" : ")", "{" : "}", "[" : "]" }

        stack = []
        for idx in s:
            if idx in "({[":
                stack.append(idx)
            elif len(stack) == 0 or opcl.get(stack.pop()) != idx:
                return False

        return len(stack) == 0 
```
#### 2回目
```python
class Solution:
    def isValid(self, s: str) -> bool:
        opcl = {"(":")", "{":"}", "[":"]"}

        stack = []

        for idx in s:
            if idx in "{[(":
                stack.append(idx)
            elif len(stack) == 0 or opcl.get(stack.pop()) != idx:
                return False

        return len(stack) == 0
```

#### 3回目
```python
class Solution:
    def isValid(self, s: str) -> bool:
        opcl = {"(":")", "{":"}", "[":"]"}

        stack = []

        for idx in s:
            if idx in "{[(":
                stack.append(idx)
            elif len(stack) == 0 or opcl.get(stack.pop()) != idx:
                return False

        return len(stack) == 0
```
- 回答を参考に実装した。最初は理解に時間がかかるかと思ったが、思ったより習熟が早く進み、3回書き直しについても1回もミスすることなく終えることができた
