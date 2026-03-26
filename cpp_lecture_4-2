# 📘 C++ 문자열 & STL 핵심 정리 (대화 요약)

---

## 1. vector 초기 상태

* `std::vector<int> v;`

  * ❌ null 아님
  * ⭕ **빈 컨테이너 (size = 0)**

---

## 2. map[key] 동작

```cpp
m[key] = value;
```

* key 없으면 자동 생성됨
* `m[key]` 접근만 해도 생성됨

⚠️ 주의:

```cpp
m["apple"] == 0 // ❌ 생성됨
```

✔ 안전:

```cpp
m.find("apple") != m.end()
```

---

## 3. std::copy / copy_n

### copy

```cpp
copy(first, last, result);
```

* 범위 복사

### copy_n

```cpp
copy_n(first, n, result);
```

* n개 복사

⚠️ vector 주의:

```cpp
values.reserve(n); // ❌ size 0
values.resize(n);  // ⭕ 사용 가능
```

또는:

```cpp
copy_n(it, n, back_inserter(values));
```

---

## 4. istream_iterator

```cpp
istream_iterator<int>(cin)
```

* 입력을 iterator처럼 사용
* `*it` 할 때마다 입력 읽음

---

## 5. string은 mutable?

* ⭕ 수정 가능

```cpp
s[0] = 'H';
```

---

## 6. string 주요 함수

### insert

```cpp
s.insert(pos, "text");
```

* 끼워 넣기

### replace

```cpp
s.replace(pos, len, "text");
```

* 삭제 후 삽입

---

## 7. 문자 치환

### 방법 1

```cpp
for (char& c : s)
```

### 방법 2

```cpp
replace(s.begin(), s.end(), '_', '-');
```

---

## 8. \r / \n 개념

| 문자   | 의미        |
| ---- | --------- |
| `\r` | 커서를 맨 앞으로 |
| `\n` | 다음 줄      |

### \r\n

* Windows 줄바꿈
* 실제로 **2개 문자**

---

## 9. EOF

* C++에서도 사용 가능
* 하지만 보통:

```cpp
while (cin >> x)
```

사용

---

## 10. string_view 핵심

* `'\0'` 없음 ❗
* 끝은 **size()로 판단**

```cpp
i + 1 < text.size()
```

---

## 11. 문자열 처리 핵심 패턴

### ❌ 잘못된 생각

* `text[i+1]` 그냥 사용
* `'\0'` 있다고 가정

### ⭕ 올바른 방식

```cpp
if (i + 1 < text.size())
```

---

## 12. normalize_newlines 핵심

목표:

* `\r\n` → `\n`
* `\r` → `\n`

### 정답 패턴

```cpp
if (c == '\r') {
    if (i + 1 < size && next == '\n') {
        push '\n';
        skip next
    } else {
        push '\n';
    }
}
```

🔥 핵심:

* **\r을 보는 순간 처리**

---

## 13. push_back vs 문자열 (추가 정리 🔥)

### ✔ 핵심 개념

👉 `push_back`은 **딱 1개의 문자(char)**만 넣을 수 있음

* char = 보통 **1바이트**
* 즉, **한 글자만 가능**

---

### ✔ 올바른 사용

```cpp
result.push_back('A');   // ⭕ 문자 1개
result.push_back('\n');  // ⭕ 개행 문자
```

---

### ❌ 잘못된 사용

```cpp
result.push_back("A");    // ❌ 문자열
result.push_back("[SP]"); // ❌ 문자열
```

👉 `"..."` 는 **const char*** (문자열 포인터)

---

### ✔ 문자열 넣는 방법

```cpp
result += "[SP]";
result.append("[SP]");
```

---

### 🔥 정리

| 함수          | 넣을 수 있는 것      |
| ----------- | -------------- |
| `push_back` | 문자 1개 (`char`) |
| `+=`        | 문자열            |
| `append`    | 문자열            |

---

## 14. 모던 C++ 스타일

### 특징

* range-based for
* switch 사용
* 한 번에 처리 (1-pass)

---

### 예시

```cpp
for (char c : text) {
    switch (c) {
        case ' ': result += "[SP]"; break;
        default: result.push_back(c);
    }
}
```

---

## 🔥 최종 핵심 요약

* vector: reserve vs resize 구분
* map[]: 자동 생성됨
* copy: 3개 인자 필요
* string_view: size 기반
* \r\n 처리: 반드시 **다음 문자 체크**
* push_back: **문자 1개만 가능**
* 문자열 처리: **한 번에 처리하는 게 정석**

---

## 💡 한 줄 정리

👉 C++ 문자열/컨테이너 문제의 핵심은
**"범위 안전 + 의도 명확 + 한 번에 처리"**
