---
layout: post
title:  "[에러 해결] 안드로이드 반복문으로 FireStore 에서 데이터를 전부 받은뒤 코드 실행하기"
subtitle:   "안드로이드 반복문으로 FireStore 에서 데이터를 전부 받은뒤 코드 실행하기"
categories: unclassified
tags: r install
comments: true
---

## 개요
> 안드로이드 환경+데이터베이스로 FireStore 를 사용할 때, 반복문으로 데이터를 갖고 와야하는 상황이 있다. 이 때, 필자는 데이터를 전부 다 받아온 이후에 코드를 실행하고 싶었는데 그렇게 하지 못해서 상당히 애를 먹었다. 문제를 해결한 방법에 관해 적었다.



이해를 돕기 위해서 임의로 FireStore 안에 데이터를 넣고, 안드로이드 측에서 데이터를 갖고오는 상황을 재현해 보겠습니다. 



- 목차
  - 설명을 돕기위한 FireStore 안의 데이터 
  - 이런 에러 상황에 놓인 이유
  - 해결 방법 & 해설



## 설명을 돕기 위한 FireStore 안의 데이터

아래 사진은 이번 게시글을 설명하기 위해 임의로 설정한 FireStore 데이터 입니다.

(현재 이미지 포스팅에 문제가 있는듯 합니다. 되는대로 고치겠습니다. 죄송합니다.)

![table1](C:\test\table1.png)

이번 설명에서 문서 안에 들어있는 필드값이 중요한 것은 아니기 때문에 문서2와 문서3에 들어있는 필드 값 설명은 제외하겠습니다.

## 이런 에러 상황에 놓인 이유

아래의 코드는 제가 FireStore에서 데이터를 받아오기 위해 안드로이드 단에서 사용한 코드 입니다.

```
for(int i=1;i<4;i++){
    DocumentReference docRef=db.collection("test_collection").document("문서"+String.valueOf(i));
    int finalI = i;
    docRef.get().addOnCompleteListener(new OnCompleteListener<DocumentSnapshot>() {
        @Override
        // onComplete 메소드는 쿼리가 전부 진행하고 난 뒤에 호출되는 함수 입니다.
        public void onComplete(@NonNull Task<DocumentSnapshot> task) {
            //쿼리 성공
            if (task.isSuccessful()) {
                DocumentSnapshot document = task.getResult();

                //쿼리 결과 docment 가 존재하는 경우
                if (document.exists()) {
                    Log.v(TAG, "DocumentSnapshot data: " + document.getData());

                    if(finalI ==3){
                        Log.v(TAG, "마지막 값을 받아 왔습니다!!");
                    }

                }

                //쿼리 결과 document 가 존재하지 않는 경우
                else {
                    Log.v(TAG, "No such document");
                }
            }

            // 쿼리 실패
            else {
                Log.v(TAG, "get failed with ", task.getException());
            }
        }
    });

}
```

onComplete 메소드에서 쿼리가 끝나고 나서 하고싶은 일을 적으면 됩니다. 

for 문 안의 i 값이 3이 되었을 때가 마지막으로 DB에서 데이터를 받고난 뒤니까 i==3 일 때, onComplete 메소드에서 제가 하고 싶은 일을 하면 된다고 생각했습니다.

하지만 로그를 확인해본 결과, i의 값이 3일 때도 모든 쿼리가 응답값까지 받아온 상태는 아니였습니다.

**실행 결과**

```
 V/MainActivity: DocumentSnapshot data: {유진이=여자, 호만이=남자, 쭈뇽이=남자}
 V/MainActivity: DocumentSnapshot data: {유진이=키167, 호만이=키180, 쭈뇽이=키194}
 V/MainActivity: 마지막 값을 받아 왔습니다!!
 V/MainActivity: DocumentSnapshot data: {유진이=45살, 호만이=22살, 쭈뇽이=24살}

```



## 해결 방법 & 해설

위의 실행 결과를 보시면 알겠지만 반복문을 표현하는 i 는 단지 몇번째로 쿼리가 반복되고 있는지 표현할 뿐, 쿼리가 다 끝났을때의 기준이 되지 못합니다. 
그래서 저는 n 번째 쿼리가 실행 중인것을 표현하기 위한 변수를 하나 만들었습니다.

변수의 이름은 last_repeat 입니다. 설명해드린 예제 상황에서는 초기값이 1로 설정 되었고 여러분들의 상황에 맞춰서 그 값을 설정하시면 됩니다. 

아래에 전체 코드를 남기겠습니다.



```
public class MainActivity extends AppCompatActivity {

    public static String TAG="MainActivity";
    FirebaseFirestore db;

    // 마지막으로 데이터를 DB에서 받아왔음을 알려주는 변수다.
    int last_repeat=1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        db = FirebaseFirestore.getInstance();

        for(int i=1;i<4;i++){
            DocumentReference docRef=db.collection("test_collection").document("문서"+String.valueOf(i));
            int finalI = i;
            docRef.get().addOnCompleteListener(new OnCompleteListener<DocumentSnapshot>() {
                @Override
                // onComplete 메소드는 쿼리가 전부 진행하고 난 뒤에 호출되는 함수 입니다.
                public void onComplete(@NonNull Task<DocumentSnapshot> task) {
                    //쿼리 성공
                    if (task.isSuccessful()) {
                        DocumentSnapshot document = task.getResult();

                        //쿼리 결과 docment 가 존재하는 경우
                        if (document.exists()) {
                            Log.v(TAG, "DocumentSnapshot data: " + document.getData());

                            if(last_repeat ==3){
                                Log.v(TAG, "마지막 값을 받아 왔습니다!!");
                            }

                            last_repeat++;
                        }

                        //쿼리 결과 document 가 존재하지 않는 경우
                        else {
                            Log.v(TAG, "No such document");
                        }
                    }

                    // 쿼리 실패
                    else {
                        Log.v(TAG, "get failed with ", task.getException());
                    }
                }
            });
        }
    }
}
```



