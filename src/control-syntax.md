# 制御構文

この節では、Scalaの制御構文について学びます。通常のプログラミング言語とくらべてそれほど突飛なものが出てくるわけではないので心配は要りません。

## 「構文」と「式」と「文」という用語について（★）

この節では「構文」と「式」と「文」という用語が入り乱れて使われて少々わかりづらいかもしれないので、先にこの3つの用語の解説をしたいと思います。

まず「構文（Syntax）」は、そのプログラミング言語内でプログラムが構造を持つためのルールです。
多くの場合、プログラミング言語内で特別扱いされるキーワード、たとえば`class`や`val`、`if`などが含まれ、そして正しいプログラムを構成するためのルールがあります。
`class`の場合であれば、`class`の後にはクラス名が続き、クラスの中身は`{`と`}`で括られる、などです。
この節はScalaの制御構文を説明するので、処理の流れを制御するようなプログラムを作るためのルールが説明されるわけです。

次に「式（Expression）」は、プログラムを構成する部分のうち、評価することで値になるものです。
たとえば`1`や`1 + 2`、`"hoge"`などです。これらは評価することにより、数値や文字列の値になります。

最後に「文（Statement）」ですが、式とは対照的にプログラムを構成する部分のうち、評価しても値にならないものです。
たとえば変数の定義である`val i = 1`は評価しても変数`i`が定義され、`i`の値が`1`になりますが、この定義全体としては値を持ちません。
よって、これは文です。

Scalaは他のCやJavaなどの手続き型の言語に比べて、文よりも式になる構文が多いです。
Scalaでは文よりも式を多く利用する構文が採用されています。これにより変数などの状態を出来るだけ排除した分かりやすいコードが書きやすくなっています。

このような言葉の使われ方に注意し、以下の説明を読んでみてください。

## `{}`式（★★★）

`{}`構文の一般形は

```scala
{ exp1; exp2; ... expN; }
```

となります。`exp1`から`expN`は式です。式が改行で区切られていればセミコロンは省略できます。`{}`式は`exp1`から`expN`を順番に評価し、`expN`を評価した値を返します。

次の式では

```tut
{ println("A"); println("B"); 1 + 2 }
```

AとBが出力され、最後の式である`1 + 2`の結果である`3`が`{}`式の値になっていることがわかります。

このことは、後ほど記述するメソッド定義などにおいて重要になってきます。Scalaでは、

```tut:silent
def foo(): String = {
  "foo" + "foo"
}
```

のような形でメソッド定義をすることが一般的ですが（後述します）、ここで`{}`は単に`{}`式であって、メソッド定義の構文に特別に`{}`が含まれているわけではありません。
ただし、クラス定義構文などにおける`{}`は別です。

## if式（★★★）

`if`式はJavaの`if`文とほとんど同じ使い方をします。`if`式の構文は次のようになります。

```scala
if （条件式） A [else B]
```

条件式は`Boolean`型である必要があります。`else B`は省略することができます。`A`は条件式が`true`のときに評価される式で、`B`は条件式が`false`のときに評価される式です。

早速`if`式を使ってみましょう。

```tut
var age = 17

if(age < 18) {
  "18歳未満です"
} else {
  "18歳以上です"
}

age = 18

if(age < 18) {
  "18歳未満です"
} else {
  "18歳以上です"
}
```

変更可能な変数`age`が18より小さいかどうかで別の文字列を*返す*ようにしています。

`if`式に限らず、Scalaの制御構文は全て式です。つまり必ず何らかの値を返します。Javaなどの言語で三項演算子`?:`を見たことがある人もいるかもしれませんが、同じように値が必要な場面で`if`式も使えます。

なお、`else`が省略可能だと書きましたが、その場合は、

```scala
if（条件式） A else ()
```

と`Unit`型の値が補われたのと同じ値が返ってきます。


### 練習問題 {#control_syntax_ex1}

`var age: Int = 5`という年齢を定義する変数と`var isSchoolStarted: Boolean = false`という就学を開始しているかどうかという変数を利用して、
1歳から6歳までの就学以前の子どもの場合に“幼児です”と出力し、それ以外の場合は“幼児ではありません”と出力するコードを書いてみましょう。

<!-- begin answer id="answer_ex1" style="display:none" -->

```tut
var age: Int = 5
var isSchoolStarted: Boolean = false
if(1 <= age && age <= 6 && !isSchoolStarted) {
  println("幼児です")
} else {
  println("幼児ではありません")
}
```

