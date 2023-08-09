---
layout: post
title: 파이썬의 효율적인 여러가지 기능들
toc: true
---

# 목록  
{:toc}
 
## 타입 힌트  

**타입 힌트**란 타입을 지정할 수 있는 것을 말한다. 파이썬은 동적 타이핑 언어임에도 3.5 버전부터 해당 기능을 지원하기 시작했다.  
타입의 선언에는 다음과 같은 방법을 이용 할 수 있다.  

~~~python
a: str = "1"
b: int = 1
~~~ 
  
일반적으로 기존에 타입 힌트를 사용하지 않을 경우의 파이썬 함수는 다음과 같이 작성되었을 것이다.  

~~~python
def fn(a):
~~~  

위처럼 빠르게 정의할 수 있다는 장점이 있지만 매개변수 a에 무엇을 넘겨줘야 하는지 어떤 형태의 값이 반환되는지 코드를 보는 사람은 알 수 없다.  
그렇다면 다음을 살펴보자  

~~~python
def fn(a: int) -> bool
~~~  

여전히 우리는 함수의 기능을 이름으로부터 추론 할 수 밖에 없겠지만 적어도 다음 함수는 int형태의 매개변수를 받고 bool 값을 리턴할 것이라는 걸 명시적으로 알 수 있게 되었다!  
그럼에도 위의 타입 힌트는 규약 같은 것이 아니므로 여전히 동적으로 할당 될 수 있다.  
예를 들어  

~~~python
a: str = 1
type(a)
# Output <class 'int'>
~~~  

온라인 코딩테스트 중이라면 pip install mypy를 이용하여 mypy를 설치하면 선언한 타입 힌트에 어긋나는 선언 발생 시  
Incompatible return valy type 오류를 발생시켜 주므로 유용하다.  

## 리스트 컴프리헨션  
**리스트 컴프리헨션**이란, 기존 리스트를 기반으로 새로운 리스트를 만들어내는 구문으로 기존 리스트를 기반으로 새로운 리스트를 만들어 내는 구문이다.  
먼저 파이썬은 map, filter와 같은 함수형 기능을 지원하며 다음과 같은 람다 표현식도 지원한다.

~~~python
list(map(lambda x : x+10, [1, 2, 3]))
# Output : [11, 12, 13]
~~~  

그러나 지금처럼 map, filter 그리고 람다 표현식을 섞어 사용하는 것 보다 리스트 컴프리헨션을 이용한다면 코드의 가독성이 더욱 좋아진다.  
다음의 코드는 1부터 10까지의 정수 중에서 홀수인 경우 2를 곱해 출력하라는 리스트 컴프리헨션이다.

~~~python
[n * 2 for n in range(1,11) if n % 2 = 1]
# Output : [2, 6, 10, 14, 18]
~~~

이걸 for 반복문을 풀어서 쓰려면 별도의 리스트를 하나 더 만들고 길어지게 될 것이다.  
번외로 버전 2.7 이후로 가능해진 딕셔너리 컴프리헨션도 있는데 이는 리스트 외에도 딕셔너리등이 가능하도록 추가됐다.  
다음의 코드를

~~~python
a = {}
for key, value in original.items():
    a[key] = value
~~~

이렇게 바꿔본다면 좀 더 읽기 편하지 않을까?

~~~python
a = {key : value for key, value in original.items()}
~~~

어떤 것을 선택하는지는 매 순간순간 우리의 결정이지만 좀 더 깔끔해보이는 방법을 이용 할 수 있도록 습관을 들여보도록 노력하자.  

## 제너레이터

**제너레이터**란 간단하게 말하자면 루프의 동작을 제어할 수 있는 루틴 형태를 말한다.  
예를 들어 숫자 1억 개를 만들어내 계산하는 프로그램을 작성한다고 가정해보자.
메모리 어딘가에 1억 개의 숫자를 보관하고 있다가 꺼내서 계산한다면 너무 비효율적이지 않을까?  
이때 **제너레이터**를 이용하면 언제든 값을 생성해낼 수 있다.  
'yield'라는 구문을 이용하면 제너레이터를 리턴 할 수 있다.  
'return'을 이용하면 값을 리턴해버리고 함수의 모든 동작을 종료하지만  
'yield'는 제너레이터가 현 시점까지 실행중이던 값을 리턴하고 함수가 맨 끝에 도달할 때까지 계속 실행된다.  
다음 코드를 보자  

~~~python
def get_natural_number():
    n = 0
    while True:
        n += 1
        yield n
~~~

이 경우 함수의 리턴 값은 다음과 같이 **제너레이터**가 된다.

~~~python
get_natural_number()
# Output : generator object get_natural_number at 0x10d3139d0
~~~

만약 다음 값을 생성하려면 **next()**함수를 이용해서 추출하면 된다. 예를 들어서 100개의 값을 생성하고 싶다면  
단순히 100번의 next()를 호출하기만 하면 된다.  

~~~python
g = get_natural_number()
for _ in range (0,100):
    print(next(g))
# Output : 
# 1
# 2
# .....
# 100
~~~

