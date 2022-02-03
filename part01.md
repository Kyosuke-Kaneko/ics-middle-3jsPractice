# canvasのリサイズ
Three.jsでの最適なリサイズ処理の方法を紹介します。この手順を覚えれば次のような希望に対応できます。

 - どんなディスプレイでもThree.jsを綺麗に見せたい
 - 異なるディスプレイでも全画面に3Dを表示させたい
 - ブラウザのサイズを変更してもThree.jsを全画面にフィットさせたい

 ## 設定方法
 ### metaタグ
 HTMLのmetaタグにviewportの設定をします。

<meta name="viewport" content="width=device-width, initial-scale=1"/>
スマートフォンのブラウザーには横幅の大きなPCサイトも柔軟に見れるよう、自動的に拡大縮小する機能があります。

width=device-widthと指定することでデバイスの最適な幅で表示させるようにします。また、初期状態で拡大・縮小していない見え方をするためにinitial-scale=1を指定します。

### スタイルシート
bodyタグに余白が存在するので、それを消すようにmargin: 0を指定します。

```css
<style>
  body {
    margin: 0;
    overflow: hidden;
  }
</style>
```

また、overflow: hiddenを指定すると、macOSのデスクトップブラウザのオーバースクロール（たとえば画面上部にスクロールすると、画面上部を一瞬超えて表示される挙動）を抑制できます。全画面コンテンツを作るときにオーバースクロールの挙動は必要ないので、overflow: hiddenを指定しておきましょう。

## Three.jsの調整
resizeイベントを監視し、画面サイズであるwindow.innerWidthとwindow.innerHeightの値を使います。

```js
// 初期化のために実行
onResize();
// リサイズイベント発生時に実行
window.addEventListener('resize', onResize);

function onResize() {
  // 画面サイズを取得
  const width = window.innerWidth;
  const height = window.innerHeight;

  // レンダラーのサイズを調整する
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(width, height);

  // カメラのアスペクト比を正す。リサイズ時にはカメラの縦横比調整
  camera.aspect = width / height;
  camera.updateProjectionMatrix();
}
```

実装のポイントは次の通りです。

 - リサイズ時にはレンダラーのサイズをsetSizeメソッドで画面幅に合わせること
 - デスクトップでは、メインディスプレイ・サブディスプレイでPixelRatioの異なる可能性があるので、リサイズイベントでsetPixelRatioメソッドでを使って更新するべき
 - リサイズ時にはカメラの縦横比が狂うので、リサイズ時に縦横比を正しく調整する
 - 画面サイズの設定処理は、初期化時もリサイズ時も同じ
 - onResize関数は初期化時とリサイズイベント発生の両方で呼び出す
