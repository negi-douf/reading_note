# Bridge

- 機能のクラスと実装のクラスを分けて、それぞれ別の階層にする
- 機能側が実装側のインスタンスを持つことで、機能と実装を繋げる
- 今までのパターンだと...
  - あるクラス Somethingに機能を追加したいとき、サブクラスとして SomethingGoodのクラスを作る  
    これで機能の階層ができる。
  - もしくは、Abstractクラスでインタフェースを規定し、Concreteクラスで中身を実装する  
    これで実装の階層ができる。
  - これらを単一のクラスにまとめてしまうと、複雑化を招くおそれがある
- そこで、Bridgeでは機能と実装を独立したクラス階層で表現したうえで、それらを繋げて利用する

## サンプルコード

- Display: 基本機能を定義したクラス
- CountDisplay: 繰り返して表示する機能を追加したクラス
- DisplayImpl: 基本機能を実装するためのメソッドを定義した抽象クラス
- StringDisplayImpl: DisplayImplを実装したクラス

機能のクラス階層:

```java
public class Display {
    private DisplayImpl impl;
    public Display(DisplayImpl impl) {
        this.impl = impl;
    }
    public void open() {
        impl.rawOpen();
    }
    public void print() {
        impl.rawPrint();
    }
    public void close() {
        impl.rawClose();
    }
    public final void display() {
        open();
        print();
        close();
    }
}

public class CountDisplay extends Display {
    public CountDisplay(DisplayImpl impl) {
        super(impl);
    }
    public void multiDisplay(int times) {
        open();
        for (int i = 0; i < times; i++) {
            print();
        }
        close();
    }
}
```

実装のクラス階層:

```java
public abstract class DisplayImpl {
    public abstract void rawOpen();
    public abstract void rawPrint();
    public abstract void rawClose();
}

public class StringDisplayImpl extends DisplayImpl {
    private String string;
    private int width;
    public StringDisplayImpl(String string) {
        this.string = string;
        this.width = string.getBytes().length;
    }
    public void rawOpen() {
        printLine();
    }
    public void rawPrint() {
        System.out.println("|" + string + "|");
    }
    public void rawClose() {
        printLine();
    }
    private void printLine() {
        System.out.print("+");
        for (int i = 0; i < width; i++) {
            System.out.print("-");
        }
    }
}
```

Main処理:

```java
public class Main {
    public static void main(String[] args) {
        Display d1 = new Display(new StringDisplayImpl("Hello, Japan."));
        Display d2 = new CountDisplay(new StringDisplayImpl("Hello, World."));
        CountDisplay d3 = new CountDisplay(new StringDisplayImple("Hello, Universe."));
        d1.display();
        d2.display();
        d3.display();
        d3.multiDisplay(5);
    }
}
```

## Bridgeのまとめ

- 登場人物
  - Abstraction: 機能の階層の最上位クラス  
  Implementorのメソッドを使った基本的な機能を定義する。
  - RefinedAbstraction: Abstractionに機能を追加したもの
  - Implementor: 実装の階層の最上位クラス  
  Abstractionのために実装すべきインタフェースを規定する。  
  ここではまだ実装しない。
  - ConcreteImplementor: Implementorの定めたインタフェースを実装する
- どんないいことがある？
  - 階層を分けたことで、機能を追加するとき、必ずしも実装の変更は必要ない  
  元からある実装の組み合わせで作れるのであれば、Implementorの使い方を変えるだけでいい。
- たとえば、OSごとの共通インタフェースを Implementorで定義して、それを継承して実装するとか
