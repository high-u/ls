---
title: "JavaScript で中心点と半径から多角形の頂点を算出する"
date: 2020-07-11T10:34:27+09:00
description: "最終的には多角形を描画するのだがその手前で、中心点と半径から正六角形の頂点を算出する必要が出たのでやってみた"
tag: ["javascript", "polygon"]
---

## 六角形と二十四角形

- この記事を参考にさせてもらいました。
  - [中心点、半径から正六角形と正二十四角形の座標を算出 | エンジニアの雑記 ～androidとかjava辺りの開発とか～](https://ameblo.jp/shockyeah/entry-10980569328.html)
- 今回の目的は、算出だけだけど、数値だけ見せられても分からないので、svg で描画もした。
- ここに置いた。
  - [CalculatePolygonWithJs - CodeSandbox](https://codesandbox.io/s/calculatepolygonwithjs-qehm2)

```js
hexagon();
polygon24();

function hexagon() {
  const p = document.getElementById("hexagon");
  const pointsString = convertPointsToString(getPolygonPoints(6, 50, 100, 100));
  p.setAttribute("points", pointsString);
}

function polygon24() {
  const p = document.getElementById("polygon24");
  const pointsString = convertPointsToString(
    getPolygonPoints(24, 50, 100, 100)
  );
  p.setAttribute("points", pointsString);
}

/**
 * ポリゴンの頂点を二次元配列で取得する
 * @param {n} num - 頂点の数（六角形なら `6`）
 * @param {r} num - 半径
 * @param {x} num - 中心点 x
 * @param {y} num - 中心点 y
 */
function getPolygonPoints(n, r, x, y) {
  const angle = Math.PI / 2.0;
  const points = [];
  for (let i = 0; i < n; i++) {
    const px = x + r * Math.cos((2.0 * i * Math.PI) / n + angle);
    const py = y - r * Math.sin((2.0 * i * Math.PI) / n + angle);
    points.push([px, py]);
  }
  console.log(points);
  return points;
}

function convertPointsToString(points) {
  return points.map((e, i, a) => (e = e.join(","))).join(" ");
}
```

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Parcel Sandbox</title>
    <meta charset="UTF-8" />
  </head>
  <body>
    <svg height="200" width="200">
      <polygon
        id="hexagon"
        points=""
        style="fill:whitesmoke;stroke:black;stroke-width:1"
      />
    </svg>
    <svg height="200" width="200">
      <polygon
        id="polygon24"
        points=""
        style="fill:whitesmoke;stroke:black;stroke-width:1"
      />
    </svg>
    <script src="src/index.js"></script>
  </body>
</html>
```
