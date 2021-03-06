---
layout: post
title: 경계 해부학
date: 2021-08-31 00:00:00
tags: [architecture]
author: mason
categories: [books,clean_architecture,part5,ch18]

---
시스템의 컴포넌트 사이 경계는 어떤 형태가 있으며 어떤 특징이 있는가?

## 경계 횡단하기
경계를 횡단하는 방식은 복잡하지 않다. 어디서 경계를 횡단하게 하는지가 중요하다.
> ‘런타임에 경계를 횡단한다’ 함은 그저 경계 한쪽에 있는 기능에서 반대편 기능을 호출하여 데이터를 전달하는 일에 불과하다. 적절한 위치에서 경계를 횡단하게 하는 비결을 소스코드 의존성 관리에 있다.

경계는 변경이 전파되는 것을 1차적으로 막아주는 방화벽 역할을 한다.
> 소스코드 모듈 하나가 변경되면, 이에 의존하는 다른 소스 코드 모듈도 변경하거나, 다시 컴파일 해서 새로 배포해야할 지도 모르기 때문이다. 경계는 이러한 변경이 전파되는 것을 막는 방화벽을 구축하고 관리하는 수단으로 존재한다.

## 두려운 단일체monolith
경계는 단일체 방식에서는 의미 없어보이지만, 설정할 수 있으며 내부 의존성 관리를 통해 경계를 설정할 수 있다.

단일체monolith 방식은 단일 파일을 배포하기 때문에 경계가 무의미할것 같지만, 그렇지 않다. 경계를 만들면 컴포넌트를 독립적으로 수행할 수 있다
> 이처럼 배포 관점에서 볼때 단일체는 경계가 드러나지 않는다. 그렇다고 해서 단일체에는 경계가 실제로 존재하지 않거나, 경계 자체가 무의미하다는 뜻은 아니다. 최종적으로는 정적으로 링크된 단일 실행파일을 만들더라도, 그 안에 포함된 다양한 컴포넌트를 개발하고 바이너리로 만드는 과정을 독립적으로 수행할 수 있게 하는 일은 대단히 가치있는 일이다.

객체지향은 다형성으로 내부의존성을 관리한다. 내부 의존성을 관리하므로써 경계를 설정한다
> 이러한 아키텍처는 거의 모든 경우에 특정한 동적 다형성에 의존하여 내부 의존성을 관리한다. 바로 이 때문에 최근 수십년 동안 객체 지향 개발이 아주 중요한 패러다임이 될 수 있었다. 객체지향이 없었다면, 또는 다형성에 해당하는 메커니즘이 없었다면, 아키텍트는 결합도를 적절히 분리하기 위해 함수를 가리키는 포인터라는 위험한 옛 관행에 기대야만 했을 것이다.

가장 단순한 경계 횡단은 저수준 클라이언트에서 고수준 서비스로 향하는 함수호출이다.
> 이 경우 런타임 의존성과 컴파일타임 의존성은 모두 같은방향, 즉 저수준 컴포넌트에서 고수준 컴포넌트로 향한다.

![제어흐름은 경계를 횡단할때 저수준에서 고수준으로 향한다](assets/images/clean/2021-09-01_10-32-45.png)

고수준 클라이언트가 저수준 서비스를 호출하는것도 가능하다.
> 고수준 클라이언트가 저수준 서비스를 호출해야 한다면 동적 다형성을 사용하여 제어 흐름과는 반대 방향으로 의존성을 역전 시킬 수 있다. 이렇게 하면 런타임 의존성은 컴파일타임 의존성과는 반대가 된다.
 
![제어흐름과는 반대로 경계를 횡단한다](assets/images/clean/2021-09-01_10-39-05.png)

## 로컬프로세스
일반적으로 우리가 개발하는 방식이다. 한개의 물리 장비 안에서 데이터를 다른 프로세스에 전달한다.
> 로컬 프로세스들은 동일한 프로세서 또는 하나의 멀티코어 시스템에 속한 여러 프로세서들에서 실행되지만, 각각이 독립된 주소 공간에서 실행된다.

여러개의 컴포넌트가 있을때 수준을 기준으로 의존성을 결정해야 한다. 저수준 컴포넌트에서 고수준 컴포넌트로 향해야 한다.
> 로컬 프로세스를 일종의 최상위 컴포넌트라고 생각하자. 즉, 로컬 프로세스는 컴포넌트간 의존성을 동적 다형성을 통해 관리하는 저수준 컴포넌트로 구성된다.

로컬프로세스에서는 경계를 지날때 ‘비용’ 이 비싸기 때문에 자주 쓰지 않아야 한다.
> 로컬 프로세스 경계를 지나는 통신에는 운영체제 호출, 데이터 마샬링 및 언마샬링, 프로세스간 문맥 교환등이 있으며, 이들은 제법 비싼 작업에 속한다.


## 서비스
서비스 사이의 수준을 확인하여 서로 의존 관계를 설정해야 한다.
서비스 경계를 지날때는 통신을 하기 때문에 매우 느려질수 있다. 이 경우 통신에 지연이 있기 때문에 이 대책을 세워야 한다.
> 서비스는 프로세스로, 일반적으로 명령행 또는 그와 동등한 시스템 호출을 통해 구동된다.
> 서비스 경계를 지나는 통신을 함수 호출에 비해 매우 느리다.
> 저수준 서비스는 반드시 고수준 서비스에 ‘플러그인’ 되어야 한다.

## 결론
일반적으로 시스템에서는 한가지 이상의 경계를 사용한다. 서비스만 사용한다거나, 로컬프로세스만 사용한다는 형식은 많지 않다.
* 단일체 방식에서도 경계는 필요하다
    * 소스코드 의존성으로 제어한다. 