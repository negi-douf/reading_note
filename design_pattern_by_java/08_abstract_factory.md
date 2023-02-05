# Abstract Factory

- abstractな部品を組み合わせて abstractな製品を作る

## サンプルコード 1

目的の出力:

```html
<html><head><title>LinkPage</title></head>
<body>
<h1>LinkPage</h1>
<ul>
<li>
新聞
<ul>
  <li><a href="https://www.asahi.com/">朝日新聞</a></li>
  <li><a gref="https://www.yomiuri.co.jp/">読売新聞</a></li>
</ul>
</li>
<li>
Yahoo!
<ul>
  <li><a href="https://yahoo.com/">Yahoo!</a></li>
  <li><a href="https://yahoo.co.jp/">Yahoo!Japan</a></li>
</ul>
</li>
  <li><a href="https://excite.com/">Excite</a></li>
  <li><a href="https://google.com/">Google</a></li>
</ul>
</li>
</ul>
<hr><address>結城 浩</address></body></html>
```

factoryパッケージ (抽象的な工場・部品・製品を含むパッケージ):

```java
package factory;
import java.io.*;
import java.util.ArrayList;

public abstract class Item {
    protected String caption;
    public Item(String caption) {
        this.caption = caption;
    }
    public abstract String makeHTML();
}

public abstract class Link extends Item {
    protected String url;
    public Link(String caption, String url) {
        super(caption);
        this.url = url;
    }
    // makeHTMLを実装していないから abstract
}

public abstract class Tray extends Item {
    protected ArrayList tray = new ArrayList();
    public Tray(String caption) {
        super(caption);
    }
    public void add(Item item) {
        tray.add(item)
    }
}

public abstract class Page {
    protected String title;
    protected String author;
    protected ArrayList content = new ArrayList();
    public Page(String title, String author) {
        this.title = title;
        this.author = author;
    }
    public void add(Item item) {
        content.add(item);
    }
    public void output() {
        try {
            String filename = title + ".html";
            Writer writer = new FileWriter(filename);
            writer.write(this.makeHTML());
            writer.close();
            System.out.println(filename + " を作成しました");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public abstract String makeHTML();
}
// ここまでが部品

public abstract class Factory {
    public static Factory getFactory(String classname) {
        Factory factory = null;
        try {
            factory = (Factory)Class.forName(classname).newInstance();
        } catch (ClassNotFoundException e) {
            System.err.println("クラス " + classname + "が見つかりません");
        } catch (Exception e) {
            e.printStackTrace();
        }
        return factory;
    }
    public abstract Link createLink(String caption, String url);
    public abstract Tray createTray(String caption);
    public abstract Page createPage(String title, String author);
}
```

パッケージを使う側:

```java
import factory.*;

public class Main {
    public static void main(String[] args) {
        if (args.length != 1) {
            System.out.println("Usage: java Main class.name.of.ConcreteFactory");
            System.out.println("Example 1: java Main listfactory.ListFactory");
            System.out.println("Example 2: java Main tablefactory.TableFactory");
            System.exit(0);
        }
        Factory factory = Factory.getFactory(args[0]);

        Link asahi = factory.createLink("朝日新聞", "https://www.asahi.com/");
        Link yomiuri = factory.createLink("読売新聞", "https://www.yomiuri.co.jp/");

        Link us_yahoo = factory.createLink("Yahoo!", "https://yahoo.com/");
        Link jp_yahoo = factory.createLink("Yahoo!Japan", "https://yahoo.co.jp/");
        Link excite = factory.createLink("Excite", "https://excite.com");
        Link google = factory.createLink("Google", "https://google.com");

        Tray traynews = factory.createTray ("Yahoo!");
        traynews.add(asahi);
        traynews.add(yomiuri);

        Tray trayyahoo = factory.createTray("Yahoo!");
        trayyahoo.add(us_yahoo);
        trayyahoo.add(jp_yahoo);

        Tray traysearch = factory.createTray("サーチエンジン");
        traysearch.add(trayyahoo);
        traysearch.add(excite);
        traysearch.add(google);

        Page page = factory.createPage("LinkPage", "結城 浩");
        page.add(traynews);
        page.add(traysearch);
        page.output();
    }
}
```

listfactoryパッケージ (具体的な工場・部品・製品を含むパッケージ):

