# C++ Ranges & Modern Syntax Cheat Sheet (요약)

오늘 질의응답에서 나온 핵심 문법들을 **파라미터 구조 중심으로 한 번에 정리**

---

## 1. std::views::iota

- 형태: `std::views::iota(start, end)`
- start부터 end 전까지 값을 lazy하게 생성하는 range
- 컨테이너를 만들지 않음

```cpp
for (auto i : std::views::iota(0uz, n)) {
    // 0 ~ n-1 (size_t 타입)
}
```

---

## 2. std::views::join

- 형태: `range | std::views::join`
- 중첩된 range(2차원)를 1차원으로 flatten

```cpp
std::ranges::copy(init | std::views::join, data);
```

---

## 3. std::ranges::copy

- 형태: `std::ranges::copy(input_range, output_iterator)`
- range의 내용을 지정된 위치에 복사
- destination은 포인터도 가능

```cpp
std::ranges::copy(v, out.begin());
std::ranges::copy(span, data);
```

---

## 4. std::ranges::transform

- unary: `std::ranges::transform(range, out, func)`
- binary: `std::ranges::transform(range1, range2, out, func)`
- element-wise 연산 수행

```cpp
std::ranges::transform(a, b, out.begin(), std::plus<>());
```

---

## 5. std::views::zip / zip_transform (C++23)

- zip: `std::views::zip(range1, range2)`
  → (a[i], b[i]) 형태로 묶음

- zip_transform: `std::views::zip_transform(func, range1, range2)`
  → 묶고 바로 함수 적용 (lazy)

```cpp
auto r = std::views::zip_transform(std::multiplies<>(), a, b);
```

---

## 6. std::plus<> / std::multiplies<>

- `std::plus<>()(a, b)` → a + b  
- `std::multiplies<>()(a, b)` → a * b  
- 내부적으로 operator+ / operator* 호출

```cpp
std::ranges::transform(a, b, out.begin(), std::plus<>());
```

---

## 7. std::span

- 형태: `std::span<T>(pointer, size)`
- 연속 메모리를 복사 없이 참조하는 view
- 내부 구조: (포인터 + 길이)

```cpp
std::span<int> s(data, rows * cols);

s[i];       // 원소 접근
s.size();   // 전체 원소 개수
s.data();   // 포인터
```

---

## 8. as_span()

```cpp
std::span<const int> as_span() const noexcept {
    return {data, rows * cols};
}
```

- 내부 배열을 span으로 감싸 반환
- `{ptr, size}`는 span 생성 문법

---

## 9. noexcept

- 형태: `void f() noexcept;`
- 예외를 던지지 않음을 보장
- 위반 시 `std::terminate()`

---

## 10. Rule of 5

```cpp
~T();
T(const T&);
T& operator=(const T&);
T(T&&) noexcept;
T& operator=(T&&) noexcept;
```

- 자원 관리 클래스에서 필수

---

## 11. self-assignment 체크

```cpp
if (this == &other) return *this;
```

- 자기 자신 대입 시 메모리 파괴 방지

---

## 12. std::ranges::equal

- 형태: `std::ranges::equal(range1, range2)`
- 두 range의 모든 원소가 같은지 비교

---

## 13. 0uz

- `0uz` = size_t 타입의 0
- iota 등에서 타입 맞추기용

---

## 🔥 핵심 요약

- `views` → 데이터 없이 “보는 방식”만 정의 (lazy)
- `ranges` → 실제 알고리즘 수행
- `span` → 포인터 + 길이 기반 안전한 배열 view
- `transform` → element-wise 연산 핵심
- `zip` → 여러 range를 인덱스 기준으로 묶기
