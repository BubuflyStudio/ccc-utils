# cocos creator 坐标相关
cocos creator 中的坐标转换是比较繁琐，但又会经常被用到的一个地方，这里主要记录这块常用的函数与属性作为之后开发的参考。

## 相关属性
### 坐标系
cocos creator 的坐标系是笛卡尔坐标系：  
1. 向上与向右是正方向  
1. 原点位于左下角

### position
1. 具体来说应该是 `node.position`  
1. 该属性的值与在 cocos creator 的节点属性面板中的配置一致  
1. 为该节点相对于其父节点所在的位置（相对坐标）  
1. `position` 的位置实际就是 `anchor` 锚点所在的位置

### 节点坐标系
以特定的节点为原点构建的坐标系，例如节点的 `position` 属性即为以其父节点为原点下的坐标系中的坐标

### 世界坐标系
原点为编辑器场景中的 (0, 0) 位置为原点构建的坐标系，当需要比较不同节点下的子节点的坐标时，可以先获取其世界坐标系中的坐标，再进行比较。

## 相关函数
### convertToNodeSpaceAR
将一个世界坐标转换为一个节点下的坐标，例如:  

```js
// 获取世界坐标 (100, 100) 相对于 nodeA 的坐标
const worldPos = cc.v2(100, 100);
const nodePos = nodeA.convertToNodeSpaceAR(worldPos);
```
这里将一个世界坐标 `(100, 100)`，转化为以 `nodeA` 的锚点为原点下的新坐标  
这里假设 `nodeA` 的世界坐标为 `(50, 200)`，那么得到的 `nodePos` 的值则为 `(50, -100)`

### convertToWorldSpaceAR
将一个节点下的坐标转换为世界坐标，例如：

```js
// 获取 nodeA 的世界坐标
const nodeAWorldPos = nodeA.parent.convertToWorldSpaceAR(nodeA.position);
```
这里为获取 `nodeA` 的世界坐标，需要注意的是由于 `nodeA.position` 是指 `nodeA` 相对于其父节点的坐标，所以这里的 `convertToWorldSpaceAR` 操作是由 `nodeA.parent` 来执行的

### convertToNodeSpace
该方法将一个世界坐标转为相对于一个节点左下角的坐标，例如：

```js
// 获取世界坐标 (100, 100) 相对于 nodeA 左下角的坐标
const worldPos = cc.v2(100, 100);
const nodePos = nodeA.convertToNodeSpace(worldPos);
```
这里将一个世界坐标 `(100, 100)`，转化为以 `nodeA` 的左下角为原点下的新坐标  
这里假设 `nodeA` 的世界坐标为 `(50, 200)`，大小为 `(200, 300)`，锚点配置为 `(0.5, 0.5)`，由此可知其左下角的世界坐标为 `(-50, 50)`，所以得到的 `nodePos` 的值为 `(150, 50)`

该方法适用于想直接获取相对于某节点左下角坐标的场景，避免计算节点左下角位置的繁琐过程。

### convertToWorldSpace
该方法将一个相对于节点左下角的坐标转换为世界坐标，例如：

```js
// 坐标 (100, 100) 是相对于 nodeA 左下角的坐标，现获取该坐标的世界坐标
const nodePos = cc.v2(100, 100);
const worldPos = nodeA.convertToWorldSpace(nodePos);
```
这里将一个以 `nodeA` 左下角为原点的坐标系下的坐标 `(100, 100)`，转换为世界坐标  
这里假设 `nodeA` 的左下角世界坐标为 `(150, 50)`，那么得到的 `worldPos` 即为 `(250, 150)`

## 补充说明
上述方法的具体表现均经过实际的实验得出，由于这块的繁琐网上的大部分总结（亦有cocos2dx的相同名称方法）要么不够完善，要么含糊其辞，或者干脆就是错误的结果。在使用之前，建议先自己试验一遍不要想当然的去做。