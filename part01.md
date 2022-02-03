# canvasのリサイズ
Three.jsでの最適なリサイズ処理の方法を紹介します。この手順を覚えれば次のような希望に対応できます。

 - どんなディスプレイでもThree.jsを綺麗に見せたい
 - 異なるディスプレイでも全画面に3Dを表示させたい
 - ブラウザのサイズを変更してもThree.jsを全画面にフィットさせたい

 ## 設定方法
 HTMLのmetaタグにviewportの設定をします。

<meta name="viewport" content="width=device-width, initial-scale=1"/>
スマートフォンのブラウザーには横幅の大きなPCサイトも柔軟に見れるよう、自動的に拡大縮小する機能があります。

width=device-widthと指定することでデバイスの最適な幅で表示させるようにします。また、初期状態で拡大・縮小していない見え方をするためにinitial-scale=1を指定します。