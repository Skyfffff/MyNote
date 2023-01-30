# LeetCode

## Hot100

### [78. 子集](https://leetcode.cn/problems/subsets/)

> 给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。
>
> 解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

*解法1：回溯算法*

思路：传入开始index进行递归回溯，回溯开始先将节点的结果添加到res里面，for循环里先将集合第一个元素添加进path，在进行递归，每层递归结束后进行回溯，删掉最后一个元素避免越来越多元素加入path，递归结束条件是开始下标大于等于给定的集合大小。

```java
    ArrayList<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> subsets(int[] nums) {
        backtracking(0, nums);
        return res;
    }

    public void backtracking(int startIndex, int[] nums) {
        res.add(new ArrayList<>(path));

        if (startIndex >= nums.length) {
            return;
        }
        for (int i = startIndex; i < nums.length; i++) {
            path.add(nums[i]);
            backtracking(i + 1, nums);
            path.removeLast();
        }
    }
```

### [461. 汉明距离](https://leetcode.cn/problems/hamming-distance/)

> 两个整数之间的 [汉明距离](https://baike.baidu.com/item/汉明距离) 指的是这两个数字对应二进制位不同的位置的数目。
>
> 给你两个整数 `x` 和 `y`，计算并返回它们之间的汉明距离。

*解法1：自己的笨方法*

思路：x和y求异或后转为二进制形式，其中数字1的个数就代表x和y两个数字对应二进制位的不同位置数目，即汉明距离。

```java
public static int hammingDistance(int x, int y) {
    String s = Integer.toString(x ^ y, 2);
    int sum = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '1') {
            sum += 1;
        }
    }
    return sum;
}
```

*解法2：调用函数法*

```java
public static int hammingDistance(int x, int y) {
    //知识点：异或、反码、原码、补码
    return Integer.bitCount(x ^ y);
}
```

### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

> 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。
>
> 给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

*解法1：回溯算法*

思路：假设digits是23两个数，那么先可以从2中的abc中选出a与3中的def选出的d组合成ad，同理选出ae、af等等，所以回溯函数应该有两个参数，一个是startIndex代表digits的开始下标，第二个参数digits代表abc、def等等英文字母。

```java
Map<Character, String> map = new HashMap<>() {
    {
        put('2', "abc");
        put('3', "def");
        put('4', "ghi");
        put('5', "jkl");
        put('6', "mno");
        put('7', "pqrs");
        put('8', "tuv");
        put('9', "wxyz");
    }
};

public List<String> letterCombinations(String digits) {
    if (digits.length() == 0) {
        return new ArrayList<>();
    }
    backtracking(0, digits);
    return res;
}

List<String> res = new ArrayList<>();
StringBuilder path = new StringBuilder();

public void backtracking(int startIndex, String digits) {
    if (startIndex == digits.length()) {
        res.add(path.toString()); //叶子结点收集结果
        return;
    }
    String s = map.get(digits.charAt(startIndex));
    for (int i = 0; i < s.length(); i++) {
        path.append(s.charAt(i));
        backtracking(startIndex + 1, digits);
        path.deleteCharAt(path.length() - 1);
    }
}
```

## 回溯算法

### [78. 子集](https://leetcode.cn/problems/subsets/)

> 给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。
>
> 解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

*解法1：回溯算法*

思路：传入开始index进行递归回溯，回溯开始先将节点的结果添加到res里面，for循环里先将集合第一个元素添加进path，在进行递归，每层递归结束后进行回溯，删掉最后一个元素避免越来越多元素加入path，递归结束条件是开始下标大于等于给定的集合大小。

```java
    ArrayList<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> subsets(int[] nums) {
        backtracking(0, nums);
        return res;
    }

    public void backtracking(int startIndex, int[] nums) {
        res.add(new ArrayList<>(path));

        if (startIndex >= nums.length) {
            return;
        }
        for (int i = startIndex; i < nums.length; i++) {
            path.add(nums[i]);
            backtracking(i + 1, nums);
            path.removeLast();
        }
    }
```

### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

> 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。
>
> 给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

*解法1：回溯算法*

思路：假设digits是23两个数，那么先可以从2中的abc中选出a与3中的def选出的d组合成ad，同理选出ae、af等等，所以回溯函数应该有两个参数，一个是startIndex代表digits的开始下标，第二个参数digits代表abc、def等等英文字母。

```java
Map<Character, String> map = new HashMap<>() {
    {
        put('2', "abc");
        put('3', "def");
        put('4', "ghi");
        put('5', "jkl");
        put('6', "mno");
        put('7', "pqrs");
        put('8', "tuv");
        put('9', "wxyz");
    }
};

public List<String> letterCombinations(String digits) {
    if (digits.length() == 0) {
        return new ArrayList<>();
    }
    backtracking(0, digits);
    return res;
}

List<String> res = new ArrayList<>();
StringBuilder path = new StringBuilder();

public void backtracking(int startIndex, String digits) {
    if (startIndex == digits.length()) {
        res.add(path.toString()); //叶子结点收集结果
        return;
    }
    String s = map.get(digits.charAt(startIndex));
    for (int i = 0; i < s.length(); i++) {
        path.append(s.charAt(i));
        backtracking(startIndex + 1, digits);
        path.deleteCharAt(path.length() - 1);
    }
}
```

### [77. 组合](https://leetcode.cn/problems/combinations/)

> 给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。
>
> 你可以按 **任何顺序** 返回答案。

*解法1：回溯算法（标准版）*

思路：假设n=4，k=2，那么也就是从1-4中求有多少种2个数组合。那么只需要回溯直到组合为2时停止就行。

```java
List<List<Integer>> res = new ArrayList<>();
List<Integer> path = new ArrayList<>();

public List<List<Integer>> combine(int n, int k) {
    backtracking(n, k, 1);
    return res;
}

public void backtracking(int n,int k,int startIndex) {
    if (path.size()==k) {
        res.add(new ArrayList<>(path)); //易错
        return;
    }
    for (int i = startIndex; i <= n; i++) {
        path.add(i);
        backtracking(n, k, i+1); // i+1易错
        path.remove(path.size() - 1);
    }
}
```

*解法2：回溯算法（剪枝版）*

思路：n-k+1怎么来的？假设n=k=4，那么也就是从1234里面挑出大小为4的组合，很明显只有一个1234，也就是说234都不用进行递归了，所以需要剪枝，经过运算n-k+1在此时也就是4-4+1刚好等于1，而之所以要加上path.size()是因为xxx

```java
List<List<Integer>> res = new ArrayList<>();
List<Integer> path = new ArrayList<>();

public List<List<Integer>> combine(int n, int k) {
    backtracking(n, k, 1);
    return res;
}

public void backtracking(int n,int k,int startIndex) {
    if (path.size()==k) {
        res.add(new ArrayList<>(path)); //易错
        return;
    }
    for (int i = startIndex; i <= n - k + 1 + path.size(); i++) { //剪枝操作
        path.add(i);
        backtracking(n, k, i+1); // i+1易错
        path.remove(path.size() - 1);
    }
}
```

