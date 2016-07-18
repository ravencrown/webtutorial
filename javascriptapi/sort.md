---
title: js排序算法
category: javascriptapi
layout: page
date: 2016-07-09
modifiedOn: 2016-07-09
---

昨天友人来京，没有及时更新博客，今天主要讲一讲用 JS 过一遍排序算法

## 插入排序

最普通的排序算法， 从数组下标1开始每增1项排序一次，越往后遍历次数越多；
插入排序是在一个已经有序的小序列的基础上，一次插入一个元素。当然，刚开始这个有序的小序列只有1个元素，就是第一个元素。比较是从有序序列的末尾开始，也就是想要插入的元素和已经有序的最大者开始比起，如果比它大则直接插入在其后面，否则一直往前找直到找到它该插入的位置。如果碰见一个和插入元素相等的，那么插入元素把想插入的元素放在相等元素的后面

**原理图**

![img1](http://upload-images.jianshu.io/upload_images/615809-bd7f2c13adb76758.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
// 插入排序 从下标1开始每增1项排序一次，越往后遍历次数越多
function insertSort(array) {
  var len = array.length,
      i, j, tmp, result;

  // 设置数组副本
  result = array.slice(0);
  for(i=1; i < len; i++){
    tmp = result[i];
    j = i - 1;
    while(j>=0 && tmp < result[j]){
      result[j+1] = result[j];
      j--;
    }
    result[j+1] = tmp;
  }
  return result;
}
```

## 二分插入排序

插入排序的一种优化实现， 通过二分法减少遍历时间。

**原理图**

![img2](http://upload-images.jianshu.io/upload_images/615809-771d11316d4d2892.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
// 先在有序区通过二分查找的方法找到移动元素的起始位置，
// 然后通过这个起始位置将后面所有的元素后移
// 数量级为O(nlog^2n)，但是元素移动次数还是O(n^2)，所以二分插入排序的时间复杂度是O(n^2)。
function bisectionSort(array) {
    var len = array.length,
        i, j, tmp, low, high, mid, result;
    // 赋予数组副本
    result = array.slice(0);
    for(i = 1; i < len; i++){
        tmp = result[i];
        low = 0;
        high = i - 1;
        while(low <= high){
      mid = parseInt((low + high)/2, 10);
      if(tmp < result[mid]) high = mid - 1;
      else low = mid + 1;
    }
    for(j = i - 1; j >= high+1; j--){
      result[j+1] = result[j];            
    }
    result[j+1] = tmp;
  }
  return result;
}
```


## 冒泡排序

很常见很容易理解的排序算法，排序思路：遍历数组，每次遍历就将最大（或最小）值推至最前。越往后遍历查询次数越少， 跟插入排序刚好相反。

**原理图**

![img3](http://upload-images.jianshu.io/upload_images/615809-709b27ca8b041a7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```javascript
// 冒泡排序 每次将最小元素推至最前
// 时间换空间
// 时间复杂度: (n - 1) + (n - 2) + ... + 1 = n * (n - 1) / 2
// 当数据为正序、则复杂度为O(0)
function sort4(array) {
    var len = array.length,
        i, j, tmp, result;
    result = array.slice(0);
    for (i = 0; i < len; i++) {
        for (j = len - 1; j > i; j--) {
            if (result[j] < result[j - 1]) {
                tmp = result[j - 1];
                result[j - 1] = result[j];
                result[j] = tmp;
            }
        }
    }
    return result;
}
```

## 快速排序

快速排序在诸多算法排序中可能不是最好的， 但个人认为在JS语言实现中是最快的！以前公司项目中对比过二分插入排序、优化冒泡排序、快速排序的JS实现执行时间，几千条数据的数组在firefox下快速排序的速度比冒泡、插入排序快3至4秒（数组元素为复杂的对象，根据对象某一属性值排序）。

**原理图**

![](http://upload-images.jianshu.io/upload_images/615809-61e8dcef25758c32.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
//（1）在数据集之中，选择一个元素作为"基准"（pivot）。
//（2）所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
//（3）对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

// 快速排序的最坏时间为O(n2), 最好时间复杂度为O(nlgn), 平均时间复杂度为O(nlgn), 空间复杂度O(log2n)~O(n)
function quickSort(array) {
    var tmp_array = array.slice(0), result,
    quickSort = function(arr) {
    	if (arr.length <= 1) { return arr; }
    	var pivotIndex = Math.floor(arr.length / 2);
        var pivot = arr.splice(pivotIndex, 1)[0];
        var left = [];
        var right = [];
        for (var i = 0; i < arr.length; i++) {
        	if (arr[i] < pivot) {
                left.push(arr[i]);
            } else {
	            right.push(arr[i]);
            }
        }
        return quickSort(left).concat([pivot], quickSort(right));
    };
    result = quickSort(tmp_array);
    return result;
}
```

## 选择排序

实现思路跟冒泡排序差不多， 可以说是冒泡排序的衍生版本；

**原理图**

![img5](http://upload-images.jianshu.io/upload_images/615809-a24549dcf73b5989.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
// 在无序区中选出最小的元素，然后将它和无序区的第一个元素交换位置。
// 原理跟冒泡排序一样，算是冒泡的衍生版本
function selectSort(array) {
    var len = array.length,
    i, j, k, tmp, result;

    result = array.slice(0);
    for (i = 0; i < len; i++) {
        k = i;
        for (j = i + 1; j < len; j++) {
            if (result[j] < result[k]) k = j;
        }
        if (k != i) {
            tmp = result[k];
            result[k] = result[i];
            result[i] = tmp;
        }
    }
    return result;
}
```






## 参考链接

- 阮一峰快速排序, [快速排序](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)
- 排序图解：js排序算法实现, [排序图解：js排序算法实现](http://www.jianshu.com/p/7e6589306a27)
- 秒杀9种排序算法(JavaScript版), [秒杀9种排序算法(JavaScript版)](http://www.cnblogs.com/JChen666/p/3360853.html)
- 各种排序算法的稳定性和时间复杂度小结, [各种排序算法的稳定性和时间复杂度小结](http://blog.csdn.net/hkx1n/article/details/3922249)













