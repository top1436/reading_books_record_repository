---
sidebar_position: 21
sidebar_label: 20. 테스트에 귀 기울이기
---

# 🌈 Chapter 20: 테스트에 귀 기울이기
- TDD는 코드를 테스트하는, 즉 기능과 성능처럼 겉으로 드러나는 품질 속성을 검증하는 활동에 관한 것이다.
- TDD는 코드의 내부 품질 속성에 관한 피드백, 즉 클래스의 결합도와 응집력, 명시적이거나 감춰진 의존성, 효과적인 정보 은닉 등 **코드를 유지 보수 가능한 상태로 유지하는 품질 속성에 관한 작업**이기도 하다.
- 테스트 악취에는 두 종류가 있다.
  - 테스트 자체가 제대로 작성돼 있지 않은, 즉 테스트가 불분명하거나 불안정하게 작성된 경우
  - 테스트에서 대상 코드가 문제라고 강조되는 경우 (테스트하기 어려운 코드)

## 📚 대체할 수 없는 객체에 대해 목 객체를 적용해야 한다(마법을 쓰지 않고)
- 전역 값을 사용해 캡슐화를 우회함으로써 컴포넌트 호출자로부터 의존성을 감출 수 있지만 그렇게 한다고 해서 의존성이 사라지지 않으며, 단지 의존성에 접근할 수 없게 될 뿐이다.
- 코드 구조화 기법 측면에서 객체 지향의 목표 중 **하나는 객체의 경계를 명확하게 보아개 하는 것이다.**
- 객체는 오직 **지역적이거나**(해당 객체의 범위 내에서 생성되고 관리되는) **명시적으로 전달되는 값과 인스턴스**를 다뤄야 한다.

## 📚 로깅은 기능이다

#### 🐶 보조 로깅
- 보조 로깅(오류와 정보)은 애플리케이션 사용자 인터페이스의 일부다. 이 메시지는 실패를 진단하거나 실행 중인 시스템의 진행 상태를 감시할 목적으로 시스템 관리자와 운영자를 비롯해 지원 담당자를 추적할 의도로 작성된 것이다.
- 보조 로깅은 감시나 실패 복구 같은 요구 사항으로부터 테스트 주도적이어야 한다.

#### 🐶 진단 로깅
- 진단 로깅(디버그와 추적)은 프로그래머를 위한 기반 구조에 해당한다. 이 메시지는 제품 코드로 전환해서는 안 되는데, 프로그래머가 현재 개발 중인 시스템 내부의 진행 상항을 이해나는 도움을 줄 목적으로 만들어지는 것이기 때문이다.
- 진단 로깅은 시스템에서 일어나는 일을 세부적으로 추적하고자 하는 프로그래머의 필요에 따라 주도된다. 그래서 진단 로깅은 테스트 주도적일 필요가 없고 진단 로깅 메시지는 보조 로그처럼 일관성을 유지할 필요가 없을지도 모른다.

## 📚 구상 클랫에 대한 목 객체 적용
- 상호 작용 테스트에 대한 한 가지 접근법은 인터페이스가 아니라 구상 클래스를 대상으로 목 객체를 적용하는 것이다. 이 기법을 사용하려면 목 객체를 적용하려는 클래스를 상속받아 테스트 내에서 호출될 메서드를 재정의 해야 한다. 우리가 생각하기에 이 기법은 다른 선택의 여지가 없을 때만 써야 한다.
- 클래스의 내부 기능을 재정의해서는 안 된다. 이렇게 하면 현재 구현의 유별난 부분에 테스트가 묶여버린다. 가시적인 메서드만 재정의해야 한다.

## 📚 값에 목 객체를 적용하지 마라
- 값(어찌 됐든 불변적이어야 할)에 목 객체를 적용할 이유는 전혀 없다. 인스턴스를 생성해서 쓰기만 하라.
- 클래스가 값이 될 가능성이 있어서 목 객체를 적용할 필요가 없는 경우에 대한 휴리스틱이 몇 가지 있다.
  1. 해당 클래스의 값은 불변적이다.
  2. 해당 타입의 인터페이스를 구현하는 클래스를 대상으로 의미 있는 이름을 생각해낼 수 없다.
- 값의 인스턴스를 생성하기가 너무 복잡해서 값을 대상으로 목 객체를 적용하고 싶은 마음이 든다면 빌더를 작성하는 방법을 고려하자(22장 참고)

## 📚 비대한 생성자
- TDD 과정이 진행되는 과정에서 생성자의 인자 목록이 길고 다루기 어려울 때가 있다. (보통한 한 번에 하나씩 객체의 의존성을 추가할 때)
- 특정 개념을 나타내는 일부 인자를 묶어 새로운 객체로 대체할 수 있다.
- 암시적인 컴포넌트를 뽑아낼 경우  두 가지 조건을 살펴보는 것으로 시작한다. 해당 클래스에서 늘 함께 사용되고 수명이 같은 인자가 여기에 해당한다. 조건이 일치하는 부분을 찾고 나면 해당 개념을 설명하는 더 나은 이름을 알아내는 좀 더 어려운 일이 남는다.

## 📚 혼동되는 객체
- 비대한 생성자를 진단하는 또 다른 방법은 객체에 역할이 너무 많아서 객체 자체의 규모가 너무 크다는 것일지도 모른다.

## 📚 너무 많은 의존성
- 비대한 생성자에 대한 세 번째 진단은 모든 인자가 객체 이웃의 유형(63 페이지)에서 정의한 이웃 유형 가운데 하나인 의존성이 아니라는 것이다.

## 📚너무 많은 예상 구문
- 테스트에 예상 구문이 너무 많으면 중요한 것이 무엇이고 실제로 뭘 테스트하는지 파악하기 어렵다.
- 테스트를 읽기 어렵게 하는 것은 모든 것이 예상 구문이라는 점으로 저눕 똑같이 중요해 보인다. 뭐가 중요하고 테스트를 통과하는지 분간할 수 없다.
- 우리는 스텁(테스트를 통과하는 데 도움이 되게끔 실제 행위를 흉내 낸 것)과 예상 구문(객체가 자기 이웃 객체와 상호 작용하는 방법에 관해 보장하려는 단언)을 구분해 의도를 좀 더 분명하게 할 수 있다.
- 예상 구문을 조금만 작성하고 너무 많은 예상 구문을 자제해야 한다.

## 📚 테스트가 우리에게 말해주는 것(여러분이 귀 기울인다면)
- 테스트 악취에 귀를 기울이는 것을 배우는 과정에서 다음과 같은 이점이 있다.

#### 🎈 지식의 초점이 특정 객체에 맞춰진다
- 지식을 특정 객체(객체 내부든, 객체에 전달하든)에 국한할 수 있다면 해당 객체의 구현은 그것을 둘러싼 콘텍스트에 독립성을 유지할 수 있다.

#### 🎈 뭔가가 명시적이라면 거기에 이름을 부여할 수 있다
- 이름을 알 수 있다면 그것의 다른 쓰임새를 찾아서 중복을 줄일 수 있는 좀 더 나은 기회를 얻는 셈이다,

#### 🎈 이름이 더 많다는 것은 도메인 정보가 더 많다는 의미다

#### 🎈 데이터 대신 행위를 전달하라
- 묻지 말고 말하라를 꾸준히 적용하면 행위를 전달하는 경향을 보이는 코딩 스타일이 만들어진다.