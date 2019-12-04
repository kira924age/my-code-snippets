# 素因数分解 (Prime Factorization)

## 概要

正の整数 n が与えられたとき, $p_k$ を素数として, 以下のように素因数分解することを考える.

$n=p_1^{e_1} \times p_2^{e_2} \times \cdot \cdot \cdot \times p_n^{e_n}$ 

このとき, $(p_k, e_k)$ を組とした情報を返すような関数を用意すると便利である.

これは`試し割り法`という単純なアルゴリズムにより時間計算量 $O(\sqrt{n})$ で実現できる.

## 実装例

### C++ (連想配列)

```cpp
#include <map>

typedef long long ll;

std::map<ll, int> prime_factor(ll n) {
  std::map<ll, int> ret;
  for (ll i = 2; i*i <= n; i++) {
    while (n % i == 0) {
      ret[i]++;
      n /= i;
    }
  }
  if (n != 1) { ret[n]++; }
  return ret;
}
```

### C++ (可変長配列)

```cpp
#include <vector>

typedef long long ll;

std::vector<std::pair<ll, int> > prime_factor(ll n) {
  std::vector<std::pair<ll, int> > ret;

  for (ll i = 2; i*i <= n; i++) {
    if (n % i != 0) { continue; }
    int exp_v = 0;
    while (n % i == 0) {
      exp_v++;
      n /= i;
    }
    ret.push_back( std::make_pair(i, exp_v) );
  }

  if (n != 1) {
    ret.push_back( std::make_pair(n, 1) );
  }

  return ret;
}
```

### Python3 (連想配列)

```python
def prime_factor(n):
    ret = {}
    p = 2

    while p*p <= n:
        while n % p == 0:
            ret[p] = ret.get(p, 0) + 1
            n //= p
        p += 1

    if n != 1: ret[n] = 1

    return ret
```

### Python3 (可変長配列)

```python
def prime_factor(n):
    ret, p = [], 2

    while p*p <= n:
        if n % p != 0:
            p += 1
            continue

        exp_v = 0
        while n%p == 0:
            exp_v += 1
            n //= p
        ret.append( [p, exp_v] )

        p += 1

    if n != 1: ret.append( [n, 1] )

    return ret
```

### Go (連想配列)

```go
func prime_factor(n int64) map[int64]int {
  ret := map[int64]int{}

  for i := int64(2); i*i <= n; i++ {
    for n%i == 0 {
      ret[i]++
      n /= i
    }
  }
  if n != 1 {
    ret[n]++
  }
  return ret
}
```

### Go (可変長配列)

```go
type Pair struct {
  First  int64
  Second int
}

func prime_factor(n int64) []Pair {
  ret := make([]Pair, 0, 0)
  var num int

  for i := int64(2); i*i <= n; i++ {
    if n%i != 0 {
      continue
    }
    num = 0
    for n%i == 0 {
      num++
      n /= i
    }
    ret = append(ret, Pair{First: i, Second: num})
  }
  if n != 1 {
    ret = append(ret, Pair{First: n, Second: 1})
  }
  return ret
}
```

## 解説

2から $\sqrt{n}$ までの数字を先頭から見ていき, n を割り切るならば割るという単純な操作 (`試し割り法`) をしている.

素数以外で割ってしまうのでは?という懸念が一瞬頭によぎるかもしれないが, 小さい方から貪欲に割れるまで割るという操作をすると,
合成数で割り切れるという自体にはなり得ない.

割り算の回数が $\log{n}$ 回程度で, $\sqrt{n}$ 回の素数の候補で割るので, 合計で $O(\log{n} + \sqrt{n}) = O(\sqrt{n})$ となる.

## 使用例

- [https://atcoder.jp/contests/abc142/tasks/abc142_d](https://atcoder.jp/contests/abc142/tasks/abc142_d)
    - [https://atcoder.jp/contests/abc142/submissions/8784354](https://atcoder.jp/contests/abc142/submissions/8784354) (C++ 連想配列)
    - [https://atcoder.jp/contests/abc142/submissions/8784445](https://atcoder.jp/contests/abc142/submissions/8784445) (C++ 可変長配列)
    - [https://atcoder.jp/contests/abc142/submissions/8684633](https://atcoder.jp/contests/abc142/submissions/8684633) (Python3 連想配列)
    - [https://atcoder.jp/contests/abc142/submissions/8785077](https://atcoder.jp/contests/abc142/submissions/8785077) (Python3 可変長配列)
    - [https://atcoder.jp/contests/abc142/submissions/8684946](https://atcoder.jp/contests/abc142/submissions/8684946) (Go 連想配列)
    - [https://atcoder.jp/contests/abc142/submissions/8685177](https://atcoder.jp/contests/abc142/submissions/8685177) (Go 可変長配列)

- [http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=NTL_1_A](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=NTL_1_A&lang=jp)
    - [http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022696](http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022696) (C++ 連想配列)
    - [http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022728](http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022728) (C++ 可変長配列)
    - [http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022742](http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022742) (Python3 連想配列)
    - [http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022763](http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022763) (Python3 可変長配列)
    - [http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022897](http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022897) (Go 連想配列)
    - [http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022920](http://judge.u-aizu.ac.jp/onlinejudge/review.jsp?rid=4022920) (Go 可変長配列)

## 関連する問題

- [https://atcoder.jp/contests/abc052/tasks/arc067_a](https://atcoder.jp/contests/abc052/tasks/arc067_a)
- [https://atcoder.jp/contests/abc110/tasks/abc110_d](https://atcoder.jp/contests/abc110/tasks/abc110_d)

## 参考文献

* 蟻本

