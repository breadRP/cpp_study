# 1️⃣ struct vs class 정리

---

### 공통점

* 기능 차이 없음
* 생성자 / 소멸자 / 함수 / 상속 동일

### 유일한 차이

| 구분     | 기본 접근 지정자 |
| ------ | --------- |
| struct | public    |
| class  | private   |

---

# 2️⃣ Semantic 개념

**semantic**

> 코드가 실제로 무엇을 의미하고 어떻게 동작하는가

syntax = 문법
semantic = 의미

---

# 3️⃣ C++ 핵심 Semantic 종류

### ① Lifetime

객체가 언제 생성되고 언제 파괴되는가

### ② Ownership

누가 자원을 삭제할 책임을 가지는가

### ③ Copy

복사 시 어떤 일이 발생하는가

### ④ Move

자원을 복사하지 않고 이전하는가

### ⑤ Value vs Reference

복사 시 독립 객체인지 공유 객체인지

### ⑥ Const

변경 불가능하다는 계약 의미

### ⑦ Storage

객체가 저장되는 메모리 영역

---

# 4️⃣ Value vs Reference Semantic

## Value Semantic

* 복사 → 완전히 독립 객체
* 서로 영향 없음
* 예측 쉬움

## Reference Semantic

* 복사 → 동일 대상 공유
* 변경 전파됨
* 소유권 문제 가능

---

# 5️⃣ 핵심 원칙

> struct / class 키워드는 semantic을 결정하지 않는다
>
> semantic은 구현 방식이 결정한다.

결정 요소:

* 멤버 타입
* 복사 정책
* 소유권 설계

---

# 6️⃣ struct 사용 관례

struct는 value semantic 타입일 때 자주 사용된다.

이유:

* 읽는 사람이 의미 추론 가능
* 불필요한 캡슐화 제거
* 의도 전달 명확

---

# 7️⃣ 타입 설계 판단 기준

타입 만들 때 핵심 질문:

> 이 타입은 복사해도 안전한가?

| 답   | 선택        |
| --- | --------- |
| YES | struct 고려 |
| NO  | class 권장  |

---

# ✅ 최종 핵심 정리

> 좋은 C++ 코드는 문법이 아니라 semantic 설계 품질로 결정된다.
