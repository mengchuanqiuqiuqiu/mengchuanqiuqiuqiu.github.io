---
title: 二、构建Three.js应用的基本组件
categories: ThreeJS学习
hidden: true
tags:
  - 前端
  - 学习
  - 记录
---

## 基本概念

### 基本对象

threejs 中有三个基本对象，分别是场景(scene)、摄像机(camera)和渲染器(renderer)。其中场景是一个**容器**，用于保存跟踪所要渲染的物体和使用的光源，如果没有 scene 对象，那么 threejs 将无法渲染任何物体。threejs。  
THREEJS.SCENE 有时也被称为场景图，可以用来保存所有图形场景的必要信息。它不仅仅是一个对象数组，还包含了场景图树形结构中的**所有节点**。

### Scene 的一些基本方法

```js
// 创建Scene
const scene = new THREE.Scene();
// 向场景中添加对象
scene.add("对象");
// 移除场景中的对象
scene.remove("对象");
// 获取场景中所有的对象列表
scene.children;
// 通过名称获取特定的对象
scene.getObjectByName("name");

// 遍历执行每一个子对象
// 将一个方法当参数传给 traverse()，这个方法会在每一个子对象上执行
scene.traverse(func);
```

### 给场景添加雾化效果

使用 fog 属性就可以为整个场景添加雾化效果。  
雾化效果：**场景中的物体离摄像机越远就越模糊**

```js
const scene = new THREE.Scene();

// 雾化方式一，雾的浓度随距离线性增长
// Fog(color,near,far)
scene.fog = new THREE.Fog(0xffffff, 0.015, 100);

// 雾化方式二，雾的浓度随距离指数增长
// FogExp2(color,浓度)
scene.fog = new THREE.FogExp2(0xffffff, 0.01);
```

### 强制场景中所有物体使用相同材质

```js
const scene = new THREE.Scene();
scene.overrideMaterial = new THREE.MeshLambertMaterial({ color: 0xfffffff });
```

## 几何体和网格

在前面的例子中我们已经使用了几何体和网格。

```js
const sphereGeomerty = THREE.SphereGeometry(4, 20, 20);
const sphereMaterial = THREE.MeshBasicMaterial({ color: 0xffffff });
const sphere = new THREE.Mesh(sphereGeomerty, sphereMaterial);
```

1. 通过 THREE.SphereGeometry 定义了物体的形状。
2. 使用 THREE.MeshBasicMeterial 定义了物体的外观和材质。
3. 将他们合并添加到场景的网格中: THREE.Mesh

### 几何体的属性和方法

和其他大多数三位库一样，在 Three.js 中的几何体基本上都是三维空间的点集和将这些点链接起来的面。
当使用 Threejs 库提供的几何体时不需要自己定义几何体的所有顶点和面。  
我们也可以通过自定义顶点和面来创建几何体,如下图，我们创建一个拥有八个顶点和十二个三角形面的立方体

```js
const vertices = [
  new THREE.Vortor3(1, 3, 1),
  new THREE.Vortor3(1, 3, -1),
  new THREE.Vortor3(1, -1, 1),
  new THREE.Vortor3(1, -1, -1),
  new THREE.Vortor3(-1, 3, 1),
  new THREE.Vortor3(-1, 3, -1),
  new THREE.Vortor3(-1, 1, -1),
  new THREE.Vortor3(-1, 1, 1),
];

const faces = [
  new THREE.Face3(0, 2, 1),
  new THREE.Face3(2, 3, 1),
  new THREE.Face3(4, 6, 5),
  new THREE.Face3(6, 7, 5),
  new THREE.Face3(4, 5, 1),
  new THREE.Face3(5, 0, 1),
  new THREE.Face3(7, 6, 2),
  new THREE.Face3(6, 3, 2),
  new THREE.Face3(5, 7, 0),
  new THREE.Face3(7, 2, 0),
  new THREE.Face3(1, 3, 4),
  new THREE.Face3(3, 6, 4),
];

const geom = new THREE.Geometry();
geom.vertices = vertices;
geom.faces = faces;
geom.computeFaceNormals();
```

上述代码中在 vertices 数组中保存了构成几何体的顶点，在 faces 数组中保存了由这些顶点连接起来创建的三角形面。  
new THREE.Face3(0,2,1)就是使用 vertices 数组中点 0，2 和 1 创建而成的三角形面。 **创建面的顶点顺序决定了某个面是面向摄像机还是背向摄像机的**。面向摄像机，顶点的顺序是顺时针，否则是逆时针的。

### 网格对象的属性和方法

我们已经知道，创建一个网格对象需要一个几何体以及一个或多个材质。当网格创建好之后，我们可以将他们添加到场景并进行渲染。下面我们来看一下网格对象提供的属性和方法。

| 方法(属性)         | 描述                                                                                                                   |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| position           | 该属性决定该对象相对于父对象的位置，通常父对象是 THREE.Scene 对象或者 THREE.Object3D 对象                              |
| rotation           | 通过该属性可以设置围绕每个轴的旋转弧度。Three.js 提供了设置相对特定轴的旋转弧度的方法，rotateX(),rotateY()和 rotateZ() |
| scale              | 该属性可以沿着 x,y 和 z 轴缩放对象                                                                                     |
| translateX(amount) | 沿 x 轴将对象平移 amount 距离                                                                                          |
| translateY(amount) | 沿 y 轴将对象平移 amount 距离                                                                                          |
| translateZ(amount) | 沿 z 轴...，也可以用 translateOnAxis(axis,distance)函数，该函数允许沿指定轴平移网格                                    |
| visible            | 该属性为 false 时，THREE.Mesh 将不会渲染到网格中                                                                       |

## 选择合适的摄像机

Three.js 库提供了两种不同的摄像机：**正交投影摄像机**和**透视投影摄像机**。  
对于**正交投影摄像机**，对象相对于摄像机的距离对渲染的结果时没有影响的，通常用于二维游戏中。
对于**透视投影摄像机**，对象距离摄像机越远，他们就会被渲染的越小。  
首先我们来看一下 THREE.PerspectiveCamera,该方法接受的参数如下图所示：

| 参数 | 描述               |
| ---- | ------------------ |
| fov  | fov 表示视场。这是 |
