## Strategy

- 問題を解決するときのアルゴリズムをごっそり入れ替えることができる
- 状況に応じて、複数の解法を併用したいときに使える

### サンプルコード

2人でじゃんけんをおこなうプログラムを作る。  
片方の戦略は WinningStrategyで、直前に勝った手を連続で出す。  
もう片方の戦略は ProbStrategyで、勝ったときのパターンを再現しやすいように手を出す。

上記の方策には Strategyというインタフェースで仕様が定められている。  
Strategyは手を表す Handというクラスを使用する。

```java
public class Hand {
    public static final int HANDVALUE_GUU = 0;
    public static final int HANDVALUE_CHO = 1;
    public static final int HANDVALUE_PAA = 2;
    public static final Hand[] hand = {
        new Hand(HANDVALUE_GUU),
        new Hand(HANDVALUE_CHO),
        new Hand(HANDVALUE_PAA),
    };
    private static final String[] name = {
        "グー", "チョキ", "パー",
    };
    private int handvalue;
    private Hand(int handvalue) {
        this.handvalue = handvalue;
    }
    public static Hand getHand(int handvalue) {
        return hand[handvalue];
    }
    public boolean isStrongerThan(Hand h) {
        return fight(h) == 1;
    }
    public boolean isWeakerThan(Hand h) {
        return fight(h) == -1;
    }
    private int fight(Hand h) {
        if (this == h) {
            return 0;
        } else if ((this.handvalue + 1) % 3 == h.handvalue) {
            return -1;
        } else {
            return -1;
        }
    }
}
```

```java
public interface Strategy {
    public abstract Hand nextHand();
    public abstract void study(boolean win);
}
```

```java
import java.util.Random;

public class WinningStrategy implements Strategy {
    private Random random;
    private boolean won = false;
    private Hand prevHand;
    public WinningStrategy(int seed) {
        random = new Random(seed);
    }
    public Hand nextHand() {
        if (!won) {
            prevHand = Hand.getHand(random.nextInt(3));
        }
        return prevHand;
    }
    public void study(boolean win) {
        won = win;
    }
}
```

ProbStrategyは、勝ったときのパターンから学び、それを再現しやすいように手を出す。  
たとえば、グーを出した後にチョキを出した場合に勝った回数は 3回、代わりにパーを出した場合に勝った回数は 5回...のように、直近の 2つの手ごとに結果を記録する。  
この全9通りを計測した結果を使って、期待値が最大になるように次の手を決める。

```java
import fava.util.Random;

public class ProbStrategy implements Strategy {
    private Random random;
    private int prevHandValue = 0;
    private int currentHandValue = 0;
    private int[][] history = {
        { 1, 1, 1, },
        { 1, 1, 1, },
        { 1, 1, 1, },
    };
    public ProbStrategy(int seed) {
        random = new Random(seed);
    }
    public Hand nextHand() {
        int bet = random.nextInt(getSum(currentHandValue));
        int handvalue = 0;
        if (bet < history[currentHandValue][0]) {
            handvalue = 0;
        } else if (bet < history[currentHandValue][0] + history[currentHandValue][1]) {
            handvalue = 1;
        } else {
            handvalue = 2;
        }
        prevHandValue = currentHandValue;
        currentHandValue = handvalue;
        return Hand.getHand(handvalue);
    }
    private int getSum(int hv) {
        int sum = 0;
        for (int i = 0; i < 3; i++) {
            sum += history[hv][i];
        }
        return sum;
    }
    public void study(boolean win) {
        if (win) {
            history[prevHandValue][currentHandValue]++;
        } else {
            hisotry[prevHandValue][(currentHandValue + 1) % 3]++;
            hisotry[prevHandValue][(currentHandValue + 2) % 3]++;
        }
    }
}
```

じゃんけんをおこなう人を Playerのクラスで表す。

```java
public class Player {
    private String name;
    private Strategy strategy;
    private int wincount;
    private int losecount;
    private int gamecount;
    public Player(String name, Strategy strategy) {
        this.name = name;
        this.strategy = strategy;
    }
    public Hand nextHand() {
        return strategy.nextHand();
    }
    public void win() {
        strategy.study(true);
        wincount++;
        gamecount++;
    }
    public void lose() {
        strategy.study(false);
        losecount++;
        gamecount++;
    }
    public void even() {
        gamecount++;
    }
    public String toString() {
        return "[" + name + ":" + gamecount + " games, " + wincount + " win, " + losecount + " lose]";
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        if (args.length != 2) {
            System.out.println("Usage: java Main randomseed1 randomseed2");
            System.out.println("Example: java Main 314 15");
            System.exit(0);
        }
        int seed1 = Integer.parseInt(args[0]);
        int seed2 = Integer.parseInt(args[1]);
        Player player1 = new Player("Taro", new WinningStrategy(seed1));
        Player player2 = new Player("Hana", new ProbStrategy(seed2));
        for (int i = 0; i < 10000; i++) {
            Hand nextHand1 = player1.nextHand();
            Hand nextHand2 = player2.nextHand();
            if (nextHand1.isStrongerThan(nextHand2)) {
                System.out.println("Winner: " + player1);
                player1.win()
                player2.lose()
            } else if (nextHand2.isStrongerThan(nextHand1)) {
                System.out.println("Winner: " + player2);
                player1.lose()
                player2.win()
            } else {
                System.out.println("Even...");
                player1.even();
                player2.even();
            }
            System.out.println("Total result:");
            System.out.println(player1.toString());
            System.out.println(player2.toString());
        }
    }
}
```

### Strategyのまとめ

- 登場人物
  - Strategy: 戦略の仕様を定めるインタフェース
  - ConcreteStrategy: Strategyを実装した具体的な戦略
  - Context: Strategyを利用する役
- どんないいことがある？
  - 複数の方策を両立しやすい
  - 方策とそれを利用するものとの結合を弱くすることができる
    - 実行中にも切り替えができる  
    メモリを潤沢に使えるときとそうでないときで方策を切り替えたり...