<!-- end answer -->

## while式（★★）

`while`式の構文はJavaのものとほぼ同じです。

```scala
while （条件式） A
```

条件式は`Boolean`型である必要があります。`while`式は、条件式が`true`の間、Aを評価し続けます。なお、`while`式も式なので値を返しますが、`while`式には適切な返すべき値がないので`Unit`型の値`()`を返します。

`Unit`型はJavaでは`void`に相当するもので、返すべき値がないときに使われ、唯一の値`()`を持ちます。

さて、while式を使って1から10までの値を出力してみましょう。

```tut
var i = 1

while(i <= 10) {
  println("i = " + i)
  i = i + 1
}
```

Javaで`while`文を使った場合と同様です。`do while`文もありますが、ほぼJavaと同じなので説明は省略します。なお、`break`文や`continue`文に相当するものはありません。

### 練習問題 {#control_syntax_ex2}

`do while`を利用して、0から数え上げて9まで出力して10になったらループを終了するメソッド`loopFrom0To9`を書いてみましょう。`loopFrom0To9`は次のような形になります。`???`の部分を埋めてください。

```tut:silent
def loopFrom0To9(): Unit = {
  var i = ???
  do {
    ???
  } while(???)
}
```

<!-- begin answer id="answer_ex2" style="display:none" -->

```tut:silent
def loopFrom0To9(): Unit = {
  var i = 0
  do {
    println(i)
    i += 1
  } while(i < 10)
}
```

```tut
loopFrom0To9()
```

<!-- end answer -->

## for式（★★★）

Scalaにはfor式という制御構文がありますが、これはJavaの制御構文と似た使い方ができるものの、全く異なる構文です。for式の本当の力を理解するには、`flatMap`, `map`, `withFilter`, `foreach`といったメソッドについて知る必要がありますが、ここでは基本的な`for`式の使い方のみを説明します。

`for`式の基本的な構文は次のようになります。

```
for(ジェネレータ1;  ジェネレータ2; ... ジェネレータn) A
# ジェネレータ1 = a1 <- exp1; ジェネレータ2 = a2 <- exp2; ... ジェネレータn = an <- expn
```

変数`a1`〜`an`までは好きな名前のループ変数を使うことができます。`exp1`から`expn`までにかける式は一般形を説明するとやっかいなので、さしあたって、ある数の範囲を表す式を使えると覚えておいてください。たとえば、`1 to 10`は1から10まで（10を含む）の範囲
で、`1 until 10`は1から10まで（10を含まない）の範囲です。

それでは、早速`for`式を使ってみましょう。

```tut
for(x <- 1 to 5; y <- 1 until 5){
  println("x = " + x + " y = " + y)
}
```

`x`を1から5までループして、`y`を1から4までループして`x`, `y`の値を出力しています。ここでは、ジェネレータを2つだけにしましたが、数を増やせば何重にもループを行うことができます。

`for`式の力はこれだけではありません。ループ変数の中から条件にあったものだけを絞り込むこともできます。`until`の後で`if x != y`と書いていますが、これは、`x`と`y`が異なる値の場合のみを抽出したものです。

```tut
for(x <- 1 to 5; y <- 1 until 5 if x != y){
  println("x = " + x + " y = " + y)
}
```

`for`式はコレクションの要素を1つ1つたどって何かの処理を行うことにも利用することができます。`"A"`, `"B"`, `"C"`,
`"D"`, `"E"`の5つの要素からなるリストをたどって全てを出力する処理を書いてみましょう。

```tut
for(e <- List("A", "B", "C", "D", "E")) println(e)
```

さらに、`for`式はたどった要素を加工して新しいコレクションを作ることもできます。先ほどのリストの要素全てに`Pre`という
文字列を付加してみましょう。

```tut
for(e <- List("A", "B", "C", "D", "E")) yield {
  "Pre" + e
}
```

ここでポイントとなるのは、`yield`というキーワードです。実は、`for`構文は`yield`キーワードを使うことで、コレクションの
要素を加工して返すという全く異なる用途に使うことができます。特に`yield`キーワードを使った`for`式を特別に
for-comprehensionと呼ぶことがあります。

### 練習問題 {#control_syntax_ex3}

