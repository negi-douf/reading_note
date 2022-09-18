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
