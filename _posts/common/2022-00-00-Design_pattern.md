---
title: "디자인 패턴 Design Pattern?"
description: "디자인 패턴 Design Pattern?, 디자인 패턴 종류, 디자인 패턴 예시, 디자인 패턴 예상 질문"
date: 2022-1-12 10:00:00 +0900
categories: [CS, Common]
tags: [Computer Science, Design Pattern]
---

# 디자인 패턴 Design Pattern?
디자인 패턴은 소프트웨어 공학에서 자주 발생하는 문제를 해결하기 위해 반복적으로 사용되는 설계 방법을 의미한다. 디자인 패턴의 개념은 1970년대 건축가인 크리스토퍼 알렉산더(Christopher Alexander)가 건축 설계에서 반복적으로 나타나는 문제를 해결하기 위해 제안한 패턴 언어에서 유래되었다. 이후 1994년, 에리히 감마(Erich Gamma), 리처드 헬름(Richard Helm), 랄프 존슨(Ralph Johnson), 존 블리시디스(John Vlissides)로 구성된 'Gang of Four(GoF)'가 소프트웨어 설계에 디자인 패턴을 적용한 책 "Design Patterns: Elements of Reusable Object-Oriented Software"를 출간하면서 소프트웨어 공학 분야에서 널리 알려지게 되었다.

디자인 패턴은 소프트웨어 개발 과정에서 발생할 수 있는 다양한 문제를 해결하기 위해 만들어졌다. 이를 통해 개발자는 코드의 재사용성을 높이고, 유지보수성을 향상시키며, 코드의 가독성을 높일 수 있다. 또한, 디자인 패턴을 사용하면 개발자 간의 의사소통이 원활해지고, 코드의 일관성을 유지할 수 있다. 디자인 패턴은 크게 생성(Creational), 구조(Structural), 행위(Behavioral) 패턴으로 분류되며, 각각의 패턴은 특정한 문제를 해결하기 위한 고유한 방법을 제공한다.



## GoF 디자인 패턴의 종류 
[디자인패턴 참조 링크](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-designpattern.html)

