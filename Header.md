# C++ Header File 정리 (Declaration / Definition / Compile / Link)

## 1. Header File의 역할

C++에서 **헤더 파일(.h)** 은 함수, 클래스, 변수 등의 **선언(declaration)을 공유하기 위한 파일**이다.

헤더는 실제 기능을 구현하지 않고 **"무엇이 존재하는지"만 알려주는 인터페이스 역할**을 한다.

예:

```cpp
// add.h
int add(int, int);
```

---

# 2. 선언(Declaration) vs 정의(Definition)

## 선언 (Declaration)

함수나 변수가 **존재한다는 사실만 알려주는 것**

```cpp
int add(int, int);
```

특징

- 함수의 존재를 컴파일러에게 알림
- 실제 기능 구현 없음

---

## 정의 (Definition)

함수의 실제 기능을 구현하는 것

```cpp
int add(int a, int b)
{
    return a + b;
}
```

특징

- 함수의 실제 동작을 구현
- 프로그램 전체에서 **한 번만 존재해야 함**

---

# 3. Header와 Source 파일 구조

C++에서는 보통 다음과 같은 구조를 사용한다.

```
add.h      → 선언 (Interface)
add.cpp    → 정의 (Implementation)
main.cpp   → 사용
```

---

## 예시

### add.h

```cpp
int add(int, int);
```

---

### add.cpp

```cpp
#include "add.h"

int add(int a, int b)
{
    return a + b;
}
```

---

### main.cpp

```cpp
#include "add.h"

int main()
{
    return add(1,2);
}
```

---

컴파일

```
g++ main.cpp add.cpp
```

---

# 4. C++ 컴파일 과정

C++ 프로그램은 **두 단계로 빌드된다**

```
Compile → Link
```

---

## 1️⃣ Compile 단계

각 `.cpp` 파일을 **개별적으로 컴파일**한다.

```
main.cpp → main.o
add.cpp → add.o
```

컴파일러가 검사하는 것

- 문법 오류
- 타입 검사
- 선언 존재 여부

컴파일러는 **정의가 존재하는지는 확인하지 않는다.**

---

## 2️⃣ Link 단계

링커가 모든 object 파일을 연결한다.

```
main.o + add.o → 실행파일
```

링커가 검사하는 것

- 함수 정의 존재 여부
- 중복 정의 여부

---

# 5. Header를 include 하는 이유

헤더는 여러 소스 파일이 **같은 인터페이스를 공유하도록 한다.**

구조

```
           add.h
         (interface)
           /   \
          /     \
     main.cpp  add.cpp
      (사용)     (구현)
```

---

# 6. 선언만 있고 정의가 없을 때

예

```
add.h
int add(int,int);
```

```
main.cpp
#include "add.h"
```

하지만 정의가 없으면

링크 단계에서 에러 발생

```
undefined reference to add
```

---

# 7. 정의가 두 개 있을 때 (ODR 위반)

예

```
add.cpp
add2.cpp
```

둘 다

```
int add(int,int)
```

정의하면

링크 에러 발생

```
multiple definition of add
```

---

# 8. ODR (One Definition Rule)

C++의 중요한 규칙

```
Declaration → 여러 번 가능
Definition → 프로그램 전체에서 하나만 가능
```

이 규칙을 **ODR (One Definition Rule)** 이라고 한다.

---

# 9. 구현 파일에서도 Header를 include 하는 이유

구현 파일에서도 헤더를 include 한다.

```cpp
#include "add.h"
```

이유

### 선언과 정의 불일치 방지

예

헤더

```cpp
int add(int,int);
```

구현

```cpp
int add(double,double)
```

컴파일러가 바로 에러를 발견한다.

이것은 **실수를 빠르게 발견하기 위한 안전장치**이다.

---

# 10. #include의 실제 의미

많은 사람들이 오해하는 부분

```
#include
```

는 **파일을 연결하는 것이 아니라 텍스트를 복사하는 것**이다.

예

```cpp
#include "add.h"

int main()
{
    return add(1,2);
}
```

전처리 후

```cpp
int add(int,int);

int main()
{
    return add(1,2);
}
```

---

# 11. 오버로딩과 Header

C++은 함수 이름만이 아니라

```
함수 이름 + 파라미터 타입
```

으로 함수를 구분한다.

예

```cpp
int add(int,int);
double add(double,double);
```

---

### 정상적인 오버로딩

```cpp
// add.h

int add(int,int);
double add(double,double);
```

```cpp
// add.cpp

int add(int a,int b)
{
    return a+b;
}

double add(double a,double b)
{
    return a+b;
}
```

---

# 12. Header에 없는 함수

구현 파일에서 자유롭게 정의할 수 있다.

```cpp
int helper(int x)
{
    return x * 2;
}
```

하지만 다른 파일에서 사용하려면 **선언이 필요하다.**

---

# 13. Header = Interface

C++ 설계 철학

```
Header → Interface
Source → Implementation
```

Header는 **외부에 공개할 API를 정의하는 계약(contract)** 역할을 한다.

---

# 14. 전체 빌드 구조

```
source files
   │
   ▼
Compile
   │
   ▼
object files (.o)
   │
   ▼
Link
   │
   ▼
Executable
```

---

# 핵심 요약

### Header의 역할

1️⃣ 선언 공유  
2️⃣ 컴파일러에게 함수 존재 알림  
3️⃣ 선언과 정의 일치 검사

---

### 컴파일 vs 링크

컴파일

```
문법 검사
타입 검사
선언 검사
```

링크

```
정의 존재 확인
중복 정의 검사
```

---

# 한 줄 정리

> **Header는 프로그램의 인터페이스를 정의하고 여러 소스 파일이 동일한 선언을 공유하도록 만드는 파일이다.**
