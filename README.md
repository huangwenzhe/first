import java.util.*;

public class Main {
    public String addBinary(String a, String b) {
        
    }

        /**
         * 给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。
         *遍历A,B所有元素并且将其和记录在map中，而后在C,D中找出与map中key值相加为0的项即为所求
         * @param A
         * @param B
         * @param C
         * @param D
         * @return
         */
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer,Integer> map = new HashMap();
        int count = 0;
        for(int a:A){
            for(int b :B){
                map.put(a+b,map.getOrDefault(a+b,0)+1);
            }
        }
        for(int c:C){
            for(int d :D){
                count += map.getOrDefault(-c-d,0);
            }
        }
        return count;
    }
    /**
     * 这个是对于三个数之和的升级版，通过四个指针进行计算四个数之和为0的元素
     * @param nums
     * @param target
     * @return
     */
    public static List<List<Integer>> fourSum(int[] nums, int target) {
            Arrays.sort(nums);
            List<List<Integer>> ret = new ArrayList<>();
            if(nums.length<4){
                return ret;
            }
            for(int i = 0;i <nums.length-3;i++){
                if(i>0&&nums[i]==nums[i-1]){
                    continue;
                }
                if(nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target){
                    break;
                }
                if(nums[i]+nums[nums.length-1]+nums[nums.length-2]+nums[nums.length-3]<target){
                    continue;
                }
                for(int j = i+1;j<nums.length-2;j++){
                    if(j>i+1&&nums[j]==nums[j-1]){
                        continue;
                    }
                    if(nums[j]+nums[j+1]+nums[j+2]+nums[i]>target){
                        break;
                    }
                    if(nums[j]+nums[nums.length-1]+nums[nums.length-2]+nums[i]<target){
                        continue;
                    }
                    int start  = j+1;
                    int end = nums.length-1;
                    while (start<end){
                        int sum = nums[start]+nums[end]+nums[i]+nums[j];
                        if(sum>target){
                            while(start<end&&nums[end]==nums[--end]);
                        }else if(sum<target){
                            while(start<end&&nums[start]==nums[++start]);
                        }else{
                            ret.add(Arrays.asList(nums[i],nums[j],nums[start],nums[end]));
                            while(start<end&&nums[start]==nums[++start]);
                            while(start<end&&nums[end]==nums[--end]);
                        }
                    }
                }
            }
            return ret;
    }
        /**
         * 使用双指针法不断地逼近target
         * @param nums
         * @param target
         * @return
         */
    public static int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closeNum = nums[0]+nums[1]+nums[2];
        for(int k = 0 ; k<nums.length-2; k++){
            int i = k+1;
            int j =nums.length-1;
            while(i<j){
                int sum = nums[k]+nums[j]+nums[i];
                if(Math.abs(sum-target)<Math.abs(closeNum-target)){
                    closeNum = sum;
                }
                if(sum>target){
                    j--;
                }else if(sum <target){
                    i++;
                }else{
                    return target ;
                }
            }
        }
        return closeNum;
    }
    /**
     * 首先进行数组排序，若排序后第一个元素大于0，则无法找到三个元素之和大于0
     * 使用双指针法，固定数组的第一个元素，将i置为数组的第二个元素，j置为数组的最后一个元素，进行遍历整个数组
     * 为了消除重复，使用while（）循环跳过相同的数字
     * @param nums
     * @return
     */
    public static List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ret = new ArrayList<>();
        for(int k = 0;k<nums.length-2;k++){
            int i = k+1;
            int j = nums.length-1;
            if(nums[k]>0){
                break;
            }
            if(k>0&&nums[k]==nums[k-1])
                continue;
            while(i<j){
                int sum = nums[i]+nums[j]+nums[k];
                if(sum>0){
                    while(i<j&&nums[i]==nums[++i]);
                }else if(sum<0){
                    while(i<j&&nums[j]==nums[--j]);
                }else{
                    ret.add(new ArrayList<Integer>(Arrays.asList(nums[k],nums[i],nums[j])));
                    while(i<j&&nums[i]==nums[++i]);
                    while(i<j&&nums[j]==nums[--j]);
                }
            }
        }
        return ret;
    }
    /**
     * 使用从后向前遍历的方法，用Second保存次大值，每次栈顶所存放的元素是当前的最大值。
     * 先将数组最后一个元素入栈，当second的值大于当前的值，则匹配成功
     * 否则说明当前的值大于second的值，即为当前的最大值，将这个值放入栈中，但是需要提前更新second的值（比当前值小的次大值）
     * @param nums
     * @return
     */
    public static boolean find132pattern(int[] nums) {
        if (nums.length < 3) {
            return false;
        }
        int second = Integer.MIN_VALUE;
        Stack<Integer> stack = new Stack<>();
        stack.add(nums[nums.length - 1]);
        for (int i = nums.length - 2; i >= 0; i--) {
            if (second > nums[i]) {
                return true;
            } else {
                while (!stack.isEmpty() && stack.peek() < nums[i]) {
                    second = Math.max(second, stack.pop());
                }
                stack.push(nums[i]);
            }
        }
        return false;
    }
    public static void main(String[] args) {
     // int nums[] = {-1,-1,-1,0,1,2,-4};
     // System.out.println(Main.find132pattern(nums));
    //  System.out.println(Main.threeSum(nums));
     //   System.out.println(Main.threeSumClosest(nums,9));
        int nums[] = {1, 0, -1, 0, -2, 2};
        System.out.println(Main.fourSum(nums,0));


    }
}