1から1000までの3つの整数a, b, cについて、三辺からなる三角形が直角三角形になるような
`a`, `b`, `c`の組み合わせを全て出力してください。直角三角形の条件にはピタゴラスの定理を
利用してください。 ピタゴラスの定理とは三平方の定理とも呼ばれ、`a ^ 2 == b ^ 2 + c ^ 2`を満たす、`a`, `b`, `c` の長さの三辺を持つ
三角形は、直角三角形になるというものです。

<!-- begin answer id="answer_ex3" style="display:none" -->

```tut:silent
for(a <- 1 to 1000; b <- 1 to 1000; c <- 1 to 1000) {
  if(a * a == b * b + c * c) println((a, b, c))
}
```

<!-- end answer -->

## match式（★★★）

`match`式は使い方によって非常に幅のある制御構造です。`match`式の基本構文は

```
マッチ対象の式 match {
  case パターン1 [if ガード1] => 式1
  case パターン2 [if ガード2] => 式2
  case パターン3 [if ガード3] => 式3
  case ...
  case パターンN => 式N
}
```

のようになりますが、この「パターン」に書ける内容が非常に多岐に渡るためです。まず、Javaのswitch-caseのような使い方を
してみます。たとえば、

```tut
val taro = "Taro"

taro match {
  case "Taro" => "Male"
  case "Jiro" => "Male"
  case "Hanako" => "Female"
}
```

のようにして使うことができます。ここで、`taro`には文字列`"Taro"`が入っており、これは`case "Taro"`にマッチするため、`"Male"`が返されます。
なお、ここで気づいた人もいるかと思いますが、`match`式も値を返します。`match`式の値は、マッチしたパターンの`=>`の右辺の式
を評価したものになります。

パターンは文字列だけでなく数値など任意の値を扱うことができます：

```tut
val one = 1

one match {
  case 1 => "one"
  case 2 => "two"
  case _ => "other"
}
```

ここで、パターンの箇所に`_`が出てきましたが、これはswitch-caseのdefaultのようなもので、あらゆるものにマッチするパターンです。
このパターンをワイルドカードパターンと呼びます。
match式を使うときは、漏れがないようにするために、ワイルドカードパターンを使うことが多いです。

### パターンをまとめる

JavaやCなどの言語でswitch-case文を学んだ方には、Scalaのパターンマッチがいわゆるフォールスルー（fall through）の動作をしないことに違和感があるかもしれません。

```tut:silent
"abc" match {
  case "abc" => println("first")   // ここで処理が終了
  case "def" => println("second") // こっちは表示されない
}
```

C言語のswitch-case文のフォールスルー動作は利点よりバグを生み出すことが多いということで有名なものでした。
JavaがC言語のフォールスルー動作を引き継いだことはしばしば非難されます。
それでScalaのパターンマッチにはフォールスルー動作がないわけですが、複数のパターンをまとめたいときのために`|`があります

```tut:silent
"abc" match {
  case "abc" | "def" =>
    println("first")
    println("second")
}
```

### パターンマッチによる値の取り出し

switch-case以外の使い方としては、コレクションの要素の一部にマッチさせる使い方があります。次のプログラムを見てみましょう。

```tut
val lst = List("A", "B", "C", "D", "E")

lst match {
  case List("A", b, c, d, e) =>
    println("b = " + b)
    println("c = " + c)
    println("d = " + d)
    println("e = " + e)
  case _ =>
    println("nothing")
}
```

ここでは、`List`の先頭要素が`"A"`で5要素のパターンにマッチすると、残りの`b`, `c`, `d`, `e`に`List`の2番目以降の要素が束縛されて、`=>`の右辺の式
が評価されることになります。match式では、特にコレクションの要素にマッチさせる使い方が頻出します。

パターンマッチではガード式を用いて、パターンにマッチして、かつ、ガード式（`Boolean`型でなければならない）にもマッチしなければ
右辺の式が評価されないような使い方もできます。

```tut
val lst = List("A", "B", "C", "D", "E")

lst match {
  case List("A", b, c, d, e) if b != "B" =>
    println("b = " + b)
    println("c = " + c)
    println("d = " + d)
    println("e = " + e)
  case _ =>
    println("nothing")
}
```

ここでは、パターンマッチのガード条件に、`List`の2番目の要素が`"B"`でないこと、という条件を指定したため、最初の条件にマッチせず
`_`にマッチしたのです。

また、パターンマッチのパターンはネストが可能です。先ほどのプログラムを少し改変して、先頭が`List("A")`であるような`List`にマッチ
させてみましょう。

