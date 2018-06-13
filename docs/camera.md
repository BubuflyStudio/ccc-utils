# cocos creator camera 相关
cocos creator 中的 camera 的处理还是比较方便的，但是由于官方文档在此并未细说，所以会有一些坑，这里记录 camera 使用中需要注意的点，供之后的开发参考。

## camera 关键属性
在给节点添加 `camera` 组件时，会发现 `camera` 组件下有两个属性，分别是 `Targets` 与 `Zoom Ratio`，此外影响 `camera` 逻辑的还有一个属性就是其 `position`。  

通过调节 `camera` 的这三个属性，即可控制 `camera` 的拍摄行为，这里介绍这三个属性在 `camera` 使用中的具体影响。

### position
`camera` 的位置，以此位置为拍摄中心，

### Targets 追踪对象
1. 当一个节点添加到 `camera` 的 `Targets` 列表中时，其本身与其子节点都将被该 `camera` 追踪  
1. 只有被追踪的对象，才会对 `camera` 的 `position` 变动做出反应。比如将 `camera` 的 `position` 向下移动时，只有位于 `Targets` 中的节点才会向上移动（显示效果上向上移动，实际 `position` 不受 `camera` 影响），而不在 `Targets` 中的节点则会保持在窗口中显示位置不变不受 `camera` 移动的影响。

### position
`camera` 的 `position` 表示该 `camera` 所拍摄的中心位置，修改该值即可实现镜头移动效果。

### Zoom Ratio 缩放比例
`Zoom Ratio` 是指拍摄的缩放比例，具体为宽高的放大倍数，即 `Zoom Ratio` 越大，`Targets` 中的节点显示的大小越大，`camera` 显示的区域越小

## 实践说明补充
按照上述说明，在实际操作中我们需要将地图、背景、角色等添加到 `camera` 的 `Targets` 中，而UI元素（血条、装备栏、技能栏之类）这类在屏幕上位置保持不变的元素则不需要添加到 `camera` 的 `Targets` 中  

## camera 关键方法
### addTarget
接受一个节点作为参数，将节点添加到 `camera` 的 `Targets` 中

```js
camera.addTarget(nodeA);
```

### removeTarget
### addTarget
接受一个节点作为参数，将节点从 `camera` 的 `Targets` 中移除

```js
camera.addTarget(nodeA);
```

### getTargets
获取所有摄像机目标节点

```js
const nodes = camera.getTargets();
```

## camera 对节点坐标的影响
当使用 `addTarget` 将节点添加到 `camera` 中时，节点的节点坐标与世界坐标均不会变，但是渲染方式会按照 `camera` 的 `position` 与 `Zoom Ratio` 对应的规则进行显示。

当使用 `prefab` 创建一个节点并添加到被 `camera` 所追踪的节点下时，TODO：表现情况有待实验