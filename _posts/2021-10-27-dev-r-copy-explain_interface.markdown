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
  - 인터페이스의 특징

## 인터페이스란 무엇인가?
우선 위키백과에서 정의하는 인터페이스 입니다.

인터페이스(interface)는 자바 프로그래밍 언어에서 클래스들이 구현해야 하는 동작을 지정하는데 사용되는 추상 자료형(자료들에 대한 연산들을 명기 하는 것.)이다. 

예제 코드를 통해서 위의 말이 무엇을 의미하는지 설명해 보겠습니다.

```
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



1.핵심 클래스가 다른 클래스로부터 의존적이지 않은 독립적인 클래스가 될 수 있다.

한가지 상황을 가정하고 이를 코드로 풀면서 설명을 진행하겠습니다.





















