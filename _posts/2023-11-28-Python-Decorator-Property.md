---
layout: post
title: 파이썬 데코레이터(@Property)에 대해 알아보자
subtitle: Property의 기능과 역할에 대해 알아보자
categories: 파이썬
tags: [파이썬]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`

### 데코레이터란?
이번 포스팅은 파이썬의 **"데코레이터(Decorator)"**라는 기능에 대해 정리해보고자 한다.

자주 사용하고, 잘 활용하면 굉장히 효율적으로 작업을 할 수 있기 때문에 한번 자세히 알아보도록 하자!

데코레이터는 말 그대로 "장식하다"라는 의미로 파이썬에서도 유사한 의미를 가지고 기능을 한다.

말 그대로 기존의 코드에 여러가지 기능을 추가하는 파이썬 구문이라고 생각하면 되겠다.

파이썬 데코레이터에는 많은 기능이 있는데 이번 포스팅에서는 "@Property"에 대해서 알아 보도록 하자!

물론, 이외의 데코레이터도 추후에 정리할 예정이다.



### @Property란?

위에서 말한 것 처럼 데코레이터 중 하나인데... 이게 참 특정 개념도 아니고 기능을 단순히 글로 정리를 하자니 어렵기도 해서.. 이번 포스팅에서는 설명보다는 간단한 예시를 통해서 어떤 기능을 하는지 정리해보려고 한다.



### Class(클래스) 필드와 인스턴스

@property에 대해서 알아보기 전에 파이썬의 Class(클래스)와 인스턴스에 대해 정말 간단하게 알아보고 가자.

```Python
class Person:
    def __init__(self, name, gender, age):
        self._name = name
        self._gender = gender
        self._age = age
```

위와 같이 "Person"로 클래스 이름을 지정해주고, 이름을 의미하는 _name, 성별을 의미하는 _gender 그리고 나이를 의미하는 _age 인스턴스 변수를 만들었다.


위에서 만든 인스턴스를 한번 출력해보자!

```Python
>>> person = Person("Haruru", "Male", 30)
>>> person._age
30

>>> person._name
"Haruru"

>> person._age + 1
31
```

위의 코드를 보면 Person 클래스를 person이라는 인스턴스(객체)로 선언했다. 그 다음, 인스턴스 속성을 읽거나 새로운 값을 계산하여 출력할 수 있다.

다만, 이런식으로 하면 한가지(?) 문제가 있는데 이러한 데이터는 외부에서 접근했을 때 무방비 상태가 된다. 다시 말해서, 객체의 일부 구현 내용을 외부의 접근으로부터 막을 수 없어 특정 조작에 의해 값이 바뀔 수 있다는 뜻이다.

그래서 이러한 외부 접근을 막기 위해 파이썬에는 **캡슐화**라는 개념이 있는데 위 코드를 보면 인스턴스 속성 앞에 "_"가 붙어 있는 것을 볼 수 있다. 완벽한 캡슐화 방식은 아니지만 가장 많이 사용되는 방식 중 하나이다. 이것도 추후 포스팅하도록 하겠다. 일단은 여기서는 넘어가자!

좀 더 자세한 예시를 통해 Property를 쓰는 이유에 대해서 알아보자



### 예시

@Property를 쉽게 이해하기 위해서 간단한 클래스를 하나 만들어 보자!

```Python
class Person:
    def __init__(self, name, gender, age):
        self._name = name
        self._gender = gender
        self._age = age

    def get_age(self):
        print('나이 출력')
        return self._age

    def set_age(self, age):
        if age < 19:
            raise ValueError ('미성년자입니다')
        print('나이 갱신')
        self._age = age
