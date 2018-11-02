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
**查看类的解析顺序**
```python
class.mro()
```
**普通继承**
```python
class Parent:
    def __init__(self):
        self.parent = 'I\'m the parent.'
        print('Parent')

    def name(self, message):
        print(message, 'from Parent')

class Child(Parent):
    def __init__(self):
        Parent.__init__(self)
        print('Child')
        
    def name(self,message):
        Parent.name(self,message)
        print('Child Name function.')
        print(self.parent)
        
if __name__=='__main__':
    Child = Child()
    Child.name('HelloWorld')
```
**super继承**
```python
class Parent(object):
    def __init__(self):
        self.parent = 'I\'m the parent.'
        print('Parent')

    def name(self, message):
        print(message, 'from Parent')


class Child(Parent):
    def __init__(self):
        super(Child, self).__init__()
        print('Child')

    def name(self, message):
        super(Child, self).name(message)
        print('Child name fuction')
        print(self.parent)
        

if __name__ == '__main__':
    child = Child()
    child.name('HelloWorld')
```
**super在多继承中的使用**
```python
class A(object):
    def act(self):
        print("call A act method ....")

class B(object):
    def act(self):
        print("call B act method ....")

class C(B, A):       # 搜索的顺序会从B开始搜索，B存在act方法则调用并返回，不再继续搜索
    def act(self):
        super().act()

if __name__ == '__main__':
    c = C()
    c.act()
```
**运行时可改变类的继承树**
```python
class X(object):
    def m1(self):
        print("call X method")

class Y(object):
    def m1(self):
        print("call Y method")

class Z(X):
    def m1(self):
        super().m1()

def test_z():
    z = Z()
    z.m1()
    print("change Z class base for Y ....")
    Z.__bases__ = (Y,)
    z.m1()

if __name__ == '__main__':
    test_z()

```
**多继承中，按照广度优先的继承树搜索关系调用父类相同名称的方法**
```python
class IA(object):
    def __init__(self):
        print("call IA init ...")

    def msg(self):
        print('IA send msg')

class IB(IA):
    def __init__(self):
        print("call IB init ....")
        super().__init__()

    def msg(self):
        print('IB send msg')
        super().msg()

class IC(IA):
    def __init__(self):
        print("call IC init ....")
        super().__init__()

    def msg(self):
        print('IC send msg')
        super().msg()

class ID(IC, IB):
    def __init__(self):
        print("call ID init ...")
        super().__init__()

    def msg(self):
        print('ID send msg')
        super().msg()

def test_ID():
    d = ID()
    print(d.msg())

if __name__ == '__main__':
    test_ID()

```