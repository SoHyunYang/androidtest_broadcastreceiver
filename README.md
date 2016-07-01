

```
스터디 진행 : 2016년 7월 2일, 양소현
최초 작성자 : 양소현
최초 작성일 : 2016년 7월 1일
마지막 수정 : 2016년 7월 2일, 양소현
```

# androidtest_broadcastreceiver

**Service**

컴퓨터 프로그램 수행 시 프로세스 내부에 존재하는 수행 경로, 즉 일련의 실행 코드. 프로세스는 단순한 껍데기일 뿐, 실제 작업은 스레드가 담당한다. 프로세스 생성 시 하나의 주 스레드가 생성되어 대부분의 작업을 처리하고 주 스레드가 종료되면 프로세스도 종료된다. 하나의 운영 체계에서 여러 개의 프로세스가 동시에 실행되는 환경이 멀티태스킹이고, 하나의 프로세스 내에서 다수의 스레드가 동시에 수행되는 것이 멀티스레딩이다


**Broadcast receiver**



안드로이드는 UI 스레드라는 것이 존재하는데 UI 와 관련된 작업은 UI스레드만 할 수 있다. 
그래서 안드로이드는 백그라운드 스레드와 UI 스레드의 통신 방법으로 핸들러(Handler)를 제공한다.
핸들러는 메시지를 받거나 보낼수 있고, 메시지에 따라 특정 작업을 실행 할 수 있다. 






##1. broadcast intent 전송하기


[thread_1.JPG](https://github.com/SoHyunYang/androidtest_thread/blob/master/thread_1.JPG?,raw=true)
[thread_2.JPG](https://github.com/SoHyunYang/androidtest_thread/blob/master/thread_2.JPG?,raw=true)
[thread_3.JPG](https://github.com/SoHyunYang/androidtest_thread/blob/master/thread_3.JPG?,raw=true)

**Button 생성 & inflation**
```XML
  

```
```JAVA

```
**Thread 생성**

```JAVA


```



##2. 동적 broadcast receiver

**textView 생성 & inflation **

```XML


```

```JAVA

```

**handler 사용하지 않았을 시 오류**

```JAVA
 

```

```JAVA


```
**handler class정의 후 handler 객체 생성**
```JAVA
 

```
```JAVA


```




##3. Pending intent
http://micropilot.tistory.com/1994참고
새 스레드는 Thread() 생성자로 만들어서 내부적으로 run()을 구현하던지, Thread(Runnable runnable) 생성자로 만들어서 Runnable 인터페이스를 구현한 객체를 생성하여 전달하던지 둘 중 하나의 방법으로 생성하게 된다. 후자에서 사용하는 것이 Runnable로 스레드의 run() 메서드를 분리한 것이다. 따라서 Runnable 인터페이스는 run() 추상 메서드를 가지고 있으므로 상속받은 클래스는 run()코드를 반드시 구현해야 한다.
앞서 언급한대로 Message가 int나 Object같이 스레드 간 통신할 내용을 담는다면, Runnable은 실행할 run() 메서드와 그 내부에서 실행될 코드를 담는다는 차이점이 있다.



**Handler 객체 생성**
```JAVA


```

**Thread 내에서 사용될 println 함수 내에서 Runnable객체 사용**
```JAVA
 
```

##4. goAsync()
http://happyourlife.tistory.com/m/post/121 참고
**정지버튼 생성**
```XML
 
```

**flag를 만들어 정지버튼을 눌렀을 때 Thread 정지시키기**
```JAVA


```


###참고문헌###
Do it! 안드로이드 앱 프로그래밍
실무에 바로 적용하는 안드로이드 프로그래밍

http://koreaparks.tistory.com/128
http://android-er.blogspot.kr/2015/04/example-of-using-alarmmanager-to.html
http://blog.naver.com/ruly2001/70166117812




