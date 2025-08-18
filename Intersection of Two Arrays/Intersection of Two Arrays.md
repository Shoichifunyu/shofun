### STEP1
- 回答を見ずに自力で解く
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums_to_cluster = defaultdict(list)
        for num1 in nums1:
            nums_to_cluster[num1].append("nums1")
        for num2 in nums2:
            nums_to_cluster[num2].append("nums2")
        return [num for num, category in nums_to_cluster.items() if "nums1" in category and "nums2" in category]
```

### STEP2
- 他の人の回答を見る
- https://github.com/h1rosaka/arai60/pull/17/files
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        def check_existence(target: int, nums: List[int]) -> bool:
            left = 0
            right = len(nums)
            while right - left > 1:
                mid = (left + right) // 2
                if nums[mid] <= target:
                    left = mid
                else:
                    right = mid

            return nums[left] == target

        if len(nums1) > len(nums2):
            larger_nums = nums1
            smaller_nums = nums2
        else:
            larger_nums = nums2
            smaller_nums = nums1

        larger_nums.sort()
        smaller_nums_set = set(smaller_nums)
        commons = []
        for num in smaller_nums_set:
            if check_existence(num, larger_nums):
                commons.append(num)

        return commons
```
- 時間計算量がO(mlogn)に抑えられる(STEP1だとO(2(m+n))となる)
- 特にSTEP2の場合、要素数が片方だけ大きい場合に有効

### STEP3
- コーディングミスせずに3回連続で記述
- 1回目
  - 所要時間約6分
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        def check_existence(target: int, nums: List[int]) -> bool:
            left = 0
            right = len(nums)
            while right - left > 1:
                mid = (left + right) // 2
                if nums[mid] <= target:
                    left = mid
                else:
                    right = mid
            return nums[left] == target

        larger_nums = nums1 if len(nums1) > len(nums2) else nums2
        smaller_nums = nums2 if larger_nums == nums1 else nums1

        larger_nums.sort()
        smaller_nums_set = set(smaller_nums)
        common_integers = []
        for num in smaller_nums_set:
            if check_existence(num, larger_nums):
                common_integers.append(num)
        return common_integers
```
- 2回目
  - 所要時間約3分
- 3回目
  - 所要時間約5分
