# C++ Core Concepts Summary

> C++ 핵심 개념 압축 정리 — 객체, 참조, 복사, STL, move semantics까지 한 번에 정리

---

# 1. 기본 문법 (C와 동일)

* 조건문: `if`, `switch`
* 반복문: `for`, `while`, `do-while`
* 제어문: `break`, `continue`

→ C 문법 알면 빠르게 넘어가도 됨

---

# 2. 함수 & 참조 (Reference)

## 참조란?

```
int a = 10;
int& r = a;   // r = a의 별명
```

특징:

* 원본 변수 직접 참조
* nullptr 불가
* 재바인딩 불가

---

## 함수 인자 전달 방식

| 방식       | 특징            |
| -------- | ------------- |
| 값 전달     | 복사됨           |
| 참조 전달    | 원본 수정         |
| const 참조 | 읽기 전용 + 복사 없음 |

```
void f(const vector<int>& v);
```

👉 큰 객체는 const reference로 전달

---

# 3. 클래스 핵심 구조

```
class A {
public:
    int x;

    A();               // 생성자
    A(const A& other); // 복사 생성자
    ~A();              // 소멸자
};
```

---

# 4. 생성자 / 소멸자

## 생성자

→ 객체 생성 시 자동 호출

종류

* 기본 생성자
* 복사 생성자
* 이동 생성자

---

## 소멸자

→ 객체 소멸 시 자동 호출

목적:

* 메모리 해제
* 파일 닫기
* 자원 정리

핵심 개념: **RAII**

---

# 5. 복사 생성자 vs 대입 연산자 ⭐

### 기준 공식

```
생성 중 복사 → 복사 생성자
생성 후 복사 → 대입 연산자
```

---

### 예제

```
A a;
A b = a;   // 복사 생성자

A c;
c = a;     // 대입 연산자
```

---

### 함수 인자 전달

```
void f(A a);
```

→ 호출 시 복사 생성자 실행

---

# 6. 깊은 복사 vs 얕은 복사

얕은 복사 문제:

```
포인터 주소만 복사됨
→ 이중 delete 위험
```

해결:

→ 복사 생성자 직접 구현

---

# 7. Move Semantics

핵심 개념:

```
복사 대신 자원 이동
```

---

### std::move 역할

```
lvalue → rvalue 캐스팅
```

실제 이동은:

```
move constructor
```

---

### 예제

```
vector<int> a = {1,2,3};
vector<int> b = std::move(a);
```

결과:

* b → 데이터 소유
* a → 비워짐

---

# 8. lvalue vs rvalue

| 종류     | 의미      |
| ------ | ------- |
| lvalue | 이름 있는 값 |
| rvalue | 임시 값    |

---

# 9. && (rvalue reference)

```
int&& x = 10;
```

의미:

> rvalue만 받을 수 있는 참조

---

# 10. STL 핵심 구조

STL = Standard Template Library

구성:

### 컨테이너

* vector
* map
* set
* string

### 알고리즘

* sort
* max
* find

### 반복자 (iterator)

컨테이너 원소 접근 인터페이스

---

## iterator 종류

| 종류            | 특징    |
| ------------- | ----- |
| random access | +n 가능 |
| bidirectional | ++ -- |
| forward       | ++만   |

---

# 11. advance()

```
advance(it, n);
```

→ iterator를 n칸 이동
(종류에 맞게 내부 동작 자동 선택)

---

# Final Summary

C++ 핵심 이해 흐름:

```
참조 → 클래스 → 복사 개념 → move → STL
```

이 흐름 이해하면 C++ 기초 완성.
