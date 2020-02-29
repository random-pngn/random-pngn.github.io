---
title: '第1回競プロ勉強会'
subtitle: '全探索'
summary: '全探索'
math: true
diagram: false
authors:
- pngn
tags:
- '競技プログラミング'
- '競プロ勉強会'
 # 作品紹介系テンプレ: 作品紹介, ゲーム, 音楽, グラフィック
 # ブログ系テンプレ: 競技プログラミング, ゲーム制作, DTM, LT会
date: "2020-02-05T00:00:00Z"
lastmod: "2020-02-05T00:00:00Z"
featured: false # ???
draft: false # true にすると非表示になる

# [必須] 同一フォルダにfeatured.jpg/pngを入れて! 絶対!

projects: [] # project関係あればその名前を入れる

# 本文の書き方はこれを参照して(https://sourcethemes.com/academic/docs/writing-markdown-latex/)
---
# はじめに
競プロ勉強会第1回です。今回は探索を中心にやっていこうと思います。基本的に今回扱うのは全通りのパターンを列挙して調べる全探索のみ扱います。  

> 目次  
>
> (水色向け)  
>
> 1. 再帰  
> 2. DFS  
> 3. BFS  
> 4. next_permutation  
> 5. bit全探索  
> 6. 半分全列挙  
>
> (青色向け)  
>
> 7. 枝刈り  
> 8. IDA\*  
> 9. A\*  

# 1. 再帰
これは探索とは直接関係無いのですが、皆さん再帰書けますか?再帰とはある概念をその概念自信で表す事です。  
例) フィボナッチ数列のn番目を求める再帰
```
int fib(int n) {
  if (n == 1 || n == 2) return 1;
  return fib(n-1) + fib(n-2);
}
```
このコードは非常に効率が悪いですが、その話はまたいずれ。  
このコードには副作用が含まれないのできれいですが、競プロでは副作用のある関数を書きがちになります。注意しましょう。  

# 2. DFS
深さ優先探索としてよく知られていますね。DFSは木やグラフを探索するアルゴリズムで、とりあえず根から下まで辿って、戻りながらまだ行って無い点があれば行くを繰り返す手法です。グラフと言いましたが、直接的なグラフじゃない一般の場合でも暗に状態がノードのグラフとみなす事で対応できます。  
突然難しい単語が出てきて頭がこんがらがっているかもしれないですが、てきとうに再帰を書くとそれがそのままDFSになります。  
DFSは探索順序が重要で無い場合に使える事が多いです。  

