>https://mp.weixin.qq.com/s?__biz=MzU3NDc1NDAzNQ==&mid=2247484800&idx=1&sn=9467806a52345974cf8bb8bc15b1a92d&chksm=fd2cd81cca5b510a63dc6f102ba2ec8e89b37f448b7ad0de880a2190b1540de9645ec0e2656c&cur_album_id=2005075592806793217&scene=190#rd

#### mapboxgl 地图样式 - 唯一值渲染


##### 方式一：使用 case 表达式
这种做法的好处是可以灵活修改每个区的颜色。

```js
"fill-color":[
    "case",
    ["boolean",["==",["get","name"],"怀柔区"],false],"#FFFFCC",
    ["boolean",["==",["get","name"],"密云区"],false],"#CCFFFF",
    ["boolean",["==",["get","name"],"平谷区"],false],"#FFCCCC",
    ["boolean",["==",["get","name"],"通州区"],false],"#FFFF99",
    ["boolean",["==",["get","name"],"房山区"],false],"#CCCCFF",
    ["boolean",["==",["get","name"],"延庆区"],false],"#FFCC99",
    ["boolean",["==",["get","name"],"门头沟区"],false],"#CCFF99",
    ["boolean",["==",["get","name"],"大兴区"],false],"#66CCFF",
    ["boolean",["==",["get","name"],"顺义区"],false],"#99CCFF",
    ["boolean",["==",["get","name"],"海淀区"],false],"#CCCCCC",
    ["boolean",["==",["get","name"],"西城区"],false],"#CCFFCC",
    ["boolean",["==",["get","name"],"东城区"],false],"#CC99CC",
    ["boolean",["==",["get","name"],"朝阳区"],false],"#99CC99",
    ["boolean",["==",["get","name"],"石景山区"],false],"#CCCC99",
    ["boolean",["==",["get","name"],"昌平区"],false],"#FF9969",
    ["boolean",["==",["get","name"],"丰台区"],false],"#999999",
    "black"
]
```

上面表达式的意思是，从数据中获取 name 属性的值，判断是哪个区，然后设置相应的颜色。

case表达式语法，详见官方说明：https://docs.mapbox.com/mapbox-gl-js/style-spec/expressions/#case

> mapboxgl 表达式的基本语法：
1、一组中括号[ ]代表一个完整的表达式，中括号中第一个参数声明表达式的类型，后面是表达式的参数。
2、表达式可以嵌套。
关于表达式的详细介绍，后续会用单独一篇文章来写，这里只做个简单说明。

在线示例：http://gisarmory.xyz/blog/index.html?demo=mapboxglStyleUniqueValue1

##### 方式二：使用 match 表达式
```js
"fill-color":[
    "match",
    ["get","name"],
    "怀柔区","#FFFFCC",
    "密云区","#CCFFFF",
    "平谷区","#FFCCCC",
    "通州区","#FFFF99",
    "房山区","#CCCCFF",
    "延庆区","#FFCC99",
    "门头沟区","#CCFF99",
    "大兴区","#66CCFF",
    "顺义区","#99CCFF",
    "海淀区","#CCCCCC",
    "西城区","#CCFFCC",
    "东城区","#CC99CC",
    "朝阳区","#99CC99",
    "石景山区","#CCCC99",
    "昌平区","#FF9969",
    "丰台区","#999999",
    "black"
]
```

在线示例：http://gisarmory.xyz/blog/index.html?demo=mapboxglStyleUniqueValue2


##### 方式三：根据 id 匹配颜色
```js
"fill-color":[
    "to-color",[
        "at",
        ["%", ["get", "adcode"], 13],
        ["literal",["#00FFCC","#CCFFFF","#FFCCCC","#FFFF99","#CCCCFF","#FFCC99","#CCFF99","#66CCFF","#99CCFF","#CCFFCC","#99CC99","#CCCC99","#FF9969"]]
    ]
]
```

上面表达式的意思是，会用数据中 adcode 的属性值除以13然后取余数，根据余数从颜色数组中取一个颜色。


翻译成js是下面这样:
```js
function getColor(feature){	//feature是geojosn格式的Feature
    const colors = ["#00FFCC","#CCFFFF","#FFCCCC","#FFFF99","#CCCCFF","#FFCC99","#CCFF99","#66CCFF","#99CCFF","#CCFFCC","#99CC99","#CCCC99","#FF9969"]
    const index = feature.properties.adcode % 13
    return colors[index]
}
```
在线示例：http://gisarmory.xyz/blog/index.html?demo=mapboxglStyleUniqueValue3


##### 方式四：从属性中取颜色
这种方式比较简单，就是直接把颜色放到数据中，通过 get 表达式获取出来直接用。缺点就是要去改数据。

在线示例：http://gisarmory.xyz/blog/index.html?demo=mapboxglStyleUniqueValue4

##### 方式五：分图层设置颜色
通过图层的 filter筛选条件实现，每个区设置成一个图层，然后设置每个图层的颜色。

图层样式设置方式：

```js
[
    {
        "id": "beijing-haidian",
        "type": "fill",
        "source": "beijing",
        "paint":{
            "fill-color":"#FFCC99"
        },
        "filter":["==",["get","name"], "海淀区"]
	},
    {
        "id": "beijing-chaoyang",
        "type": "fill",
        "source": "beijing",
        "paint":{
            "fill-color":"#FFCCCC"
        },
        "filter":["==",["get","name"], "朝阳区"]
    }
    ...
]
```
这种方式的缺点也很明显：图层由一个变成了16个，style 文件会变的很啰嗦，图层管理不太方便。

在线示例：http://gisarmory.xyz/blog/index.html?demo=mapboxglStyleUniqueValue5