```tut
val lst = List(List("A"), List("B", "C", "D", "E"))

lst match {
  case List(a@List("A"), x) =>
  println(a)
  println(x)
  case _ => println("nothing")
}
```

`lst`は`List("A")`と`List("B", "C", "D", "E")`の2要素からなる`List`です。ここで、match式を使うことで、先頭が`List("A")`であるというネスト
したパターンを記述できていることがわかります。また、パターンの前に`@`がついているのはasパターンと呼ばれるもので、`@`の後に続くパターンに
マッチする式を`@`の前の変数（ここでは`a`）に束縛します。`as`パターンはパターンが複雑なときにパターンの一部だけを切り取りたい時に便利です。

### 型によるパターンマッチ

パターンとしては値が特定の型に所属する場合にのみマッチするパターンも使うことができます。値が特定の型に
所属する場合にのみマッチするパターンは、`名前:マッチする型`の形で使います。たとえば、以下のようにして使うことができます。
なお、`AnyRef`型は、Javaの`Object`型に相当する型で、あらゆる参照型の値を`AnyRef`型の変数に格納することができます。

```tut
import java.util.Locale

val obj: AnyRef = "String Literal"

obj match {
  case v:java.lang.Integer =>
    println("Integer!")
  case v:String =>
    println(v.toUpperCase(Locale.ENGLISH))
}
```

`java.lang.Integer`にはマッチせず、`String`にマッチしていることがわかります。このパターンは例外処理や`equals`の定義などで使うことがあります。
型でマッチした値は、その型にキャストしたのと同じように扱うことができます。
たとえば、上記の式で`String`型にマッチした`v`は`String`型のメソッドである`toUpperCase`を呼びだすことができます。
しばしばScalaではキャストの代わりにパターンマッチが用いられるので覚えておくとよいでしょう。

### JVMの制約による型のパターンマッチの落とし穴

ただし、型のパターンマッチで注意しなければならないことが1つあります。
Scalaを実行するJVMの制約により、型変数を使った場合、正しくパターンマッチがおこなわれません。

たとえば、以下の様なパターンマッチをREPLで実行しようとすると、警告が出てしまいます。

```tut
val obj: Any = List("a")
obj match {
  case v: List[Int]    => println("List[Int]")
  case v: List[String] => println("List[String]")
}
```

型としては`List[Int]`と`List[String]`は違う型なのですが、パターンマッチではこれを区別できません。

最初の2つの警告の意味はScalaコンパイラの「型消去」という動作により`List[Int]`の`Int`の部分が消されてしまうのでチェックされないということです。

結果的に2つのパターンは区別できないものになり、パターンマッチは上から順番に実行されていくので、2番目のパターンは到達しないコードになります。
3番目の警告はこれを意味しています。

型変数を含む型のパターンマッチは、

```tut:silent
obj match {
  case v: List[_] => println("List[_]")
}
```

このようにワイルドカードパターンを使うとよいでしょう。

### 練習問題 {#control_syntax_ex4}

```tut:silent
new scala.util.Random(new java.security.SecureRandom()).alphanumeric.take(5).toList
```

以上のコードを利用して、 最初と最後の文字が同じ英数字であるランダムな5文字の文字列を1000回出力してください。
`new scala.util.Random(new java.security.SecureRandom()).alphanumeric.take(5).toList`という値は、
呼びだす度にランダムな5個の文字（`Char`型）のリストを与えます。なお、以上のコード
で生成されたリストの一部分を利用するだけでよく、最初と最後の文字が同じ英数字である
リストになるまで試行を続ける必要はありません。これは、`List(a, b, d, e, f)`が得られた
場合に、`List(a, b, d, e, a)`のようにしても良いということです。

<!-- begin answer id="answer_ex4" style="display:none" -->

```tut:silent
for(i <- 1 to 1000) {
  val s = new scala.util.Random(new java.security.SecureRandom()).alphanumeric.take(5).toList match {
    case List(a,b,c,d,_) => List(a,b,c,d,a).mkString
  }
  println(s)
}
```

<!-- end answer -->

ここまで書いただけでも、`match`式はswitch-caseに比べてかなり強力であることがわかると思います。ですが、`match`式の力はそれにとどまりません。
後述しますが、パターンには自分で作ったクラス（のオブジェクト）を指定することでさらに強力になります。