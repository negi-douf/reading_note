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