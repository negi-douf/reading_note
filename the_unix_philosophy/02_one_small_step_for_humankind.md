# 02: One Small Step for Humankind

## Small is Beautiful

プログラムを書くときは小さなものから始めて、小さいまま保っておく  
案外、本当に必要な部分より周辺処理のほうが大きくなることが多い  
**本当に** その機能が必要か、そのオプションは必須なのか、しっかりと考えよう

## やさしいソフトウェア工学

小さいことによるメリットとは何だろう？  

- わかりやすい  
特定の目的にフォーカスされているから
- 保守しやすい
- システムリソースにやさしい  
占有するメモリが小さくて済む
- 他のツールと組み合わせやすい  
巨大なツールはわかりもしない未来をあれこれ考えて肥大化させている  
一方、小さなツールは不確実な未来のことを最初から諦めている  
たくさん予測してもどのみち想定外の未来になる  
ならば、最初から変更に素早く対応できるよう小さくとどめておいたほうがいい

## ひとつのプログラムにはひとつのことをうまくやらせる

lsコマンドを再実装することを考えてみる  
ファイル・ディレクトリのリストを、ある程度フォーマットを調整しつつ表示できる  
ただ、この整形は lsに **必要** な機能だろうか？  
lsはファイル・ディレクトリを行単位で出力することにフォーカスできるのではないか？  
そのうえで、フォーマットは別のコマンドでやらせればいい  

ひとつのことに集中しようとすると、やりたいことの本質に近づくことができる