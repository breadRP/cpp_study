# C++ STL sort 정리

## 1. 기본 개념

`sort`는 C++ STL `<algorithm>`에 있는 정렬 함수이다.

기본 형태

```
sort(begin, end);
```

* `[begin, end)` 범위를 정렬
* 기본 정렬 기준: **오름차순 (`<`)**
* 시간복잡도: **O(N log N)**

---

# 2. 기본 사용 예

```
#include <vector>
#include <algorithm>

vector<int> v = {5,2,4,1};

sort(v.begin(), v.end());
```

결과

```
1 2 4 5
```

---

# 3. 내림차순 정렬

방법 1: comparator 사용

```
bool cmp(int a, int b)
{
    return a > b;
}

sort(v.begin(), v.end(), cmp);
```

결과

```
5 4 2 1
```

방법 2: reverse iterator

```
sort(v.rbegin(), v.rend());
```

---

# 4. comparator (커스텀 정렬 기준)

형태

```
bool cmp(A a, A b)
```

의미

```
true  → a가 b보다 앞에 온다
false → b가 a보다 앞에 온다
```

즉 comparator는 항상

```
a가 먼저 와야 하는 조건
```

을 return 하면 된다.

핵심 공식

```
return "a가 앞에 올 조건"
```

---

# 5. comparator 예제

오름차순

```
bool cmp(int a, int b)
{
    return a < b;
}
```

내림차순

```
bool cmp(int a, int b)
{
    return a > b;
}
```

문자열 길이순

```
bool cmp(string a, string b)
{
    return a.size() < b.size();
}
```

---

# 6. 여러 조건 정렬

예:
1️⃣ 길이 짧은 것
2️⃣ 길이 같으면 사전순

```
bool cmp(const string& a, const string& b)
{
    if (a.size() == b.size())
        return a < b;

    return a.size() < b.size();
}
```

사용

```
sort(v.begin(), v.end(), cmp);
```

---

# 7. sort 내부 동작

sort는 정렬 과정에서 계속 comparator를 호출한다.

예

```
cmp(a,b)
cmp(a,c)
cmp(b,c)
...
```

하지만 모든 쌍을 비교하는 것은 아니다.

비교 횟수

```
O(N log N)
```

---

# 8. string 정렬

문자열 내부 문자 정렬

```
string s = "dcba";

sort(s.begin(), s.end());
```

결과

```
abcd
```

문자열 벡터 정렬

```
vector<string> v = {"banana","apple","cat"};

sort(v.begin(), v.end());
```

결과

```
apple
banana
cat
```

---

# 9. STL 범위 생성자

컨테이너 복사

```
vector<string> v(s.begin(), s.end());
```

의미

```
s의 모든 원소를 vector v에 복사
```

동일 코드

```
vector<string> v;

for(auto it = s.begin(); it != s.end(); it++)
{
    v.push_back(*it);
}
```

---

# 10. 람다 comparator (현대 C++)

```
sort(v.begin(), v.end(), [](string a, string b)
{
    return a.size() < b.size();
});
```

---

# 핵심 정리

기본 sort

```
sort(begin, end);
```

커스텀 정렬

```
sort(begin, end, cmp);
```

comparator 의미

```
cmp(a,b) → a가 앞이면 true
```

시간복잡도

```
O(N log N)
```