아울러서 제너레이터는 다음과 같이 여러 타입의 값을 하나의 함수에서 생성하는 것도 가능하다.

~~~python
def generator():
    yield 1
    yield '02'
    yield True

g = generator()

g
# Output : generator object generator at 0x10d3139d0
next(g)
# Output : 1
next(g)
# Output : '02'
next(g)
# Output : True
~~~

### range()

제너레이터의 방식을 활용하는 대표적인 함수로 range()가 있다. 주로 for 문에서 이용된다.  
다음은 range() 함수의 쓰임이다.

~~~python
list(range(5))
# Output : [0, 1, 2, 3, 4]
range(5)
# Output : range(0, 5)
type(range(5))
# Output : <class 'range'>
for i in range(5)
print(i, end=' ')
# Output : 0 1 2 3 4
~~~

이 코드에서 range()는 generator와 같이 클래스를 리턴하며 loop에서 이용할 경우 내부적으로 next()를 호출하듯 매번 다음 숫자를 생성해내므로 next()를 명시하지 않아도 된다.  
다 알겠는데 그럼 range()를 쓰는 이유는 뭘까?  
다음의 코드를 보자, 100만개의 숫자를 생성하는 두가지 방법이다.

~~~python
a = [n for n in range(1000000)]
b = range(1000000)
~~~

실제로 len()으로 길이 비교를 해보면 동일한 100만개가 출력되고 비교 연산자로 연산해도 True를 리턴한다.  
그러나 이미 a에는 생성된 값이 담겨 있고, b는 생성해야 한다는 조건만 존재한다.

~~~python
b
# Output : range(0, 1000000)
type(b)
# Output : <class 'range'>
~~~

잘 안 와닿는다면 a와 b 둘 사이의 메모리 점유율을 비교해보면 된다.

~~~python
sys.getsizeof(a)
# Output : 8697464
sys.getsizeof(b)
# Output : 48
~~~

더 깊게 생각해본다면 심지어 b는 100만개가 아니라 더 많은 변수를 가정하더라도 여전히 동일한 메모리 점유율을 가지고 있다.  
앞서 말했듯이 생성 조건만 보관하고 있기 때문이다.  
그렇다면 변수가 생성 되어있는 a와 달리 b는 조건만 가지고 있는 클래스이니 인덱스 접근이 바로바로 불가능할까?

~~~python
b[999]
# Output : 999
~~~

바로바로 접근할때마다 생성을 알아서 갈겨주기 때문에 불편 없이 이용 가능하다.

## enumerate

**enumerate**는 여러가지 자료형을 인덱스를 포함한 enumerate 객체로 리턴한다.  
다음과 같이 사용한다.

~~~python
a = [1, 2, 3, 2, 45, 2, 5]
a
# Output : [1, 2, 3, 2, 45, 2, 5]
enumerate(a)
# Output : <enumerate object at 0x1010f83f0>
list(enumerate(a))
# Output :[(0, 1), (1, 2), (2, 3), (3, 2), (4, 45), (5, 2), (6, 5)]
~~~

이처럼 list()로 결과를 추출해주는데, 인덱스를 자동으로 부여해주기 때문에 굉장히 편리하게 활용 가능하다.  
만약 a = ['a1', 'b1', 'c1'] 이라는 리스트가 있을 때 인덱스와 함께 추출하려면 어떻게 해야할까?  
단순 루프문 내에서 따로따로 추출도 가능하겠지만 enumerate를 한번 이용해보자면

~~~python
for i, v in enumerate(a):
    print(i, v)
~~~

인덱스와 값 모두 깔끔하게 처리된다.

## pass
코드의 골격을 잡아두고 내부의 동작은 차근차근 구현해야 할 때가 많다.  
이때 다음과 같이 코딩하면 에러가 발생한다.

~~~python
class MyClass(object): 
    def method_a(self):

    def method_b(self):
        print("This is for b")
    
c = MyClass()
# Output : 
# def method_b(self)
# IndentionError: expected an indented block
~~~

놀랍게도 method_a가 아무런 처리를 하지 않았기 때문에 method_b에서 난리가 나게 된다.  
당연히 필요한 오류이나 개발 사이즈가 커진다면 생각보다 처리하기가 번거롭고 까다로울것이다.  
이때 method_a 자리에 pass를 추가해주면 

~~~python
class MyClass(object): 
    def method_a(self):
        pass
    def method_b(self):
        print("This is for b")
    
c = MyClass()
# Output : This is for b
~~~
정상적으로 작동한다.
이때의 pass는 null 연산으로 작동하여 아무것도 하지 않는 기능이다.
굉장히 유용하게 이용 할 수 있다.

## locals()

**locals()**는 복잡하게 설명할 필요 없이 로컬에 선언된 모든 변수를 조회해서 띄워주는 강력한 명령어이다.  
디버깅에 크게 도움이 되며 메소드 체이닝 방법으로 스코프를 조절하여 클래스의 특정 메소드 내부나 단순 함수 내부 등 범위를 조절하여 이용 가능하다.