---
layout: post
title: ButterKnife 라이브러리 설정
tag: ButerKnife, Android
---

# Butterknife + 코드포맷 설정



귀찮은 findViewById 를 생략하게 해주는 Butterknife 라는 아주 좋은 라이브러리.

하지만 이걸 그냥 사용한다면 코드 자동 정렬을 할때 아래와 같이 만들어진다. 



```java
//소개 사이트를 보고 이걸 원했으면 ,
@BindView TextView name;
@BindView TextView age;
@BindView Button btn_save;//간결!

//git 등을 위하여 reformat을 할때 이렇게 스튜디오가 만들어준다.
@BindView 
TextView name;
@BindView 
TextView age;
@BindView 
Button btn_save; //BindView 어노테이션 때문에 계속 줄바꿈을 한다.
```



화면에 들어가는 뷰 요소가 많다면 쓸데없이 개행을 많이 하는것 같은 느낌이 든다. 매번 reformat 후 바꾸어 줄수도 없다. 

그렇다면 reformat 설정을 바꾸어보자. 



![annotationsetting]({{ sites.url }}/assets/annotationsetting.png)



설정에 Editor/Code Style/java 로 이동후 wrapping and braces 로 들어간후 , field annotations 의 설정을 `do not wrap` 으로 바꾸어 주면 된다 .