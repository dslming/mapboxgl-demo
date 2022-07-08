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


```js
class LineStyleLayer {
    createBucket(){

    }
}

class WorkerTile{
    parse(){
        const bucket = buckets[LineStyleLayer.id] = LineStyleLayer.createBucket({
                    index: featureIndex.bucketLayerIDs.length,
                    layers: family,
                    zoom: this.zoom,
                    canonical: this.canonical,
                    pixelRatio: this.pixelRatio,
                    overscaling: this.overscaling,
                    collisionBoxArray: this.collisionBoxArray,
                    sourceLayerIndex,
                    sourceID: this.source,
                    enableTerrain: this.enableTerrain,
                    projection: this.projection.spec,
                    availableImages
                });
                bucket.populate(features, options, this.tileID.canonical, this.tileTransform);
    }
}
```


```js
class LineBucket{

}
```
