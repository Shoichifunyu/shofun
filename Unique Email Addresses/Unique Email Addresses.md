### STEP1
- 回答を見ずに解いてみる
```python
class Solution:
    def numUniqueEmails(self, emails: List[str]) -> int:
        new_emails= []
        for email in emails:
            local_name = email.split('@')[0]
            domain_name = email.split('@')[1]
            new_local_name = ''
            for word in local_name:
                if word == '.':
                    continue
                elif word == '+':
                    break
                else:
                    new_local_name += word
            new_emails.append(new_local_name+'@'+domain_name)
        return len(set(new_emails))
```
- for文の二重ループがある。local nameの長さをM、domain nameの長さをNとした時の時間計算量はO(M(M+N))

### STEP2
- 他の人の回答を確認
```python
class Solution:
    def numUniqueEmails(self, emails: List[str]) -> int:
        canonicalized_emails = set()
        valid_letters = set(string.ascii_lowercase + "@+.")        
        def get_canonicalized_version_if_valid(email):
            for c in email:
                if c not in valid_letters:
                    return None
            if '@' not in email:
                return None
            local_domain = email.split("@")
            if len(local_domain) != 2:
                return None
            local, domain = local_domain
            if len(local) == 0 or len(domain) == 0:
                return None
            local =  local.replace(".", "")
            if "+" in local:
                local = local.split("+")[0]
            return local + "@" + domain

        for email in emails:
            canonicalized_email = get_canonicalized_version_if_valid(email)
            if canonicalized_email is not None:
                canonicalized_emails.add(canonicalized_email)

        return len(canonicalized_emails)
```
- バリデーションチェックをして、無効なものはNoneを返すようにしている
  - 実用的
- splitやreplaceなど、pythonの関数をうまく利用して1文字ずつの探索を行わないようにしている

### STEP3
- 間違えずに3回連続で記述する
- 1回目
- 2回目
- 3回目
