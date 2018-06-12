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
`Zoom Ratio` 是指拍摄的缩放比例，显示效果为：`实际宽|高 / Zoom Ratio`

## 实践说明补充
按照上述说明，在实际操作中我们需要将地图、背景、角色等添加到 `camera` 的 `Targets` 中，而UI元素（血条、装备栏、技能栏之类）这类在屏幕上位置保持不变的元素则不需要添加到 `camera` 的 `Targets` 中  

## camera 关键方法
// TODO addTarget removeTarget getTargets

## camera 对节点世界坐标的影响
// TODO 一些复杂的地方需要具体实验，特别是 prefab 被创建后添加到 camera 中里时的位置表现需要额外注意