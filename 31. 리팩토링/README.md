# 31. 리팩토링
TDD에서는 리팩토링을 특이한 방법으로 사용한다.  
일반적으로 리팩토링은 어떤 상황에서도 프로그램의 의미론을 변경해서는 안 된다.  
하지만 TDD에서 우리가 신경 쓰는 부분은 현재 이미 통과한 테스트들뿐이다.  
예를 들어 TDD에서는 상수를 변수로 바꾸고 양심에 거리낌 없이 이를 리팩토링이라고 부른다.  
왜냐하면 이 행위가 통과하는 테스트의 집합에 아무 변화도 주지 않기 때문이다.
‘관측상의 동치성’이 성립되려면 충분한 테스트를 가지고 있어야 한다.  
충분한 테스트란, 현재 가지고 있는 테스트들에 기반한 리팩토링이 추측 가능한 모든 테스트에 기반한 리팩토링과 동일한 것으로 여겨질 수 있는 상태를 말한다.


## 차이점 일치시키기
비숫해 보이는 두 코드 조각을 합치려면 어떻게 해야할까?  
일단 두 코드가 단계적으로 닮아가게끔 수정한다. 그리고 이 둘이 완전히 동일해지면 둘을 합친다.  
이 리팩토링은 모든 규모의 작업에서 발생한다.
 - 두 반복문의 구조가 비슷하다. 이 둘을 동일하게 만들고 나서 하나로 합친다.
 - 조건문에 의해 나눠지는 두 분기의 코드가 비슷하다. 이 둘을 동일하게 만들고 나서 조건문을 제거한다.
 - 두 클래스가 비슷하다. 이 둘을 동일하게 만들고 나서 하나를 제거한다.


## 변화 격리하기
객체나 메서드의 일부만 바꾸려면 어떻게 해야 할까? 일단, 바꿔야 할 부분을 격리한다.  
일단 바꿀 부분을 격리하고 나서 바꾸는 작업을 수행하면 작업을 되돌리기도 매우 수월하다는 사실을 알게 될 것이다.  
변화를 격리하기 위해 사용할 수 있는 몇가지 방법에는 메서드 추출하기(가장 일반적), 객체 추출하기, 메서드 객체(Method Object) 등이 있다.


## 데이터 이주시키기
표현 양식을 변경하려면 어떻게 해야 할까? 일시적으로 데이터를 중복시킨다.

### 방법
내부에서 외부로 변화시키는 방법은 다음과 같다.  
내부에서 외부로의 변화란 내부의 표현 양식을 변경한 후 외부 인터페이스를 변화시키는 방법을 말한다.  
 - 새로운 포맷의 인스턴스 변수를 추가한다.
 - 기존 포맷의 인스턴스 변수를 세팅하는 모든 부분에서 새로운 인스턴스 변수도 세팅하게 만든다.
 - 기존 변수를 사용하는 모든 곳에서 새 변수를 사용하게 만든다.
 - 기존 포맷을 제거한다.
 - 새 포맷에 맞게 외부 인터페이스를 변경한다.

때로는 API를 먼저 변화시키기를 원할 때도 있다. 그럴땐 다음처럼 한다.
 - 새 포맷으로 인자를 하나 추가한다.
 - 새 포맷 인자에서 이전 포맷의 내부적 표현양식으로 번역한다.
 - 이전 포맷 인자를 삭제 한다.
 - 이전 포맷을 사용하는 것들을 새 포맷으로 바꾼다.
 - 이전 포맷을 지운다.


## 메서드 추출하기
길고 복잡한 메서드를 읽기 쉽게 만들려면 어떻게 할까?  
긴 메서드의 일부분을 별도의 메서드로 분리해내고 이를 호출하게 한다.

### 방법
 - 기존의 메서드에서 별도의 메서드로 분리할 수 있을 만한 부분을 찾아 낸다. 반복문 내부의 코드나 반복문 전체, 혹은 조건문의 가지들이 일반적인 후보다.
 - 추출할 영역의 외부에서 선언된 임시 변수에 대해 할당하는 문장이 없는지 확인한다.
 - 추출할 코드를 복사해서 새 코드에 붙인다.
 - 원래 메서드에 있던 각각의 임시 변수와 매개 변수 중 새 메서드에서도 쓰이는 게 있으면, 이들을 새 메서드의 매개 변수로 추가한다.
 - 기존의 메서드에서 새 메서드를 호출한다.

일부 서로 비슷한 내용이 있는 두 메서드에서 중복을 제거하기 위해 메서드 추출하기를 사용하기도 한다. 두 메서드의 비슷한 부분을 하나의 메서드로 추출해낸다.  
메서드를 작은 조각으로 나누는 것은 때때로 그 정도가 지나칠 수도 있다.  
더 나아갈 방법이 보이지 않을 때에는, 새로운 방식으로 메서드 추출하기를 하기 위해 일단 모든 코드를 한 자리에 모아놓고 메서드 인라인 리팩토링을 자주 사용한다.


