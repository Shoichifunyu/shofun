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

- 感想
  - 回答を参考に実装した。最初は理解に時間がかかるかと思ったが、思ったより習熟が早く進み、3回書き直しについても1回もミスすることなく終えることができた
