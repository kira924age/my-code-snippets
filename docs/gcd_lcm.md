# 最大公約数・最小公倍数 (Euclidean Algorithm)

## 概要

ユークリッドの互除法 (Euclidean Algorithm) により
2つの自然数 x, y の最大公約数 gcd(x, y), 最大公約数 lcm(x, y) を求める. x, y の最大公約数が求まれば最小公倍数は lcm(x, y) = x / gcd(x, y) * y で求めることができる.

-  計算量: $ O(\log(min(x, y))) $

## 実装例

### C/C++ (再帰関数)

```cpp
typedef long long LL;

LL gcd(LL x, LL y) {
  return y ? gcd(y, x % y) : x;
}

LL lcm(LL x, LL y) {
  return x/gcd(x, y)*y;
}
```

### C/C++ (非再帰)

```cpp
typedef long long LL;

LL gcd(LL x, LL y) {
  while (y != 0) {
    LL r = x % y;
    x = y;
    y = r;
}
  return x;
}

LL lcm(LL x, LL y) {
  return x/gcd(x, y)*y;
}
```

### Python3 (再帰関数)

```
def gcd(x, y):
    return x if y == 0 else gcd(y, x % y)

def lcm(x, y):
    return x/gcd(x, y)*y
```

### Python3 (非再帰)

```python
def gcd(x, y):
    while y != 0:
        x, y = y, x % y
    return x

def lcm(x, y):
    return x/gcd(x, y)*y
```

## 解説

ユークリッドの互除法は2つの自然数 x, y について, gcd(x, y) が gcd(y, x % y) に等しいことを利用し, 第二引数が 0 になるまで操作を繰り返すことで最大公約数を求めている.

gcd(x, y) が gcd(y, x % y) と等しいことを示してみる.

d = gcd(y, x % y) とする.
このとき d は y および x % y の公約数である.

適当な整数を q とすると

x = y * q + x % y で, y * q および x % y の公約数は d であるから d は x の公約数でもある.

したがって, gcd(y, x % y) の最大公約数が d なら gcd(x, y) の最大公約数も d となる.

次にその逆を示す.

すなわち, d = gcd(x, y) としたとき
d = gcd(y, x % y) を示す.

適当な整数を q とすると

x = y * q + x % y

が成り立つ.

このとき, d (= gcd(x, y)) は x, y を割り切るので x % y = x - y * q も割り切る.

以上のことから gcd(x, y) = gcd(y, x % y) であることが示された.

- 図形的な捉え方

最大公約数は幅, 高さがa, b の長方形を埋め尽くすことができるような正方形のなかで最大のものの一辺の長さに等しい.





## 使用例

- 

## 参考文献

- 蟻本

- [https://ja.wikipedia.org/wiki/ユークリッドの互除法](https://ja.wikipedia.org/wiki/%E3%83%A6%E3%83%BC%E3%82%AF%E3%83%AA%E3%83%83%E3%83%89%E3%81%AE%E4%BA%92%E9%99%A4%E6%B3%95)

