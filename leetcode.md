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

2. 在《英雄联盟》的世界中，有一个叫 “提莫”的英雄，他的攻击可以让敌方英雄艾希（编者注：寒冰射手）进入中毒状态。现在，给出提莫对艾希的攻击时间序列和提莫攻击的中毒持续时间，你需要输出艾希的中毒状态总时长。

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

2. 给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

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

4. 给定一个非空且只包含非负数的整数数组 nums，数组的度的定义是指数组里任一元素出现频数的最大值。

   你的任务是在 nums 中找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度。

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

5. 给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。

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

6. 给定一位研究者论文被引用次数的数组（被引用次数是非负整数）。编写一个方法，计算出研究者的 h 指数。

   h 指数的定义：h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。且其余的 N - h 篇论文每篇被引用次数 不超过 h 次。

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

2. 给你一个长度为 n 的整数数组，请你判断在 最多 改变 1 个元素的情况下，该数组能否变成一个非递减数列。

   我们是这样定义一个非递减数列的： 对于数组中任意的 i (0 <= i <= n-2)，总满足 nums[i] <= nums[i + 1]。

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

**二维数组及滚动数组**

1. 给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。

   ```
   输入: 5
   输出:
   [
        [1],
       [1,1],
      [1,2,1],
     [1,3,3,1],
    [1,4,6,4,1]
   ]
   ```

   ```js
   const generate = (numRows) => {
      const nums = [];
      for (let i = 0; i < numRows; i++) {
         const arr = [];
         arr[0] = 1;
         arr[i] = 1;
         for (let j = 1; j < i; j++) {
            arr[j] = nums[i - 1][j - 1] + nums[i - 1][j];
         }
         nums.push(arr);
      }
      return nums;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/pascals-triangle/solution/yang-hui-san-jiao-by-leetcode-solution-lew9/) | leetcode

2. 给定一个非负索引 k，其中 k ≤ 33，返回杨辉三角的第 k 行。

   ```
   输入: 3
   输出: [1,3,3,1]
   ```

   ```js
   const getRow = (rowIndex) => {
      const arr = [];
      arr[0] = 1;
      for (let i = 1; i < rowIndex + 1; i++) {
         arr[i] = 1;
         for (let j = i - 1; j >= 1; j--) {
            arr[j] = arr[j] + arr[j - 1]
         }
      }
      return arr;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/pascals-triangle-ii/solution/yang-hui-san-jiao-ii-by-leetcode-solutio-shuk/) | leetcode

3. 包含整数的二维矩阵 M 表示一个图片的灰度。你需要设计一个平滑器来让每一个单元的灰度成为平均灰度 (向下舍入) ，平均灰度的计算是周围的8个单元和它本身的值求平均，如果周围的单元格不足八个，则尽可能多的利用它们。

   ```
   输入:
   [[1,1,1],
    [1,0,1],
    [1,1,1]]
   输出:
   [[0, 0, 0],
    [0, 0, 0],
    [0, 0, 0]]
   解释:
   对于点 (0,0), (0,2), (2,0), (2,2): 平均(3/4) = 平均(0.75) = 0
   对于点 (0,1), (1,0), (1,2), (2,1): 平均(5/6) = 平均(0.83333333) = 0
   对于点 (1,1): 平均(8/9) = 平均(0.88888889) = 0
   
   ```

   ```js
   const imageSmoother = (img) => {
      const x = img.length, y = img[0].length;
      const nums = Array(x).fill().map(() => Array(y).fill());
      for (let i = 0; i < x; i++) {
         for (let j = 0; j < y; j++) {
            let l = r = i, t = b = j;
            if (i >= 1) l = i - 1;
            if (i < x - 1) r = i + 1;
            if (j >= 1) t = j - 1;
            if (j < y - 1) b = j + 1;
            let sum = 0, count = (r - l + 1) * (b - t + 1);
            for (let i = l; i <= r; i++) {
               for (let j = t; j <= b; j++) {
                  sum += img[i][j];
               }
            }
            nums[i][j] = Math.floor(sum / count);
         }
      }
      return nums;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/image-smoother/solution/100-zhao-dao-shang-xia-xian-yu-zuo-you-xian-by-ooo/) | leetcode

4. 给定一个初始元素全部为 0，大小为 m*n 的矩阵 M 以及在 M 上的一系列更新操作。

   操作用二维数组表示，其中的每个操作用一个含有两个正整数 a 和 b 的数组表示，含义是将所有符合 0 <= i < a 以及 0 <= j < b 的元素`M[i][j]`的值都增加 1。

   在执行给定的一系列操作后，你需要返回矩阵中含有最大整数的元素个数。

   ```
   输入: 
   m = 3, n = 3
   operations = [[2,2],[3,3]]
   输出: 4
   解释: 
   初始状态, M = 
   [[0, 0, 0],
    [0, 0, 0],
    [0, 0, 0]] 
   
   执行完操作 [2,2] 后, M = 
   [[1, 1, 0],
    [1, 1, 0],
    [0, 0, 0]] 
   
   执行完操作 [3,3] 后, M = 
     [[2, 2, 1],
      [2, 2, 1],
      [1, 1, 1]]
   ```

   ```js
   const maxCount = (m, n, ops) => {
      let r = m, b = n;
       for (let v of ops) {
         r = Math.min(r, v[0]), b = Math.min(b, v[1]);
       }
       return r * b;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/range-addition-ii/solution/fan-wei-qiu-he-ii-by-leetcode/) | leetcode

