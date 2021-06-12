### 数组

**数组的遍历**

1. 给定一个二进制数组，计算其中最大连续 1 的个数。

   ```
   输入：[1,1,0,1,1,1]
   输出：3
   解释：开头的两位和最后的三位都是连续 1 ，所以最大连续 1 的个数是 3.
   ```

   > [查看详情](https://leetcode-cn.com/problems/max-consecutive-ones/solution/yi-ci-bian-li-485-zui-da-lian-xu-1de-ge-o30sy/) | leetcode

   ```JS
   // 遍历数组若值为 1，则 count++ 并更新 max，否则重置 count 为 0
   // 时间复杂度 O(n)，空间复杂度 O(1)
   const findMaxConsecutiveOnes = (arr) => {
      let max = 0
      let count = 0
      for (let v of arr) {
         if (v === 1) {
            count++
            max = count > max ? count : max
         } else {
            count = 0
         }
      }
      return max
   }
   ```

2. 在《英雄联盟》的世界中，有一个叫 “提莫” 的英雄，他的攻击可以让敌方英雄艾希（编者注：寒冰射手）进入中毒状态。现在，给出提莫对艾希的攻击时间序列和提莫攻击的中毒持续时间，你需要输出艾希的中毒状态总时长。

   你可以认为提莫在给定的时间点进行攻击，并立即使艾希处于中毒状态。

   ```
   输入: [1,4], 2
   输出: 4
   原因: 第 1 秒初，提莫开始对艾希进行攻击并使其立即中毒。中毒状态会维持 2 秒钟，直到第 2 秒末结束。
   第 4 秒初，提莫再次攻击艾希，使得艾希获得另外 2 秒中毒时间。
   所以最终输出 4 秒。
   ```

   > [查看详情](https://leetcode-cn.com/problems/teemo-attacking/solution/shou-hua-tu-jie-jian-dan-er-you-qu-de-ti-mu-yi-ci-/) | leetcode

   ```js
   // 若相邻的两个时间序列处于不叠加状态则持续时间不变，否则持续时间为两者的差值
   // 时间复杂度 O(n)，空间复杂度 O(1)
   const findPoisonedDuration = (timeSeries, duration) => {
      if (timeSeries.length === 0) return 0
      let count = duration
      for (let i = 1; i < timeSeries.length; i++) {
         const diff = timeSeries[i] - timeSeries[i - 1]
         count += diff >= duration ? duration : diff
      }
      return count
   };
   ```

3. 给你一个非空数组，返回此数组中 第三大的数 。如果不存在，则返回数组中最大的数。

   ```
   输入：[2, 2, 3, 1]
   输出：1
   解释：注意，要求返回第三大的数，是指在所有不同数字中排第三大的数。
   此例中存在两个值为 2 的数，它们都排第二。在所有不同数字中排第三大的数为 1 。
   ```

   ```js
   // 过滤重复值，依次比较大数值并逐级替换
   // 时间复杂度 O(n)，空间复杂度 O(1)
   const thirdMax = (nums) => {
      let max1 = max2 = max3 = Number.MIN_SAFE_INTEGER
      for (let v of nums) {
         if (v === max1 || v === max2) continue
         if (v > max1) {
            [max1, max2, max3] = [v, max1, max2]
         } else if (v > max2) {
            [max2, max3] = [v, max2]
         } else if (v > max3) {
            max3 = v
         }
      }
      return (max3 !== Number.MIN_SAFE_INTEGER) ? max3 : max1
   }
   ```

   > [查看详情](https://leetcode-cn.com/problems/third-maximum-number/solution/414-di-san-da-de-shu-by-i3raztjhzn-4pgu/) | leetcode

4. 给你一个整型数组 nums ，在数组中找出由三个数组成的最大乘积，并输出这个乘积。

   ```
   输入：nums = [1,2,3]
   输出：6
   ```

   ```js
   // 求出数组中最大的三个数以及最小的两个数，并对其乘积进行比较
   // 时间复杂度 O(n)，空间复杂度 O(1)
   const maximumProduct = (nums) => {
      let max1 = max2 = max3 = Number.MIN_SAFE_INTEGER
      let min1 = min2 = Number.MAX_SAFE_INTEGER
      for (let v of nums) {
         if (v > max1) {
            [max1, max2, max3] = [v, max1, max2]
         } else if (v > max2) {
            [max2, max3] = [v, max2]
         } else if (v > max3) {
            max3 = v
         }
         if (v < min1) {
            [min1, min2] = [v, min1]
         } else if (v < min2) {
            min2 = v
         }
      }
      return Math.max(min1 * min2 * max1, max1 * max2 * max3)
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/maximum-product-of-three-numbers/solution/san-ge-shu-de-zui-da-cheng-ji-by-leetcod-t9sb/) | leetcode

**统计数组中的元素**

1. 给定一个整数数组 a，其中1 ≤ a[i] ≤ n （n为数组长度）, 其中有些元素出现两次而其他元素出现一次。

   找到所有出现两次的元素。

   你可以不用到任何额外空间并在O(n)时间复杂度内解决这个问题吗？

   ```
   输入:
   [4,3,2,7,8,2,3,1]

   输出:
   [2,3]
   ```

   ```js
   // 原地哈希将第一次出现的数字置为相反数，继续遍历时若是负数则为重复值
   // 时间复杂度 O(n)，空间复杂度 O(1)
   const findDuplicates = (nums) => {
      const arr = [];
      for (let v of nums) {
         if (nums[Math.abs(v) - 1] < 0) {
            arr.push(Math.abs(v));
         } else {
            nums[Math.abs(v) - 1] *= -1;
         }
      }
      return arr;
   };
   ```
   > [查看详情](https://leetcode-cn.com/problems/find-all-duplicates-in-an-array/solution/you-ya-shi-xian-yuan-di-ha-xi-qiao-yong-p8p43/) | leetcode

2. 给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

   找到所有在 [1, n] 范围之间没有出现在数组中的数字。

   您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

   ```
   输入:
   [4,3,2,7,8,2,3,1]

   输出:
   [5,6]
   ```

   ```js
   // 利用抽屉原理，第一次遍历后负数代表被占掉，第二次遍历缺失的数字对应值为正数
   // 时间复杂度 O(n)，空间复杂度 O(1)
   const findDisappearedNumbers = (nums) => {
      for (let v of nums) {
         nums[Math.abs(v) - 1] > 0 && (nums[Math.abs(v) - 1] *= -1)
      }
      let i = 0;
      const arr = [];
      for (; i < nums.length; i++) {
         if (nums[i] > 0) {
            arr.push(i + 1);
         }
      }
      return arr;
   };
   ```
   > [查看详情](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/solution/448-zhao-dao-suo-you-shu-zu-zhong-xiao-s-4njf/) | leetcode

3. 集合 s 包含从 1 到 n 的整数。不幸的是，因为数据错误，导致集合里面某一个数字复制了成了集合里面的另外一个数字的值，导致集合 丢失了一个数字 并且 有一个数字重复 。

   给定一个数组 nums 代表了集合 S 发生错误后的结果。

   请你找出重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

   ```
   输入：nums = [1,2,2,4]
   输出：[2,3]
   ```

   ```js
   // 利用原地哈希查找重复值和缺失值的方法
   // 时间复杂度 O(n)，空间复杂度 O(1)
   const findErrorNums = (nums) => {
      let target;
      for (let v of nums) {
         nums[Math.abs(v) - 1] > 0 ? nums[Math.abs(v) - 1] *= -1 : target = Math.abs(v);
      }
      let i = 0;
      for (; i < nums.length; i++) {
         if (nums[i] > 0) {
            break;
         }
      }
      return [target, i + 1];
   };
   ```
   > [查看详情](https://leetcode-cn.com/problems/set-mismatch/solution/cuo-wu-de-ji-he-by-leetcode/) | leetcode

3. 给定一个非空且只包含非负数的整数数组 nums，数组的度的定义是指数组里任一元素出现频数的最大值。

   你的任务是在 nums 中找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度。

   ```
   输入：[1, 2, 2, 3, 1]
   输出：2
   解释：
   输入数组的度是2，因为元素1和2的出现频数最大，均为2.
   连续子数组里面拥有相同度的有如下所示:
   [1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
   最短连续子数组[2, 2]的长度为2，所以返回2.
   ```

   ```js
   // 遍历数组将次数、起点、终点记录到哈希表中
   // 时间复杂度 O(n)，空间复杂度 O(n)
   const findShortestSubArray = (nums) => {
      const obj = {};
      let max = 0;
      let minLen = 0
      for (let i = 0; i < nums.length; i++) {
         if (!obj[nums[i]]) {
            obj[nums[i]] = [1, i, i];
         } else {
            obj[nums[i]][0]++;
            obj[nums[i]][2] = i;
            if (obj[nums[i]][0] > max) {
               max = obj[nums[i]][0];
               minLen = obj[nums[i]][2] - obj[nums[i]][1]
            } else if (obj[nums[i]][0] === max) {
               minLen = Math.min(obj[nums[i]][2] - obj[nums[i]][1], minLen)
            }

         }
      }
      return minLen + 1;
   };
   ```
   > [查看详情](https://leetcode-cn.com/problems/degree-of-an-array/solution/zhi-pao-yi-ci-xun-huan-bian-pao-bian-ji-lu-li-yong/) | leetcode

4. 给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。

   进阶：你可以实现时间复杂度为 O(n) 并且只使用常数级别额外空间的解决方案吗？

   ```
   输入：nums = [3,4,-1,1]
   输出：2
   ```

   ```js
   // 将非正数更改为大于或等于 N + 1 的值不会占用缺失数字的下标，之后再参照原地哈希取得缺失的数字
   // 时间复杂度 O(n)，空间复杂度 O(n)
   const firstMissingPositive = (nums) => {
      for (let i = 0; i < nums.length; i++) {
         if (nums[i] <= 0) nums[i] = nums.length + 1;
      }
      for (let v of nums) {
         nums[Math.abs(v) - 1] > 0 && (nums[Math.abs(v) - 1] *= -1);
      }
      let i = 0;
      for (; i <= nums.length; i++) {
         if (nums[i] > 0) {
            return i + 1;
         }
      }
      return nums.length + 1;
   };
   ```
   > [查看详情](https://leetcode-cn.com/problems/first-missing-positive/solution/que-shi-de-di-yi-ge-zheng-shu-by-leetcode-solution/) | leetcode

5. 给定一位研究者论文被引用次数的数组（被引用次数是非负整数）。编写一个方法，计算出研究者的 h 指数。

   h 指数的定义：h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。且其余的 N - h 篇论文每篇被引用次数 不超过 h 次。

   例如：某人的 h 指数是 20，这表示他已发表的论文中，每篇被引用了至少 20 次的论文总共有 20 篇。

   ```
   输入：nums = [3,4,-1,1]
   输出：2
   ```

   ```js
   // 对进行计数，再倒序进行比较，返回最近且最大的下标
   // 时间复杂度 O(n)，空间复杂度 O(n)
   const hIndex = (nums) => {
      const arr = new Array(nums.length + 1).fill(0);
      for (let v of nums) {
         v = Math.min(v, nums.length);
         arr[v]++;
      }
      let count = 0;
      for (let i = arr.length - 1; i >= 0; i--) {
         count += arr[i];
         if (count >= i) return i;
      }
      return count;
   }
   ```
   > [查看详情](https://leetcode-cn.com/problems/h-index/) | leetcode

**数组中的改变、移动**

1. 给定一个长度为 n 的 非空 整数数组，每次操作将会使 n - 1 个元素增加 1。找出让数组所有元素相等的最小操作次数。

   ```
   输入：
   [1,2,3]
   输出：
   3
   解释：
   只需要3次操作（注意每次操作会增加两个元素的值）：
   [1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
   ```

   ```js
   const minMoves = (nums) => {
      let count = 0;
      const min = Math.min(...nums);
      for (let v of nums) {
         count += v - min;
      }
      return count;
   }
   ```
   
   > [查看详情](https://leetcode-cn.com/problems/minimum-moves-to-equal-array-elements/solution/zui-xiao-yi-dong-ci-shu-shi-shu-zu-yuan-su-xiang-d/) | leetcode

2. 给你一个长度为 n 的整数数组，请你判断在 最多 改变 1 个元素的情况下，该数组能否变成一个非递减数列。

   我们是这样定义一个非递减数列的： 对于数组中任意的 i (0 <= i <= n-2)，总满足 nums[i] <= nums[i + 1]。

   ```
   输入: nums = [4,2,3]
   输出: true
   解释: 你可以通过把第一个4变成1来使得它成为一个非递减数列。
   ```

   ```js
   // 遍历数组，如果当前项大于下一项，对比上一项和下一项。
   // 若上一项大于下一项，那么将下一项修改为当前项，否则不修改。
   const checkPossibility = (nums) => {
      let count = 0;
      for (let i = 0; i < nums.length; i++) {
         if (nums[i] > nums[i + 1]) {
            nums[i - 1] > nums[i + 1] && (nums[i + 1] = nums[i])
            count++;
            if (count === 2) return false;
         }
      }
      return true;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/non-decreasing-array/solution/fei-di-jian-shu-lie-by-leetcode-solution-zdsm/) | leetcode

3. 给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

   ```
   示例:

   输入: [0,1,0,3,12]
   输出: [1,3,12,0,0]
   说明:

   必须在原数组上操作，不能拷贝额外的数组。
   尽量减少操作次数。
   ```

   ```js
   // 双指针遍历交换
   const moveZeroes = (nums) => {
      let i = 0;
      let j = 0;
      for (; i < nums.length; i++) {
         if (nums[i] !== 0) {
            if (j < i) {
               nums[j] = nums[i];
               nums[i] = 0;
            }
            j++;
         }
      }
      return nums;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/move-zeroes/solution/dong-hua-yan-shi-283yi-dong-ling-by-wang_ni_ma/342802) | leetcode

**旋转数组**

1. 给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

   ```
   示例:

   输入: nums = [1,2,3,4,5,6,7], k = 3
   输出: [5,6,7,1,2,3,4]
   解释:
   向右旋转 1 步: [7,1,2,3,4,5,6]
   向右旋转 2 步: [6,7,1,2,3,4,5]
   向右旋转 3 步: [5,6,7,1,2,3,4]

   ```

   ```js
   const reverse = (nums, start, end) => {
      while (start < end) {
         [nums[start], nums[end]] = [nums[end], nums[start]];
         start++;
         end--;
      }
   }  

   // 先旋转数组一次，再旋转 k 位置两侧的数组项
   const rotate = (nums, k) => {
      const len = nums.length;
      if (k > len) k %= len;
      reverse(nums, 0, len - 1);
      reverse(nums, 0, k - 1);
      reverse(nums, k, len - 1);
      console.log(nums);
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/rotate-array/solution/xuan-zhuan-shu-zu-by-leetcode-solution-nipk/) | leetcode

