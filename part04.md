# Three.jsの高速化：ジオメトリの結合
## 前提
GPUを用いるコンテンツにおいてドローコール（描画の命令）の回数はパフォーマンスに大きく影響します。ドローコールの回数が少なくすれば、描画の負荷が下がります。これはThree.jsに限らずWebGL・OpenGLの一般的な知識として覚えておいてください。

## Three.jsでのジオメトリの結合
大量の3Dオブジェクトを表示する場合は、3Dオブジェクトのジオメトリ（3D形状における頂点の座標群）を結合することによってGPUに対するドローコールを少なくできます。<br>
3Dオブジェクトを一個一個3D空間に追加して表示するよりは、大量の立方体をまとめた巨大な3Dオブジェクトを1個だけ3D空間に追加したほうが負荷は少なくなります。

冒頭の最適化前のデモは一個一個の立方体に対してドローコールが発生しています。sceneオブジェクトに追加されているメッシュの個数が多いです。

```js
// 1辺あたりに配置するオブジェクトの個数
const CELL_NUM = 20;

// 共通マテリアル
const material = new THREE.MeshNormalMaterial();
// Box
for (let i = 0; i < CELL_NUM; i++) {
  for (let j = 0; j < CELL_NUM; j++) {
    for (let k = 0; k < CELL_NUM; k++) {
      // 立方体個別の要素を作成
      const mesh = new THREE.Mesh(
        new THREE.BoxGeometry(5, 5, 5),
        material
      );

      // XYZ座標を設定
      mesh.position.set(
        10 * (i - CELL_NUM / 2),
        10 * (j - CELL_NUM / 2),
        10 * (k - CELL_NUM / 2)
      );

      // メッシュを3D空間に追加
      scene.add(mesh);
    }
  }
}
```

Three.jsでは、THREE.BufferGeometryUtils.mergeBufferGeometries()メソッドで結合できます。このメソッドはThree.js本体のコードに含まれていないので注意ください。公式GitHubのexamples/js/utilsフォルダーにJavaScriptファイルがあるので、これをscript要素で読み込みます。作業用フォルダーにBufferGeometryUtils.jsファイルをコピーしておきましょう。該当ファイルはこちらからダウンロードできます。