1. 생성(Creational) 패턴
    - 추상팩토리(Abstract Factory)
        - 구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴
        - 동일한 주제의 다른 팩토리를 묶어줍니다. </br>
       [추가 참조 링크](https://robin00q.tistory.com/84?category=984980)
    - 팩토리 메서드(Factory Method)
        - 객체 생성 처리를 서브 클래스로 분리해 처리하도록 캡슐화하는 패턴
        - 생성할 객체의 클래스를 국한하지 않고 객체 생성합니다.</br>
        [추가 참조 링크](https://robin00q.tistory.com/85?category=984980)
    - 싱글턴(Singleton)
        - 전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴
        - 한 클래스에 한 객체만 존재하도록 제한합니다.</br>
        [추가 참조 링크](https://gdtbgl93.tistory.com/21?category=755764)

2. 구조(Structural) 패턴
    - 컴포지트(Composite)
        
        여러 개의 객체들은 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 패턴</br>
        [추가 참조 링크](https://gdtbgl93.tistory.com/146?category=755764)
        
    - 데코레이터(Decorator)
        
        객체의 결합을 통해 기능을 동적으로 유연하게 확장할 수 있게 해주는 패턴</br>
        [추가 참조 링크](https://gdtbgl93.tistory.com/9)

    - 퍼사드(Facade)

        어떤 서브시스템에서 일련의 인터페이스에 대한 통합된 인터페이스를 제공합니다. 퍼사드에서 고수준 인터페이스를 정의하기 때문에 서브시스템을 더 쉽게 사용할 수 있습니다.

        [추가 참조 링크](https://gdtbgl93.tistory.com/21?category=755764)
        
3. 행위(Behavioral) 패턴
    - 옵서버(Observer)
        
        한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되도록 일대다 객체 의존 관계를 구성하는 패턴
        
    - 스테이트(State)
        
        행위를 클래스로 캡슐화해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴
        
    - 스트래티지(Strategy)
        
        행위를 클래스로 캡슐화해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴</br>
        [참조 링크](https://gdtbgl93.tistory.com/6?category=755764)
        
    - 템플릿 메서드(Template Method)
        
        어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴
        
    - 커맨드(Command)
        
        실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴</br>
        [참조 링크](https://gdtbgl93.tistory.com/23?category=755764)
        
***
# 면접질문
## DesignPattern 질문 리스트
<br>

#### 1. 디자인 패턴이 뭔가요?
- 개발을 할 때 비슷한 유형의 문제에 대한 재사용가능한 해결방안입니다.
- 특정 구현방식이 아니며 문제를 해결하는데에 쓰이는 템플릿 혹은 아이디어입니다.
- 패턴은 문제 혹은 해결책에서의 유사점을 말합니다.
- 디자인 패턴은 context, problem, solution의 구조로 되어있습니다.
   > context : 문제가 발생하는 상황 기술<br>
   > problem : 패턴이 적용되어 해결해야 할 여러 디자인 이슈 기술, 여러 제약사항과 영향력도 고려해야한다.<br>
   > solution : 문제를 해결하도록 설계를 구성하는 요소, 그 요소들 사이의 관계, 책임, 협력관계를 기술

<br>

#### 2. 싱글톤(Singleton) 패턴이 무엇인가요?, 종류는 어떤것이 있나요?
- 싱글톤 패턴은 클래스의 객체는 단 하나만 존재하도록 클래스를 구현하는 패턴을 말합니다.
- 객체를 생성할 때마다 메모리 영역을 할당받지만 하나의 객체만 생성가능하므로 메모리낭비를 방지할 수 있습니다.
- 싱글톤으로 구현한 인스턴스는 전역이므로 다른 클래스의 인스턴스들이 데이터를 공유하는 것이 가능합니다.
- 주로 공통된 객체를 여러개 생성해야 할 경우에 사용되며 또한 인스턴스가 하나만 존재하는 것을 보증하고 싶을 때 사용합니다. ex) DB 커넥션풀, 스레드풀, 캐시 등
- 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유하면 다른 클래스간의 결합도가 높아지므로 OCP가 위배될 수 있습니다. 따라서 반드시 싱글톤이 필요한 상황이 아니라면 지양하는 것이 좋다고 합니다.

***
### !! 디자인 패턴이란?
> 자주 사용하는 설계 패턴을 정형화 해서 이를 유형별로 가장 최적의 방법으로 개발을 할 수 있도록 정해둔 설계
알고리즘과 유사하지만, 명확하게 정답이 있는 형태는 아니며, 프로젝트의 상황에 맞추어 적용 가능

1. 팩토리 메서드에 대해서 설명 해보라
    - 객체 생성 처리를 서브 클래스로 분리 해 처리하도록 캡슐화하는 패턴
    - 즉, 객체의 생성 코드를 별도의 클래스/메서드로 분리함으로써 객체 생성의 변화에 대비하는 데 유용하다.
    - 특정 기능의 구현은 개별 클래스를 통해 제공되는 것이 바람직한 설계다.
        - 기능의 변경이나 상황에 따른 기능의 선택은 해당 객체를 생성하는 코드의 변경을 초래한다.
        - 상황에 따라 적절한 객체를 생성하는 코드는 자주 중복될 수 있다.
        - 객체 생성 방식의 변화는 해당되는 모든 코드 부분을 변경해야 하는 문제가 발생한다.

2. 데코레이터 패턴은 뭔가?
    - 구조(Structural)패턴의 하나
    - 기본 기능에 추가할 수 있는 많은 종류의 부가 기능에서 파생되는 다양한 조합을 동적으로 구현할 수 있는 패턴
    - 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 각 추가 기능을 Decorator 클래스로 정의 한 후 필요한 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계 하는 방식이다.

3. 데코레이터 패턴을 사용해본적이 있는가? 혹은 언제 사용하는가? 
    - 프로젝트의 인증인가 부분 설명 login decorator

4. 커맨드 패턴은 뭔가?
    - 행위(behavior)패턴 중 하나이다
    - 실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴

5. 커맨트 패턴을 사용하는 이유는?
    - 이벤트가 발생했을 때 실행될 기능이 다양하면서도 변경이 필요한 경우에 이벤트를 발생시키는 클래스를 변경하지 않고 재사용하고자 할 때 유용하다.

6. 옵저버 패턴이란?
    - 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴입니다. 어떤 객체의 변경 사항이 발생하였을때 이와 연관된 객체들에게 알려주는 디자인 패턴

        > 장점
        > 1. 실시간으로 한 객체의 변경사항을 다른 객체에 전파할 수 있습니다.
        > 2. 느슨한 결합으로 시스템이 유연하고 객체간의 의존성을 제거할 수 있다.
        >
        > 단점
        > 1. 너무 많이 사용하게 되면, 상태 관리가 힘들 수 있습니다
        > 2. 데이터 배분에 문제가 생기면 자칫 큰 문제로 이어질 수 있습니다.
        > [참조](https://coding-factory.tistory.com/710)

7. 퍼사드 패턴이란?
    - 여러 개의 객체와 실제 사용하는 서브 객체의 사이에 복잡한 의존 관계가 있을 때, 중간에 facade라는 객체를 두고 Interface만을 활용하여 기능을 사용하는 방식

8. 스트래지 패턴이란?
    - 객체 지향의 핵심
    - 유사한 행위들을 캡슐화하여, 객체의 행위를 바꾸고 싶은 경우 직접 변경하는 것이 아닌 전략만 변경하여 유연하게 확장하는 패턴

> <b>사용 예시</b>
> - DatabaseController=> SingletonPattern을 사용하여 데이터베이스를 제어하는 하나의 인스턴스만을 생성
> - DatabasePool => ObjectPool Pattern을 사용하여 데이터베이스 객체를 미리 생성하여 Performance 향상
> - UnitFactory => FactoryPattern을 사용하여 객체 생성을 최적화 + Singleton Pattern을 사용하여 하나의 공장을 사용
> - BaseFrame => ObserverPattern을 사용하여 사용자의 정보가 생신되면 View의 값들도 갱신되게 함
> - PlayerInfo => StrategyPattern을 사용하여 상황에 따라 다른 스킬을 사용</br>
>
> 출처: https://mangkyu.tistory.com/95 [MangKyu's Diary]

## 디자인 패턴은 항상 유용한가?
>디자인 패턴은 여러 문제를 해결해준다. 특히 언어차원에서 힘든 문제를 해결해주는 매우 고마운 패턴들이 많다.
>
>그러나 과하게 쓰는건 절대 좋지 않다.
>
>디자인 패턴을 쓰면 쓸수록 추상화가 계속해서 진행되므로 코드 깊이가 깊어져서 읽기 매우 힘들어진다.
>
>또한 진입장벽이 높아지는 결과를 낳기도 한다.

## 혹시 디자인 패턴에 대해 아는 것이 있으신가요??
[참조 링크](https://jeong-pro.tistory.com/98)
1. 템플릿 메소드 패턴(Template method pattern)

- 템플릿, 말 그대로 템플릿을 만들어주고 특정 메서드 안을 채워넣기만 하면 되는 디자인 패턴이다. (PPT 템플릿처럼 제목, 목차, 내용, 질의응답 칸이 있고 거기에 맞게 글을 채워넣듯)

    예를 들어 하나의 기능(게임 접속 과정(GameConnectMgr)에 4가지의 절차(보안->인증->권한->접속)를 개발해야 할 때, 필요에 따라 변경되는 부분을 하위클래스에 위임하도록 하고 템플릿을 보내주는 방법이다.

    A->B->C->D, A->B->C'->D, A->B->C''->D 처럼 다양하게 존재할 때 A->B->□->D 라는 템플릿을 만들어주고 하위클래스에서 구현하며, 기능을 작동시키는 방법이다.

    * 특히 C메서드는 절차이므로 외부에 공개되면 안되지만 상속받은 하위클래스에서는 접근가능해야하므로 protected 접근지정자를 사용함을 유의해야한다. + 인터페이스는 public 메서드를 사용해야하므로 추상클래스로 만들어 사용해야 한다

2. 스트래티지 패턴(Strategy pattern)

- 전략패턴! 은 쉽게 말해서 상속받은 객체마다 다를 수 있는 행위부분(메서드)을 캡슐화해 교환하여 사용하는 패턴이다.

    예를 들어 'LOL(게임)'에 챔피언이라는 추상클래스가 있고 그것을 상속받는 자식클래스를 만들 때, '기본공격' 메서드를 상속받아 재정의한다고 가정한다면(누군가는 원거리공격을 하고 누군가는 근접공격, 망치, 단검등...), 잘 사용할 수 있을 것 같다.

    하지만 기본공격이 없는 챔피언이 추가되거나 새로운 기능을 가진 챔피언이 추가된다면 해당되지 않는 메서드를 가지고 있어야 하거나 다시 제거시 번거로움이 있다.

    인터페이스로 기능마다 만들려고 한다면 많은 기능이 있을 때 객체마다 다르게 implements해야하는 단점이 있다.

    그래서 이것을 해결하기 위해서 <b>변경이 많은 부분은 인터페이스로 정의하고 인터페이스 변수를 자식클래스가 가지고 있는 방법으로 하면 자식클래스에서 인터페이스의 메서드를 부르게만 해놓아서 기능을 위임하는 방법을 사용하는 것</b>이다.

3. 팩토리 메소드 패턴(Factory Method Pattern)

- 팩토리 메소드 패턴은 객체 생성을 직접하지 않고 하위 클래스가 어떤 객체 생성을 할지 결정하도록 위임하는 디자인 패턴이다.

    Item 인터페이스를 만들고 안에 use() 라는 메서드를 만든다.

    Item을 implements하는 아이템, 예를 들면 Hp포션, Mp포션이 있다고 가정하면 Hp포션과 Mp포션을 직접 생성하는 것이 아니라 팩토리 메소드에서 생성하게 하는 것이다.

    팩토리 메소드(Creator)라는 추상클래스를 만들고 추상클래스안에 create()라는 추상메서드를 만든다.

    이 추상메서드 create()는 일련의 프로세스가 존재할 것이다.(ex. DB에서 만들 Hp포션정보 획득->객체 생성->객체 생성 로그 저장)

    그러면 이 것을 받는 extends 하는 하위클래스(PotionCreator)를 만들고 여기서 들어오는 인자(String)로 알맞은 객체를 생성한다. 