---
layout: post
category : datastruts
date: 2013/01/01 00:00:00 
title: 排序之冒泡排序
tags : datastruts
---

##概述
冒泡排序：它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。

这个算法的名字由来是因为越大的元素会经由交换慢慢“浮”到数列的顶端，故名

##算法原理
冒泡排序算法的运作如下：（从后往前）

- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
- 针对所有的元素重复以上的步骤，除了最后一个。
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较

##算法分析

###时间复杂度

若文件的初始状态是正序的，一趟扫描即可完成排序。所需的关键字比较次数$$C$$和记录移动次数$$M$$均达到最小值:$$C_{min}=n-1$$,$$M_{min}=0$$。所以，冒泡排序最好的时间复杂度为$$O(n)$$ 。

若初始文件是反序的，需要进行$$n-1$$趟排序。每趟排序要进行$$n-i$$次关键字的比较($$1≤i≤n-1$$),且每次比较都必须移动记录三次来达到交换记录位置。在这种情况下，比较和移动次数均达到最大值：

$$C_{max} = \left(n(n-1)\over 2 \right) = O(n^2)$$

$$M_{max} = \frac{3n(n-1)}{2} = O(n^2)$$

冒泡排序的最坏时间复杂度为$$O(n^2)$$ 。

综上，因此冒泡排序总的平均时间复杂度为$$O(n^2)$$ 

###算法稳定性

冒泡排序就是把小的元素往前调或者把大的元素往后调。比较是相邻的两个元素比较，交换也发生在这两个元素之间。所以，如果两个元素相等，我想你是不会再无聊地把他们俩交换一下的；如果两个相等的元素没有相邻，那么即使通过前面的两两交换把两个相邻起来，这时候也不会交换，所以相同元素的前后顺序并没有改变，所以冒泡排序是一种稳定排序算法。

###算法描述


package cn.liuyiyou.sort;

/**
 * 冒泡排序：
 * User: liuyiyou
 * Date: 14-8-21
 * Time: 下午4:04
 */
public class BubbleSort {


    /**
     * 这个是冒泡排序，冒泡排序是待排序的数和后面相邻的数比较大小
     * 注意内层循环中j的取值变化，之前i的初始值是1，j的临界值是array.length。
     * @param array
     */
    public static void bubbleSort1(int[] array){
        for (int i=0;i<array.length;i++){
            for (int j = 0; j<array.length-i-1;j++ ){
                if (array[j] > array[j+1]){
                    SortUtil.swap(array,j,j+1);
                }
            }
            SortUtil.print(array, i);
        }
    }


    /**
     *
     * @param array
     */
    public static void bubbleSort2(int[] array){
        for (int i=0;i<array.length;i++){
            for (int j = 0; j< i;j++ ){
                if (array[i]<array[j]){
                    SortUtil.swap(array,i,j);
                }
            }
            SortUtil.print(array, i);
        }
    }


    public static void bubbleSort3(int[] array) {
        for (int i = 0; i < array.length - 1; i++) { // 最多做n-1趟排序
            // 对当前无序区间score[0......length-i-1]进行排序(j的范围很关键，这个范围是在逐步缩小的)
            for (int j = 0; j < array.length - i - 1; j++) {
                if (array[j] < array[j + 1]) { // 把小的值交换到后面
                    SortUtil.swap(array, j, j + 1);
                }
            }

            SortUtil.print(array, i);
        }
    }





    public static void main(String [] args){


        int [] array = SortUtil.array;
        System.out.println("原始排序结果：");
        SortUtil.print(array);
        bubbleSort3(array);
        System.out.println("最终排序结果：");
        SortUtil.print(array);


    }

}

