# Tesla-Tagged LeetCode
## Table of Contents
1. Easy
   1. [Maximum Number of Balloons](#maximum-number-of-balloons)

---
## Easy
### Maximum Number of Balloons
Given a string `text`, you want to use the characters of `text` to form as many instances of the word "**balloon**" as possible.

You can use each character in `text` **at most once**. Return the maximum number of instances that can be formed.

**Example 1**
```text
Input: text = nlaebolko
Output: 1
```

**Example 2**
```text
Input: text = loonbalxballpoon
Output: 2
```

**Solution**
```
def maxNumberOfBalloons(self, text: str) -> int:
    unique_letters = "balon"
    counts = []
    
    for l in unique_letters:
        counts.append(text.count(l))
        
    counts[2] = counts[2] // 2
    counts[3] = counts[3] // 2
    
    return min(counts)
```
`counts[2]` and `counts[3]` are where the double letters, "l" and "o", occur. To avoid over-counting `balloon`, divide their counts by the number of times they occur in the `text`.