# C++ 핵심 정리 (질문 기반 요약)

---

## 📌 erase 사용법

### 1. vector에서 특정 위치 삭제

```cpp
v.erase(v.begin() + i);
```

### 2. 구간 삭제

```cpp
v.erase(v.begin() + start, v.begin() + end);
```

### 3. 값 기준 삭제 (erase-remove idiom)

```cpp
v.erase(remove(v.begin(), v.end(), value), v.end());
```

---

## 📌 find와 인덱스

```cpp
auto it = find(v.begin(), v.end(), value);
int idx = it - v.begin();
```

* `find`는 iterator 반환
* 인덱스는 직접 계산해야 함

---

## 📌 erase-remove idiom

```cpp
v.erase(remove(v.begin(), v.end(), value), v.end());
```

* `remove` → 값 밀기 (삭제 아님)
* `erase` → 실제 삭제

---

## 📌 대문자 변환

```cpp
#include <cctype>

char c = toupper(c);
```

문자열:

```cpp
transform(s.begin(), s.end(), s.begin(), ::toupper);
```

---

## 📌 std::string_view

### 특징

* 문자열 복사 없이 참조
* 읽기 전용
* 빠름

### 위험

```cpp
std::string_view sv;

{
    std::string s = "hello";
    sv = s;
} // dangling 발생
```

---

## 📌 string_view 오류 원인

```
Assertion '__pos < this->_M_len' failed
```

👉 인덱스 범위 초과

---

## 📌 startsWith 구현

```cpp
bool startsWith(std::string_view str, std::string_view prefix) {
    if (str.size() < prefix.size()) return false;
    return str.substr(0, prefix.size()) == prefix;
}
```

---

## 📌 switch문

```cpp
switch (x) {
    case 1:
        break;
    default:
        break;
}
```

### 특징

* break 없으면 fall-through
* 정수/char/enum만 가능

---

## 📌 enum + switch

```cpp
enum class Color { RED, GREEN, BLUE };

switch (c) {
    case Color::RED:
        break;
}
```

---

## 📌 클래스 메서드 정의

### 내부 정의

```cpp
class A {
public:
    void f() {}
};
```

### 외부 정의

```cpp
void A::f() {}
```

---

## 📌 멤버 변수 값 저장

```cpp
class A {
public:
    int x;

    void set(int x) {
        this->x = x;
    }
};
```

---

## 📌 생성자 초기화

```cpp
A(int val) : x(val) {}
```

---

## 📌 map 삽입

```cpp
m["key"] = value;
m.insert({"key", value});
m.emplace("key", value);
```

### 차이

* `[]` → 덮어쓰기 가능
* `insert` → 중복 무시
* `emplace` → 성능 좋음

---

## 📌 map 정렬 (추가된 핵심)

### ✔ 기본 정렬 (자동)

```cpp
map<int, string> m;
```

👉 **key 기준 오름차순 자동 정렬**

```
입력: (3,c), (1,a), (2,b)
결과: (1,a), (2,b), (3,c)
```

---

### ✔ 중요한 포인트

👉 **정렬 기준은 항상 key**

* key = 정렬 기준
* value = 정렬 안 함

---

### ❌ value 기준 정렬 불가능

```cpp
map<int, string> m;
```

👉 string(value)은 정렬에 영향 없음

---

### ✔ key 내림차순

```cpp
map<int, string, greater<int>> m;
```

---

### ✔ value 기준 정렬 방법

```cpp
vector<pair<int, string>> v(m.begin(), m.end());

sort(v.begin(), v.end(), [](auto &a, auto &b) {
    return a.second < b.second;
});
```

---

## 📌 const 멤버 함수

```cpp
void printSummary() const;
```

### 의미

* 멤버 변수 수정 불가
* const 객체에서도 호출 가능

---

# 🔥 전체 핵심 요약

* erase는 iterator 기반
* remove는 삭제 아님 (밀기)
* string_view는 빠르지만 위험
* map은 key 기준 자동 정렬
* value 정렬은 vector로 변환 필요
* enum + switch = 깔끔한 분기
* const 함수 = 읽기 전용

---
# C++ map, getline, find, UTF-8 (★) 처리 정리

## 1. map은 인덱스로 접근 가능?

❌ 불가능

* `map`은 key 기반 컨테이너
* 내부는 정렬된 트리 구조

```cpp
m[0]; // index 아님 → key = 0
```

---

## 2. map 순회 방법

### range-based for

```cpp
for (const auto& p : m) {
    std::cout << p.first << " " << p.second;
}
```

### 구조 분해 (C++17)

```cpp
for (const auto& [k, v] : m) {
    std::cout << k << " " << v;
}
```

---

## 3. map에서 [] vs at vs find

| 방법       | key 없으면    | 특징    |
| -------- | ---------- | ----- |
| `[]`     | 생성됨        | 쓰기 가능 |
| `at()`   | 예외         | 안전    |
| `find()` | end()/npos | 가장 안전 |

---

## 4. const에서 [] 못 쓰는 이유

```cpp
ratings_[key]; // ❌
```

* `[]`는 없으면 삽입 → 수정 발생
* const 함수에서는 금지

✔ 해결:

```cpp
ratings_.at(key);
```

---

## 5. getline 핵심

### 기본

```cpp
std::getline(std::cin, s);
```

✔ 개행(\n)까지 읽고 **버림**

---

### cin >> 다음에 문제 발생

```cpp
std::cin >> n;
std::getline(std::cin, s); // ❌
```

✔ 해결:

```cpp
std::cin.ignore();
```

---

## 6. enum vs enum class

### enum

```cpp
enum Genre { Action, Drama };
```

### enum class

```cpp
enum class Genre { Action, Drama };
```

✔ 사용:

```cpp
Genre::Action
```

---

## 7. find() (string)

### 기본 사용

```cpp
auto pos = str.find("A");
```

* 찾으면 → index
* 못 찾으면 → `npos`

---

### 패턴

```cpp
if (pos != std::string::npos)
```

---

## 8. npos란?

```cpp
std::string::npos
```

👉 "못 찾았다"를 의미하는 값

---

## 9. starsToInt (★ 처리)

### 올바른 코드

```cpp
int starsToInt(std::string_view stars)
{
    int count = 0;
    size_t pos = 0;

    while ((pos = stars.find(u8"★", pos)) != std::string_view::npos)
    {
        count++;
        pos += 3;
    }

    return count;
}
```

---

## 10. UTF-8 주의

* `★` = 3바이트
* `char` 하나로 처리 불가

✔ 반드시:

```cpp
u8"★"
```

---

## 11. std::find vs string.find

| 함수            | 용도     |
| ------------- | ------ |
| `std::find`   | 단일 요소  |
| `string.find` | 문자열 패턴 |

---

## 🔥 핵심 요약

* map은 index 없음
* `[]`는 삽입까지 수행
* `find()`는 npos 반환
* `getline()`은 개행 제거
* UTF-8 문자는 문자열로 처리
* `while ((pos = find()) != npos)` 패턴 필수

