---
layout: default
title:  "Design pattern"
date:   2022-10-01 11:07:53 +0900
categories: dev
---

ToC
- API Design pattern
    - reference : https://en.wikipedia.org/wiki/[Facade_pattern Adapter_pattern Decorator_pattern SOLID]
- UI Design pattern
    - reference 
        - www.geeksforgeeks.org/difference-between-mvc-mvp-and-mvvm-architecture-pattern-in-android/
        - microservices.io

---

## API Design pattern
![](/assets/design_pattern_api.png)

1. Fasade : Simplified interface. Improve the readability and usability

1. Adapter : Converts one interface to another so that it matches what the client is expecting
    - The wrapper must respect a particular interface and must support polymorphic behavior.

1. Decorator: Dynamically adds responsibility to the interface by wrapping the original code

1. SOLID principle
    - Single-responsibility: Every class should have only one responsibility.
    - Open–closed: Be open for extension, but closed for modification.
    - Liskov substitution:
        - Any instance of Base class can be replaced with any instance of Base::Derived class - upcasting.
    - Interface segregation: Clients should not be forced to depend upon interfaces that they do not use.
    - Dependency inversion: Depend upon abstractions, not concretions.

1. And so on
    - DRY(Don't repeat yourself)
    - YAGNI(You aren't going to need it)
    - KISS(Keep it short and simple)


## UI Design pattern

1. MVC
    ```
             ------- C -------
            |                 |
     (update data)       (update UI)
            ↓                 ↓
            M <--(get data)-- V
    ```

    - Model : Data + Status + Logic(Create/Delete/Update/Query on DB)
    - View : Model을 반영한 UI 표현
    - Controller : Model & View 연결하고 관리. User input 수신
    - Pros & Cons
        - Pros : Model이 독립적. 간단한 앱에 적합
        - Cons : Contorller와 View의 결합도 높음. Controller 역할 커지면, 유지보수 어려워짐

1. MVP
    ```
      --(notify)----------->   <-(send user action)--
    M                        P                        V
      <-(own & update data)-   --(update UI)-------->
    ```
    - Model : Data + Status + Login
    - (Passive)View : User input 수신. Presenter를 소유하며, 액션 전달
    - Presenter : Model 소유하며, Model 갱신되면 View에 전달
    - Pros & Cons
        - Pros : View & Model 간 의존도 낮춤
        - Cons
            - MVC보다 명확한 구분으로 코드가 커지고, 비효율적인 구조가 될 수 있음 -> 유지보수 어려움
            - Presenter가 커지면, MVC와 비슷한 문제 발생

1. MVVM
    ```
      --(notify)------->    <-(own)--------------------------
    M                    VM                                   V
      <-(own & update)--    <--(binding data & user action)->
    ```
    - Model : Data + Status + Login
    - View : User input 수신
    - ViewModel : Model 소유 & 업데이트. 업데이트 이벤트 및 액션을 View에 전달
    - Pros & Cons
        - Pros : UI 개발에 적합. ViewModel이 View에 의존성 없어 모듈화가 높고 유지보수 간결. View는 ViewModel과의 바인딩을 통해 자동화 & 모듈화
        - Cons : MVP보다 View의 책임 범위가 더 크다.

1. Microservice Architecture
    - independently deployable, loosely coupled, components
    - Pros & Cons
        - Pros : easy development, deploy & maintainance
        - Cons : increase system complexity, network latency, difficult to monitor or debug

1. Spring MVC
TBC

