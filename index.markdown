# Clojure 入門

* author: ka
  * Homepage: [kaosfield.net](http://kaosfield.net)
  * Twitter: [@ka_](https://twitter.com/ka_)
  * GitHub: [kaosf](https://github.com/kaosf)
* date: 2014-02-09


## Clojure とは

JVM の上で動く言語の一つ

Lisp


## Leiningen

これから Leiningen というツールを使って  
Clojure という言語での

* プログラムを書いて実行
* プロジェクトを作成
* ライブラリを作成
* アプリケーションを動かす

などの操作を行う


## Leiningen (2)

Leiningen は他にも  
依存しているライブラリをインストールしたり  
実行可能なファイルを作成したり出来る

プラグインで機能拡張も出来る


## How to install JDK

Ubuntu

```sh
sudo aptitude install openjdk-7-jdk
```

Fedora

```sh
sudo yum install -y java-1.7.0-openjdk java-1.7.0-openjdk-devel
```

他のディストリビューションでも  
OpenJDK 7 をインストールすれば OK


## How to install JDK

Windows or Mac

Oracle のサイトから DL & Install


## How to install Leiningen

Linux or Mac

```sh
cd $HOME/local/bin
wget https://raw.github.com/technomancy/leiningen/stable/bin/lein
chmod +x lein
./lein
```

何処か PATH の通ったところに `lein` をダウンロードして  
実行権限を与えて実行


## How to install Leiningen

Windows

[leiningen-win-installer](http://leiningen-win-installer.djpowell.net/)

ここに従えば OK

JDK のインストールも必要です


## Lisp とは

世界で二番目に古いプログラミング言語

S-Expression (S 式) で記述する

S は Symbol


## ポーランド記法

前置記法とも言う

関数 (演算子も関数と考える) を前置する

我々が普段算数・数学で使うのは中置記法


## ポーランド記法の例 1

中置: `1 + 2`

前置: `(+ 1 2)`


## ポーランド記法の例 2

中置: `1 + 2 * 3`

前置: `(+ 1 (* 2 3))`

演算子の優先順位を考える必要が無い


## ポーランド記法の例 3

`f` という関数があり整数二つを引数にとるとする

数学・C 言語など: `f(1, 2)`

ポーランド記法: `(f 1 2)`


## Lisp の特徴

コードとデータが等価

マクロという強力な機能  
(いわゆる C 言語等のマクロとは出来ることのレベルが違う)


## Clojure プログラムの動かし方

REPL (Read Evaluate Print Loop) を使う

```sh
lein repl
```

で起動する

Windows の方はアプリケーションとしても用意されている


## 前置記法を実際に書く

REPL で

```clj
(+ 1 2)
```

と打ってみよう

```
3
```

が返ってくる


## 実際に書く (2)

```clj
(* 2 3)
```

これは当然

```
6
```

になる


## 実際に書く (3)

```clj
(+ (* 3 4) 5)
```

3 掛ける 4 の 12 に 5 を足して

```
17
```

となる


## ファイルに記述して実行

`a.clj` を用意して

```
(+ (+ 1 (* 2 3)) (* 4 5))
```

とでも書いておく

これは `(1 + (2 * 3)) + (4 * 5)` 相当である


## REPL から実行

```clj
(load-file "a.clj")
```

とすれば OK

```
27
```

になったはず


## 変数定義

```clj
(def x 10)
```

これで `10` という値に `x` が束縛された

```clj
x ;-> 10
```

これで `x` 自体が評価される

```clj
(+ x 20) ;-> 30
```

これは `(+ 10 20)` と同等である


## コメント

`;` から行末まではコメントである

`;-> 10` と書いてあるのは  
評価した結果が `10` になったということ

この書き方は他の言語でもよくある


## 関数定義

```clj
(defn f [x] (+ x 1))
```

これで引数 `x` に `1` を足す関数 `f` が定義された

```clj
(f 2) ;-> 3
```

先ほどの説明のようにこれで  
`f` に引数として `2` を渡すことになる


## 関数定義

```clj
(defn g [x y] (+ (* x 10) y))
```

2 引数以上の関数は上記のように定義する

```clj
(g 1 2) ;-> 12
```

これで `x` に `1`, `y` に `2` が渡される


## 無名関数

```clj
(fn [x] (+ x 1))
```

名前の無い関数


## どうやって呼び出す

もしもその関数に `f` という名前が付いていれば

```clj
(f 1)
```

で良い

ならば

```clj
((fn [x] (+ x 1)) 1) ;-> 2
```

で良い


## リスト

```clj
'(1 2 3 4)
```

これで `1` から `4` までの要素が並んだリストが表現出来る

`(` の前についているクオートは評価を抑制するためのもの

普通に `(1 2 3 4)` としてしまうと `1` という関数に  
`2 3 4` を引数として渡すと解釈される

`1` は関数ではないのでエラーになる


## first rest 関数

```
(first '(1 2 3 4)) ;-> 1
```

これでリストの先頭の `1` が取れる

```
(rest '(1 2 3 4)) ;-> (2 3 4)
```

これでリストの先頭以外の残り `(2 3 4)` が取れる

他の Lisp ではこの関数は `car` `cdr` として存在したりする


## cons 関数

```clj
(cons 1 '(2 3 4)) ;-> (1 2 3 4)
```

1 つの要素とリストを連結して新しいリストを作る

cons は construct の略


## リストの正体

```clj
(1 2 3 4)
```

というリストは

```clj
(cons 1 (cons 2 (cons 3 (cons 4 '()))))
```

という形になっている

`first` と `rest` は `(cons x y)` について

```clj
(first (cons x y)) ;-> x
(rest (cons x y)) ;-> y
```

という関係を持っている


## 直接 n 番目の要素にアクセス

`nth` という関数がある

```clj
(nth '(10 20 30 40) 2) ;-> 30
```

2 番目だけは `second` という関数もある

```clj
(second '(10 20 30 40)) ;-> 20
```


## 条件分岐

```clj
(if true 1 2) ;-> 1
```

```clj
(if false 3 4) ;-> 4
```

```clj
(if (= 1 2) 10 20) ;-> 20
```


## リスト操作の種々の関数


### map

全要素に特定の関数を適用した結果のリストを得る

```clj
(map (fn [x] (* x 2)) '(1 2 3)) ;-> (2 4 6)
```


### reduce

全要素を特定の関数で畳み込む

```clj
(reduce + 0 '(1 2 3)) ;-> 6
```

```clj
(+ (+ (+ 0 1) 2) 3)
```

と等価になる


## 余談

※実際は `+` は可変長引数関数であり `(+ 1 2 3)` で `6` になる


## reduce 例 2

```clj
(reduce (fn [x y] (+ (* x 10) y)) 0 '(1 2 3))
```

これは

```clj
(defn f [x y] (+ (* x 10) y))
(f (f (f 0 1) 2) 3)
```

と等価なので `123` となる


## filter

全要素を特定の関数でフィルタリングして  
残ったもののみで構成されたリストを得る

```clj
(filter (fn [x] (>= x 5)) '(1 2 3 4 5 6 7 8 9)) ;-> (5 6 7 8 9)
```


## 無限リスト

遅延評価の仕組みのおかげで無限長のリストを扱える

```clj
(repeat 1) ;-> (1 1 1 1 1 1 1 ...)
```

このままでは REPL が応答しなくなる (Ctrl + C で中断可能)


### repeat

与えられた引数を無限に要素に持つリストを返す

```clj
(repeat 10) ;-> (10 10 10 ...)
```


### cycle

与えられた引数のリストを繰り返す無限長リストを返す

```clj
(cycle '(1 2 3)) ;-> (1 2 3 1 2 3 1 2 3 ...)
```


### iterate

与えられた関数を繰り返し適用して  
得られる要素が並ぶリストを返す

```clj
(iterate (fn [x] (* x 10)) 1) ;-> (1 10 100 1000 10000 ...)

(iterate (fn [x] (+ x 1)) 0) ;-> (0 1 2 3 4 ...)
```


## 無限リストから有限の要素を取り出す

### take

```clj
(take 5 (repeat 1)) ;-> (1 1 1 1 1)

(take 10 (cycle '(3 2 1))) ;-> (3 2 1 3 2 1 3 2 1 3)
```

もちろん `take` は有限長のリストにも使える

```clj
(take 5 '(1 2 3 4 5 6 7)) ;-> (1 2 3 4 5)
```


## 文字列

```clj
"abc" ;-> "abc"
```

```clj
(str "abc" "def") ;-> "abcdef"
```

`str` という文字列を作る関数がある

```clj
(str 1) ;-> "1"
```


## Hello World!

```clj
(println "Hello World!")
```

`println` は文字列を行として標準出力に表示する関数


## Set

集合

```clj
#{1 2 3} ;-> #{1 2 3}
```

```clj
(set '(1 2 3)) ;-> #{1 2 3}
```


### 要素追加

```clj
(conj #{1 2 3} 4) ;-> #{1 2 3 4}
(conj #{1 2 3} 1) ;-> #{1 2 3}
```

集合なので同じ要素は 2 つ以上無い

`conj` は conjunction


## Hash-Map

```clj
{:a 1 :b 2}
```

key と value で作られた hash-map

上記の例は `:a` というキーの値が `1`，  
`:b` というキーの値が `2` のマップである．

```clj
(hash-map :a 1 :b 2) ;-> {:a 1 :b 2}
```

上記のような作り方もある


### キーワード

```clj
:a ;-> :a
```

`:` に文字列が続く形のものがキーワード

文字列のようなもの

```clj
(keyword "a") ;-> :a
```

このような作り方もある


### Map の種々の操作

キーを指定して値を取り出す

```clj
(:a {:a 1 :b 2}) ;-> 1
```

Clojure ではキーワードが Map を引数にとる  
関数のように振る舞える

```clj
({:a 1 :b 2} :a) ;-> 1
```

Map もキーワードを引数にとる  
関数のように振る舞える


## 今回は説明しないこと

* 再帰関数
* loop recur
* 名前空間
* 参照
* マクロの作り方


ここから実用的な言語の話をします


## プロジェクト作成

Leiningen でアプリケーションのプロジェクトを作成

```sh
lein new app myapp
```


## 実行可能 jar 作成

```sh
cd myapp
lein uberjar
```


## 実行してみる

```sh
java -jar target/myapp-0.1.0-SNAPSHOT-standalone.jar
```

```txt
Hello, World!
```


## プロジェクトの中身

`src/myapp/core.clj` がソースファイル

`-main` という関数が main 関数

ここから起動する


## 引数を受け取ってみる

```clj
(ns myapp.core
  (:gen-class))

(defn -main
  "I don't do a whole lot ... yet."
  [& args]
  (println "Hello, World!"))
```

次のように改造

```clj
(ns myapp.core
  (:gen-class))

(defn -main
  "I don't do a whole lot ... yet."
  [x]
  (println "Hello, World!")
  (println "The argument \"x\" is " x))
```


## 引数を与えて実行

```sh
java -jar target/myapp-0.1.0-SNAPSHOT-standalone.jar 10
```

```txt
Hello, World!
The argument "x" is  10
```


## ライブラリを使ってみる

`math.combinatorics` というライブラリがある

[clojure/math.combinatorics](https://github.com/clojure/math.combinatorics)

数学での順列・組み合わせを扱うライブラリ


## 依存関係を追加

`project.clj` を編集する

```clj
(defproject myapp ...
  ...
  :dependencies [[org.clojure/clojure "1.5.1"]]
  ...)
```

以下のように編集

```clj
(defproject myapp ...
  ...
  :dependencies [[org.clojure/clojure "1.5.1"]
                 [org.clojure/math.combinatorics "0.0.6"]]
  ...)
```


## combinations 関数

`combinations` という関数に  
リストと要素数を渡すと  
その組み合わせを列挙したリストを返してくれる

たとえば `(combinations '(1 2 3) 2)` は  
`((1 2) (1 3) (2 3))` と評価される

2 引数を受け取り `xCy` を計算してみる


## main 関数改造

以下の様に改造

```clj
(ns myapp.core
  (:gen-class)
  (:require [clojure.math.combinatorics :as combo]))

(defn -main
  "I don't do a whole lot ... yet."
  [x y]
  (println "Hello, World!")
  (println
    (str "x = " x ", y = " y ", xCy = "
      (count (combo/combinations (range 0 (Integer. x)) (Integer. y))))))
```


## 改造した箇所について


### ライブラリ読み込み

```clj
(:require [clojure.math.combinatorics :as combo])
```

これは今は詳しく説明しません


### range 関数

範囲からリストを作成して返す

```clj
(range 0 10) ;-> (0 1 2 3 4 5 6 7 8 9)

(range 1 5) ;-> (1 2 3 4)
```


### count 関数

リストの長さ (要素の数) を返す

```clj
(count '(1 1 1 1)) ;-> 4
(count '()) ;-> 0
```


### Integer

```clj
(Integer. "123") ;-> 123
```

とりあえずここでは文字列を数字に変えるために使う

こうやって使うもんだと今日だけ覚えて下さい


### 総括

引数で受け取った `x` `y` は文字列なので整数に直し

`range` でリストを作り

`combinations` で組み合わせのリストを取得し

その数を数える


## 動かしてみる

```sh
lein uberjar
java -jar target/myapp-0.1.0-SNAPSHOT-standalone.jar 10 2
```

```txt
Hello, World!
x = 10, y = 2, xCy = 45
```


## ライブラリを作ってみる

```sh
lein new default mylib
```


## 関数を作る

`src/mylib/core.clj` に以下を追加

```clj
(defn f [x] (+ x 1))
```


## ローカルへのインストール

```sh
lein install
```

これでホームディレクトリ (場所は環境によりけり) の  
`.m2` というディレクトリにインストールされる

`m2` という名前は Maven 由来


## myapp から使ってみる


### 依存関係追加

`project.clj` の `:dependencies` に以下を追加

```clj
[mylib/mylib "0.1.0-SNAPSHOT"]
```


### ソース改造

`src/myapp/core.clj` を以下のように変更

```clj
(ns myapp.core
  (:gen-class)
  (:require [clojure.math.combinatorics :as combo])
  (:require [mylib.core :refer :all]))

(defn -main
  "I don't do a whole lot ... yet."
  [x y]
  (println "Hello, World!")
  (println
    (str "x = " x ", y = " y ", xC(y + 1) = "
      (count (combo/combinations (range 0 (Integer. x)) (f (Integer. y)))))))
```


## 動かしてみる

```sh
lein uberjar
java -jar target/myapp-0.1.0-SNAPSHOT-standalone.jar 10 1
```

```txt
Hello, World!
x = 10, y = 1, xC(y + 1) = 45
```


## 今回は説明しないこと

* GUI とか
* require や ns の詳細
* ライブラリを公開する方法


ごめんなさい説明しないのではなく


出来ません


Clojure まだまだ初心者なんです


## Web Application 作成


## Ring を使って基礎から

Ring とは

Ruby でいう Rack

Python でいう WSGI (こっちはよく知りませんが…)


## Compojure

Ring で 0 から構築するのは勉強になる

が時間が掛かりそうなので少しだけ楽をする

Compojure というライブラリを使う

[weavejester/compojure](https://github.com/weavejester/compojure)


## プロジェクト作成

```sh
lein new compojure mycompojureapp
```


## 動かしてみる

```sh
lein ring server-headless
```

ブラウザで `http://localhost:3000` にアクセス


## 動かしてみる方法その 2

```sh
lein ring server
```

ブラウザも自動で開く


## ルーティング追加

```clj
(GET "/hoge" [] "GET /hoge")
```

自動でソースも再読み込みされる

```clj
(GET "/hoge/fuga" [] "GET /hoge/fuga")
```


## 参考書籍 1

[プログラミングClojure 第2版  
![](http://ssl.ohmsha.co.jp/imgm/978-4-274-06913-0.gif)
](http://www.amazon.co.jp/dp/4274069133)


## 参考書籍 2

[おいしい Clojure 入門  
![](http://image.gihyo.co.jp/assets/images/cover/2013/thumb/TH160_9784774159911.jpg)
](http://www.amazon.co.jp/dp/4774159913)


## 勉強に使えるサイト

* [4clojure](https://www.4clojure.com/)


## 今回作ったもの

* このスライド: [kaosf/20140209-clojure-getting-started](https://github.com/kaosf/20140209-clojure-getting-started)
* myapp: [kaosf/20140209-clojure-getting-started-myapp](https://github.com/kaosf/20140209-clojure-getting-started-myapp)
* mylib: [kaosf/20140209-clojure-getting-started-mylib](https://github.com/kaosf/20140209-clojure-getting-started-mylib)
* mycompojureapp: [kaosf/20140209-clojure-getting-started-mycompojureapp](https://github.com/kaosf/20140209-clojure-getting-started-mycompojureapp)
