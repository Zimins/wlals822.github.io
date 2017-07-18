---
layout: post
title: MPAndroidChart 사용법
tags: android, MPAndroidChart, 안드로이드그래프
categories: android
image_url: https://camo.githubusercontent.com/b4854180e5d01005bf22e7f97b0ca4b9d514c03a/68747470733a2f2f7261772e6769746875622e636f6d2f5068696c4a61792f4d5043686172742f6d61737465722f64657369676e2f666561747572655f677261706869632e706e67
---



# MPAndroid chart 사용하기

안드로이드에 멋들어진 그래프를 만들어보기 위해 MPAndroidChart라는 라이브러리를 사용해보았다. 

https://github.com/PhilJay/MPAndroidChart.git



데모 앱으로는 단기 프로젝트로 만들어보고 있는 앱인데, 폰에 오는 카드사, 은행 메시지를 파싱해서 돈을 얼마나 썻는지 확인하는 기능을 가지고 있다. 

예제 repo : https://github.com/wlals822/MsgGrapher.git



예제의 전체 흐름으로는 

1. BroadCastReceiver을 사용하여 메시지 도착 이벤트 확인
2. 메시지에서 사용 금액과 날짜를 파싱
3. Andriod Realm DB 에 해당 데이터를 저장 
4. DB 가 변경될 때 콜백을 날려 그래프를 새로그림 







여기까지 되어있다. 


사용 설정은 gradle 을 통하여 편하게! 

**1. Gradle dependency 추가 ** 

-  프로젝트 레벨의 `build.gradle`:

```gradle
allprojects {
	repositories {
		maven { url "https://jitpack.io" }
	}
}
```
-  앱 수준의  `build.gradle`:

```gradle
dependencies {
	compile 'com.github.PhilJay:MPAndroidChart:v3.0.2'
}
```





기본적인 사용방법은 

1. 사용을 원하는 chart를  xml에 추가 

2. 원하는 activity thread 에서 `findViewById` 를 통하여 초기화. 

3. `Entry`객체를 사용하는 List 생성

   ```java
   //사용 예제가 BarChart 라서 BarEntry 객체를 사용하였습니다. 
   List<BarEntry> entries = new ArrayList<BarEntry>();
   ```

4. 반복문 혹은 원하는 방법으로 entries 에 요소를 넣는다.

   ```java
   // 이런 식으로! 생성자의 첫째 인자는 x, 두번때 인자는 y축이 된다. 
   entries.add(new BarEntry((float) i, sumOfDay));
   ```

5. 해당 `entries`를 그래프에 설정해준다! 완성! 

   ```java
   BarDataSet barDataSet = new BarDataSet(entries, "witdraw per day");

   BarData barData = new BarData(barDataSet);

   moneyBarHorizontal.setData(barData);
   moneyBarHorizontal.setFitBars(false);
   moneyBarHorizontal.animateXY(1000,1000);
   moneyBarHorizontal.invalidate();
   ```

   ​

예제 코드의 핵심적인 부분은 아래와 같다. 

```java
//OnCreate 에서 singleton 으로 받아온 realm 객체에 listener 설정
void OnCreate() {
    .//other codes 
    .
    .
    realm.addChangeListener(new RealmChangeListener<Realm>() {
            @Override
            public void onChange(Realm realm) {
                drawChart();
                // TODO: 2017. 7. 17. refresh graph
            }
    });
}
    
    
//차트를 설정하는 헬퍼 메소드 

    private void drawChart() {

        List<BarEntry> entries = new ArrayList<BarEntry>();

        for (int i = 1;i <= 31;i++) {
          //realm 에 쿼리 (7월달로 고정되어 있다. )
            RealmResults<MsgInfo> dayResult = 
              realm.where(MsgInfo.class)
              		.equalTo("month", 7)
              		.equalTo("day", i).findAll();
            float sumOfDay = dayResult.sum("withdrawAmount").floatValue();
            if (sumOfDay != 0) {
                entries.add(new BarEntry((float) i, sumOfDay));
            }
        }

        RealmResults<MsgInfo> monthResult = 
          realm.where(MsgInfo.class).equalTo("month", 7).findAll();
        sumOfMonth = monthResult.sum("withdrawAmount").floatValue();
        sumOfMonthText.setText(String.valueOf(sumOfMonth));
        BarDataSet barDataSet = new BarDataSet(entries, "witdraw per day");


        BarData barData = new BarData(barDataSet);

        moneyBarHorizontal.setData(barData);
        moneyBarHorizontal.setFitBars(false);
        moneyBarHorizontal.animateXY(1000,1000);
        moneyBarHorizontal.invalidate();
    }
```



아래와 같은 그래프가 만들어진다. 

![MsgGrapherScreenshot]({{ sites.url }}/assets/MsgGrapher.png)