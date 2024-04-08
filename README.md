整理了LeetCode部分题解（java版本）

## 记录总结

01.使××最大值最小，一般为二分搜索类型；

02.0-1背包问题通常使用动态规划解决；

03.遇到字符串s尽可能转化为字符数组；

04.用到复杂数据结构，如set，频繁创建不如.clear()

05. int num; num & (-num)可得到num二进制最低位为1的位。

06.四数平方定理：任意一个正整数，可表示为至多4个平方数之和。且当n ≠ 4k * (8m+7)时，正整数可表示为至多3个平方数之和。

07.格雷码计算公式。Gi = (i >> 1) ^ i；{0 ≤ i ＜ 2n}。

    08.「在一维数组中找到第一个满足某条件的数」--单调栈。

    09.ASCII码共128个，存储所有英文字符。Unicode码可表示所有语言字符，不止128。

    10.卡塔兰数：C0 = 1，Cn+1 = Cn * 2 * (2n+1) / (n + 2)。

11.求解A+B+C+D+...+N=0问题时，可转化为(A+B+C+...+T) = -(T+1+T+2+..+N)问题，T一般取中间值。


1.将课程看成节点，依赖看成边-->问题转化为拓扑排序问题

2.求出拓扑排序最有时间复杂度为O(n+m),确认图是否有拓扑排序时间复杂度也为O(n+m)。

3.带环图无拓扑排序，有向五环图拓扑排序可能不止一种。（n/m分别为节点数、边数）

## 1.有序数组中，寻找 值==target 的下标索引
若不存在：

    ①寻找 < target 的最近索引；
    
    ②寻找 > target 的最近索引；

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        //最终结果--「right，targetIndex, left」,其中right + 1 = left
        while (left <= right) {
            int mid = ((right - left) >> 1) + left;
            if（nums[mid] == target） {	//找到下标索引，返回
                return mid;
            }
            if (nums[mid] < target) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }

        return left;							//找到 > target的最近索引。注意：若数组元素全员 > target，运行结果「right = n - 1, left = n」
        return right;							//找到 < target的最近索引。注意：若数组元素全员 < target，运行结果「right = -1，left = 0」
    }
}
```

## 2.快排

①严蔚敏版；②自己思考版本。自己版本效率似乎较低。
```java
class Solution {
    private void quickSort(int[] nums, int left, int right) {
         if (left >= right) {
            return;
         }
         int index = sort(nums, left, right);
         quickSort(nums, left, index - 1);
         quickSort(nums, index + 1, right);
    }  

    private int sort(int[] nums, int left, int right) {
        int pivot = nums[left];
        while (left < right) {
            while (left < right && pivot <= nums[right]) {
                right --;
            }
            nums[left] = nums[right];
            while (left < right && nums[left] <= pivot) {
                left ++;
            }
            nums[right] = nums[left];
        }
        nums[left] = pivot;
        return left;
    }
}
```
```java
public class Main {
    private static void quickSort(int[] nums, int left, int right) {
         if (left >= right) {
            return;
         }
         int index = sort(nums, left, right);
         quickSort(nums, left, index - 1);
         quickSort(nums, index + 1, right);
    }  
    
    private static int sort2(int[] nums, int left, int right) {
        int left0 = left;
        int pivot = nums[left];
        while (left < right) {
            while (left < right && pivot <= nums[right]) {
                right --;
            }
            while (left < right && nums[left] <= pivot) {
                left ++;
            }
            swap(nums, left, right);
        }
        swap(nums, left0, left);
        return left;
    }    
    
    private static void swap(int[] nums, int left, int right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
    
}
```

