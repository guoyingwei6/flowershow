---
created: 2024-07-16 22:28
updated: 2025-06-10 09:55
---

## 交集并集补集差集


### 一、交集

sort a.txt b.txt | uniq -d

### 二、并集

sort a.txt b.txt | uniq 

### 三、差集

a.txt-b.txt:

sort a.txt b.txt b.txt | uniq -u

b.txt - a.txt:

sort b.txt a.txt a.txt | uniq -u

### 四、相关的解释

使用sort可以将文件进行排序，可以使用sort后面的玲玲，例如 -n 按照数字格式排序，例如 -i 忽略大小写，例如使用-r 为逆序输出等

uniq为删除文件中重复的行，得到文件中唯一的行，后面的命令 -d 表示的是输出出现次数大于1的内容 -u表示的是

输出出现次数为1的内容，那么对于上述的求交集并集差集的命令做如下的解释：

  

sort a.txt b.txt | uniq -d：将a.txt b.txt文件进行排序，uniq使得两个文件中的内容为唯一的，使用-d输出两个文件中次数大于1的内容，即是得到交集

sort a.txt b.txt | uniq ：将a.txt b.txt文件进行排序，uniq使得两个文件中的内容为唯一的，即可得到两个文件的并集

sort a.txt b.txt b.txt | uniq -u：将两个文件排序，最后输出a.txt b.txt b.txt文件中只出现过一次的内容，因为有两个b.txt所以只会输出只在a.txt出现过一次的内容，即是a.txt-b.txt差集

但是！！！ a.txt 一定要保证本身没有重复啊，一定要先去重才行！！！

对于b.txt-a.txt为同理