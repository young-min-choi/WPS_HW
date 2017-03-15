# 41일차 

####실수 리스트 

-     

---
####더 찾아봐야할 내용 및 질문 

- Python super()에 대한 이해
	- python super는 2가지 유용성을 가진다. 첫 번째로, 상속관계에서 자식클래스가 부모 클래스를 참조할때 명시적(not explicitly name Parent Class' name)으로 부모 클래스의 이름을 나타낼 필요가 없이 super()로 참조하는 것이 가능하다. 
	- **진짜 중요한 것은 multiple inheritance관계에서 python super()가 가지는 특이함으로 dynamic execution env를 지원한다는 것이다.**
	
	- 공식 다큐멘테이션에서 super는 다음과 같이 정의된다. 
	- **Return a proxy object that delegates method calls to a parent or sibling class of type. This is useful for accessing inherited methods that have been overridden in a class. The search order is same as that used by getattr() except that the type itself is skipped.**
	- 2단계 위의 할아버지 클래스를 참조하려면 super([class_name].__base__[0])과 같은 방법이 있다. 
	- Nesting super doesn't work because the type of super isn't the parent type.
	- super는 2개의 argument를 받을 수 있는데 하나는 type이고 하나는 object이다. 이 부분도 이해가 명확하지 않다.
	- MRO(Method Resolution Order)는 파이썬 컴파일시 상속관계에 순서를 정의하는 내용이다? [참조_파이썬_슈퍼튜토리얼](http://www.python-course.eu/python3_multiple_inheritance.php)
	- 다음 부분은 이해가 되지 않는 부분이다. 
> super(), in a class method, is equivalent to super(ReferenceToClassMethodIsBeingDefinedIn, self), where ReferenceToClassMethodIsBeingDefinedIn is determined at compile time, attached to the method as a closure named __class__ and super() will look up both from the calling frame at runtime. But you don't actually need to know all this.


---

####수업로드 

- 장고 REST 튜토리얼 



	
---
####수업진행 포인트

- argument에 사전 넣기