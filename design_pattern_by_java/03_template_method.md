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