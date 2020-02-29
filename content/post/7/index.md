---
title: '第2回競プロ勉強会'
subtitle: '数学的な問題'
summary: '数学的な問題'
math: true
diagram: false
authors:
- pngn
tags:
- '競技プログラミング'
- '競プロ勉強会'
 # 作品紹介系テンプレ: 作品紹介, ゲーム, 音楽, グラフィック
 # ブログ系テンプレ: 競技プログラミング, ゲーム制作, DTM, LT会
date: "2020-02-12T00:00:00Z"
lastmod: "2020-02-12T00:00:00Z"
featured: false # ???
draft: false # true にすると非表示になる

# [必須] 同一フォルダにfeatured.jpg/pngを入れて! 絶対!

projects: [] # project関係あればその名前を入れる

# 本文の書き方はこれを参照して(https://sourcethemes.com/academic/docs/writing-markdown-latex/)
---
# はじめに
競プロ勉強会第2回です。今回は数学的な話を中心にします。

> 目次  
>
> (水色向け)  
>
> 1. 逆元  
> 2. Combination  
> 3. 素数  
> 4. gcd  
> 5. 繰り返し二乗法
>
> (青色向け)  
>
> 6. 包除原理  
> 7. FFT  
> 8. 形式的冪級数  
> 9. 行列  
> 10. 対称性のある数え上げ  

# 1. 逆元  
xを整数、pを素数とすると、xの逆元yは$y \equiv x^{-1} (mod p)$と表す事ができ、$xy \equiv 1 (mod p)$を満たす整数です。これはyの取りうる値を0以上p-1以下とするとただ一つに定まります。  
(証明)a, bを0以上p-1以下とすると、$ax \equiv bx (mod p)$を満たす時$a = b$となるため。  
これ単体で聞かれる事はあまり無いですが、確率をmod pで整数で答えよ、や次のCombinationで用います。  
逆元を求める方法はフェルマーの小定理+繰り返し二乗法を用いる方法か拡張ユークリッドの互除法を用いる方法があります。  
フェルマーの小定理: 素数pとpと互いに素な整数aに対して$a^{p-1} \equiv 1$が成り立つ。  
(証明)$a^0$, $a^1$, ..., $a^{p-2}$はpで割ったあまりは全て異なる。  
同様に$a^1$, $a^2$, ..., $a^{p-1}$はpで割ったあまりは全て異なる。  
よって$a^0 \equiv a^{p-1}$

# 2. Combination  
みんな大好きCombinationです。nCkです。$\binom{n}{k}$と書きます。  
$\binom{n}{k} = \frac{n!}{k!(n-k)!}$ですね。これは横にk個、縦にn-k個の辺があるグリッドグラフで左下の点から右上の点に行く最短経路の総数ですね。  

$n!, (n!)^{-1} (mod p)$を前計算することでO(1)で求められます。  
ここで使える有用なテクニックなのですが、  
p = (p/n) * n + p%n  
(p/n) * n $\equiv$ -p%n (mod p)  
$n^{-1} \equiv$ -(p%n)$^{-1}$ * (p/n) (mod p)  
つまり、n!の逆元とp%(n+1)の逆元が求まれば(n+1)!の逆元が求まるわけです。p%(n+1)はn以下の整数なので、下から順番に記録しておくことで求められます。よって前計算はO(n)です。  

<基本公式>  
 - $\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$  
   - 最初の1個を取るor取らない  
 - $k\binom{n}{k} = n\binom{n-1}{k-1}$  
   - $\frac{n!}{(n-k)!(k-1)!}$  
 - $\binom{n}{k} = \binom{n}{n-k}$  
   - 念の為  

<公式>  
 - $\sum_{i=0}^{n} \binom{n}{i} = 2^n$
   - $(1 + 1)^n$の展開
 - $\sum_{i=0}^{n/2} \binom{n}{2i} = 2^{n-1}$
   - 基本公式1を使ってΣを展開
 - $\sum_{i=0}^{k} \binom{n+i}{i} = \binom{n+k+1}{k}$
   - 横k縦nのグリッドグラフの全ての上の部分への行き方の総和
   - 横k縦n+1のグリッドグラフの右上への行き方と等しい
   - 一番上の縦の辺は必ず1箇所通るから
 - $\sum_{i=0}^{k} \sum_{j=0}^{l} \binom{i+j}{i} = \binom{k+l+2}{k+1}$
   - 公式3を2回
 - $\sum_{i=0}^{k} \binom{n+i}{i} \binom{m-i}{k-i} = \binom{n+m+1}{k}$
   - 公式3の一般化
   - 横k縦n+m-k+1の縦をnとm-kに辺を1行あけて分割
 - $\sum_{i=0}^{k} \binom{n}{i} \binom{m}{k-i} = \binom{n+m}{k}$
   - 横k縦n+m-kを斜めに切る感じ
   - 左は下からn, 右は上からmの点を通る直線

[練習問題](https://atcoder.jp/contests/abc034/tasks/abc034_c)

# 3. 素数  
素数かどうかの判定はO($\sqrt{n}$)でできる。  

```
bool is_Prime(int n) {
  for (int i = 2; i * i <= n; i++) {
    if (n % i == 0) return false;
  }
  return true;
}
```

同様に約数列挙とかもできる。素数を前計算で求める方法は、

```
vector<int> get(int mx) {
  vector<int> prime;
  for (int i = 2; i <= mx; i++) {
    bool ok = true;
    for (int j : prime) {
      if (i % j == 0) {
        ok = false;
        break;
      }
    }
    if (ok) {
      prime.push_back(mx);
    }
  }
  return prime;
}
```

但し、これはそこそこ遅い。n<=10^6くらいで素数かどうかを判定するならエラトステネスのふるいを使うとよい。

```
bool isPrime[1000001]; // trueで初期化
void init(int mx) {
  isPrime[0] = isPrime[1] = false;
  for (int i = 2; i <= mx; i++) {
    if (isPrime[i]) {
      for (int j = 2; i * j <= mx; j++) isPrime[i * j] = false;
    }
  }
}
```

[練習問題](https://atcoder.jp/contests/abc084/tasks/abc084_d)

# 4. gcd  
普通のgcd

```
int gcd(int a, int b) {
  if (b == 0) return a;
  return gcd(b, a%b);
}
```

拡張gcd ax+by=gとなるx, yもついでに求める。  

```
int extgcd(int a, int b, int &x, int &y) {
  int d = a;
  if (b != 0) {
    d = extgcd(b, a%b, y, x);
    y -= (a/b) * x;
  } else {
    x = 1; y = 0;
  }
  return d;
}
```

# 5. 繰り返し二乗法  
$a^b$をO(log b)で解く。  

```
int pw(int a, int b, int mod) {
  if (b == 0) return 1;
  if (b % 2 == 1) return pw(a, b-1, mod) * a % mod;
  int temp = pw(a, b/2, mod);
  return temp * temp % mod;
}
```

[練習問題](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=NTL_1_B&lang=jp)

行列とかにも使える。  

## 水色向け練習問題
[練習問題1](https://atcoder.jp/contests/abc110/tasks/abc110_d)  
[練習問題2](https://atcoder.jp/contests/abc132/tasks/abc132_d)  
[練習問題3](https://atcoder.jp/contests/abc013/tasks/abc013_4)  

# 6. 包除原理  
集合の数え上げの手法。ある部分集合がn個の集合に含まれる時、$(-1)^{n+1}$をかける。具体的には集合をbitで管理して、-1の(bitが立ってる個数+1)乗する。

(工事中)  
