# Python 객체지향

## 객체지향

### OOP(Object-Oriented Programming)

프로그램 설계방법론이자 개념의 일종.
프로그램을 수많은 '객체(Object)'라는 기본 단위로 나누고 이들의 상호작용으로 서술하는 방식
객체란 하나의 역할을 수행하는 '메소드와 변수(데이터)'의 묶음으로 봐야한다.

예를들어 '사과'는 클래스, '내가 어제 먹은 사과 중 하나는' 실체(instance)라고한다.

```python
# Singer라는 클래스생성
class Singer:
	def sing(self):
		return "Lalala~"
    
# 빅뱅이라는 객체(인스턴스) 생성
bigbang = Singer()
# 빅뱅 객체에서 Singer클래스의 sing메서드 사용
bigbang.sing()
```



```python
# 캐릭터 클래스
class Amazon:
	strength = 20
	dexterity = 25
	vitality = 20
	energy = 15
	
	def attack(self):
		return "Jab!!!"
    
# 캐릭터 객체(인스턴스)생성
jane = Amazon()
mary = Amazon()
```

### self

**self**는 그 클래스의 객체를 가리킨다.
jane과 mary가 똑같은 attack 메서드를 가지기 때문에 서로 구별하기 위해서 사용
따라서 메서드를 정의할 때는 항상 **self** 인자를 사용해야 한다.

```python
# 새로운 메서드 추가
def exercise(self):
	self.strength += 2
	self.dexterity += 3
	self.vitality += 1

# 캐릭터 생성
eve = Amazon()
# 훈련 메서드 실행
eve.exercise()
# 힘 능력치
eve.strength
```

### 상속(Inheritance)

어떤 클래스를 만들 때 처음부터 모든 것을 새로 만들 필요없이, 핵심적인 성질을 갖고 있는 다른 클래스로부터 상속을 받아서 새로운 클래스를 만든다.

### 객체 속의 객체

```python
# Fridge 클래스생성
class Fridge:
	# 생성자 인스턴스를 생성하면 자동적으로 실행된다.
    def __init__(self):
        # 처음에는 냉장고 문이 닫혀있다.
        self.isOpened = False
        # 아무것도 없다
        self.foods = []
    
    # open 메서드를 실행
    def open(self):
        # 냉장고 문이 열린다.
        self.isOpened = True
        print '냉장고 문을 열었어요...'
    
    # thing라는 매개변수를 받는다.
    def put(self, thing):
        # 냉장고 문이 열려 있으면
        if self.isOpened:
            # Food객체를 추가한다.
            self.foods.append(thing)
            print '냉장고 속에 음식이 들어갔네...'
        else:
            print '냉장고 문이 닫혀있어서 못넣겠어요...'
    
    # 냉장고 문을 닫는다.
    def close(self):
        self.isOpened = False
        print '냉장고 문을 닫았어요...'

class Food:
    pass
```

