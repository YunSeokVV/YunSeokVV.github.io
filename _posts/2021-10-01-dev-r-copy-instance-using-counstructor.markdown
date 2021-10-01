---
layout: post
title:  "[자바] 생성자를 이용해서 인스턴스를 복사하는 방법"
subtitle:   "R 설치 및 환경구성(10분만에 끝내는)2"
categories: dev
tags: r install
comments: true
---

## 개요
> 자바에서 생성자를 이용해 인스턴스를 복사하는 방법을 알려주는 글입니다. 
> 예제 코드를 통해서 방법을 알아봅시다.

- 목차
  - 생성자를 이용해서 인스턴스 복사하기

## 생성자를 이용해서 인스턴스 복사하기
예제 코드

```java
// 설명을 돕기위해 만든 쿠키 클래스입니다. 
class Cookie{
	// 쿠키의 맛을 표현하는 변수입니다.
	String flavor;
	// 쿠키의 모양을 표현하는 변수입니다.
	String shape;
	// 쿠키의 가격을 표현하는 변수입니다.
	int cost;
	
	// 이 클래스의 생성자 입니다. 
	Cookie(){
		// this() 를 사용하면 자기 자신의 생성자를 호출합니다. 세번째로 정의한 생성자가 호출됩니다. 
		//this() 를 사용하는 이유는 생성자의 초기화 과정을 반복하지 않기 위해서 입니다.
		this("초코맛","곰돌이 모양",4000);
	}
	
	// 오버로딩을 통해 원하는 형태의 생성자를 만들 수 있습니다. 이 생성자가  인스턴스를 복사하는 역할을 해줍니다.
	Cookie(Cookie cookie){
		flavor=cookie.flavor;
		shape=cookie.shape;
		cost=cookie.cost;
	}

	Cookie(String flavor, String shape, int cost) {
		//this 키워드는 현재 클래스의 인스턴스를 가리킵니다. 현재 클래스의 인스턴스에 있는 속성이나 함수를 제어하기 위해서는 this 예약어를 사용합니다.
		this.flavor=flavor;
		this.shape=shape;
		this.cost=cost;
	}
	
}

public class CookieMain {
	
	public static void main(String[] args) {

		// 생성자를 복사하기 위해서 인스턴스인 cookie1 를 생성합니다.
		Cookie cookie1=new Cookie();
		
		// 생성자를 이용해서 cookie1 인스턴스를 복사합니다.
		Cookie cookie2=new Cookie(cookie1);
		
		// 각 인스턴스의 속성값을 로그를 통해 확인합니다.
		System.out.println("cookie1의 flavor="+cookie1.flavor+", shape="+cookie1.shape+", cost="+cookie1.cost);
		System.out.println("cookie2의 flavor="+cookie2.flavor+", shape="+cookie2.shape+", cost="+cookie2.cost);
		
		// cookie1 인스턴스의 속성값을 변경시켜 봅니다.
		cookie1.cost=6000;
		System.out.println("cookie1.cost=6000; 수행 후");
		System.out.println("cookie1의 flavor="+cookie1.flavor+", shape="+cookie1.shape+", cost="+cookie1.cost);
		System.out.println("cookie2의 flavor="+cookie2.flavor+", shape="+cookie2.shape+", cost="+cookie2.cost);	
		
	}

}

```



실행 결과

```
cookie1의 flavor=초코맛, shape=곰돌이 모양, cost=4000
cookie2의 flavor=초코맛, shape=곰돌이 모양, cost=4000
cookie1.cost=6000; 수행 후
cookie1의 flavor=초코맛, shape=곰돌이 모양, cost=6000
cookie2의 flavor=초코맛, shape=곰돌이 모양, cost=4000

```

