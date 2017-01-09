# *****args, ******kwargs
## *****args
list 혹은 tuple을 함수의 parameter로 취할 수 있다. 선언을 특별히 하거나 일반 함수에 넣을 수 있다.

### 일반 함수에 넣는 방법
```python
def repeat (function, params, times):
    for calls in range (times):
        function (*params)

def foo (a, b):
    print ('{} are {}'.format (a, b) )

repeat(foo, ['roses', 'red'], 4)
repeat(foo, ['violets', 'blue'], 4)
```

### 선언을 특별히 하는 방법
```python
def print_everything(*args):
    for count, thing in enumerate(args):
        print '{0}. {1}'.format(count, thing)
```

args란 단어를 지킬 필요는 없다.


## ******kwargs
dictionary를 parameter로 취할 수 있다. kw는 keyword에 약자이고 value뿐만 아니라 key가 추가로 있다고 이해하자. 동일하게 두가지 방법이 있다.

### 일반 함수에 넣는 방법
```python
def test_dic(a="1",b="2"):
    print(a)
    print(b)

kw = {"a":"3","b":"4"}
test_dic(**kw)
# <result>
# 3
# 4
```

### 선언을 특별히 하는 방법
```python
def table_things(**kwargs):
   for name, value in kwargs.items():
       print '{0} = {1}'.format(name, value)

table_things(apple = 'fruit', cabbage = 'vegetable')
# <result>
# cabbage = vegetable
# apple = fruit
```

이 또한 kwargs란 단어를 지킬 필요없다.


## *****args와 ******kwargs의 관용적 표현
```python
def test_func(*args, **kwargs):
    print(args)
    print(kwargs)

test_func(1,2,3, a=4,b=5)
# <result>
# (1, 2, 3)
# {'b': 5, 'a': 4}
```
