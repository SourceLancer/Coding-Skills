# 面试技巧篇 -- 编码风格 (Coding Style)

## Coding Style 在面试中有什么影响？
算法面试已经是绝大多数high-tech公司的敲门砖，这也导致了求职者花费大量时间准备算法刷题，编码风格作为代码的脸面却往往被忽略。
在网上也经常有人吐槽，为什么算法写出来了面试还是没过？这种情况大概率是你的编码风格出卖了你。

对于一个有多年经验的程序员，代码风格是刻在骨子里的。就如同写字一般，写的一首好字的人会更容易给别人留下较好印象。相应的，好的代码风格会让你的面试效果事半功倍，不健康的代码风格却也可能会导致你的面试功亏一篑。要知道，面试官是在寻找自己未来的同事，如果一个人算法能力再强，但写出的代码可读性很差让人头疼，你会愿意让他成为你未来的同事吗？答案是显而易见的。

## Coding Style 实例
很多开源的 Coding Style 文档可以在网上直接查找， 比如[Google的开源项目风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/)。但对于准备面试，这些文档显得过于冗长，缺少针对性。在一个45分钟的面试中，我们能被问到的问题大多可以在300行代码之内解决，工程量小。

本文将帮助你在10分钟内掌握面试相关的Coding Style技巧。

### 面试中需不需要写代码注释？
答案是不需要，面试的代码量很小，通常是一眼就可以看懂，面试官不会将注释做为考察代码能力的点。适当添加注释来辅助自己完成算法是被允许的。

### 命名规则
在所有的编码规则中，命名规则是面试中一定逃不掉且也是对代码可读性影响最大的。只要牢记以下几点命名规则，瞬间就可以让你的形象从代码小白变成老程序员。

#### 使用驼峰式命名法（Camel Case）
- 函数名，类名使用大驼峰式命名法(Upper Camel Case)，例：UpperCamelCase。
- 变量名使用小驼峰式命名法(Lower Camel Case)，例: lower_camel_case。
- 尽量不要缩写。不要担心命名过长，约定俗成的缩写除外（如：num）。

下面以leet code上的高频面试题“[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)”的Python代码为例说明不同命名风格之间的差距：

***代码小白↓***
```
def substring(s):  # 函数名应遵循 Upper Camel Case 命名规则
  n = len(s)  # 意义不明确的变量命名
  ans = 0  # 不建议的缩写
  mp = {} # 不建议的缩写
  
  i = 0  # 意义不明确的变量命名
  for j in range(n): # 循环中的i, j是约定俗成的方式，是被允许的
    if s[j] in mp:
      i = max(mp[s[j]], i)
      
    ans = max(ans, j - i + 1)
    mp[s[j]] = j + 1

  return ans
```

***潜在的同事↓***
```
def LengthOfLongestSubstring(input_string: str) -> int:  # 函数名遵循 Upper Camel Case 命名规则
  longest_length = 0  # 变量名遵循 Lower Camel Case 命名规则
  hash_map = {}  # 意义明确的变量名
  
  index_left = 0 # 意义明确的变量名
  for index_right in input_string:
    character = input_string[i]
    if character in hash_map:
      index = max(character, index_right)
    
    longest_length = max(longest_length, index_right - index_left + 1)
    hash_map[character] = index_right + 1
    
  return longest_length
```

从以上两段代码的比较中我们可以看出，在算法完全相同的前提下，良好的命名规则会大大的提升代码的可读性，也是编程经验的最直接的表现。

### 代码模块化
- 当代码量稍大，或明显有重复应用的子函数时，应有意识的将代码拆分成若干个子函数，提高代码的可读性。
- 函数内部，尽量减少嵌套（如：if...elif...else...）
- 函数中要加必要的 corner case test。

下面是以经典题目“[Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)”为例说明如何代码模块化。

***代码小白↓***
```
def letterCombinations(digits: str) -> List[str]:
  # 代码格式可读性差
  mapper = {'2' : ['a','b','c'],'3' : ['d','e','f'],'4' : ['g','h','i'],'5' : ['j','k','l'],'6' : ['m','n','o'],'7' : ['p','q','r','s'],'8' : ['t','u','v'],'9' : ['w','x','y','z']}
  l = len(digits)  # 意义不明确的变量命名
  if l == 0:  # 应尽量减少嵌套
    return []
  elif l == 1:
    return mapper[digits]
  else:
    idx = 1
    array = []
    
    # 可读性差
    while idx < l:
      newarray = []  # 变量命名未遵循Lower Camel Case命名规则。
      for combo1 in array:
        newarray.append(combo1)
        for combo2 in mapper[digits[idx]]:
          newarray.append(combo1 + combo2)
      array = copy.deepcopy(newarray)
    idx += 1
    
    return newarray
```

***潜在的同事↓***
```
def LetterCombinations(digits: str) -> List[str]:
  # 代码格式可读性好
  mapper = {'2' : ['a','b','c'],
            '3' : ['d','e','f'],
            '4' : ['g','h','i'],
            '5' : ['j','k','l'],
            '6' : ['m','n','o'],
            '7' : ['p','q','r','s'],
            '8' : ['t','u','v'],
            '9' : ['w','x','y','z']}

  length = len(digits)
  # Corner case test.
  if not length:
    return []
  if length == 1:
    return mapper[digits]
    
  return SearchAlgorithm(digits, hash_map)

# 子函数使代码逻辑更简洁
def SearchAlgorithm(digits: str, hash_map: Dict[str, List[str]])-> List[str]:
  if not digits:
    return []
  if len(digits) == 1:
    return hash_map[digits[0]]

  pivot = int(len(digits) / 2)
  left = SearchAlgorithm(digit_lst[:pivot])
  right = SearchAlgorithm(digit_lst[pivot:])

  result = []
  for i in left:
    for j in right:
      result.append(i+j)

  return result 
```

从以上两段代码可以看出，合理的运用子函数，会大大提升代码可读性。同时，子函数的使用会更方便递归调用算法的实现。


只要在日常的算法练习中留心***命名规则***和***算法模块化***这两点，会让你在面试中的表现事半功倍，成为面试官的首选。

[NEXT 面试过程中的高频算法](https://sourcelancer.github.io/RegularlyAppearedTopics/)
