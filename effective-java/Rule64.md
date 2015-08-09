# Rule 64 - 실패 원자성 달성을 위해 노력하라

**예외를 던지고 난 뒤에도 객체의 상태가 잘 정의된, 사용 가능한 상태로 남아있으면 좋다. 설사 어떤 연산을 수행하던 중이었다 해도 말이다.** 호출자 측에서 
오류상태를 복구할 가능성이 있는 점검지정 예외의 경우에는 특히 더 그렇다. 

> 일반적으로 메서드 호출이 정상적으로 처리되지 못한 객체의 상태는, 메서드 호출 전 상태와 동일해야 한다. 이 속성을 만족하는 메서드는 **실패 원자성** (Failure Atomicity) 를 갖추었다고 말한다.

## 실패 원자성 달성 방법 1 - 변경 불가능 객체

## 실패 원자성 달성 방법 2 - 실패할 가능성이 있는 코드를 객체 상태 변경 코드 이전에 배치하기

예를 들어 `TreeMap` 의 경우 맵 안에 보관되어 있는 원소들은 특정 순서로 정렬되는데, `TreeMap` 에 추가할 원소는 순서(`Ordering`) 비교가 가능한 자료형이어야 한다. 
엉뚱한 자료형의 원소를 넣으려고 하면, 트리를 실제로 변경하기 전에 트리 안에서 해당 원소를 찾다가 `ClassCastException` 이 발생할 것이다.

## 실패 원자성 달성 방법 3 - 객체의 임시 복사본에서 작업하고, 연산이 끝난 후에야 복사본을 반영하기
 
데이터를 임시 자료구조에 복사한 다음에 훨씬 신속하게 수행될 수 있는 연산은 이 방법이 더 적절하다. 
예를 들어 `Collections.sort` 는 원소들을 참조하는 비용을 줄이기 위해, 정렬 전에 배열에 복사한다. 성능 문제 때문에 내린 조치인데, 
그 덕에 정렬이 실패해도 원래 리스트에는 아무런 손상이 없다.

## 실패 원자성은 권장되나, 언제나 달성할 수 있는 것은 아니다.

예를 들어, 객체를 여러 스레드가 적절한 동기화 없이 변경하는 경우, 객체의 일관성은 깨질 수 있다. 
그러니 `ConcurrentModificationException` 이 발생한 뒤에는 객체의 상태는 망가져 있으리라 보는것이 더 맞다. 

명심할 것은, 예외와는 달리 **오류(error)** 는 복구가 불가능하며, 오류를 던지는 경우에는 실패 원자성을 보존하려 애쓸 필요가 없다.

실패 원자성을 달성할 수 있다 하더라도, 비용이나 복잡성이 심각하게 늘어나는 경우도 있다. 그러나, 이슈가 무엇인지 파악하고 나면 
실패 원자성을 공짜로 달성할 수 있는 경우도 많다.

> **메서드 명세에 포함된 예외가 발생하더라도, 객체의 상태는 메서드 호출 이전과 동일하게 보장되야 하며 이 규칙을 지키지 못할 경우 객체의 상태가 어떻게 변하는지를 반드시 API 문서에 기술해야 한다.**


 
 