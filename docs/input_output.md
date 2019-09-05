# 標準入出力 (Standard Input-Output)

## 概要

競技プログラミングでは改行区切りや空白区切りで, 整数値や文字列が標準入力で与えられることが多い.

改行区切り, 空白区切りで整数値が与えられたとき, 入力をとる方法を述べる.

## 解説

例として, 100個の整数値が空白区切りおよび改行区切りで与えられたときのことを考える.

### C++

C++ でも `<cstdio>` ヘッダをインクルードすることで, C言語の scanf, printf 関数を使うことができる.

空白区切り, 改行区切りともに, 以下のようなコードで入力を取ることできる.

scanf, printf は後述する cin, cout より一般に高速に動作する.

```cpp
#include <cstdio>

int main() {
  int a[100];

  for (int i = 0; i < 100; i++) {
    // 入力を取る
    scanf("%d", &a[i]);
  }

  for (int i = 0; i < 100; i++) {
    // 出力する
    printf("%d\n", a[i]);
  }

  return 0;
}
```

C++ では標準ライブラリである `<iostream>` をインクルードすることで, `std::cin`, `std::cout` といった機能を使用できる.

`std::cin`, `std::cout` は関数ではなく, クラスのオブジェクトであり, `>>` や `<<` と組み合わせて使うことで,
入出力を簡単に行える.

`cin`, `cout` は `scanf`, `printf` よりも速度は遅いが, 型を指定する必要がなく楽なため,
競技プログラミングではよく使われる.

また, 以下のコードを冒頭に書くことによって `cin`, `cout` を高速化することできる.

```cpp
cin.tie(0);
ios::sync_with_stdio(false);
```

これを書くことで, どの程度速度が向上するかは以下の記事によくまとめられている.

- [http://tatanaideyo.hatenablog.com/entry/2014/10/24/214714](http://tatanaideyo.hatenablog.com/entry/2014/10/24/214714)

```cpp
#include <iostream>

using namespace std;

int main() {
  cin.tie(0);
  ios::sync_with_stdio(false);

  int a[100];

  for (int i = 0; i < 100; i++) {
    // 入力をとる
    cin >> a[i];
  }

  for (int i = 0; i < 100; i++) {
    // 出力をする
    cout << a[i] << endl;
  }

  return 0;
}
```

### Python3

Python3 では入力は `input()` 関数を用いる.
この関数は入力から 1 行を読み込み, 文字列に変換したものを返す.

一つの整数が与えられるとするならば, 以下のように int に parse すればいい.

```python
a = int(input())
```

複数のデータが与えられたとき, 空白区切りのときは以下のように, split と map を用いるか,

```python
a = list( map(int, input().split()) )
```

内包表記を使って, 文字列から数値に変換する.

```pyhton
a = [ int(x) for x in input().split() ]
```

改行区切りで与えられたときは, 以下のように `append` してもいいが,

```python
a = []
for _ in range(100):
    a.append(int(input()))
```

以下のように内包表記を使うと簡潔に書ける.

```python
a = [ int(input()) for _ in range(100) ]
```

### Go

Go で標準入力をとるとき, 一番楽な方法は `fmt.Scan` を使うことである.
しかし, `fmt.Scan` はとても遅く, `fmt.Scan` を使うと下記の例のようにアルゴリズム的に正しい解法でも
入力がボトルネックとなり TLE となるケースも存在する.

- [https://codeforces.com/contest/1203/submission/59936305](https://codeforces.com/contest/1203/submission/59936305)

そこで以下のように, `bufio` の `Scanner` を用いて高速に読み込み, parse して値を返す関数を作成する.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

var sc = bufio.NewScanner(os.Stdin)

func nextInt() int {
	sc.Scan()
	i, e := strconv.Atoi(sc.Text())
	if e != nil {
		panic(e)
	}
	return i
}

func nextInt64() int64 {
	sc.Scan()
	i, e := strconv.ParseInt(sc.Text(), 10, 64)
	if e != nil {
		panic(e)
	}
	return i
}

func main() {
	sc.Split(bufio.ScanWords)
	var a []int = make([]int, 100)
	for i := 0; i < 100; i++ {
		a[i] = nextInt()
	}
	for i := 0; i < 100; i++ {
		fmt.Println(a[i])
	}
}
```

!!! warning
    `bufio.Scanner` は1行あたり `bufio.MaxScanTokenSize` 分だけしか読み込めず, それ以降は無視される.
    `MaxScanTokenSize` はデフォルトで 64 * 1024 という値である.
    以下のように `Buffer` を使うと, `Scan` で使用される初期 buffer の最大サイズを指定できる.
    buffer の最大サイズは int に収まる範囲である程度大きい 1e9 程度にしておけばいいだろう.

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

var sc = bufio.NewScanner(os.Stdin)

func nextInt() int {
	i, e := strconv.Atoi(nextString())
	if e != nil {
		panic(e)
	}
	return i
}

func nextInt64() int64 {
	i, e := strconv.ParseInt(nextString(), 10, 64)
	if e != nil {
		panic(e)
	}
	return i
}

func nextString() string {
	sc.Scan()
	return sc.Text()
}

func main() {
	sc.Buffer(make([]byte, 0), 1000000009)
	sc.Split(bufio.ScanWords)
	//
}
```

これを使うと `fmt.Scan()` で TLE だった先程の問題も時間以内解くことができる.

- [https://codeforces.com/contest/1203/submission/60132736](https://codeforces.com/contest/1203/submission/60132736)

## 使用例

### 空白区切りで入力が与えられたとき

- [https://atcoder.jp/contests/abc105/tasks/abc105_d](https://atcoder.jp/contests/abc105/tasks/abc105_d)
    - [https://atcoder.jp/contests/abc105/submissions/7339261](https://atcoder.jp/contests/abc105/submissions/7339261) (C++)
    - [https://atcoder.jp/contests/abc105/submissions/7339147](https://atcoder.jp/contests/abc105/submissions/7339147) (Python3)
    - [https://atcoder.jp/contests/abc105/submissions/7339839](https://atcoder.jp/contests/abc105/submissions/7339839) (Go)

### 改行区切りで入力が与えられたとき

- [https://atcoder.jp/contests/abc063/tasks/arc075_b](https://atcoder.jp/contests/abc063/tasks/arc075_b)
    - [https://atcoder.jp/contests/abc063/submissions/7340489](https://atcoder.jp/contests/abc063/submissions/7340489) (C++)
    - [https://atcoder.jp/contests/abc063/submissions/7340570](https://atcoder.jp/contests/abc063/submissions/7340570) (Python3)
    - [https://atcoder.jp/contests/abc063/submissions/7340805](https://atcoder.jp/contests/abc063/submissions/7340805) (Go)

## 参考文献

- [http://tatanaideyo.hatenablog.com/entry/2014/10/24/214714](http://tatanaideyo.hatenablog.com/entry/2014/10/24/214714)
- [https://qiita.com/tnoda_/items/b503a72eac82862d30c6](https://qiita.com/tnoda_/items/b503a72eac82862d30c6)
- [https://golang.org/pkg/bufio/](https://golang.org/pkg/bufio/)

