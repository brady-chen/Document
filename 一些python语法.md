### 1.字符串格式化
- 建议：python3.6以上推荐使用**f-string**，其他版本使用**format**

**f-string示例:**
```python
a = 'hello'
b = 'world'
string = f'{a} {b}'
print(string)
>>>hello world
```
- 说明：连接少量字符串的情景中，使用+号的性能最优，但是使用+号的代码可读性最差。如果使用Python 3.6或以上版本，可以使用f-string来解决这个问题，f-string的性能比format方法和%操作符的性能都要高，可读性也比+号好

### 2.if true的条件判断
```python
a = True
# 第一种
if a:
  print(a)
# 第二种
if a is True:
  print(a)
# 第三种
if a == True:
  print(a)
```
- 建议：推荐使用第一种
- 说明：第一种方法的性能最高，并且语法上也更加简洁。

### 3.if false的条件判断
```python
a = 1
# 第一种
if a is not 2:
  print(True)
# 第二种
if not a is 2:
  print(True)
# 第三种
if a!= 2:
  print(True)
```
- 建议:推荐第一种
- 说明：第一种性能最高，第二和第三在语法上不推荐

### 4.is 和 == 的区别
```python
# 字符串中的is或==判断结果相同
a = 'hello'
b = 'hello'
a == b
>>>True
a is b
>>>True
# 整数中的-5到255的小整数对象池，使用is或==判断结果相同
a = 200
b = 200
a == b
>>>True
a is b
>>>True
a = -6
b = -6
a == b
>>>True
a is b
>>>False
```
- 说明：只有数值型和字符串型，并且在通用对象池中的情况下，a is b 才为 True，否则当 a 和 b 是 int，str，tuple，list，dict 或 set 型时，a is b 均为 False。
- 参考地址：[https://www.cnblogs.com/CheeseZH/p/5260560.html](https://www.cnblogs.com/CheeseZH/p/5260560.html)

### 5.下划线的区别
模式|举例|含义
:-|:-|:-
单前下划线|_var|命名约定，仅供内部使用的内部变量，主要起提示作用
单末尾下划线|var_|命名约定，避免与Python关键字命名冲突
双前下划线|__var|私有变量、私有函数
首尾双下划线|\__var__|表示Python语言定义的特殊方法/魔术方法，或者是系统定义的变量，避免使用这种命名方式
单下划线|_|临时变量
### 6.继承父类的两种方式
```python
print('hello world')
```