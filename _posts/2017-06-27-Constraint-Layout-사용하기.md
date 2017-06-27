---
layout: post
title: Constraint Layout 사용하기 (Relative positioning)
tags: android, ConstraintLayout
categories: android
image_url: https://wlals822.github.io/assets/constraintLayout/top.png
---

# ConstraintLayout: Relative positioning





안드로이드 레이아웃을 만들게 되면 조금만 레이아웃이 복잡해져도 깊은 계층구조로 만들어지게 되며 성능에 영향을 미치게 될 수도 있다.

이를 google은 ConstraintLayout 을 통해 해결해보고자 했다.

구글 개발자 가이드를 참조하여 학습.

https://developer.android.com/reference/android/support/constraint/ConstraintLayout.html





># ConstraintLayout
>
>`public class ConstraintLayout `
>```extends ViewGroup ```
>
>A `ConstraintLayout` is a `ViewGroup` which allows you to position and size widgets in a flexible way.
>
>위치와 사이즈를 유연하게 조정할 수 있는 뷰그룹 이라고 한다. API 9 (진저브레드) 까지 지원하고 있다.





LinearLayout 에서는 orientation을 지정하고, RelativeLayout에서는 위치관계를 명시하듯이 ConstraintLayout에서는 아래와 같은 속성들을 사용한다.

* Relative Positioning
* Margins
* Centering Positioning
* Visibility behavior
* Dimension constraints
* Chains
* Virtual Helpers objects

*기존의 뷰그룹과 다른점은 , 거의 xml 코드를 건드리지 않는다는 것. 마우스로 관계를 붙이고 코드는 자동 생성된다. *





###  Relative positioning

ConstraintLayout의 기초! 다른 레이아웃과 상대적으로 어떻게 되는지 설정해준다.

ConstraintLayout을 사용하기로 하고 xml파일의 디자인 메뉴를 보자. 왼쪽 팔레트의 뷰를 끌어다가 화면에 배치가 가능하고, 각 뷰를 클릭하면 Relative postioning을 가능하게 해주는 점이 있다. 이를 드래그 하면 화살표가 나오면서 다른 뷰에 붙일 수 있다.

아래와 같이 만든다면,

![스크린샷 2017-06-27 오후 8.19.05]({{ sites.url }}/assets/constraintLayout/스크린샷 2017-06-27 오후 8.19.05.png)

실제 폰에서는 아래와 같다. 좌상단 버튼을 다른곳으로 옮겨도 그 버튼에 설정된 의존관계가 없어 좌상단에 배치된다. (RelativeLayout 처럼)

![스크린샷 2017-06-27 오후 8.20.33]({{ sites.url }}/assets/constraintLayout/스크린샷 2017-06-27 오후 8.20.33.png)

옮겨도 마찬가지!

![constRelative]({{ sites.url }}/assets/constraintLayout/constRelative.gif)



주의해야 할 점은 화살표를 시작하는 점이 위아래 이면 다른 뷰에 위, 아래에만 붙일 수 있고, 왼쪽 오른쪽이면 다른 뷰의 왼쪽 오른쪽에만 붙일 수 있다.

![스크린샷 2017-06-27 오후 8.59.54]({{ sites.url }}/assets/constraintLayout/top.png)

![스크린샷 2017-06-27 오후 8.59.36]({{ sites.url }}/assets/constraintLayout/side.png)

> 애초에 붙일수 있는 점이 그렇게 나온다.

화살표를 시작한 점을 다시 누르면 의존 제거가 가능하다. (빨간색으로 점등)

![스크린샷 2017-06-27 오후 9.00.04]({{ sites.url }}/assets/constraintLayout/delete.png)





각 뷰를 클릭하면 아래에 두개의 버튼이 나오는데, 이는 아래와 같다.

![스크린샷 2017-06-27 오후 9.05.54]({{ sites.url }}/assets/constraintLayout/twobutton.png)

왼쪽버튼은 그 뷰에 할당된 모든 화살표를 제거해준다. 즉 아무런 의존이 없어진다.

오른쪽 버튼은 텍스트 베이스라인의 Relative postioning 을 설정해준다. ![스크린샷 2017-06-27 오후 9.06.26]({{ sites.url }}/assets/baseline.png)



여기까지, Relative Positioning 의 기초였다!