5. 给定一个二维的甲板， 请计算其中有多少艘战舰。 战舰用 'X'表示，空位用 '.'表示。 你需要遵守以下规则：

   给你一个有效的甲板，仅由战舰或者空位组成。
   战舰只能水平或者垂直放置。换句话说,战舰只能由 1xN (1 行, N 列)组成，或者 Nx1 (N 行, 1 列)组成，其中N可以是任意大小。
   两艘战舰之间至少有一个水平或垂直的空位分隔 - 即没有相邻的战舰。

   ```
   示例 :  

   X..X
   ...X
   ...X
   在上面的甲板中有2艘战舰。
   ```

   进阶:

   你可以用一次扫描算法，只使用O(1)额外空间，并且不修改甲板的值来解决这个问题吗？

   ```js
   const countBattleships = (board) => {
      const rows = board.length;
      const cols = board[0].length;
      let count = 0;
      for (let i = 0; i < rows; i++) {
         for (let j = 0; j < cols; j++) {
            if (board[i][j] === 'X' && (i === 0 || board[i - 1][j] === '.') && (j === 0 || board[i][j - 1] === '.')) count++;
         }
      }
      return count;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/range-addition-ii/solution/fan-wei-qiu-he-ii-by-leetcode/https://leetcode-cn.com/problems/battleships-in-a-board/solution/qian-lu-qi-qu-wang-wo-men-ke-yi-hu-xiang-4h10/) | leetcode   

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

2. 给定一个长度为 n 的整数数组 A 。

   假设 Bk 是数组 A 顺时针旋转 k 个位置后的数组，我们定义 A 的“旋转函数” F 为： 

   F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]。 

   计算F(0), F(1), ..., F(n-1)中的最大值。

   ```
   A = [4, 3, 2, 6]  
   F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
   F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
   F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
   F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26  
   所以 F(0), F(1), F(2), F(3) 中的最大值是 F(3) = 26 。
   
   ```

   ```js
   // 递推 f(n) = f(n-1) + sum - n * nums[n - 1]
   const maxRotateFunction = (nums) => {
      let count, sum;
      count = nums.reduce((a, b, i) => a + b * i, 0);
      sum = nums.reduce((a, b) => a + b);
      let max = Number.MIN_SAFE_INTEGER;
      for (let i = 0, len = nums.length; i < len; i++) {
         count += sum - len * nums[len - 1 - i];
         if (count > max) max = count;
      }
      return max;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/rotate-function/solution/oncuo-wei-xiang-jian-by-xiaohu9527-ftzh/) | leetcode

**特定顺序遍历二维数组**

