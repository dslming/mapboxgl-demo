```js
this._map.addLayer({
    id: this._playId,
    type: "line",
    source: this._playId,
    layout: {
        "line-cap": "round",
        "line-join": "round"
    },
    paint: {
        "line-color": "#aaaaaa",
        "line-width": 10
    }
});
```

```js
addLayer() =>

createStyleLayer() {
  new LineStyleLayer()
}
```

渲染：

```js
class Painter {
    render() {
        this.renderLayer();
    }

    renderLayer() {
        drawLine();
    }
}
```

```js
class Map {
    _update() {
        Painter.render();
    }
}
```
