# Whitted-Style Ray Tracing

## 基本原理

从摄像机往场景中各个位置投射光线(穿过成像平面的各个像素点)，模拟光线在场景内不断弹射的过程，把光线投射产生的像素值记录到成像平面上

![image-20210921135725251](Whitted-Style.assets\image-20210921135725251.png)

## 技术细节

### Ray-Surface Intersection

- 光线表示：起点 + 方向
- 表面(隐式表面)：
  - 点同时在光线和平面上，联立方程，解方程组
  - Triangle Mesh(分别与Mesh上的每个三角形求交)
    - 基本原理
      - 与三角形所在平面求交
      - 判断交点是否在三角形内
    - 快速算法 （**Möller Trumbore Algorithm**）
      - 平面上的点表示成三角形的重心坐标，与光线求交
        - **O** + `t` **D**
        - `(1-b1-b2)` **P1** + `b1` **P1** + `b2` **P2**
      - 判断重心坐标是否合法
        - 1-b1-b2, b1, b2  都在 [0, 1] 

### Accelerating Ray-Surface Intersection

#### Ray-Intersection with Box(AABB)

- Box 可以理解成 由三对平面围成的交集
- 分别与 三对平面求交，然后求 交集
  - 求交
    - O + tD
    - t_enter = max(t_min)
    - t_exit = min(t_max)
  - 判断物理意义
    - if (t_enter < t_exit)  光线有进入Box
    - if( t_exit < 0 )  Box在光线背后
    - if( t_exit >=0 && t_enter <0 ) 光源在 Box内
  - 总结
    - t_enter < t_exit && t_exit >=0， 则有交点

####  Using AABBs to accelerate ray tracing

- Uniform Grids
- Spatial Partitions
  - Oct-Tree (每次在每个维度都一分为二)
  - KD-Tree  (每次只在一个维度一分为二)
  - BSP-Tree(以任意的角度一分为二，不限于x,y,z轴方向)
  - KD-Tree Problems: 
    - 不好构造， 三角形与Box求交 很困难
    - 物体可能在多个Box内
    - 引出 Object Partitions
- Object Partitions
  - BVH (Bounding Volume Hierarchy BVH)



##### KD-Tree

![image-20210921135829569](Whitted-Style.assets\image-20210921135829569.png)

##### Building BVH

- Find bounding box 

- Recursively split set of objects in two subsets 

- **Recompute** the bounding  box of the subsets 

- Stop when necessary 

- Store objects in each leaf  node

![image-20210921141544762](Whitted-Style.assets\image-20210921141544762.png)

##### Traversing BVH

![image-20210921142203958](Whitted-Style.assets\image-20210921142203958.png)