---
layout: post
category : datastruts
date: 2013/01/01 04:00:00 
title: 希尔排序
tags : datastruts
---


##概述
希尔排序和直接插入排序一样，都是插入排序，但是它改进了直接插入排序，该方法又称缩小增量排序

##算法原理
希尔算法的运作如下：（从后往前）

先取一个小于n的整数d1作为第一个增量，把文件的全部记录分组。所有距离为d1的倍数的记录放在同一个组中。先在各组内部进行直接插入排序，然后，取第二个增量d2&lt;d1重复上述分组和排序，直至所取的增量dt＝1，即所有记录放在同一组中进行直接插入排序为止。

一般的初次取序列的一半为增量，以后每次减半，直到增量为1。

##算法流程图
![Alt text]({{ BASE_PATH }}/images/shell-sort1.jpg "希尔排序流程图")
![Alt text]({{ BASE_PATH }}/images/shell-sort.jpg "希尔排序流程图")


##算法分析

###时间复杂度

希尔排序的时间复杂度和增量选取有关，如使用希尔增量时间复杂度为$$O(n^2)$$ ，而Hibbaard增量的希尔排序的时间复杂度为$$O(n^\frac{3}{2})$$ ,希尔排序时间复杂度下届是$$n*log2n$$。希尔排序没有快速排序算法快$$O(n*logn)$$ 


###算法稳定性
由于多次插入排序，我们知道一次插入排序是稳定的，不会改变相同元素的相对顺序，但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱，所以shell排序是不稳定的。

###和直接排序进行比较


来看一下希尔排序和快速排序的差别和联系

会发现基本思想和直接插入排序一模一样，只是多了一个曾脸的变化，即外面的while循环和那个increment = increment /2

{% highlight java %}

//直接插入排序
public static void insertSort1(int[] array) {

        //从位置1开始，0做为哨兵
        for (int i = 1; i < array.length; i++) {
            //保存当前位置到临时变量
            int temp = array[i];
            //将当前位置赋给紧邻的未排序的起点
            int j = i;
            //如果未排序的数小于已排序的树
            if (array[i - 1] > array[i]) {
                //向后移动一个位置
                while (j > 0 && array[j - 1] > temp) {     //只能是temp，不能是array[i] ,如果是array[i],在执行下面这句话的时候，array[i]会改的值被改变了
                    array[j] = array[j - 1];
                    j--;
                }
                //将当前位置存放为i
                array[j] = temp;    //同上，比如i为1的时候array[i]=3，array[j-1]为1，执行while里面的前移之后，array[j]=array[i]=3.而实际的情况应该是array[j]
            } else {
                System.out.println(i);
            }

        }
    }


//希尔排序
public static void shellSort1(int[] array){

        //增量 ,初始一般为数组的一半，最后为1
        int increment = array.length/2;
        //控制增量变化，一直到1
        while (increment>0){
            for (int i = increment ; i < array.length; i++){
                //保存当前位置到临时变量
                int temp = array[i];
                //将当前位置赋给紧邻的未排序的起点
                int j = i;
                //如果未排序的数小于已排序的树
                if (array[i-increment]>array[i]){
                    //向后移动increment个位置
                    while (j>=increment && array[j-increment]> temp){     
                        array[j] = array[j-increment];
                        j = j - increment;
                    }
                    //将当前位置存放为i
                    array[j] = temp;    //同上，比如i为1的时候array[i]=3，array[j-1]为1，执行while里面的前移之后，array[j]=array[i]=3.而实际的情况应该是array[j]
                } else {
                    System.out.println(i);
                }

            }
            //增量为上一次的一半。因为是int类型，所以最后会到1（0就不进行除了）
            increment = increment/2;

        }

    }



{% endhighlight %}



###算法描述


{% highlight java %}

package cn.liuyiyou.sort;

/**
 * User: liuyiyou
 * Date: 13-1-1
 * Time: 下午8:16
 */
public class ShellSort {


    /**
     * 这个可以和前面的快速插入排序进行比较，会发现方法一模一样，只是快速插入的增量一直为1.而希尔排序增量是慢慢变小的。
     * */
    public static void shellSort1(int[] array){

        int increment = array.length/2;
        while (increment>0){
            for (int i = increment ; i < array.length; i++){
                int temp = array[i];
                int j = i;
                if (array[i-increment]>array[i]){
                    while (j>=increment && array[j-increment]> temp){    
                        array[j] = array[j-increment];
                        j = j - increment;
                    }
                    array[j] = temp;    
                } else {
                    System.out.println(i);
                }

            }

            increment = increment/2;

        }

    }

    public static void shellSort2(int[] array) {
        int count = 0;
        int j = 0;
        int temp = 0;
        for (int increment = array.length / 2; increment > 0; increment /= 2) {
            count++;
            for (int i = increment; i < array.length; i++) {
                temp = array[i];
                for (j = i; j >= increment; j -= increment) {
                    if(temp > array[j - increment]){
                        array[j] = array[j - increment];
                    }else{
                        break;
                    }
                }
                array[j] = temp;
            }
        }
        System.out.println(count);
    }



    public static void shellSort3(int[] array) {
        int j = 0;
        int temp = 0;
        int increment = array.length/2;
        while (increment>0){
            for (int i = increment; i < array.length; i++) {
                temp = array[i];
                j = i;
                while (j >= increment) {
                    if(temp > array[j - increment]){
                        array[j] = array[j - increment];
                    }else{
                        break;
                    }

                    j = j - increment;
                }
                array[j] = temp;


            }
            increment = increment/2;
        }
    }



    public static void main(String[] args) {

        int [] array = SortUtil.array;
        System.out.println("原始排序结果：");
        SortUtil.print(array);
        shellSort1(array);
        System.out.println("最终排序结果：");
        SortUtil.print(array);

    }
}


{% endhighlight %}
