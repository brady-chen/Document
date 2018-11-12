# 关于python的知识与技巧
## 一、python代码小技巧
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

## 二、面向对象
### 1.关于继承
- 面向对象编程 (OOP) 语言的一个主要功能就是“继承”。继承是指这样一种能力：它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。
- 通过继承创建的新类称为“子类”或“派生类”，被继承的类称为“基类”、“父类”或“超类”，继承的过程，就是从一般到特殊的过程。在某些 OOP 语言中，一个子类可以继承多个基类。但是一般情况下，一个子类只能有一个基类，要实现多重继承，可以通过多级继承来实现。
- 继承概念的实现方式主要有2类：实现继承、接口继承：
 1. 实现继承是指使用基类的属性和方法而无需额外编码的能力。
 2. 接口继承是指仅使用属性和方法的名称、但是子类必须提供实现的能力(子类重构爹类方法)。
- 在考虑使用继承时，有一点需要注意，那就是两个类之间的关系应该是“属于”关系。例如，Employee 是一个人，Manager 也是一个人，因此这两个类都可以继承 Person 类。但是 Leg 类却不能继承 Person 类，因为腿并不是一个人。
- OO开发范式大致为：划分对象→抽象类→将类组织成为层次化结构(继承和合成) →用类与实例进行设计和实现几个阶段。

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
### 7.关于多态
- 在一个函数中，如果我们接收一个变量 x，则无论该 x 是 Person、Student还是 Teacher，都可以正确打印出结果：

```python
class Person:
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender

    def who_am_i(self):
        return 'I am a Person, my name is %s' % self.name

class Student(Person):
    def __init__(self, name, gender, score):
        Person.__init__(self, name, gender)
        # super(Student, self).__init__(name, gender)
        self.score = score

    def who_am_i(self):
        return 'I am a Student, my name is %s' % self.name

class Teacher(Person):
    def __init__(self, name, gender, course):
        Person.__init__(self, name, gender)
        # super(Teacher, self).__init__(name, gender)
        self.course = course

    def who_am_i(self):
        return 'I am a Teacher, my name is %s' % self.name

def who_am_i(x):
    print(x.who_am_i())

p = Person('Tim', 'Male')
s = Student('Bob', 'Male', 88)
t = Teacher('Alice', 'Female', 'English')

who_am_i(p)
who_am_i(s)
who_am_i(t)
```

运行结果：
I am a Person, my name is Tim
I am a Student, my name is Bob
I am a Teacher, my name is Alice

- 这种行为称为多态。也就是说，方法调用将作用在 x 的实际类型上。s 是Student类型，它实际上拥有自己的 whoAmI()方法以及从 Person继承的 whoAmI方法，但调用 s.whoAmI()总是先查找它自身的定义，如果没有定义，则顺着继承链向上查找，直到在某个父类中找到为止。

## 三、Python基础

### 1.Python中浅拷贝与深拷贝问题
```
# 对可变的数据类型进行引用时，需进行深拷贝，否则不会开辟新内存来存放新对象j
# 单层级使用copy()
a = [1, 2, 3]
b = a.copy()
b = [3, 4, 5]
print(a, b)
>>>[1, 2, 3] [3, 4, 5]
# 多层级使用deepcopy
import copy
a = [1, 2, [2, 4, 5]]
b = copy.deepcopy(a)
a[2][0] = 3
print(a, b)
>>>[1, 2, [3, 4, 5]] [1, 2, [2, 4, 5]]
```

### 2.数据类型
**标准数据类型**
python3中有六个标准的数据类型
- Number(数字)
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）
其中：
- **不可变数据（3个）**：Number（数字）、String（字符串）、Tuple（元组）；
- **可变数据（3个）**：List（列表）、Dictionary（字典）、Set（集合）。