# Builder

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