[練習問題](https://atcoder.jp/contests/abc114/tasks/abc114_c)
解答例

# 3. BFS
幅優先探索です。これはDFSとは探索順序が異なり、根から近い順に探索するアルゴリズムです。例えば迷路を解くプログラムで使われます。  

実装方法はおおまかに  

```
queue X
Xに初期値を追加
while (!(Xが空)) {
  temp = Xの先頭要素
  Xの先頭要素の削除
  tempを元に探索し、次の探索位置をXに追加
}
```

でいけます。queueは最初に入れた値が最初に出てくるデータ構造です。恐らくこれだと分からないでしょうが、実際に練習問題を解いてみると理解できるでしょう。  
queueをstackにするとdfsに、priority_queueにするとダイクストラ法になります。  
但し、この形だと再帰のように探索の結果をまとめ上げるのが苦手です。一長一短ですね。  

[練習問題](https://atcoder.jp/contests/abc007/tasks/abc007_3)
解答例

# 4. next_permutation
これはC++の関数で、[1, 2, ... ,n]の並び替えn!通りを列挙してくれる関数です。使い方は少々独特で、

```
//#include<algorithm>
int n = 5, d[5] = {1, 2, 3, 4, 5}; // 昇順に入れておく
do {
  // 処理
} while (next_permutation(d, d+n));

```

多分O($n!n$)

[練習問題](https://atcoder.jp/contests/abc150/tasks/abc150_c)
解答例

# 5. bit全探索
例えばn<=20くらいで、n個の商品を買うor買わないの$2^n$通り調べる必要がある時、i番目の商品をi番目のビットで管理する方法です。i番目のビットが1なら買う、0なら買わないみたいに管理します。  

例) n<=20でn個の商品に値段$d_i$がある。取りうる値段を全て列挙せよ。(重複している場合は重複している数だけ列挙する)  

```
int n = 5, d[5] = {100, 20, 1000, 2, 30}; // dは商品の値段
for (int i = 0; i < (1<<n); i++) {
  int sum = 0;
  for (int j = 0; j < n; j++) {
    if (i & (1 << j)) {
      // 買う
      sum += d[j];
    }
  }
  printf("%d\n", sum);
}
```

O($2^nn$)です。  
そもそもbit演算に慣れていない人も多いと思います。簡単にまとめると、  

| 演算   | 説明                |
| ------ | ------------------- |
| a << b | aを左にbだけずらす  |
| a >> b | aを右にbだけずらす  |
| &      | bit積               |
| \|     | bit和               |
| ^      | xor                 |

です。詳細は検索して  

上のコードはbitを使わずdfsを用いて書く事もできます。  

```
int n = 5, d[5] = {100, 20, 1000, 2, 30}; // dは商品の値段
void dfs(int num, int sum) {
  if (num == n) {
    printf("%d\n", sum);
    return;
  }
  dfs(num+1, sum);
  dfs(num+1, sum + d[num]);
}
// dfs(0, 0)で呼び出し
```

dfsでも書けるようになった方が良いでしょう。上のコードは副作用を含むのであまりきれいでは無いですが...  

[練習問題](https://atcoder.jp/contests/abc128/tasks/abc128_c)
解答例

# 6. 半分全列挙
n<=40くらいで、全探索するとO($2^n$)かかるけど、半分に分けてソートし、二分探索と組み合わせる事でO($2^{\frac{n}{2}}n$)に抑える方法。稀によく出る。  

例) n<=40 でn個の商品があり、値段$a_i$が与えられる。値段の和がちょうどkとなる選び方は何通り?  

```
int n, a[40], k;
int half = n/2, rest = n - half;
vector<int> d;

// 前半2^half通り全列挙
for (int i = 0; i < (1<<half); i++) {
  int sum = 0;
  for (int j = 0; j < half; j++) {
    if (i & (1 << j)) {
      sum += a[j];
    }
  }
  d.push_back(sum);
}
sort(d.begin(), d.end());

int ans = 0;
// 後半2^rest通り全列挙
for (int i = 0; i < (1<<rest); i++) {
  int sum = 0;
  for (int j = 0; j < rest; j++) {
    if (i & (1 << j)) {
      sum += a[half + j];
    }
  }
  ans += upper_bound(d.begin(), d.end(), k - sum) - lower_bound(d.begin(), d.end(), k - sum);
  // 少々テクニカル
  // lower_bound, upper_boundは第三引数以上/より大きい最初の要素のポインタorイテレータを返す関数
  // ポインタやイテレータの引き算はその間の要素数になる
  // vectorのイテレータの引き算はO(1)だが、mapやsetとかのイテレータの引き算はO(n)だった気がするので注意
}

printf("%lld\n", ans);
```

半分を全列挙するのにbit全探索を用いるときれいに書ける事が多いです。  

[練習問題](https://atcoder.jp/contests/arc017/tasks/arc017_3)
解答例

## 水色向け練習問題
[練習問題1](https://atcoder.jp/contests/abc088/tasks/abc088_d)  
[練習問題2](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DPL_1_H&lang=jp)  
[練習問題3](https://atcoder.jp/contests/agc033/tasks/agc033_a)  
[練習問題4](https://atcoder.jp/contests/abc054/tasks/abc054_c)  

# 7. 枝刈り
枝刈りとは再帰関数で最適解となりえないと判断するとそこで探索を打ち切る事で高速化する手法です。アリ本では数独の問題が載っていました。計算量の解析が難しく、AtCoderの問題であまり見ない気がしますね。僕が解けない難しい問題だとあるのかも...何にせよ、マラソンなどで使えるテクニックな気がしますね。

# 8. 分枝限定法
(工事中)  
