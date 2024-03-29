# code_complete part 1

## Books Information

|                        |                                                       |
| :--------------------- | :---------------------------------------------------- |
| タイトル               | CODE COMPLETE 第2版 上 完全なプログラミングを目指して |
| 著者                   | スティーブ・マコネル                                  |
| 電子書籍版データ作成日 | 2014.03.27                                            |

## Memo

- ソフトウェア開発におけるコーディング・デバッグを中心とした作業を「コンストラクション (建設)」とよぶ
- 多くの場合、プロジェクトで最も長い時間を要するのはこのコンストラクションであるため、これを改善することで生産性は非常に上がると筆者は述べている
- コンストラクションの生産性には 10 - 20倍の個人差があるといわれているらしい
- プログラミングにおける最大の課題は、問題を概念化すること
- 種々の問題をよりよく解決するコツとして、メタファ (たとえ) の重要性が説かれている
- コンストラクションに入る前の準備を疎かにすると、後半で変更が多発し高くつく  
一方で、準備を入念にした場合でも誤ることはある。慎重に設計したとしても、それをかたくなに盲信してはならない。
- 顧客の要求は変化するもの  
プロジェクトに長く携わるにつれて、開発者も顧客も課題への理解度が上がっていく。  
そのため、当初と要求が変わってくることはよくある。  
平均的なプロジェクトでは要求のうち約25%が変更されるとのこと。  
それを受け入れるかどうかスケジュール・コストと一緒に判断。
- アーキテクチャ (上位の設計) では、構成だけでなく思想も書くべき
- 設計は予測可能な結果が保証されたプロセスではなく、大雑把なやりかた、試してみないとうまくいくかわからない方法になりやすい
- 作り込まれたオブジェクトは、一度に 1つのことに集中できるように問題を分けている  
複雑な課題をいかにして細分化するか...
- 本が完成するのは、他に追加できるものがなくなったときではなく、これ以上削除するものがなくなったときである  
ソフトウェアも同じ。
- ソフトウェアの複雑化を回避するための工夫
  - 循環依存は避ける  
  クラスAと クラスBが違いに参照しあっている状況のこと。
  - 情報を隠蔽する  
  たとえば、いろんな箇所に登場する共通作業を生の処理として書いていると、変更があったときにそれら全てを修正しなければならない。  
  それを関数として定義していれば影響を抑えられる。
  - グローバル変数には常に関数を通してアクセスする
- クラス・関数を細かく分けていると、パフォーマンスのボトルネックを解消するときにも変更が小規模で済む
- 優れた設計者は将来的に変更されそうな箇所を見抜いていた
  - 業務ルール・ハードウェア依存部分・入出力とか...
  - 三大美徳でいうところの短気かな？
- モジュール間の結合の度合いを測る
  - サイズ  
  インタフェースの大きさ。  
  引数が多いルーチンは結合が強い。関数そのものが多い場合も結合が強い。
  - 可視性  
  モジュール間の結合が目に見えること。  
  どんな引数、どんな戻り値をやりとりするかが見えること。
  - 柔軟性  
  モジュール間結合をどの程度簡単に変更できるか。
- 積極的にデザインパターンを採用しよう  
そのソフトウェアについて理解するまでのステップが短くなる。  
ただしパターンにとらわれないように。
- 設計は反復的なプロセスであると理解する
- クラスの使用法がわからないときの正しい対処プロセス
  1. 実装を見たりしない
  2. 開発者に「わからない」と伝える
  3. 開発者はその場で答えたりしなし
  4. インタフェースの仕様書を書き直して渡す
- クラスのメンバデータが 7つを超えたら把握できなくなると思ったほうがいい
- 継承は適切に使わなければ複雑化を助長する  
階層は 2 - 3くらいに留めておいたほうがいい。
- エラーの数は複雑さに応じて増えていく  
設計を単純化するために、生成するインスタンスの数、呼び出す関数の数などをなるべく小さくする
- クラスは現実のモデルを抽象化するだけでなく、グローバルデータを隠ぺいしたり制御を一元化したりという使い方もある
- 複雑な条件判断は適切な名前で関数化しよう
- 役割があいまいな関数はシンプルなものに比べてエラー混入率が 7倍になっていた
- 凝集度
  - 機能的凝集: sin()とか GetCustomerName()のように処理をひとつだけ実行する  
  基本的にはこれを目指す。
  - 他にもあったが省略する
- 関数名が思い浮かばないときは設計を再考しよう
- 複雑な処理が要求される場合、関数が 100-200行に増えてもいいと書いてあるが、これはおそらく高い凝集度が達成されているという前提があるのだろう
- 好ましくない状態のうち、予想されるものにはエラー処理を、あり得ないはずのものには assertを使うといい
- エラー処理を考えるときには、動作を停止するか、不正確な結果を出してでも処理を継続するかなどを見極める
- ローカルで処理できる例外はその場で処理する
  - 呼び出し元に返すパターンもありだが、それは隠蔽のポリシーと競合してしまう  
  返すなら実装の中身を意識させないように、抽象化する・独自の例外を作るとかの工夫をしたほうがいい。
- 外部からデータをもらったときは検証する
  - そのうえで、内部からもらったときに assertを仕込むといい  
  ここでエラーになったらそれはデータのエラーではなくプログラムのエラー。
- 定数は最初にまとめて宣言してもいいが、変数は使う箇所の近くで宣言する
- 1つの変数に 1つの目的を
  - 悪い例  
    - 変数 pageCountの値は印刷するページ数を表すが、-1の場合はエラーが発生したことを表す
- 変数名は方法 (how) ではなくもの (what) を基準に決めよう  
inputRecより employeeDataのほうが何を指すかわかりやすいよね、みたいな。
- 短い変数名を見つけたら、他に最適な名前がないか考えよう  
研究によると平均 10-16文字の変数名だとデバッグにかかる時間が最も短くなるらしい。  
数行の限られたスコープで使う変数ならあえて短い名前をつけてもいいかも。
  - 同様に、ループ変数には短い名前を使うことも多いが、ループがネストする場合・ループ変数を外でも使う場合は長い名前を付けると理解しやすくなる  
  `employeeIndex` 、 `departmentIndex` とか。
- 変数や関数などの名前は読み手にとって重要なもの
- 処理の本文で使う数字は 0と 1だけにできると読みやすくなる  
つまり、マジックナンバーは全て名前付き定数として用いる。
- 配列の代わりにキューやスタックなどを使うことを提案する勢力がいる  
インデックスでランダムにアクセスしてしまうとエラーが起こりやすい。
- データを構造体 (やクラスなど) にまとめておくと、複数のデータの初期化や交換などが 1行でできる
- 引数を増やすより構造体 (やクラスなど) をひとつ関数に渡すほうが要領がいい  
これは実際やるけど、そのまま隠蔽に反してしまってるかも...
- 肥大化したオブジェクトと付き合い続けるよりはグローバル変数にするほうがマシ
- if文では、正常に処理されるケース・最も一般的なケースを最初に書き、エラーケース・マイナーケースは2番目以降にまとめる  
混在させると正常なパターンでどこを通るのかがわかりにくく、追跡が難しくなる。  
