# 2020-8-10
453.给定一个长度为 n 的非空整数数组，找到让数组所有元素相等的最小移动次数。每次移动将会使 n - 1 个元素增加 1。
    示例:
    输入:
    [1,2,3]
    输出:
    3
    解释:
    只需要3次移动（注意每次移动会增加两个元素的值）：
    [1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
题解:
思路：首先数组排序找到最大最小元素max,min,然后用diff = max - min来更新数组元素，即除最大元素外的所有元素增加diff，更新后的数组开头元素 a'[0] 变成了 a[0]+diff=a[n-1]。
因此，a'[0] 等于上一步中最大的元素 a[n-1]。由于数组排过序，直到 i-2 的元素都满足 a[j]>=a[j-1]。因此，更新之后，a'[n-2] 即为最大元素。而 a[0] 依然是最小元素。
于是，在第二次更新时，diff=a[n-2]-a[0]。更新后 a''[0] 会成为 a'[n-2]，与上一次迭代类似。
然后，由于 a'[0] 和 a'[n-1] 相等，在第二次更新后，a''[0]=a''[n-1]=a'[n-2]。于是，最大的元素为 a[n-3]。
于是，我们可以继续这样，在每一步用最大最小值差更新数组。
下面进入第二步。第一步中，我们假设每一步会更新数组 aaa 中的元素。但事实上，我们不需要这么做。这是因为，即使是在更新元素之后，我们要登记的 diff 差值也不变，因为 max 和 min 增加的数字相同。
class Solution {
    public int minMoves(int[] nums) {
        Arrays.sort(nums);
        int count = 0;
        for(int i = nums.length - 1;i > 0;i--){
            count += nums[i] - nums[0];
        }
        return count;
    }
}
455. 分发饼干
      假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。对每个孩子 i ，都有一个胃口值 gi ，
      这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j ，都有一个尺寸 sj 。
      如果 sj >= gi ，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。
      注意：
      你可以假设胃口值为正。
      一个小朋友最多只能拥有一块饼干。
      示例 1:
      输入: [1,2,3], [1,1]
      输出: 1
      解释: 
      你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
      虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
      所以你应该输出1。
思路：
    给一个孩子的饼干应当尽量小并且又能满足该孩子，这样大饼干才能拿来给满足度比较大的孩子。
    因为满足度最小的孩子最容易得到满足，所以先满足满足度最小的孩子。
题解：
     class Solution {
        public int findContentChildren(int[] g, int[] s) {
            Arrays.sort(g);
            Arrays.sort(s);
            int i = 0,j = 0;
            for(;i < g.length && j < s.length;){
                if(g[i] <= s[j])
                    i++;
                j++;
            }
            return i;
        }
    }
459. 重复的子字符串
      给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。
      示例 1:
      输入: "abab"
      输出: True
      解释: 可由子字符串 "ab" 重复两次构成。
思路：创建一个新的字符串str,它等于原来的字符串S再加上S自身，这样其实就包含了所有移动的字符串。
      比如字符串：S = acd，那么str = S + S = acdacd
      acd移动的可能：dac、cda。其实都包含在了str中了。就像一个滑动窗口
      一开始acd (acd) ，移动一次ac(dac)d,移动两次a(cda)cd。循环结束
      所以可以直接判断str中去除首尾元素之后，是否包含自身元素。如果包含。则表明存在重复子串。
题解：
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s+s).substring(1,(s+s).length() - 1).contains(s);
    }
}

5. 最长回文子串
    给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
    示例 1：
    输入: "babad"
    输出: "bab"
    注意: "aba" 也是一个有效答案。
 思路：根据回文子串的定义，枚举所有长度大于等于 222 的子串，依次判断它们是否是回文；
        在具体实现时，可以只针对大于“当前得到的最长回文子串长度”的子串进行“回文验证”；
        在记录最长回文子串的时候，可以只记录“当前子串的起始位置”和“子串长度”，不必做截取。   
题解：
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        if(len < 2) return s;
        int maxLen = 1;
        int begin = 0;
        char ans[] = s.toCharArray();
        for(int i = 0;i < len - 1;i++){
            for(int j = i + 1;j < len;j++){
                if(j - i + 1 > maxLen && validPalindromic(ans,i,j)){
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substring(begin,begin + maxLen);
    }
    private boolean validPalindromic(char [] ans,int left,int right){
        while(left < right){
            if(ans[left] != ans[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
}

6.Z 字形变换
    将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
    比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
    L   C   I   R
    E T O E S I I G
    E   D   H   N
    之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
    请你实现这个将字符串进行指定行数变换的函数：
    string convert(string s, int numRows);
    示例 1:
    输入: s = "LEETCODEISHIRING", numRows = 3
    输出: "LCIRETOESIIGEDHN"
思路：按顺序遍历字符串 s；res[i] += c： 把每个字符 c 填入对应行 sis_isi；i += flag： 更新当前字符 c 对应的行索引； flag = - flag： 在达到 ZZZ 字形转折点时，执行反向。
题解：
class Solution {
    public String convert(String s, int numRows) {
        if(numRows < 2) return s;
        List<StringBuilder> rows = new ArrayList<StringBuilder>();
        for(int i = 0;i < numRows;i++)
            rows.add(new StringBuilder());
        int i = 0,flag = -1;
        for(char c : s.toCharArray()){
            rows.get(i).append(c);
           //如果索引到达转折点执行反向：flag变号；
            if(i == 0 || i == numRows - 1)
                flag = - flag;
            i += flag;
        }
        StringBuilder res = new StringBuilder();
        for(StringBuilder row : rows)
            res.append(row);
        return res.toString();
    }
}
