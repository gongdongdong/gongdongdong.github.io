# 红黑树

红黑树也是一种二叉排序树，但是它有如下的特殊性质

## 红黑树的性质
1. 所有的节点要么是红色的要么是黑色的
2. 顶点是黑色的
3. 所有到子孙节点的路径，黑色点的个数是相等的
4. 红色节点的两个子节点都是黑色的
5. 每个叶子节点都是黑色的


## 红黑树的应用

红黑树的应用比较广泛，主要是用它来存储有序的数据，它的时间复杂度是O(lgn)，效率非常之高。
例如，Java集合中的TreeSet和TreeMap，C++ STL中的set、map，以及Linux虚拟内存的管理，都是通过红黑树去实现的。

## 红黑树的时间复杂度和相关证明

红黑树的时间复杂度为: O(lgn)
下面通过“数学归纳法”对红黑树的时间复杂度进行证明。

定理：一棵含有n个节点的红黑树的高度至多为2log(n+1).

证明：
    "一棵含有n个节点的红黑树的高度至多为2log(n+1)" 的逆否命题是 "高度为h的红黑树，它的包含的内节点个数至少为 2h/2-1个"。
    我们只需要证明逆否命题，即可证明原命题为真；即只需证明 "高度为h的红黑树，它的包含的内节点个数至少为 2h/2-1个"。

    从某个节点x出发（不包括该节点）到达一个叶节点的任意一条路径上，黑色节点的个数称为该节点的黑高度(x's black height)，记为bh(x)。关于bh(x)有两点需要说明： 
    第1点：根据红黑树的"特性(5) ，即从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点"可知，从节点x出发到达的所有的叶节点具有相同数目的黑节点。这也就意味着，bh(x)的值是唯一的！
    第2点：根据红黑色的"特性(4)，即如果一个节点是红色的，则它的子节点必须是黑色的"可知，从节点x出发达到叶节点"所经历的黑节点数目">= "所经历的红节点的数目"。假设x是根节点，则可以得出结论"bh(x) >= h/2"。进而，我们只需证明 "高度为h的红黑树，它的包含的黑节点个数至少为 2bh(x)-1个"即可。
    
    到这里，我们将需要证明的定理已经由
"一棵含有n个节点的红黑树的高度至多为2log(n+1)" 
    转变成只需要证明
"高度为h的红黑树，它的包含的内节点个数至少为 2bh(x)-1个"。


下面通过"数学归纳法"开始论证高度为h的红黑树，它的包含的内节点个数至少为 2bh(x)-1个"。

(01) 当树的高度h=0时，
    内节点个数是0，bh(x) 为0，2bh(x)-1 也为 0。显然，原命题成立。

(02) 当h>0，且树的高度为 h-1 时，它包含的节点个数至少为 2bh(x)-1-1。这个是根据(01)推断出来的！

    下面，由树的高度为 h-1 的已知条件推出“树的高度为 h 时，它所包含的节点树为 2bh(x)-1”。
    
    当树的高度为 h 时，
    对于节点x(x为根节点)，其黑高度为bh(x)。
    对于节点x的左右子树，它们黑高度为 bh(x) 或者 bh(x)-1。
    根据(02)的已知条件，我们已知 "x的左右子树，即高度为 h-1 的节点，它包含的节点至少为 2bh(x)-1-1 个"；
    
    所以，节点x所包含的节点至少为 ( 2bh(x)-1-1 ) + ( 2bh(x)-1-1 ) + 1 = 2^bh(x)-1。即节点x所包含的节点至少为 2bh(x)-1。
    因此，原命题成立。
    
    由(01)、(02)得出，"高度为h的红黑树，它的包含的内节点个数至少为 2^bh(x)-1个"。
    因此，“一棵含有n个节点的红黑树的高度至多为2log(n+1)”。

-------------

## 红黑树的插入
红黑树中的左右旋转和平衡二叉树的左右旋转是一样的，只是具体的条件不同。

红黑树插入的时候，是红色的节点，先按照二叉排序树的插入方法插入，然后做调整。

红黑树插入有五个case

github 上有java红黑树实现的代码，如下：
链接： [java数据结构详细](https://github.com/phishman3579/java-algorithms-implementation 'pl')



```
    /**
     * Post insertion balancing algorithm.
     * 
     * @param begin
     *            to begin balancing at.
     * @return True if balanced.
     */
    private void balanceAfterInsert(RedBlackNode<T> begin) {
        RedBlackNode<T> node = begin;
        RedBlackNode<T> parent = (RedBlackNode<T>) node.parent;

        if (parent == null) {
            // Case 1 - The current node is at the root of the tree.
            node.color = BLACK;
            return;
        }

        if (parent.color == BLACK) {
            // Case 2 - The current node's parent is black, so property 4 (both
            // children of every red node are black) is not invalidated.
            return;
        }

        RedBlackNode<T> grandParent = node.getGrandParent();
        RedBlackNode<T> uncle = node.getUncle(grandParent);
        if (parent.color == RED && uncle.color == RED) {
            // Case 3 - If both the parent and the uncle are red, then both of
            // them can be repainted black and the grandparent becomes
            // red (to maintain property 5 (all paths from any given node to its
            // leaf nodes contain the same number of black nodes)).
            parent.color = BLACK;
            uncle.color = BLACK;
            if (grandParent != null) {
                grandParent.color = RED;
                balanceAfterInsert(grandParent);
            }
            return;
        }

        if (parent.color == RED && uncle.color == BLACK) {
            // Case 4 - The parent is red but the uncle is black; also, the
            // current node is the right child of parent, and parent in turn
            // is the left child of its parent grandparent.
            if (node == parent.greater && parent == grandParent.lesser) {
                // right-left
                rotateLeft(parent);
 
                node = (RedBlackNode<T>) node.lesser;
                parent = (RedBlackNode<T>) node.parent;
                grandParent = node.getGrandParent();
                uncle = node.getUncle(grandParent);
            } else if (node == parent.lesser && parent == grandParent.greater) {
                // left-right
                rotateRight(parent);

                node = (RedBlackNode<T>) node.greater;
                parent = (RedBlackNode<T>) node.parent;
                grandParent = node.getGrandParent();
                uncle = node.getUncle(grandParent);
            }
        }

        if (parent.color == RED && uncle.color == BLACK) {
            // Case 5 - The parent is red but the uncle is black, the
            // current node is the left child of parent, and parent is the
            // left child of its parent G.
            parent.color = BLACK;
            grandParent.color = RED;
            if (node == parent.lesser && parent == grandParent.lesser) {
                // left-left
                rotateRight(grandParent);
            } else if (node == parent.greater && parent == grandParent.greater) {
                // right-right
                rotateLeft(grandParent);
            }
        }
    }
```

1. 如果是根节点就变成黑色(The current node is at the root of the tree.)
2. 如果父亲节点是黑色的(The current node's parent is black)
3. 如果父亲和叔叔节点都是红色的，那么它们两个都改成黑色，把祖父改成红色，当成是插入的点继续递归（If both the parent and the uncle are red, then both of them can be repainted black and the grandparent becomes red）
4. 如果父亲是红色的，叔叔是黑色的，而且插入的红点在祖父的左子树，在父亲的有节点，需要对父亲做左旋转，然后当前节点换成父亲节点，插入的节点变成父亲节点，如果在付清在右子树类似。（The parent is red but the uncle is black; also, the current node is the right child of parent, and parent in turn is the left child of its parent grandparent.）
5. 如果父亲是红色的，叔叔是黑色的，先把祖父节点染红，父亲节点染黑，如果父亲在左子树，当前节点在父亲节点的左子树上，需要对祖父节点做右旋转，如果父亲在右子树上，处理方法类似。（The parent is red but the uncle is black, the current node is the left child of parent, and parent is the left child of its parent G.）


## 红黑树的删除

删除的情况，需要找到那个替换被删除的点，然后类似插入的调整整个红黑树。