```
아까 작성한 Person 클래스에 get_age()와 set_age() 인스턴스 메소드를 추가해서 작성해놨다.

get_age() 메소드는 나이를 출력하는 기능을 하고, set_age() 메소드는 19살 아래일 때는 '미성년자입니다'를 출력하도록 하고, 19살 이상일 때는 나이를 그대로 반환하도록 만들어두었다.

이렇게 새로 만든 인스턴스 메소드는 각각의 역할이 있다. Getter과 Setter로 각각의 역할이 있는데 이 두 개념이 어떤건지 알아보고 넘어가자!



### Getter/Setter
위의 코드를 설명할 때 한가지 더 정리해야 할 개념(메소드)이 있다. Getter와 Setter인데, 일반적으로 Getter 메소드는 데이터를 읽는 역할을 하고, Setter 메소드는 읽어온 값을 변경해주는 역할을 한다.

위에서 get_age() 메소드가 Getter 역할을 하고, set_age() 메소드는 Setter 역할을 한다고 보면 되겠다.

그런데 꼭 Getter과 Setter을 사용해야 하는가라는 궁금증이 생길 수 있다.

물론 꼭 써야 한다는 규칙은 없지만 클래스의 필드에 직접 접근하는 것을 방지하고, 객체의 데이터는 함부로 공개하지 않는다는 차원에서 개인적으로 사용하는 것을 권장하고 싶다.



#### 출력

위 클래스를 다시 한번 출력해보자.

```Python
>>> person = Person('Haruru', 'Male', 30)

>>> print(person.get_age())
나이 출력
30
```

우선 첫 번째 case를 보면 get_age() 메소드를 실행했을 때, 인스턴스 변수를 그대로 출력해준다. 여기까지는 다른 점은 없다.

```Python
>>> person.set_age(25)

>>> print(person.get_age())
나이 갱신
나이 출력
25
```

두 번째 case도 보면 set_age() 메소드를 통해서 다시 젊어지고 싶은 Haruru의 나이를 25세로 새로 갱신해줬다.

그 다음 get_age() 메소드를 출력해보면 '나이 갱신'을 출력한 뒤, 바로 나이가 출력되는 것이 아니라 '나이 출력'이라는 텍스트가 한번 더 출력되는 것을 볼 수 있다.

유추해보자면 get_age()를 실행했을 때 _age가 바로 출력되는 것이 아니라 set_age() 메소드를 통해서 나이 인자가 한번 갱신되고 그 다음 get_age()가 실행되는 것으로 볼 수 있다.

표면적으로는 단순히 일반 필드에 그대로 접근하는 것처럼 보일 수 있지만, 내부적으로는 Getter와 Setter 메소드가 동작되는 것을 알 수 있다.



### 다시 한번 확인해보자!

위의 클래스에서 set_age() 메소드에서 19세 미만이면 ValueError가 뜨게끔 작성해놨다.

이번에는 15세를 넣어서 위에서 말한 흐름이 맞는지 확인해보면 아마 제대로 작성을 했다면 ValueError가 뜰 것이다.

```Python
>>> person.set_age(15)

>>> print(person.get_age())
ValueError: 미성년자입니다
```


### @Property를 직접 사용해보자!

이제 Getter와 Setter 메소드를 어느정도 이해했으니 우리가 알고 싶은 @Property에 대해서 공부해보자

위의 클래스를 @Protperty를 사용해서 실행해보자

```Python
class Person:
    def __init__(self, name, gender, age):
        self._name = name
        self._gender = gender
        self._age = age

    @property
    def age(self):
        print('나이 출력')
        return self._age

    @age.setter
    def age(self, age):
        if age < 19:
            raise ValueError ('미성년자입니다')
        print('나이 갱신')
        self._age = age
```

위의 코드를 보면 각 메소드의 내용은 전과 다름이 없다. 하지만 메소드의 명칭(head)을 보면 이전과 다르다는 것을 알 수 있다.

이 상태에서 아래와 같이 실행해보자

```Python
>>> person = Person('Haruru', 'Male', 30)
>>> person.age
나이 출력
30
```

이전의 코드라면 age라는 변수를 출력해야 했지만, @property를 사용한 후에는 get_age() 메소드의 결과를 출력했다. 아마 감이 좋은 사람은 이렇게만 보고 대충 어떤 기능을 하는지 알 수 있을 것이다.

하지만 감이 좋지 못한 나는 좀 더 살펴보려고 한다.

