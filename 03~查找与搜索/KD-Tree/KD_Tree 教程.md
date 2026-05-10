# KD-Tree (K-Dimensional Tree) 深度教程

**KD-Tree**（K 维树）是一种用于组织 k 维空间中点集的二叉搜索树（BST）。它主要用于多维空间中的关键数据操作，如**范围搜索**和**最近邻搜索 (Nearest Neighbor Search, NNS)**。

---

## 1. 核心概念

普通的二叉搜索树（BST）只能处理一维数据（如整数）。当数据变为多维（如 2D 坐标 `(x, y)` 或 3D 坐标 `(x, y, z)`）时，普通的 BST 就失效了。

KD-Tree 通过**轮流选择坐标轴**来划分空间：

- **根节点**：按 X 轴划分（左子树 x < root.x，右子树 x >= root.x）。
- **深度 1**：按 Y 轴划分。
- **深度 2**：按 Z 轴划分（如果是 3D）。
- **深度 3**：回到 X 轴，循环往复。

### 空间划分示意

想象一个二维平面，KD-Tree 就像不断地用垂直线和水平线将平面切割成更小的矩形区域，每个区域包含一个数据点。

---

## 2. 构建算法 (Build)

构建一棵平衡的 KD-Tree 是为了保证搜索效率。

**步骤**：

1.  **确定划分轴 (Axis)**：通常根据深度选择，`axis = depth % k`（k 是维度）。或者选择方差最大的轴（数据分布最散的轴）。
2.  **确定切分点 (Pivot)**：为了平衡树，通常选择当前轴上的**中位数**点。
3.  **递归构建**：
    - 将小于中位数的点放入左子树。
    - 将大于等于中位数的点放入右子树。

**复杂度**：

- 时间复杂度：$O(N \log N)$（因为每次寻找中位数排序或 QuickSelect 需要 $O(N)$，树深 $\log N$）。
- 空间复杂度：$O(N)$。

---

## 3. 最近邻搜索 (Nearest Neighbor Search, NNS)

这是 KD-Tree 最核心的应用。给定一个查询点 $Q$，找到树中距离 $Q$ 最近的点 $P$。

**算法流程**：

1.  **二叉搜索（下探）**：
    从根节点出发，根据划分轴的值，向左或向右递归，直到达到叶子节点。记录路径。

    - _注意：此时找到的叶子节点不一定是最近的，但它是一个很好的初始“当前最佳点 (Current Best)”。_

2.  **回溯（Backtracking）**：
    沿着递归路径向上回溯。
    - **更新最佳点**：计算当前节点与 $Q$ 的距离，如果更近，则更新“当前最佳点”。
    - **剪枝判断（核心）**：
      - 以 $Q$ 为圆心，当前最小距离为半径画一个超球体。
      - 检查**父节点的分割超平面**是否与该超球体相交。
      - **相交**：说明超平面的另一侧可能存在更近的点，必须进入另一侧子树查找。
      - **不相交**：说明另一侧不可能有更近的点，直接剪枝（忽略另一侧子树），继续向上回溯。

---

## 4. Python 代码实现

以下是一个完整的 2D KD-Tree 实现，包含构建和最近邻搜索。

```python
import math

class Node:
    def __init__(self, point, left=None, right=None, axis=0):
        self.point = point  # 数据点，例如 (2, 3)
        self.left = left    # 左子树
        self.right = right  # 右子树
        self.axis = axis    # 当前节点划分的轴 (0=x, 1=y, ...)

def build_kdtree(points, depth=0, k=2):
    """
    构建 KD-Tree
    :param points: 点列表 [(x1, y1), (x2, y2), ...]
    :param depth: 当前深度
    :param k: 维度
    """
    if not points:
        return None

    # 1. 选择轴：轮流选择
    axis = depth % k

    # 2. 排序并找中位数
    points.sort(key=lambda x: x[axis])
    median = len(points) // 2

    # 3. 递归构建
    return Node(
        point=points[median],
        left=build_kdtree(points[:median], depth + 1, k),
        right=build_kdtree(points[median + 1:], depth + 1, k),
        axis=axis
    )

def distance_squared(p1, p2):
    """欧几里得距离的平方（避免开根号，提高效率）"""
    return sum((a - b) ** 2 for a, b in zip(p1, p2))

def nearest_neighbor(root, target):
    """
    寻找最近邻
    :return: (best_point, best_dist)
    """
    if root is None:
        return None, float('inf')

    # 初始化 best 为当前节点
    best_node = root.point
    best_dist = distance_squared(target, root.point)

    # 1. 二叉搜索向下递归
    axis = root.axis
    diff = target[axis] - root.point[axis]

    # 决定先走哪边
    near_branch = root.left if diff < 0 else root.right
    far_branch = root.right if diff < 0 else root.left

    # 递归搜索“近”的一边
    branch_best, branch_dist = nearest_neighbor(near_branch, target)

    # 更新当前最佳
    if branch_dist < best_dist:
        best_node = branch_best
        best_dist = branch_dist

    # 2. 回溯与剪枝
    # 如果 (目标点到分割面的距离)^2 < 当前最小距离，说明“远”的一边可能有更近的点
    if diff ** 2 < best_dist:
        branch_best, branch_dist = nearest_neighbor(far_branch, target)
        if branch_dist < best_dist:
            best_node = branch_best
            best_dist = branch_dist

    return best_node, best_dist

# --- 测试代码 ---
if __name__ == "__main__":
    data_points = [(2, 3), (5, 4), (9, 6), (4, 7), (8, 1), (7, 2)]
    tree = build_kdtree(data_points)

    target_point = (9, 2)
    nearest, dist_sq = nearest_neighbor(tree, target_point)

    print(f"数据点: {data_points}")
    print(f"目标点: {target_point}")
    print(f"最近邻: {nearest}, 距离: {math.sqrt(dist_sq):.4f}")
```

---

## 5. 复杂度与优缺点

### 复杂度

- **构建**：$O(N \log N)$。
- **搜索 (NNS)**：
  - 平均情况：$O(\log N)$。
  - 最坏情况：$O(N)$（可能需要遍历整棵树）。

### 维度灾难 (Curse of Dimensionality)

KD-Tree 在低维空间（如 $k \le 20$）表现优异。但当维度很高时，KD-Tree 的效率会急剧下降，退化为线性扫描（即 $O(N)$）。

- **经验法则**：若数据点数量 $N$ 不远大于 $2^k$，KD-Tree 的效果通常不如暴力搜索。

### 优缺点总结

- **✅ 优点**：
  - 对于低维数据，查询效率极高。
  - 结构简单，易于实现。
  - 支持范围查询（Range Search）和 K 近邻（KNN）。
- **❌ 缺点**：
  - **高维失效**：不适合高维特征向量检索。
  - **难以平衡**：一旦构建完成，动态插入/删除节点可能破坏平衡，需要复杂的再平衡机制或定期重构。

---

## 6. 应用场景

1.  **计算机图形学**：光线追踪（Ray Tracing）中加速光线与物体的求交检测。
2.  **地理信息系统 (GIS)**：查找最近的兴趣点（POI），如“离我最近的加油站”。
3.  **机器学习**：KNN（K-Nearest Neighbors）算法的加速实现（如 `sklearn.neighbors.KDTree`）。
