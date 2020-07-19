---
title: "javascript のラインチャートライブラリをいくつく見てみた"
date: 2020-07-03T00:36:00+09:00
description: "ラインチャートを使う必要性が出たのでいくつか見てみた。それだけ。"
tags: ["javascript", "chart"]
---

## C3.js と Billboard.js

- 下記は Billboard.js を利用した。C3.js も可能。

```html
<body>
<head>
<script src="https://d3js.org/d3.v5.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/billboard.js/2.0.0-next.7/billboard.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/billboard.js/2.0.0-next.7/billboard.css">
</head>
<body>
<div id="timeseries"></div>
<script>
var chart = bb.generate({
    data: {
        x: 'x',
        xFormat: '%Y-%m-%dT%H:%M:%S.%LZ',
        // columns: [
        //     [
        //         'x',
        //         '2013-01-01T00:00:00.000Z',
        //         '2013-01-01T02:02:00.000Z',
        //         '2013-01-01T04:04:00.000Z',
        //         '2013-01-01T08:08:00.000Z',
        //         '2013-01-01T16:16:00.000Z'
        //     ],
        //     ['data1', 30, 200, 100, 400, 150],
        //     ['data2', 130, 340, 200, 500, 250]
        // ]
        rows: [
            ["x", "data1", "data2"],
            ["2013-01-01T00:00:00.000Z",  30, 130],
            ["2013-01-01T02:02:00.000Z", 200, 340],
            ["2013-01-01T04:04:00.000Z", 100, 200],
            ["2013-01-01T08:08:00.000Z", 400, 500],
            ["2013-01-01T16:16:00.000Z", 150, 250]
        ]
    },
    axis: {
        x: {
            type: 'timeseries',
            tick: {
                // format: '%Y-%m-%d %H:%M:%S'
                format: '%H:%M'
            }
        }
    },
    bindto: "#timeseries"
});
</script>
</body>
</html>
```
