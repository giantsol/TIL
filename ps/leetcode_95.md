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