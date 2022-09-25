# design_pattern_by_java

## Books Information

|          |                                    |
| :------- | :--------------------------------- |
| タイトル | Java言語で学ぶデザインパターン入門 |
| 著者     | 結城浩                             |
| 発行日   | 2013.09.10                         |

## Foreword

- デザインパターンはクラスの再利用化を促進するもの

## Iterator

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

## Adapter

- 間を取り持つもの  
すでに提供されているものを必要なものに変換する。
- Wrapperパターンとよばれることも
- 2種類ある
  - 継承を使ったもの
  - 委譲を使ったもの

### 継承を使ったもの

実装のモチベーション、流れは以下の通り。

1. とあるクラスAが提供されている
2. 自分の欲しい処理はインタフェースBで定義されている
3. クラスAにはすでに欲しい処理に近い関数が実装されているが、インタフェースBに準じた名前で呼びたい
4. そこで、クラスAを継承しつつインタフェースBを実装した新たなクラスCを定義する
5. これで、既存の処理を流用しつつ好きな使い方ができる

### 委譲を使ったもの

継承を使ったものでは、自分の欲しい処理がインタフェースBで定義されていた。  
しかし、委譲を使ったものではそれが抽象クラスBとして定義されている。  
その場合、言語によっては同時に複数のクラスを継承することはできないため、継承とは違う方法で解決しなければならない。  

そこで、継承ではなくインスタンスを保持し利用することで解決する。

1. とあるクラスAが提供されている
2. 自分の欲しい処理は抽象クラスBで定義されている
3. クラスAにはすでに欲しい処理に近い関数が実装されているが、クラスBに準じた名前で呼びたい
4. そこで、クラスBを継承しつつ中ではインスタンスAの処理を呼び出すだけのクラスを定義し、それをクラスCとする
5. これで、既存の処理を流用しつつ好きな使い方ができる

### Adapterのまとめ

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

## Template Method
- Abstractによって処理の流れ (テンプレートメソッド) だけを定義し、詳細の実装はサブクラスに任せる
- 登場人物
  - AbstractClass: 抽象メソッドを宣言し、テンプレートを表す  
  テンプレートメソッドはここで実装する。
  - ConcreteClass: 抽象メソッドを実装する  
  ここで定義されたものはテンプレートメソッドから呼び出される。
- どんないいことがある？
  - 処理の概念を統一できる

### サンプルコード

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

## Factory Method

- Template Methodと似ている
- インスタンス生成の方法を統一しつつ、詳細な実装はサブクラスに委ねる
- 登場人物は以下の通り
  - Product: 生成される側のインタフェースを定める
  - Creator: Productを生成する側のインタフェースを定める
  - ConcreteProduct: Productの中身を実装するもの
  - ConcreteCreator: Creatorの中身を実装するもの

### サンプルコード

フレームワーク側:

```java
public abstract class Product {
    public abstract void use();
}

public abstract class Factory {
    public final Product create(String owner) {
        Product p = createProduct(owner);
        registerProduct(p);
        return p;
    }
    protected abstract Product createProduct(String owner);
    protected abstract void registerProduct(Product product);
}
```

フレームワークを利用する側:

```java
import java.util.*;

public class IDCard extends Product {
    private String owner;
    IDCard(String owner) {
        System.out.println(owner + "のカードを作ります");
        this.owner = owner;
    }
    public void use() {
        System.out.println(owner + "のカードを使います");
    }
    public String getOwner() {
        return owner;
    }
}

public class IDCardFactory extends Factory {
    private List owners = new ArrayList();
    protected Product createProduct(String owner) {
        return new IDCard(owner);
    }
    protected void registerProduct(Product product) {
        owners.add(((IDCard)product).getOwner());
    }
    public List getOwners() {
        return owners;
    }
}
```

### Factory Methodのまとめ

- どんないいことがある？
  - どんなサブクラスを定義しようとも、Productを作るときの書き方がクラス名に依存しない  
  Productの生成は `createProduct()` に統一されており、この部分が変更に強くなる。

