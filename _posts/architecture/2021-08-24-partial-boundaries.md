---
layout: post
current: post
navigation: True
title: 부분적 경계
date: 2021-08-24 00:00:00
tags: [architecture]
class: post-template
subclass: 'post tag-architecture'
permalink: /clean_architecture/partial-boundaries
author: mason
---
부분적 경계란?
아키텍처 경계를 완벽하게 하기 위해서는 손이 많이가는데, 이것을 위한 구현방법
책에서는 마지막단계 건너뛰기, 일차원 경계, 퍼사드 세 가지 방법을 제시한다

> 아키텍처 경계를 완벽하게 만드는 데는 비용이 많이 든다. 쌍방향의 다형적 Boundary interface, Input과 Output을 위한 데이터구조
> …
> 이렇게 만들려면 엄청난 노력을 기울여야 하고, 유지하는 데도 또 엄청난 노력이 든다.
>
![양방향경계](https://lucid.app/publicSegments/view/08cd361f-fcda-4bcb-89f3-ae6c4e6bf786/image.png)

여기서는 일차원경계, 퍼사드를 살펴볼텐데 실제 프로젝트를 진행하면서 고려했던 것들을 확인할 수 있었다

## 일차원경계
경계를 양방향 인터페이스가 아닌, 1개의 인터페이스로 유지하는것 일반적으로 MVC 구조에서 사용된다. 책에서는 의존성 역전이 깨지는 상황이 발생할 수 있다고 지적하고 있다. 하지만 현실적으로 여러가지문제(대표적으로 일정) 때문에 대부분 선택하는 상황이 아닐까 싶다

![일차원경계](https://lucid.app/publicSegments/view/82d98158-56a2-418b-aec0-6eeaa4187ed3/image.png)

## 퍼사드Facade
퍼사드에서는 의존성 역전까지 포기한다. 클라이언트는 서비스에 직접 접근하지 않는 것만 신경쓴다.
![퍼사드](https://lucid.app/publicSegments/view/f2e280e2-db1b-4517-881b-535f15a3258f/image.png)

## 결론
경계를 부분적으로 구현하는 방법을 제시했다. 어떤 방법을 쓸지는 아키텍처가 장단점을 확인하여 적용한다.
실제로 모든 코드에 양방향 경계를 적용하기는 어려운것 같다. 특히 여러명의 개발자가 보는 코드에서 구조를 바꾸는것은 쉽지 않다. 본문에서는 인터페이스, 데이터구조, 격리의 의존성을 얘기하고 있지만, 경험상 추가로 패키지 규칙부터 시작해서 개발자들이 동감할 수 있는 기준도 만들어야 한다. 
