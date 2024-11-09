# B-Tree

## 基本概念

B-Tree 中的一条数据记录定义为二元组 [key, data]：

- key：记录的键值（在不同记录中互不相同）
- data：记录中除 key 外的数据

## 结构特性

### 基本性质

1. 节点特性：

   - key 从左到右非递减排列
   - 所有节点组成树结构
   - 每个指针要么为 null，要么指向另一个节点

2. 节点容量：

   - 最少：1 个 key 和 2 个指针
   - 最多：2d-1 个 key 和 2d 个指针
   - key 和指针互相间隔，节点两端是指针

3. 叶节点特性：
   - 所有指针均为 null
   - 所有叶节点深度相同（等于树高 h）

### 父子节点排序规则

1. 最左指针规则：若某指针在节点最左边且不为 null，其指向节点的所有 key < 当前节点第一个 key

2. 最右指针规则：若某指针在节点最右边且不为 null，其指向节点的所有 key > 当前节点最后一个 key

3. 中间指针规则：若某指针位于相邻 key（keyi 和 keyi+1）之间且不为 null，其指向节点的所有 key 满足：
   keyi < key < keyi+1

![B-Tree 示意](https://assets.ng-tech.icu/item/20230407224316.png)

## 查找算法

### 查找过程

1. 从根节点开始进行二分查找
2. 如果找到 key，返回对应的 data
3. 如果未找到，递归查找相应区间的子节点
4. 直到找到目标节点或遇到 null 指针

### 算法实现

```c
BTree_Search(node, key) {
    if(node == null) return null;

    foreach(node.key) {
        if(node.key[i] == key) return node.data[i];
        if(node.key[i] > key) return BTree_Search(point[i]->node, key);
    }
    return BTree_Search(point[i+1]->node, key);
}

data = BTree_Search(root, my_key);
```

## 性能特性

- 对于度为 d 的 B-Tree，索引 N 个 key：
  - 树高上限：logd((N+1)/2)
  - 查找复杂度：O(logdN)
- B-Tree 是一个高效的索引数据结构
