# design_pattern_by_java

## Books Information

|          |                                    |
| :------- | :--------------------------------- |
| タイトル | Java言語で学ぶデザインパターン入門 |
| 著者     | 結城浩                             |
| 発行日   | 2013.09.10                         |

## Memo

- デザインパターンはクラスの再利用化を促進するもの

### Iterator

- 要素の集合をひとつひとつ簡単に取り出せるパターン
- 登場人物
  - Iterator: 反復処理を定めたインタフェース
  - ConcreteIterator: Iteratorを実装するクラス
  - Aggregate: Iteratorを扱うためのインタフェース
  - ConcreteAggregate: Aggregateを実装するクラス
- ConcreteAggregateと ConcreteIteratorが別で存在していることが特徴  
ひとつの Aggregateに対して複数の Iteratorを持たせることができる。
- この構成にすることで、for/whileの書き方が Aggregateの実装に依存しない  
Aggregateの変更に強くなる。

### Adapter

- 間を取り持つもの  
すでに提供されているものを必要なものに変換する。
- Wrapperパターンとよばれることも
- 2種類ある
  - 継承を使ったもの
  - 委譲を使ったもの

#### 継承を使ったもの

実装のモチベーション、流れは以下の通り。

1. とあるクラスAが提供されている
2. 自分の欲しい処理はインタフェースBで定義されている
3. クラスAにはすでに欲しい処理に近い関数が実装されているが、インタフェースBに準じた名前で呼びたい
4. そこで、クラスAを継承しつつインタフェースBを実装した新たなクラスCを定義する
5. これで、既存の処理を流用しつつ好きな使い方ができる

#### 委譲を使ったもの

継承を使ったものでは、自分の欲しい処理がインタフェースBで定義されていた。  
しかし、委譲を使ったものではそれが抽象クラスBとして定義されている。  
その場合、言語によっては同時に複数のクラスを継承することはできないため、継承とは違う方法で解決しなければならない。  

そこで、継承ではなくインスタンスを保持し利用することで解決する。

1. とあるクラスAが提供されている
2. 自分の欲しい処理は抽象クラスBで定義されている
3. クラスAにはすでに欲しい処理に近い関数が実装されているが、クラスBに準じた名前で呼びたい
4. そこで、クラスBを継承しつつ中ではインスタンスAの処理を呼び出すだけのクラスを定義し、それをクラスCとする
5. これで、既存の処理を流用しつつ好きな使い方ができる

#### Adapterのまとめ

- 登場人物
  - Target: いま必要なメソッドを定めるもの
  - Client: Targetの処理を必要とするもの
  - Adapter: 既存の処理を Targetに適合させるもの
  - Adaptee: Adapterによって調整されるもの
- どんないいことがある？
  - すでに実装したものを転用することができる
    - テストされた部分を変更することなく使える
    - バグが出たとしても、Adapter側に問題があることが自明である
  - 互換性をもたせるのに有用
  - 動くコードがなくとも入出力の仕様だけで実装を進めることができる

### Template Method
- Abstractによって処理の流れ (テンプレートメソッド) だけを定義し、詳細の実装はサブクラスに任せる
- 登場人物
  - AbstractClass: 抽象メソッドを宣言し、テンプレートを表す  
  テンプレートメソッドはここで実装する。
  - ConcreteClass: 抽象メソッドを実装する  
  ここで定義されたものはテンプレートメソッドから呼び出される。
- どんないいことがある？
  - 処理の概念を統一できる

#### サンプルコード

```java
public abstract class AbstractDisplay {
    public abstract void open();
    public abstract void print();
    public abstract void close();
    public final void display() {
        open();
        print();
        close();
    }
}

public class CharDisplay extends AbstractDisplay {
    private char ch;
    public CharDisplay(char ch) {
        this.ch = ch;
    }
    public void open() {
        System.out.print("<<");
    }
    public void print() {
        System.out.print(ch);
    }
    public void close() {
        System.out.println(">>");
    }
}
```
