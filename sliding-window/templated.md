/*
 * 滑动窗口 通用解题模板
 * 适用：求子数组/子串 → 最长/最短/恰好满足某条件 的问题
 * 满足以下条件：
     1、操作对象：连续的线性结构：子数组、子串、连续区间
     2、求解目标：最长长度 / 最短长度 / 满足条件的数量 / 最优值
     3、窗口特性：窗口扩大时状态单调变化（和变大、计数增加等），不满足条件时只需收缩左指针即可重新合法。
 */

```java
public class SlidingWindowTemplate {
    // 返回值 T 可根据题目替换：int/String/boolean等
    public int slidingWindowTemplate(int[] nums) {
        // ========== 1. 边界预处理 ==========
        // 空数组/长度为0直接返回默认值
        if (nums == null || nums.length == 0) {
            return 0; 
        }

        // ========== 2. 初始化核心变量 ==========
        int left = 0;                // 左指针：固定初始位置
        int n = nums.length;         // 数组长度
        // 窗口状态：记录窗口内信息（计数/和/最值等）
        int windowState = 0;         
        // 答案：存储最终结果（初始值按题目设：0 / 最大数 / 最小数）
        int ans = 0;                 

        // ========== 3. 右指针遍历：扩展窗口 ==========
        for (int right = 0; right < n; right++) {
            // ✅ 第一步：右指针加入窗口，更新窗口状态
            windowState = updateWindowAdd(nums[right]);

            // ========== 4. 窗口非法：收缩左指针 ==========
            // 条件：窗口不满足题目要求 → 移动left缩小窗口
            while (isWindowInvalid(windowState)) {
                // ❌ 收缩窗口：移除左指针元素，更新状态
                windowState = updateWindowRemove(nums[left]);
                left++; // 左指针右移
            }

            // ========== 5. 窗口合法：更新答案 ==========
            // 此时 [left, right] 一定是有效窗口
            ans = updateAnswer(ans, left, right);
        }

        return ans;
    }

    // ===================== 工具方法（按题目改写） =====================
    // 右指针加入：更新窗口状态
    private int updateWindowAdd(int num) {
        // 示例：windowState += num;
        return 0;
    }

    // 左指针移出：更新窗口状态
    private int updateWindowRemove(int num) {
        // 示例：windowState -= num;
        return 0;
    }

    // 判断窗口是否非法（需要收缩）
    private boolean isWindowInvalid(int state) {
        // 示例：state > target / 重复字符等
        return false;
    }

    // 窗口合法时，计算并更新最优解
    private int updateAnswer(int ans, int left, int right) {
        // 示例：ans = Math.max(ans, right - left + 1);
        return ans;
    }
}