```java
package listfactory;
import factory.*;
import java.util.Iterator;

public class ListFactory extends Factory {
    public Link createLink(String caption, String url) {
        return new ListLink(caption, url);
    }
    public Tray createTray(String caption) {
        return new ListTray(caption);
    }
    public Page createPage(String title, String author) {
        return new ListPage(title, author);
    }
}

public class ListLink extends Link {
    public ListLink(String caption, String url) {
        super(caption, url);
    }
    public String makeHTML() {
        return "  <li><a href=\"" + url + "\">" + caption + "</a></li>\n";
    }

    public class ListTray extends Tray {
        public ListTray(String  caption) {
            super(caption);
        }
        public String makeHTML() {
            StringBuffer buffer = new StringBuffer();
            buffer.append("<li>\n");
            buffer.append(caption + "\n");
            buffer.append("<ul>\n");
            Iterator it = tray.iterator();
            while (it.hasNext()) {
                Item item = (Item)it.next();
                buffer.append(item.makeHTML());
            }
            buffer.append("</ul>\n");
            buffer.append("</li>\n");
            return buffer.toString();
        }
    }

    public class ListPage extends Page {
        public ListPage(String title, String author) {
            super(title, author);
        }
        public String makeHTML() {
            StringBuffer buffer = new StringBuffer();
            buffer.append("<html><head><title>" + title + "</title></head>\n");
            buffer.append("<body>\n");
            buffer.append("<h1>" + title + "</h1>\n");
            buffer.append("<ul>\n");
            Iterator it = content.iterator();  // 継承したフィールド
            while (it.hasNext()) {
                Item item = it.next();
                buffer.append(item.makeHTML());
            }
            buffer.append("</ul>\n");
            buffer.append("<hr><address>" + author + "</address>");
            buffer.append("</body></html>\n");
            return buffer.toString();
        }
    }
}
```

tablefactoryパッケージ (具体的な工場・部品・製品を含むパッケージ):

```java
package tablefactory;
import factory.*;
import java.util.iterator;

public class TableFactory extends Factory {
    public link createLink(String caption, String url) {
        return new TableLink(caption, url);
    }
    public Tray createTray(String caption) {
        return new TableTray(caption);
    }
    public Page createPage(String title, String author) {
        return new TablePage(title, author);
    }
}

public class TableLink extends Link {
    public TableLink(String caption, String url) {
        super(caption, url);
    }
    public String makeHTML() {
        return "<td><a href=\"" + url + "\">" + caption + "</a></td>\n";
    }
}

public class TableTray extends Tray {
    public TableTray(String caption) {
        super(caption);
    }
    public String makeHTML() {
        StringBuffer buffer = new StringBuffer();
        buffer.append("<td>");
        buffer.append("<table width=\"100%\" border=\"1\"><tr>");
        buffer.append("<td bgcolor=\"#cccccc\" align=\"center\" colspan=\"" + tray.site() + "\"><b>" + caption + "</b></td>");
        buffer.append("</tr>\n");
        buffer.append("<tr>\n");
        Iterator it = tray.iterator();
        while (it.hasNext()) {
            Item item = (Item)it.next();
            buffer.append(item.makeHTML());
        }
        buffer.append("</tr></table>");
        buffer.append("</td>");
        return buffer.toString();
    }
}

public class TablePage extends Page {
    public TablePage(String title, String author) {
        super(title, author);
    }
    public String makeHTML() {
        StringBuffer buffer = new StringBuffer();
        buffer.append("<html><head><title>" + title + "</title></head>\n");
        buffer.append("<body>\n");
        buffer.append("<h1>" + title + "</h1>\n");
        buffer.append("<table width=\"80%\" border=\"3\">\n");
        Iterator it = content.iterator();
        while (it.hasNext()) {
            Item item = (Item)it.next();
            buffer.append("<tr>" + item.makeHTML() + "</tr>");
        }
        buffer.append("</table>\n");
        buffer.append("<hr><address>" + author + "</address>");
        buffer.append("</body></html>\n");
        return buffer.toString();
    }
}
```

### Abstract Factoryのまとめ

- 登場人物
  - AbstractProduct: 抽象的な部品・製品のインタフェースを定める
  - AbstractFactory: AbstractProductのインスタンスを作るためのインタフェースを定める
  - Client: AbstractFactoryと AbstractProductのインタフェースを使って仕事をする
  - ConcreteProduct: AbstractProductのインタフェースを実装する
  - ConcreteFactory: AbstractFactoryのインタフェースを実装する
- 新たな工場を追加しても、Client側を変える必要がない
- 部品の種類を追加するのは大変
- 使う側は「どのセットか」のみを意識していて、「どの部品を組み合わせるか」を知らなくても使える
