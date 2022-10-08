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