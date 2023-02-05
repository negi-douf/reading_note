# Prototype

- 既存のインスタンスをコピーして新しいインスタンスを作る

## サンプルコード

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

## Prototypeのまとめ

- 登場人物
  - Prototype: 既存のインスタンスをコピーするための処理を定めるインタフェース
  - ConcretePrototype: Prototypeを具体的に実装するもの
  - Client: ConcretePrototypeを使うもの
- どんないいことがある？
  - 純粋にインスタンスのコピーができる
  - クラス名を指定せずインスタンスを生成する手段がある  
  クラス名による束縛・密結合を避けることができる。
