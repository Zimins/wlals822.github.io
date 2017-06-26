---
layout: post
title: socket.io , android 
tags: android, socket.io
categories: android
image_url: https://11458-presscdn-0-62-pagely.netdna-ssl.com/wp-content/uploads/2015/07/socket-logo.png
---



# 소켓 io 와 안드로이드 클라이언트  



> 개발환경
>
> node + socket io
>
> android client 



소켓을 사용하여 다른 앱에게 메시지를 보내기 위해 만들어보는 프로젝트 .



node 로 간단한 소켓 서버를 만들기 위해 socket.io 모듈 사용!

https://socket.io/

```shell
npm install socket.io --save
```



서버측 index.js

```javascript

var http = require('http').createServer(app);
var io = require('socket.io')(http);
// socket io 가져오기 

//연결 이벤트 시에 콜백으로 socket이 매개변수로 들어가며, 안쪽에서 이벤트에 따른 분기처리를 한다. 
io.on('connection', function(socket) {
  console.log("user connect");

  socket.on('lightOn', function(){
    console.log("light on");
    io.emit('lightOn', "light");
  });

  socket.on('lightOff', function(){
    console.log("light off");
    io.emit('lightOff', "light");
  });

});

//서버를 시작한다. 
http.listen(3000, function(){
  console.log("server on 3000");
});
```



안드로이드 클라이언트의 경우 직접다룰 수도 있지만 역시 socket io 에서 제공하는 라이브러리를 사용했다. 

```a
dependencies {
  	...생략 
    compile ('com.github.nkzawa:socket.io-client:0.6.0'){
      //경고 메시지가 있어 추가했지만 없어도 되는듯.
        exclude group: 'org.json', module: 'json'
    }
}
```



클래스에 socket 을 선언했으며 초기화 블럭 사용

```java
//MainActivity 
private Socket socket;
    {
        try{
            socket = IO.socket("http://***.***.***.***:***");
          //https로 사용했다가 삽질..
        } catch (URISyntaxException ue) {
            ue.printStackTrace();
        }
    }
```



이후 socket.connect() 

클라에서 실행한 코드이며 연결만 되어있다. 그리고 토글버튼을 통한 메시지 emit

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
      
        socket.connect();
        
        bulbButton = (ToggleButton) findViewById(R.id.bulbButton);
        audioButton = (ToggleButton) findViewById(R.id.audioButton);

        bulbButton.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
                if(b) {
                    socket.emit("lightOn");
                } else {
                    socket.emit("lightOff");
                }
            }
        });
    }
```



이벤트 리스닝은 아래와 같다. 다른 앱의 MainActivity 코드 .



```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

	//메시지와 콜백을 지정. 이후 멤버변수로 리스너를 선언해준다. 
        socket.on("lightOn", onLightOn);
        socket.on("lightOff", onLightOff);
        socket.connect();
    }


// ex) onLightOff Listener ! call() 을 오버라이딩 한다. 
    private Emitter.Listener onLightOff = new Emitter.Listener() {
        @Override
        public void call(Object... args) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(context, "trun off light", Toast.LENGTH_SHORT).show();
                    bulbImage.setImageResource(R.drawable.bulboff);
                }
            });
        }
    };
```



사용이 끝나면 연결해제 해준다. 

```java
  @Override
    protected void onDestroy() {
        super.onDestroy();
        socket.disconnect();
        socket.off("lightOn", onLightOn);
        socket.off("lightOff", onLightOff);
    }
```



어려운 난이도는 아니었으며, socket io 라이브러리 덕분에 socket 에 대한 기본적인 이해만 있다면 쉽게 소켓 사용이 가능했다. 





참고글 : socket io 튜토리얼, socket io android 문서 



