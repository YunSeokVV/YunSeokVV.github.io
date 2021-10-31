---
layout: post  
title: "안드로이드 커스텀 토글 버튼"  
subtitle: "안드로이드 커스텀 토글 버튼"  
categories: examples 
tags: review book 스타트업 창업 사업 실전 법무 가이드 계약 투자 동업 회사 연구 외주 IPO        
comments: true  
---  

> ## 개요
>
> > 자바에서 사용하는 인터페이스가 무엇인지 설명해줍니다. 
>
> - 목차
>   - 예제 프로젝트 깃허브 주소
>   - 구현절차 설명
>   - 결과화면
>
> 글을 쓰기 앞전에 말씀 드리고 싶은게 있습니다. 이 예제는 엄연히 얘기하면 토글 버튼은 아닙니다. 그냥 TextView 두개를 이어 붙여서 색을 변환 시켜가며 토글버튼처럼 보이게끔 구현한 뷰 입니다. 
>
> 아마 필요하신 분이 계실거라고 생각하고 글을 써봅니다. 
>
> ## 예제 프로젝트 깃허브 주소
>
> https://github.com/YunSeokVV/AndroidCustomToggle
>
> 예제를 보시면 내용이 그렇게 어려운게 없기 때문에, 그냥 프로젝트를 다운받으셔도 충분히 공부하실 수 있습니다. 
>
> ## 구현 절차 설명
>
> 우선 메인 액티비티의 xml 파일입니다.
>
> activity_main.xml
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
>     xmlns:app="http://schemas.android.com/apk/res-auto"
>     xmlns:tools="http://schemas.android.com/tools"
>     android:layout_width="match_parent"
>     android:layout_height="match_parent"
>     android:background="@color/white"
>     tools:context=".MainActivity">
>
>     <androidx.appcompat.widget.AppCompatButton
>         android:id="@+id/hot_btn"
>         android:layout_width="200dp"
>         android:layout_height="100dp"
>         android:layout_marginStart="16dp"
>         android:background="@drawable/hot_button_white"
>         android:text="HOT"
>         android:textColor="@color/black"
>         android:textStyle="bold"
>         app:layout_constraintBottom_toBottomOf="parent"
>
>         app:layout_constraintEnd_toStartOf="@+id/ice_btn"
>         app:layout_constraintHorizontal_bias="0.5"
>         app:layout_constraintStart_toStartOf="parent"
>         app:layout_constraintTop_toTopOf="parent" />
>
>     <androidx.appcompat.widget.AppCompatButton
>         android:id="@+id/ice_btn"
>         android:layout_width="200dp"
>         android:layout_height="100dp"
>         android:background="@drawable/iced_button_radius"
>         android:text="ICED"
>
>         android:textColor="@color/white"
>         android:textStyle="bold"
>         app:layout_constraintBottom_toBottomOf="parent"
>
>         app:layout_constraintEnd_toEndOf="parent"
>         app:layout_constraintHorizontal_bias="0.5"
>         app:layout_constraintStart_toEndOf="@+id/hot_btn"
>         app:layout_constraintTop_toTopOf="parent" />
>
> </androidx.constraintlayout.widget.ConstraintLayout>
> ```
>
> 다음은 메인 엑티비티 입니다.
>
> MainActivity.java
>
> ```java
> package com.example.customtoggle;
>
> import androidx.appcompat.app.AppCompatActivity;
> import androidx.appcompat.widget.AppCompatButton;
> import androidx.core.content.ContextCompat;
>
> import android.os.Bundle;
> import android.view.View;
>
> public class MainActivity extends AppCompatActivity {
>
>     AppCompatButton hot_btn;
>     AppCompatButton ice_btn;
>
>     @Override
>     protected void onCreate(Bundle savedInstanceState) {
>         super.onCreate(savedInstanceState);
>         setContentView(R.layout.activity_main);
>
>         hot_btn=(AppCompatButton)findViewById(R.id.hot_btn);
>         ice_btn=(AppCompatButton)findViewById(R.id.ice_btn);
>
>         hot_btn.setOnClickListener(new View.OnClickListener(){
>             @Override
>             public void onClick(View view)
>             {
>                 ChooseHot();
>             }
>         });
>
>         ice_btn.setOnClickListener(new View.OnClickListener(){
>             @Override
>             public void onClick(View view)
>             {
>                 ChooseIced();
>             }
>         });
>
>     }
>
>     // 뜨거운 음료를 선택 했을 때 버튼을 토글 버튼을 설정하는 메소드 입니다.
>     private void ChooseHot(){
>         hot_btn.setBackground(ContextCompat.getDrawable(this, R.drawable.hot_button_radius));
>         hot_btn.setTextColor(getResources().getColor(R.color.white));
>
>         ice_btn.setBackground(ContextCompat.getDrawable(this, R.drawable.iced_button_white));
>         ice_btn.setTextColor(getResources().getColor(R.color.black));
>
>     }
>
>     // 차가운 음료를 선택 했을 때 버튼을 토글 버튼을 설정하는 메소드 입니다.
>     private void ChooseIced(){
>         hot_btn.setBackground(ContextCompat.getDrawable(this, R.drawable.hot_button_white));
>         hot_btn.setTextColor(getResources().getColor(R.color.black));
>
>         ice_btn.setBackground(ContextCompat.getDrawable(this, R.drawable.iced_button_radius));
>         ice_btn.setTextColor(getResources().getColor(R.color.white));
>
>     }
> }
> ```
>
> 여기서 부터 설명을 잘 들으셔야 합니다. TextView를 두개 이어 붙인 것이기 때문에, 리소스 파일을 따로 만들 겁니다.
>
> res-drawable-values-colors.xml 파일을 열고 아래 두줄의 내용을 추가해주세요
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <resources>
>     <color name="purple_200">#FFBB86FC</color>
>     <color name="purple_500">#FF6200EE</color>
>     <color name="purple_700">#FF3700B3</color>
>     <color name="teal_200">#FF03DAC5</color>
>     <color name="teal_700">#FF018786</color>
>     <color name="black">#FF000000</color>
>     <color name="white">#FFFFFFFF</color>
>     <!--    추가해주세요-->
>     <color name="red">#F63636</color>
>     <color name="blue">#1C6BE2</color>
> </resources>
> ```
>
> res-drawable-hot_button_white.xml 파일을 추가해 줍니다.
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <shape xmlns:android="http://schemas.android.com/apk/res/android"
>     android:padding="10dp"
>     android:shape="rectangle" >
>     <solid android:color="@color/white" />
>     <stroke
>
>         android:width="1dp"
>         android:color="#C4C4C4" /> //선색
>     <corners
>         android:bottomLeftRadius="1000dp"
>         android:topLeftRadius="1000dp"
>
>         />
> </shape>
> ```
>
> 
>
> res-drawable-hot_button_radius.xml 파일을 추가해 줍니다.
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <shape xmlns:android="http://schemas.android.com/apk/res/android"
>     android:padding="10dp"
>     android:shape="rectangle" >
>     <solid android:color="@color/red" />
>     <stroke
>
>         android:width="1dp"
>         android:color="#C4C4C4" /> //선색
>     <corners
>         android:bottomLeftRadius="1000dp"
>         android:topLeftRadius="1000dp"
>
>         />
> </shape>
> ```
>
> res-drawable-iced_button_radius.xml 파일을 추가해 줍니다.
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <shape xmlns:android="http://schemas.android.com/apk/res/android"
>     android:padding="10dp"
>     android:shape="rectangle" >
>     <solid android:color="@color/blue" />
>     <stroke
>         android:width="1dp"
>         android:color="#C4C4C4" /> //선색
>     <corners
>         android:bottomRightRadius="1000dp"
>         android:topRightRadius="1000dp"
>         />
> </shape>
> ```
>
> res-drawable-iced_button_white.xml 파일을 추가해 줍니다.
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <shape xmlns:android="http://schemas.android.com/apk/res/android"
>     android:padding="10dp"
>     android:shape="rectangle" >
>     <solid android:color="@color/white" />
>     <stroke
>         android:width="1dp"
>         android:color="#C4C4C4" /> //선색
>     <corners
>         android:bottomRightRadius="1000dp"
>         android:topRightRadius="1000dp"
>         />
> </shape>
> ```
>
> 파일을 빠트린 것 없이 추가하셨다면 커스텀 토글 예제를 보실 수 있으실겁니다. 감사합니다.
>
> ## 결과 화면
>
> <iframe width="956" height="538" src="https://user-images.githubusercontent.com/43668299/139562339-bd5a6c97-847c-4ed9-90f9-a015b14ca4f7.mp4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
