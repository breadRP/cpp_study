# C++ 분할 컴파일 & 네임스페이스 정리 (복습용)

---

# 1️⃣ 분할 컴파일 (Separate Compilation)

## 정의

분할 컴파일이란  
프로그램을 여러 `.cpp` 파일로 나누어 각각 독립적으로 컴파일한 후  
링커가 이를 연결하여 실행 파일을 만드는 방식이다.

```
main.cpp      ┐
math.cpp      ├─ 각각 컴파일 → .o 생성
util.cpp      ┘
                  ↓
               링커
                  ↓
               실행파일
```

---

## 목적

- 모듈화
- 코드 관리 용이
- 빌드 속도 향상
- 협업 가능
- 라이브러리 제작 가능

---

# 2️⃣ 선언 vs 정의

## 선언 (Declaration)

```cpp
double add(double, double);
```

- 함수 존재 정보 제공
- 구현 없음
- 보통 헤더에 작성

---

## 정의 (Definition)

```cpp
double add(double a, double b) {
    return a + b;
}
```

- 실제 실행 코드
- cpp 파일에 작성

---

# 3️⃣ 컴파일 단계 vs 링크 단계

## 컴파일러

- 문법 검사
- 선언 존재 확인
- .o 파일 생성

> 컴파일러는 선언이 필요하다.

---

## 링커

- 정의 위치 탐색
- 여러 .o 연결
- 실행 파일 생성

> 링커는 정의가 필요하다.

---

# 4️⃣ 헤더 파일 역할

헤더는:

- 함수 선언
- 클래스 선언
- 타입 정의

를 담는다.

헤더는 **컴파일 대상이 아니라 포함되는 파일**이다.

```
#include "math.h"
```

→ 전처리 단계에서 내용 복사

---

## 헤더 작성 규칙

✔ using namespace 금지  
✔ std::vector 같이 전체 이름 명시  
✔ 인터페이스만 작성  

---

# 5️⃣ 링커 동작 원리

링커는:

- 자동으로 모든 파일을 찾지 않는다
- 컴파일 명령에 포함된 파일만 연결한다
- -l 옵션으로 라이브러리 지정

---

## 주요 옵션

| 옵션 | 역할 |
|------|------|
| -I | 헤더 검색 경로 추가 |
| -L | 라이브러리 검색 경로 추가 |
| -l | 특정 라이브러리 링크 |

---

# 6️⃣ static 키워드

static의 의미는 위치에 따라 다름.

## 함수 내부 static 변수

- 프로그램 종료까지 유지

## 전역 static 변수 / 함수

- 파일 내부에서만 접근 가능
- internal linkage

```
static int helper();
```

→ 다른 파일에서 접근 불가  
→ 잘못 사용하면 undefined reference 발생

---

# 7️⃣ 익명 namespace

```
namespace {
    int helper();
}
```

- 파일 내부 전용
- static과 동일한 internal linkage
- C++ 권장 방식

> 익명 namespace = 파일 단위 private 영역

---

# 8️⃣ namespace의 역할

namespace는:

- 이름 충돌 방지
- 논리적 코드 그룹화

파일 분리와는 별개의 개념이다.

---

## std namespace

표준 라이브러리는 모두 std namespace 안에 정의되어 있다.

```
std::vector
std::string
std::cout
```

using은 단지 이름을 줄이는 문법이다.

```
using std::vector;
```

---

# 9️⃣ using의 정확한 의미

using은:

- 이름 생략 허용
- 접근 권한 변경 아님
- 링크와 무관

```
include → 선언 확보
link → 정의 확보
using → 이름 축약
```

---

# 🔟 실무 규칙 요약

✔ 헤더에서 using namespace 금지  
✔ 헤더에서는 std:: 명시  
✔ 내부 helper 함수는 static 또는 익명 namespace 사용  
✔ 인터페이스와 구현 분리  

---

# 최종 개념 구조

```
소스코드
   ↓
컴파일러 (선언 필요)
   ↓
오브젝트 파일
   ↓
링커 (정의 필요)
   ↓
실행파일
```

---

# 핵심 문장 요약

- 분할 컴파일은 모듈화를 위한 빌드 방식이다.
- 컴파일러는 선언이 필요하고, 링커는 정의가 필요하다.
- 헤더는 인터페이스, cpp는 구현이다.
- namespace는 이름 충돌 방지용 기능이다.
- static과 익명 namespace는 파일 내부 전용 심볼을 만든다.
- using은 단순 이름 생략 문법이다.


---

# 11️⃣ ODR (One Definition Rule)

## 정의

**ODR (One Definition Rule)** 이란  
프로그램 전체에서 동일한 엔티티의 정의는 반드시 하나만 존재해야 한다는 규칙이다.

> 선언은 여러 번 가능하지만 정의는 하나만 가능하다.

---

## ODR이 적용되는 대상

다음 항목들은 프로그램 전체에서 정의가 하나만 존재해야 한다.

- 전역 변수
- 함수
- 클래스 타입
- static 객체
- namespace 객체

---

## ODR 위반 예시

### a.cpp

```cpp
int x = 10;
```

### b.cpp

```cpp
int x = 20;
```

컴파일 후 링크하면:

```
multiple definition of x
```

→ ODR 위반

---

## 왜 ODR이 필요한가?

링커는 심볼 이름 기준으로 연결한다.

```
x
```

이 심볼이 여러 개 존재하면 링커는 어떤 정의를 사용할지 결정할 수 없다.

따라서 C++ 규칙:

> 동일 심볼 정의는 하나만 존재해야 한다.

---

## 선언 vs 정의 (ODR 관점)

| 구분 | 개수 허용 |
|------|-----------|
선언 | 여러 개 가능 |
정의 | 하나만 가능 |

---

## 자주 발생하는 ODR 오류

### ❌ 헤더에 함수 정의 작성

```cpp
// wrong.h
int add(int a,int b){
    return a+b;
}
```

이 헤더를 여러 cpp에서 include하면

→ 함수 정의가 여러 개 생성됨  
→ 링크 에러 발생

---

### ✔ 해결 방법

헤더에는 선언만 작성

```cpp
// correct.h
int add(int,int);
```

구현은 cpp에서

```cpp
// correct.cpp
int add(int a,int b){
    return a+b;
}
```

---

## ODR 예외 (여러 정의 허용되는 경우)

다음은 여러 정의가 있어도 허용된다.

- inline 함수
- 템플릿
- constexpr 함수
- 클래스 정의 (동일 내용일 경우)

이들은 특별 규칙이 적용된다.

---

## ODR 위반 주요 에러 메시지

대표적인 링커 에러:

```
multiple definition of symbol
duplicate symbol
redefinition of
```

이 에러가 나오면 대부분 ODR 문제다.

---

## ODR과 링크 단계 관계

ODR 검사는 컴파일 단계가 아니라 **링크 단계에서 발생**한다.

```
컴파일 → 통과
링크 → 실패 (ODR 발견)
```

---

## ODR 핵심 규칙 요약

- 선언은 여러 번 가능
- 정의는 하나만 가능
- 헤더에는 정의 넣지 말 것
- 전역 변수 정의는 하나만 둘 것

---

## ODR 한 줄 핵심

> 프로그램 전체에서 동일 심볼 정의는 반드시 하나만 존재해야 한다.

---

## 현재 이해 위치

이제 이해한 개념 흐름:

```
분할 컴파일
→ 컴파일러
→ 링커
→ static
→ namespace
→ using
→ ODR
```

이 단계까지 이해했다면:

> C++ 빌드 구조 핵심 개념을 모두 이해한 상태다.