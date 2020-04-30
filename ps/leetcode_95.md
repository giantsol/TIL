# 95. Unique Binary Search Trees II

[Link](https://leetcode.com/problems/unique-binary-search-trees-ii/)

```java
public List<TreeNode> generateTrees(int n) {
    if (n == 0) {
        return new ArrayList<>();
    }
    return genTrees(1, n);
}

private List<TreeNode> genTrees(int start, int end) {
    final List<TreeNode> trees = new ArrayList<>();
    if (start > end) {
        trees.add(null);
        return trees;
    }

    for (int i = start; i <= end; i++) {
        List<TreeNode> lTrees = genTrees(start, i - 1);
        List<TreeNode> rTrees = genTrees(i + 1, end);
        for (TreeNode lTree : lTrees) {
            for (TreeNode rTree : rTrees) {
                final TreeNode root = new TreeNode(i, lTree, rTree);
                trees.add(root);
            }
        }
    }

    return trees;
}
```

ㄹㅇㅋㅋ.. discussion꺼 보고 베꼈는데 어떻게 되는건지 몰겠다 ㅋㅋㅋㅋㅋㅋ
내일 다시 봐야지!!!

---

수학이네 흠.

- G(n): the num of unique BST for a sequence of length n
- F(i, n), 1 <= i <= n: the num of unique BST, where i is the root of the BST and sequence ranges from 1 to n.

이라고 할 때.. n = 3이면 곧 G(3)의 값을 리턴해야 한다는 것인디.

G(n) = F(1, n) + F(2, n) + ... + F(n, n) 이다.

그리고 F(i, n) = G(i - 1) * G(n - i), 1 <= i <= n, G(i - 1)은 왼쪽 subtree의 모든 갯수, G(n - i)은 오른쪽 subtree의 모든 갯수.

왼쪽 subtree와 오른쪽 subtree의 모든 조합에 대해 현재 i에 해당하는 root를 덧붙이면 항상 unique하므로 곱셈 갯수만큼 된다.

대충은 알겠다. 비슷한 문제를 접한다? 그럼 못푼다 ㅋㅋㅋㅋㅋㅋㅋ 그래도 기억해두려고 해야지!!


