---
title:  "[디자인 패턴]-Proxy Pattern"
search: false
categories: 
  - 디자인 패턴
last_modified_at: 2021-11-18T08:06:00-05:00
---

설계 패턴을 공부하고 정리하는 공간.



## Proxy 패턴이란? 
프록시 패턴이란 가상의 객체로 실제 기능을 수행하는 객체를 control하는 패턴이다. 무슨 말인지 간단한 상황 예시와 코드로 설명하겠다.

나는 지금 지도를 포함한 화면을 만들고 싶은데 시간이 없으므로 <br> 지도 Loading은 라이브러리를 구매할 것이다.  
실제 기능을 수행하는 Class는 내가 수정할 수 없다는 뜻.

구현체의 <b>Interface</b>는 아래와 같고
```java
public interface Map {
    void loadMap();
}
```

구매한 <b>Library</b>는 아래와 같다. 얍!
```java
//해당 클래스는 수정 불가능한 상황
public class RealMap implements Map{
    public RealMap() {
        //객체 생성과 동시에  Map Loading & draw 서비스를 호출 한다고 가정
        this.loadMap();
    }

    @Override
    public void loadMap() {
        //Map Loading 서비스를 호출하고 많은 시간을 소요한다고 가정
        System.out.println("Request Service.., Map Loading...........Loding....Loding..");
        System.out.println("Map loaded!!!!!!!!!!!!!");
    }
}
 ```


## Proxy 패턴 적용 전 
```java
package com.ejcompany.pattern.proxy;

public class MapLoadTestDrive {
    public static void main(String[] args) {
        System.out.println("==============================================Proxy Pattern 적용전==============================================");
        Map realMap = new RealMap();
    }
}
```

<b>결과</b>
![icon](/assets/images/Proxy_output1.png)

위와 같은 코든느 객체 생성과 동시에 Map을 로딩하므로 초기 화면을 띄우는 데 아주 많은 시간이 소요되고,<br>
바쁘다 바빠 현대사회에선 n초의 빈 화면을 수용할 수 없다. <br>다수의 사람들은 자신의 노트북의 성능을 의심하거나 화를 내며 냅다 화면을 꺼버리겠지(So do I)

## Proxy 패턴 적용 후
서비스를 이용하는 고객들의 화를 어서 빨리 잠재울 필요가 있으므로 Map 로딩이 되기 전에 초기 화면을 먼저 보여줘버릴 의무가 있겠다. <br>당신의 노트북은 고장나지 않았어요.

<b>Proxy Class</b>

```java
package com.ejcompany.pattern.proxy;

public class ProxyMap implements Map{
    Map realMap;
    public ProxyMap() {
        System.out.println("Initial page loaded!!!!!!!!!!!!!");
    }

    @Override
    public void loadMap() {
        realMap = new RealMap();
    }
}

```

```java
public class MapLoadTestDrive {
    public static void main(String[] args) {
        //System.out.println("==============================================Proxy Pattern 적용전==============================================");
        //Map realMap = new RealMap();
        //System.out.println("==============================================Proxy Pattern 적용전==============================================");


        System.out.println("==============================================Proxy Pattern 적용 후==============================================");
        Map proxyMap = new ProxyMap();
        //다른 동작들을 하다가 실제 Loading이 필요한 시점에 loadMap함수 호출 (스크롤 내리면 호출 등)
        proxyMap.loadMap();
        System.out.println("==============================================Proxy Pattern 적용 후==============================================");
    }
}
```

<b>결과</b>
![icon](/assets/images/Proxy_output2.png)
<br>

Proxy 패턴을 적용하여 실객체(RealMap)를 직접 제어하였을 때의 단점을 보완하였다. <br>
ProxyMap 객체생성 시 초기화면을 먼저 띄우고, Map을 Load해야 할 시점에 loadMap()함수를 호출하여 실제 객체(RealMap)을 생성한다. <br>
지도는 이 시점에 로딩되므로 내 컴퓨터 고장났나? 너무 화나! 하는 고객은 많이 줄 것이다. 



## 장단점
Sample을 작성하면서 느낀 프록시 패턴의 장점은 2가지다.
1. 시간이 오래 걸리는 객체 로딩 전 사전처리 로직을 삽입할 수 있다. (다만 실제 기능에 영향을 주면 안됨!)
2. 실제 인스턴스의 resource에 직접 접근하지 않는다. (비단 프록시만의 장점은 아닐 것이라는 책임님의 의견과 내 동의가 반영됨.)

Sample을 작성하면서 느낀 프록시 패턴의 단점도 2가지다.
1. 복잡한 애플리케이션이라면, 구현체가 어디에 있는 지 찾아야 할 때 시간이 많이 소요될 것이다.
2. 실제 객체를 생성하기 위해서 Proxy객체'도' 생성해야 한다.

## TODO
위에 작성한 Sample은 여러 Proxy 패턴 중 가상 프록시(Virtual Proxy)이다. 이 외에도 원격 프록시(Remote Proxy), 보호 프록시(Protection Proxy)등이 있고, 시간을 내서 나머지도 정리할 예정이다. <br>
Spring Frame Work는 어디에 어떻게 프록시 패턴을 적용시켰는가?에 대해서도 정리할 예정.

<br>그리고 블로그 너비 늘리기..사이드바 메뉴 추가...등...