## Prototype

- 既存のインスタンスをコピーして新しいインスタンスを作る

### サンプルコード

フレームワーク側:

```java
import java.util.*;

public interface Product extends Cloneable {
    public abstract void use(String s);
    public abstract Product createClone();
}

public class Manager {
    private HashMap showcase = new HashMap();
    public void register(String name, Product proto) {
        showcase.put(name, proto);
    }
    public Product create(String protoname) {
        product p = (Product)showcase.get(protoname);
        return p.createClone();
    }
}
```

フレームワークを利用する側:

- MessageBox: 文字列 (s) を特定の文字 (decochar) で囲って出力する
- UnderlinePen: 文字列 (s) の下に特定の文字 (ulchar) を付けて出力する

```java
public class MessageBox implements Product {
    private char decochar;
    public MessageBox(char decochar) {
        this.decochar = decochar;
    }
    public void use(String s) {
        int length = s.getBytes().length;
        for (int i = 0; i < length + 4; i++) {
            System.out.print(decochar);
        }
        System.out.println("");
        System.out.println(decochar + " " + s + " " + decochar);
        for (int i = 0; i < length + 4; i++) {
            System.out.print(decochar);
        }
        System.out.println("");
    }
    public Product createClone() {
        Product p = null;
        try {
            p = (Product)clone();
        } catch (CloneNotSupportedException e) {
            e.printStarckTrace();
        }
        return p;
    }
}

public class UnderlinePen implements Product {
    private char ulchar;
    public UnderlinePen(char ulchar) {
        this.ulchar = ulchar;
    }
    public void use(String s) {
        int length = s.getBytes().length;
        System.out.println("\"" + s + "\"");
        System.out.print(" ");
        for (int i = 0; i < length; i++) {
            System.out.print(ulchar);
        }
        System.out.println("");
    }
    public Product createClone() {
        Product p = null;
        try {
            p = (Product)clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return p;
    }
}
```

Mainクラス:

```java
public class Main {
    public static void main(String[] args) {
        Manager manager = new Manager();
        UnderlinePen upen = new UnderlinePen('~');
        MessageBox mbox = new MessageBox('*');
        MessageBox sbox = new MessageBox('/');
        manager.register("strong message", upen);
        manager.register("warning box", mbox);
        manager.register("slash box", sbox);

        Product p1 = manager.create("strong message");
        p1.use("Hello, world.");
        // "Hello, world."
        //  ~~~~~~~~~~~~~

        Product p2 = manager.create("warning box");
        p2.use("Hello, world.");
        // *****************
        // * Hello, world. *
        // *****************

        Product p3 = manager.create("slash box");
        p3.use("Hello, world.");
        // /////////////////
        // / Hello, world. /
        // /////////////////
    }
}
```

### Prototypeのまとめ

- 登場人物
  - Prototype: 既存のインスタンスをコピーするための処理を定めるインタフェース
  - ConcretePrototype: Prototypeを具体的に実装するもの
  - Client: ConcretePrototypeを使うもの
- どんないいことがある？
  - 純粋にインスタンスのコピーができる
  - クラス名を指定せずインスタンスを生成する手段がある  
  クラス名による束縛・密結合を避けることができる。

## Builder

- 構造をもつ大きなものを段階的に組み上げていくもの
- 登場人物
  - Builder: インスタンスを段階的に作るためのインタフェースを定める
  - ConcreteBuilder: Builderを具体的に実装するクラス  
  結果を取得するメソッドも定義されたり。
  - Director: Builderのインタフェースを使ってインスタンスを生成する  
  あくまでインタフェースに定められている処理のみを使うことで、ConcreteBuilderが複数種類あったとしてもみな同じように使える。  
  Builderを簡単に交換して使いまわせるということ。
  - Client: Builderパターンを利用する  
  つまり、ConcreteBuilderと Directorを利用することになる。
