---
layout: post
title:  "[자바] 인터페이스(Interface)란 무엇인가?"
subtitle:   "인터페이스란 무엇인가?"
categories: dev
tags: r install
comments: true
---

## 개요
> 자바에서 사용하는 인터페이스가 무엇인지 설명해줍니다. 


- 목차
  - 인터페이스란 무엇인가?
  - 인터페이스를 쓰는 이유

## 인터페이스란 무엇인가?
우선 위키백과에서 정의하는 인터페이스 입니다.

인터페이스(interface)는 자바 프로그래밍 언어에서 클래스들이 구현해야 하는 동작을 지정하는데 사용되는 추상 자료형(자료들에 대한 연산들을 명기 하는 것.)이다. 

예제 코드를 통해서 위의 말이 무엇을 의미하는지 설명해 보겠습니다.

```java
//인터페이스를 정의하는 방법입니다. 예약어인 interface 를 사용해서 정의합니다.
interface Food{
	//인터페이스의 메소드는 몸통을 가질 수 없습니다. 규칙입니다.
	public String Name(String name);
	public void Taste(String taste);
}

// Cooker 클래스에서 Food를 상속합니다. 이 때, 사용하는 예약어는 implements 입니다.
class Cooker implements Food{

	// Cooker 클래스가 구현해야 하는 동작을 지정합니다.
	@Override
	public String Name(String name) {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public void Taste(String taste) {
		// TODO Auto-generated method stub
		
	}
	
}
```

위에서 "클래스들이 구현해야 하는 동작을 지정하는데 사용" 이라고 얘기 했습니다. 여기서 클래스는 Cooker 입니다. Cooker 클래스가 인터페이스인 Food 를 상속하게 되면 반드시 메소드를 재정의(오버라이드)해서 다시 선언해줘야 합니다.

## 인터페이스를 쓰는 이유

1.핵심 클래스가 다른 클래스로부터 의존적이지 않은 독립적인 클래스가 될 수 있다.

한가지 상황을 가정하고 이를 코드로 풀면서 설명을 진행하겠습니다.
1.난 음식점에서 손님들에게 음식을 서빙하는 웨이터다.
2.손님들이 오면 음식을 서빙한다. (치킨과 햄버거)

```java
class Food{
	String name;
	
	public void setName(String name) {
		this.name=name;
	}
}

class Chicken extends Food{
	
}

class Hambuger extends Food{
	
}

public class Waiter{
	
	public void Serve(Chicken chicken) {
		System.out.println("serve chicken");
	}
	
	public void Serve(Hambuger hambuger) {
		System.out.println("serve hambuger");
	}	
	
	public static void main(String[] args) {
		Waiter waiter=new Waiter();
		
		Chicken chicken=new Chicken();
		Hambuger hambuger=new Hambuger();
		
		waiter.Serve(chicken);		//실행결과 serve chicken
		waiter.Serve(hambuger);		//실행결과 serve hambuger
	}
}
```

하지만 여기서 새로운 음식 메뉴가 계속해서 추가 된다면 웨이터는 음식이 추가될 때 마다 아래와 같은 메소드를 추가해줘야 합니다.

```java
....

	public void Serve(Ramyun ramyun) {
		System.out.println("serve ramyun");
	}	
	
		public void Serve(Cake cake) {
		System.out.println("serve cake");
	}	
	
...	
```

매우 귀찮은 작업입니다. 이 때, interface 를 사용합니다. 앞에서 정의 했던 햄버거와 치킨 클래스에 인터페이스를 구현해보겠습니다. 이 때 예약어 **implements** 를 사용합니다.

```java
interface Serve{
	
}

class Chicken extends Food implements Serve{

	
}

class Hambuger extends Food implements Serve{
	
}
```



변경전의 코드

```java
	public void Serve(Chicken chicken) {
		System.out.println("serve chicken");
	}
	
	public void Serve(Hambuger hambuger) {
		System.out.println("serve hambuger");
	}	
```

변경후의 코드

```java
	public void Serve(Serve serve) {
		System.out.println("serve chicken");
	}
```

Chicken 과 Hambuger 객체가 각각 따로 필요로 했지만 이를 Serve 라는 인터페이스 하나로 대체할 수 있게 됐습니다.
chicken과 hambuger 는 각각  Chicken,Hambuger 객체이기도 하지만 Serve 인터페이스의 객체이기도 합니다. 때문에 Serve 자료형 타입으로도 사용할 수 있습니다. 
(이런 경우를 자바의 다형성이라고 합니다. 하나의 객체가 여러가지 타입을 가질 수 있는것을 의미 합니다. 자바에서는 이러한 다형성을 부모 클래스 타입의 참조 변수로 자식 클래스 타입의 인스턴스를 참조할 수 있도록 구현하고 있습니다.)

보통 중요 클래스(Waiter)를 작성하는 입장이라면, 클래스의 구현체(여기서는 햄버거, 치킨에 해당한다.)와 상관없이 인터페이스를 기준으로  중요 클래스를 작성해야만 합니다. 구현체인 치킨, 햄버거, 피자, 족발.. (배고프다)는 늘어가지만 인터페이스인 Serve 는 하나이기 때문입니다.

이번에는 인터페이스의 코드를 조금 수정해서 웨이터가 어떤 음식이든 서빙을 할 수 있도록 해보겠습니다.
(지금 코드의 상태로라면 손님이 햄버거를 시켜도 치킨을 먹어야 합니다.. )

Serve 인터페이스를 아래와 같이 수정해 봅시다.

```java
interface Serve{
	public String getName();
}
```

인터페이스의 메소드는 입출력에 대한 정의만 할 수 있습니다. 그게 규칙입니다. 여기까지 코드를 손봤다면 Chicken 클래스와 Hambuger 클래스의 메소드에서 Serve 인터페이스의 getName 메소드를 구현해야 한다고 에러가 뜰 것입니다.

구현해 봅시다!

```java
interface Serve{
	public String getName();
}

class Chicken extends Food implements Serve{

	@Override
	public String getName() {
		return "chicken";
	}

	
}

class Hambuger extends Food implements Serve{

	@Override
	public String getName() {
		return "hambuger";
	}
	
}
```

이제 Waiter 클래스도 아래와 같이 변경이 가능 합니다.

```java
public class Waiter{
	
	public void Serve(Serve serve) {
		System.out.println("serve "+serve.getName());
	}
	public static void main(String[] args) {
		Waiter waiter=new Waiter();		
		Chicken chicken=new Chicken();
		Hambuger hambuger=new Hambuger();
		waiter.Serve(chicken);					//print serve chicken
		waiter.Serve(hambuger);					//print serve hambugher
	}
}
```

실행을 해보면 각각 출력이 되는 것을 확인할 수 있습니다.