## 메서드 인라인
너무 꼬여있거나 산재한 제어 흐름을 단순화 하려면 어떻게 할까?  
메서드를 호출하는 부분을 호출될 메서드의 본문으로 교체한다.

### 방법
 - 메서드를 복사한다.
 - 메서드 호출하는 부분을 지우고 복사한 코드를 붙인다.
 - 모든 형식(formal) 매개 변수를 실제(actual) 매개 변수로 변경한다. 예를 들어 만약약 reader.getNext()같은 매개 변수를 전달했다면, 이를 지역 변수에 할당해주어야 할 것이다.


## 인터페이스 추출하기
자바 오퍼레이션에 대한 두 번째 구현을 추가하려면 어떻게 해야 할까?  
공통되는 오퍼레이션을 담고 잇는 인터페이스를 만들면 된다.

### 방법
 - 인터페이스를 선언한다. 때론 새로 추가될 인터페이스의 이름으로 기존 클래스의 이름을 사용해야 하는 경우가 있는데, 그런 경우라면 인터페이스를 추가하기 전에 기존 클래스의 이름을 변경해주어야 한다.
 - 기존 클래스가 인터페이스를 구현하도록 만든다.
 - 필요한 메서드를 인터페이스에 추가한다. 필요하다면 클래스에 존재하는 메서드들의 가시성을 높여준다.
 - 가능한 모든 곳의 타입 선언부에서 클래스 이름 대신 인터페이스 이름을 사용하게 바꾼다.


## 메서드 옮기기
메서드를 원래 있어야 할 장소로 옮기려면 어떻게 해야할까?  
어울리는 클래스에 메서드를 추가해주고, 그것을 호출하게 하라.

### 방법
 - 메서드를 복사한다.
 - 원하는 클래스에 붙이고 이름을 적절히 지어준 다음 컴파일한다.
 - 원래 객체가 메서드 내부에서 참조된다면, 원래 객체를 새 메서드의 매개 변수로 추가한다. 원래 객체의 필드들이 참조되고 있다면 그것들도 매개 변수로 추가한다. 만약 원래 객체의 필드들이 갱신된다면 포기해야 한다.
 - 원래 메서드의 본체를 지우고, 그곳에 새 메서드를 호출하는 코드를 넣는다.

### 메서드 옮기기의 훌륭한 세가지 속성
 - 코드에 대한 깊은 이해가 없더라도 언제 이 리팩토링이 필요한지 쉽게 알아낼 수 있다. 다른 객체에 대한 두 개 이상의 메시지를 보내는 코드를 볼 때마다 메서드 옮기기를 해주면 된다.
 - 리팩토링 절차가 빠르고 안전하다.
 - 리팩토링 결과가 종종 새로운 사실을 알려준다.


## 메서드 객체
여러 개의 매개 변수와 지역 변수를 갖는 복잡한 메서드를 어떻게 표현할까?  
메서드를 꺼내서 객체로 만든다.

### 방법
 - 메서드와 같은 매개 변수를 갖는 객체를 만든다.
 - 메서드의 지역 변수를 객체의 인스턴스 변수로 만든다.
 - 원래 메서드와 동일한 내용을 갖는 run()이라는 이름의 메서드를 만든다.
 - 원래 메서드에서는 새로 만들어진 클래스의 인스턴스를 생성하고 run()을 호출한다.

메서드 객체는 시스템에 완전히 새로운 로직을 추가하고자 할 때 유용하며, 메서드 추출하기를 적용할 수 없는 코드를 간결하게 만들기 위한 용도로도 적합하다


## 매개 변수 추가
메서드에 매개 변수를 추가하려면?

### 방법
 - 메서드가 인터페이스에 선언되어 있다면 일단 인터페이스에 매개 변수를 추가한다.
 - 매개 변수를 추가한다.
 - 컴파일 에러가 여러분에게 어딜 고쳐야 하는지 알려줄 것이다. 이것을 이용하라.


## 메서드 매개 변수를 생성자 매개 변수로 바꾸기
하나 이상의 메서드의 매개 변수를 생성자로 옮기려면 어떻게 할까?

### 방법
 - 생성자에 매개 변수를 추가한다.
 - 매개 변수와 같은 이름을 갖는 인스턴스 변수를 추가한다.
 - 생성자에서 인스턴스 변수의 값을 설정한다.
 - 'parameter'를 'this.parameter'로 하나씩 찾아 바꾼다.
 - 매개 변수에 대한 참조가 더 이상 존재하지 않으면 해당 매개 변수를 메서드와 모든 호출자에서 제거한다.
 - 이제 필요 없어진 'this.'를 제거한다.
 - 변수명을 적절히 변경한다.