1. 给你一个 m 行 n 列的矩阵 matrix，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。

   ```
   输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
   输出：[1,2,3,6,9,8,7,4,5]
   ```

   ```js
   const spiralOrder = (matrix) => {
      let left = 0, right = matrix[0].length - 1, top = 0, bottom = matrix.length - 1;
      const res = [];
      while (left <= right && top <= bottom) {
         for (let j = left; j <= right; j++) {
            res.push(matrix[left][j])
         }
         for (let i = top + 1; i <= bottom; i++) {
            res.push(matrix[i][right])
         }
         if (left < right && top < bottom) {
            for (let j = right - 1; j > left; j--) {
               res.push(matrix[bottom][j])
            }
            for (let i = bottom; i > top; i--) {
               res.push(matrix[i][left])
            }
         }
         [left, right, top, bottom] = [++left, --right, ++top, --bottom]
      }
      return res;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/spiral-matrix/solution/luo-xuan-ju-zhen-by-leetcode-solution/) | leetcode


2. 给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。

   ```
   输入：n = 3
   输出：[[1,2,3],[8,9,4],[7,6,5]]
   ```

   ```js
   const generateMatrix = (n) => {
      let top = 0, bottom = n - 1, left = 0, right = n - 1;
      let count = 0
      const res = Array(n).fill(0).map(v => Array(n).fill(0))
      while (left <= right && top <= bottom) {
         for (let j = left; j <= right; j++) {
            res[top][j] = ++count;
         }
         for (let i = top + 1; i <= bottom; i++) {
            res[i][right] = ++count;
         }
         if (left < right && top < bottom) {
            for (let j = right - 1; j > left; j--) {
               res[bottom][j] = ++count;
            }
            for (let i = bottom; i > top; i--) {
               res[i][left] = ++count;
            }
         }
         [top, bottom, left, right] = [++top, --bottom, ++left, --right]
      }
      return res;
   };
   ```

   > [查看详情](https://leetcode-cn.com/problems/spiral-matrix-ii/solution/luo-xuan-ju-zhen-ii-by-leetcode-solution-f7fp/) | leetcode

3. 给定一个含有 M x N 个元素的矩阵（M 行，N 列），请以对角线遍历的顺序返回这个矩阵中的所有元素，对角线遍历如下图所示。

   ```
   输入:
   [
    [ 1, 2, 3 ],
    [ 4, 5, 6 ],
    [ 7, 8, 9 ]
   ]

   输出:  [1,2,4,7,5,3,6,8,9]
   ```

   ```js
   const findDiagonalOrder = (mat) => {
      const rows = mat.length, cols = mat[0].length;
      let i = 0, j = 0, k = 0, flag = true;
      const res = [];
      while (i < rows && j < cols) {
         res[k++] = mat[i][j];
         let _i = flag ? i - 1 : i + 1;
         let _j = flag ? j + 1 : j - 1;
         if (_i < 0 || _i === rows || _j < 0 || _j === cols) {
            if (flag) {
               j < cols - 1 ? j++ : i++;
            } else {
               i < rows - 1 ? i++ : j++;
            }
            flag = !flag;
         } else {
            i = _i;
            j = _j;
         }
      };
      return res;
   }
   ```

   > [查看详情](https://leetcode-cn.com/problems/diagonal-traverse/solution/dui-jiao-xian-bian-li-by-leetcode/) | leetcode

**二维数组变换**

   1. 在MATLAB中，有一个非常有用的函数 reshape，它可以将一个矩阵重塑为另一个大小不同的新矩阵，但保留其原始数据。

      给出一个由二维数组表示的矩阵，以及两个正整数r和c，分别表示想要的重构的矩阵的行数和列数。

      重构后的矩阵需要将原始矩阵的所有元素以相同的行遍历顺序填充。      

      如果具有给定参数的reshape操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

      ```
      输入: 
      nums = 
      [[1,2],
       [3,4]]
      r = 1, c = 4
      输出: 
      [[1,2,3,4]]
      ```   

      ```js
      const matrixReshape = (mat, r, c) => {
         const rows = mat.length, cols = mat[0].length;
         if (rows * cols !== r * c) return mat;
         const res = Array(r).fill(0).map(() => Array(c).fill(0));
         let _i = 0, _j = 0;
         for (let i = 0; i < rows; i++) {
            for (let j = 0; j < cols; j++) {
               res[_i][_j] = mat[i][j];
               if (_j < c - 1) {
                  _j++;
               } else {
                  _i++;
                  _j = 0;
               }
            }
         }
         return res;
      };
      ```   

      > [查看详情](https://leetcode-cn.com/problems/reshape-the-matrix/solution/zhong-su-ju-zhen-by-leetcode-solution-gt0g/) | leetcode

   2. 给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

      你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

      ```
      输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
      输出：[[7,4,1],[8,5,2],[9,6,3]]
      ```

      ```js
      const rotate = (matrix) => {
         const rows = matrix.length, cols = matrix[0].length;
         for (let i = 0; i < Math.floor(rows / 2); i++) {
            for (let j = 0; j < cols; j++) {
               [matrix[i][j], matrix[rows - 1 - i][j]] = [matrix[rows - 1 - i][j], matrix[i][j]]
            }
         }
         for (let i = 0; i < rows; i++) {
            for (let j = 0; j < i; j++) {
               [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]]
            }
         }
         return matrix;
      };
      ```
      
      > [查看详情](https://leetcode-cn.com/problems/rotate-image/solution/xuan-zhuan-tu-xiang-by-leetcode-solution-vu3m/) | leetcode

   3. 给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。

      ```
      输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
      输出：[[1,0,1],[0,0,0],[1,0,1]]
      ```

      ```js
      const setZeroes = (matrix) => {
         const rows = matrix.length, cols = matrix[0].length;
         let flagRow0 = false, flagCol0 = false
         for (let j = 0; j < cols; j++) {
            if (matrix[0][j] === 0) flagRow0 = true;
         }
         for (let i = 0; i < rows; i++) {
            if (matrix[i][0] === 0) flagCol0 = true;
         }
         for (let i = 1; i < rows; i++) {
            for (let j = 1; j < cols; j++) {
               if (matrix[i][j] === 0)  matrix[i][0] = matrix[0][j] = 0;
            }
         }
         for (let i = 1; i < rows; i++) {
            for (let j = 1; j < cols; j++) {
               if (matrix[0][j] === 0 || matrix[i][0] === 0) matrix[i][j] = 0;
            }
         }
         for (let j = 0; j < cols; j++) {
            if (flagRow0) matrix[0][j] = 0;
         }
         for (let i = 0; i < rows; i++) {
            if (flagCol0) matrix[i][0] = 0;
         }
         return matrix;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/set-matrix-zeroes/solution/ju-zhen-zhi-ling-by-leetcode-solution-9ll7/) | leetcode

   4. 根据 百度百科 ，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。

      给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：1 即为活细胞（live），或 0 即为死细胞（dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：     

      如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
      如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
      如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
      如果死细胞周围正好有三个活细胞，则该位置死细胞复活；
      下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。给你 m x n 网格面板 board 的当前状态，返回下一个状态。

      ```
      输入：board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
      输出：[[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
      ```

      ```js
      const gameOfLife = (board) => {
         const rows = board.length, cols = board[0].length;
         for (let i = 0; i < rows; i++) {
            for (let j = 0; j < cols; j++) {
               let x1 = i - 1 < 0 ? 0 : i - 1;
               let x2 = i + 1 > rows - 1 ? rows - 1 : i + 1;
               let y1 = j - 1 < 0 ? 0 : j - 1;
               let y2 = j + 1 > cols - 1 ? cols - 1 : j + 1;
               let count = 0;
               for (let _i = x1; _i <= x2; _i++) {
                  for (let _j = y1; _j <= y2; _j++) {
                     if (_i === i && _j === j) continue;
                     if (Math.abs(board[_i][_j]) === 1) count++;
                  }
               }
               if (board[i][j] === 1) {
                  if (count < 2 || count > 3) board[i][j] = -1
               } else if (board[i][j] === 0) {
                  if (count === 3) board[i][j] = 2
               }
            }
         }
         for (let i = 0; i < rows; i++) {
            for (let j = 0; j < cols; j++) {
               if (board[i][j] === 2) {
                  board[i][j] = 1
               } else if (board[i][j] === -1) {
                  board[i][j] = 0
               }
            }
         }
         return board;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/game-of-life/solution/sheng-ming-you-xi-by-leetcode-solution/) | leetcode

**前缀和数组**

   1. 给定一个整数数组  nums，求出数组从索引 i 到 j（i ≤ j）范围内元素的总和，包含 i、j 两点。

      实现 NumArray 类：

      NumArray(int[] nums) 使用数组 nums 初始化对象
      int sumRange(int i, int j) 返回数组 nums 从索引 i 到 j（i ≤ j）范围内元素的总和，包含 i、j 两点（也就是 sum(nums[i], nums[i + 1], ... , nums[j])）

      ```
      输入：
      ["NumArray", "sumRange", "sumRange", "sumRange"]
      [[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
      输出：
      [null, 1, -1, -3]    

      解释：
      NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
      numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
      numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
      numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
      ```

      ```js
      var NumArray = function(nums) {
         const len = nums.length, _nums = Array(len + 1).fill(0);
         for (let i = 0; i < len; i++) {
            _nums[i + 1] = _nums[i] + nums[i];
         }
         this._nums = _nums;
      };    

      NumArray.prototype.sumRange = function(left, right) {
         const nums = this._nums;
         return nums[right + 1] - nums[left];
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/range-sum-query-immutable/solution/qu-yu-he-jian-suo-shu-zu-bu-ke-bian-by-l-px41/) | leetcode

   2. 给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 (row1, col1) ，右下角为 (row2, col2) 。

      ```
      给定 matrix = [
        [3, 0, 1, 4, 2],
        [5, 6, 3, 2, 1],
        [1, 2, 0, 1, 5],
        [4, 1, 0, 1, 7],
        [1, 0, 3, 0, 5]
      ]     

      sumRegion(2, 1, 4, 3) -> 8
      sumRegion(1, 1, 2, 2) -> 11
      sumRegion(1, 2, 2, 4) -> 12
      ```

      ```js
      var NumMatrix = function(matrix) {
         const rows = matrix.length, cols = matrix[0].length;
         const nums = Array(rows + 1).fill(0).map(() => Array(cols + 1).fill(0));
         for (let i = 0; i < rows; i++) {
            for (let j = 0; j < cols; j++) {
               nums[i + 1][j + 1] = nums[i + 1][j] + nums[i][j + 1] - nums[i][j] + matrix[i][j];
            }
         }
         this.nums = nums;
      };    

      NumMatrix.prototype.sumRegion = function(row1, col1, row2, col2) {
         const nums = this.nums;
         return nums[row2 + 1][col2 + 1] - nums[row2 + 1][col1] - nums[row1][col2 + 1] + nums[row1][col1];
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/solution/er-wei-qian-zhui-he-jian-dan-tui-dao-tu-sqekv/) | leetcode

   3. 给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。


      ```
      输入: [1,2,3,4]
      输出: [24,12,8,6]
      ```

      ```js
      const productExceptSelf = (nums) => {
         const len = nums.length, res = [];
         res[0] = 1;
         for (let i = 0; i < len - 1; i++) {
            res[i + 1] = res[i] * nums[i];
         }
         for (let i = len - 1, r = 1; i >= 0; i--) {
            res[i] *= r;
            r *= nums[i];
         }
         return res;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/product-of-array-except-self/solution/chu-zi-shen-yi-wai-shu-zu-de-cheng-ji-by-leetcode-/) | leetcode

### 字符串

**字符**

   1. 给定一个单词，你需要判断单词的大写使用是否正确。

      我们定义，在以下情况时，单词的大写用法是正确的：

      全部字母都是大写，比如"USA"。
      单词中所有字母都不是大写，比如"leetcode"。
      如果单词不只含有一个字母，只有首字母大写， 比如 "Google"。
      否则，我们定义这个单词没有正确使用大写字母。

      ```
      输入: "USA"
      输出: True
      ```

      ```js
      const detectCapitalUse = (word) => {
         let len = word.length, count = 0;
         for(let i = 1; i < len; i++) {
             if(word[i] === word[i].toUpperCase()) count++;
         }
         if(count === 0 || (count === len - 1 && word[0].toUpperCase() === word[0])) return true;
         return false; 
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/detect-capital/solution/ctong-ji-da-xiao-xie-zi-mu-hou-pan-duan-psly2/) | leetcode

**回文串的定义**

   1. 给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

      说明：本题中，我们将空字符串定义为有效的回文串。

      ```
      输入: "A man, a plan, a canal: Panama"
      输出: true
      ```

      ```js
      const isPalindrome = (s) => {
         const _s = s.toLowerCase().match(/[0-9A-Za-z]/g);
         if (_s == null) return true;
         let i = 0, j = _s.length - 1;
         while (i < j) {
            if (_s[i] !== _s[j]) return false;
            i++;
            j--;
         }
         return true;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/valid-palindrome/solution/yan-zheng-hui-wen-chuan-by-leetcode-solution/) | leetcode

**公共前缀**

   1. 编写一个函数来查找字符串数组中的最长公共前缀。

      如果不存在公共前缀，返回空字符串 ""。

      ```
      输入：strs = ["flower","flow","flight"]
      输出："fl"
      ```

      ```js
      const commonPrefix = (str1, str2) => {
        const len = Math.min(str1.length, str2.length);
        let str = '';
        for (let i = 0; i < len; i++) {
          if (str1[i] !== str2[i]) break;
          str += str1[i]
        }
        return str;
      }     

      const longestCommonPrefix = (strs) => {
        let str = strs[0];
        for (let i = 1, len = strs.length; i < len; i++) {
          str = commonPrefix(str, strs[i]);
        }
        return str;
      }
      ```

      > [查看详情](https://leetcode-cn.com/problems/longest-common-prefix/solution/zui-chang-gong-gong-qian-zhui-by-leetcode-solution/) | leetcode

**单词**

   1. 统计字符串中的单词个数，这里的单词指的是连续的不是空格的字符。

      请注意，你可以假定字符串里不包括任何不可打印的字符。

      ```
      输入: "Hello, my name is John"
      输出: 5
      解释: 这里的单词是指连续的不是空格的字符，所以 "Hello," 算作 1 个单词。
      ```

      ```js
      const countSegments = (s) => {
        let count = 0;
        for (let i = 0, len = s.length; i < len; i++) {
          const cur = s[i], next = s[i + 1];
          if (cur !== ' ' && (next === ' ' || i === len - 1)) count++;
        }
        return count;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/number-of-segments-in-a-string/solution/zi-fu-chuan-zhong-de-dan-ci-shu-by-leetcode/) | leetcode

   2. 给你一个字符串 s，由若干单词组成，单词之间用空格隔开。返回字符串中最后一个单词的长度。如果不存在最后一个单词，请返回 0 。

      单词 是指仅由字母组成、不包含任何空格字符的最大子字符串。

      ```
      输入：s = "Hello World"
      输出：5
      ```

      ```js
      const lengthOfLastWord = (s) => {
        let count = 0;
        for (let len = s.length, i = len - 1; i >= 0; i--) {
          const cur = s[i], next = s[i - 1];
          if (cur === ' ') continue;
          count++;
          if (next === ' ') break;
        }
        return count;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/length-of-last-word/solution/hua-jie-suan-fa-58-zui-hou-yi-ge-dan-ci-de-chang-d/) | leetcode

**字符串的反转**

   1. 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

      不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。      

      你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

      ```
      输入：["h","e","l","l","o"]
      输出：["o","l","l","e","h"]
      ```

      ```js
      const reverseString = (s) => {
        let i = 0,
          j = s.length - 1;
        while (i < j) {
          [s[i], s[j]] = [s[j], s[i]];
          i++;
          j--;
        }
        return s;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/reverse-string/solution/fan-zhuan-zi-fu-chuan-by-leetcode-solution/) | leetcode

   2. 给定一个字符串 s 和一个整数 k，你需要对从字符串开头算起的每隔 2k 个字符的前 k 个字符进行反转。

      如果剩余字符少于 k 个，则将剩余字符全部反转。
      如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

      ```
      输入: s = "abcdefg", k = 2
      输出: "bacdfeg"
      ```

      ```js
      const reverse = (arr, start, end) => {
        while (start < end) {
          [arr[start], arr[end]] = [arr[end], arr[start]];
          start++;
          end--;
        }
        return arr;
      }     

      const reverseStr = (s, k) => {
        const arr = s.split('');
        for (let i = 0, len = arr.length; i < len; i = 2 * k + i) {
          let j = Math.min(i + k - 1, len - 1);
          reverse(arr, i, j);
        }
        return arr.join('');
      };
      ```
      > [查看详情](https://leetcode-cn.com/problems/reverse-string-ii/solution/fan-zhuan-zi-fu-chuan-ii-by-leetcode/) | leetcode

   3. 给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

      ```
      输入："Let's take LeetCode contest"
      输出："s'teL ekat edoCteeL tsetnoc"
      ```

      ```js
      const reverse = (arr, start, end) => {
        while (start < end) {
          [arr[start], arr[end]] = [arr[end], arr[start]];
          start++;
          end--;
        }
        return arr;
      }     

      const reverseWords = (s) => {
        const arr = s.split('');
        for (let i = 0, _i = 0, len = arr.length; i < len; i++) {
          if (arr[i] === ' ' || i === len - 1) {
            let j = arr[i] === ' ' ? i - 1 : i;
            reverse(arr, _i, j);
            _i = i + 1;
          }
        }
        return arr.join('');
      };
      ```
      
      > [查看详情](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/) | leetcode

   4. 给你一个字符串 s ，逐个翻转字符串中的所有 单词 。

      单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。      

      请你返回一个翻转 s 中单词顺序并用单个空格相连的字符串。    

      说明：      

      输入字符串 s 可以在前面、后面或者单词间包含多余的空格。
      翻转后单词间应当仅用一个空格分隔。
      翻转后的字符串中不应包含额外的空格。

      ```
      输入：s = "the sky is blue"
      输出："blue is sky the"
      ```

      ```js
      const reverseWords = (s) => {
        let i = 0, j = s.length - 1;
        while (s[i] === ' ') i++;
        while (s[j] === ' ') j--;
        const arr = [];
        let str = ''
        while (i <= j) {
          const cur = s[i], next = s[i + 1];
          if (cur !== ' ') str += s[i];
          if (cur !== ' ' && (next === ' ' || i === j)) {
            arr.unshift(str);
            str = '';
          }
          i++;
        }
        return arr.join(' ');
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/reverse-words-in-a-string/solution/fan-zhuan-zi-fu-chuan-li-de-dan-ci-by-leetcode-sol/) | leetcode

**字符的统计**

   1. 给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

      ```
      s = "leetcode"
      返回 0     

      s = "loveleetcode"
      返回 2
      ```

      ```js
      const firstUniqChar = (s) => {
        const obj = {};
        let target = -1;
        for (let v of s) {
          obj[v] = obj[v] ? ++obj[v] : 1;
        }
        for (let i = 0, len = s.length; i < len; i++) {
          if (obj[s[i]] === 1) {
            target = i;
            break;
          } 
        }
        return target;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/reverse-words-in-a-string/solution/fan-zhuan-zi-fu-chuan-li-de-dan-ci-by-leetcode-sol/) | leetcode

   2. 给定两个字符串 s 和 t，它们只包含小写字母。

      字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母。

      请找出在 t 中被添加的字母。   

      ```
      输入：s = "abcd", t = "abcde"
      输出："e"
      解释：'e' 是那个被添加的字母。
      ```

      ```js
      const findTheDifference = (s, t) => {
        const obj = {};
        for (let v of s) {
          obj[v] = obj[v] ? ++obj[v] : 1;
        }
        for (let v of t) {
          if (obj[v]) {
            --obj[v];
            if (obj[v] < 0) return v;
          } else {
            return v;
          }
        }
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/find-the-difference/solution/zhao-bu-tong-by-leetcode-solution-mtqf/) | leetcode

   3. 给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串 magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。

      (题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。杂志字符串中的每个字符只能在赎金信字符串中使用一次。)

      ```
      输入：ransomNote = "a", magazine = "b"
      输出：false
      ```

      ```js
      const canConstruct = (ransomNote, magazine) => {
        const obj = {};
        for (let v of magazine) {
          obj[v] = obj[v] ? ++obj[v] : 1;
        }
        for (let v of ransomNote) {
          if (obj[v]) {
            --obj[v];
            if (obj[v] < 0) return false;
          } else {
            return false;
          }
        }
        return true;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/ransom-note/solution/ji-lu-zi-mu-de-chu-xian-ci-shu-bing-bi-j-zksx/) | leetcode

   4. 给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

      ```
      输入: s = "anagram", t = "nagaram"
      输出: true
      ```

      ```js
      const isAnagram = (s, t) => {
        if (s.length !== t.length) return false;
        const obj = {};
        for (let v of s) {
          obj[v] = obj[v] ? ++obj[v] : 1;
        }
        for (let v of t) {
          if (obj[v]) {
            --obj[v];
            if (obj[v] < 0) return false;
          } else {
            return false;
          }
        }
        return true;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/valid-anagram/solution/you-xiao-de-zi-mu-yi-wei-ci-by-leetcode-solution/) | leetcode 


   5. 给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

      ```
      输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
      输出:
      [
        ["ate","eat","tea"],
        ["nat","tan"],
        ["bat"]
      ]
      ```

      ```js
      const groupAnagrams = (strs) => {
        const obj = {};
        for (let v of strs) {
          const _v = Array.from(v).sort().toString();
          obj[_v] ? obj[_v].push(v) : obj[_v] = [v];
        }
        return Object.values(obj);
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/group-anagrams/solution/zi-mu-yi-wei-ci-fen-zu-by-leetcode-solut-gyoc/) | leetcode 

   6. 给定一个字符串，请将字符串里的字符按照出现的频率降序排列。

      ```
      输入:
      "tree"      

      输出:
      "eert"      

      解释:
      'e'出现两次，'r'和't'都只出现一次。
      因此'e'必须出现在'r'和't'之前。此外，"eetr"也是一个有效的答案。
      ```

      ```js
      const frequencySort = (s) => {
        const obj = {};
        for (let v of s) {
          obj[v] = obj[v] ? ++obj[v] : 1;
        }
        const arr = Object.entries(obj).sort((a, b) => b[1] - a[1]);
        let str = '';
        for (let [key, value] of arr) {
          str += key.repeat(value);
        }
        return str;
      };
      ```
      
      > [查看详情](https://leetcode-cn.com/problems/sort-characters-by-frequency/solution/gen-ju-zi-fu-chu-xian-pin-lu-pai-xu-by-l-zmvy/) | leetcode 

   7. 给定一个非空字符串，其中包含字母顺序打乱的英文单词表示的数字0-9。按升序输出原始的数字。

      注意:      

      输入只包含小写英文字母。
      输入保证合法并可以转换为原始的数字，这意味着像 "abc" 或 "zerone" 的输入是不允许的。
      输入字符串的长度小于 50,000。

      ```
      输入: "owoztneoer"

      输出: "012" (zeroonetwo)
      ```

      ```js
      const countDigits = (str, arr) => {
        const obj = {};
        for (let v of arr) {
          obj[v] = 0;
        }
        for (let v of str) {
          ++obj[v];
        }
        return obj;
      }     

      const originalDigits = (s) => {
        const digits = ['z', 'o', 'w', 'h', 'u', 'f', 'x', 's', 'g', 'i'];
        const obj = countDigits(s, digits);
        obj['h'] -= obj['g'];
        obj['f'] -= obj['u'];
        obj['s'] -= obj['x'];
        obj['i'] -= (obj['f'] + obj['x'] + obj['g']);
        obj['o'] -= (obj['z'] + obj['w'] + obj['u']);
        let str = '';
        for (let i = 0, len = digits.length; i < len; i++) {
          str += i.toString().repeat(obj[digits[i]]);
        }
        return str;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/reconstruct-original-digits-from-english/solution/cong-ying-wen-zhong-zhong-jian-shu-zi-by-leetcode/) | leetcode 

   8. 在二维平面上，有一个机器人从原点 (0, 0) 开始。给出它的移动顺序，判断这个机器人在完成移动后是否在 (0, 0) 处结束。

      移动顺序由字符串表示。字符 move[i] 表示其第 i 次移动。机器人的有效动作有 R（右），L（左），U（上）和 D（下）。如果机器人在完成所有动作后返回原点，则返回 true。否则，返回 false。     

      注意：机器人“面朝”的方向无关紧要。 “R” 将始终使机器人向右移动一次，“L” 将始终向左移动等。此外，假设每次移动机器人的移动幅度相同。

      ```
      输入: "UD"
      输出: true
      解释：机器人向上移动一次，然后向下移动一次。所有动作都具有相同的幅度，因此它最终回到它开始的原点。因此，我们返回 true。
      ```

      ```js
      const judgeCircle = (moves) => {
        let x = 0, y = 0;
        for (let v of moves) {
          if (v === 'R') {
            x++;
          } else if (v === 'L') {
            x--;
          } else if (v === 'U') {
            y++;
          } else if (v === 'D') {
            y--;
          }
        }
        return x === 0 && y === 0;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/robot-return-to-origin/solution/ji-qi-ren-neng-fou-fan-hui-yuan-dian-by-leetcode-s/) | leetcode 

   9. 给定一个字符串来代表一个学生的出勤记录，这个记录仅包含以下三个字符：

      'A' : Absent，缺勤
      'L' : Late，迟到
      'P' : Present，到场
      如果一个学生的出勤记录中不超过一个'A'(缺勤)并且不超过两个连续的'L'(迟到),那么这个学生会被奖赏。    

      你需要根据这个学生的出勤记录判断他是否会被奖赏。

      ```
      输入: "PPALLP"
      输出: True
      ```

      ```js
      const checkRecord = (s) => {
        let countA = 0, j = -1;
        for (let i = 0, len = s.length; i < len; i++) {
          if (s[i] === 'A') {
            countA++;
            if (countA === 2) return false;
          }
          if (s[i] !== 'L') {
            j = i;
          } else {
            if (i - j === 3) return false;
          }
        }
        return true;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/student-attendance-record-i/solution/xue-sheng-chu-qin-ji-lu-i-by-leetcode/) | leetcode 

   10. 给定一个字符串 s，计算具有相同数量 0 和 1 的非空（连续）子字符串的数量，并且这些子字符串中的所有 0 和所有 1 都是连续的。
      
      重复出现的子串要计算它们出现的次数。

      ```
      输入: "00110011"
      输出: 6
      解释: 有6个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。       

      请注意，一些重复出现的子串要计算它们出现的次数。       

      另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。
      ```

      ```js
      const countBinarySubstrings = (s) => {
        let i = 0, last = 0, res = 0, n = s.length;
        while (i < n) {
          const num = s[i];
          let count = 0;
          while (i < n && s[i] === num) {
            ++i;
            ++count;
          }
          res += Math.min(count, last);
          last = count;
        }
        return res;
      };
      ```

      > [查看详情](https://leetcode-cn.com/problems/count-binary-substrings/solution/ji-shu-er-jin-zhi-zi-chuan-by-leetcode-solution/) | leetcode 









