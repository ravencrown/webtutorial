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

**原理图**

![img1](http://upload-images.jianshu.io/upload_images/615809-bd7f2c13adb76758.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
// 插入排序 从下标1开始每增1项排序一次，越往后遍历次数越多
function sort1(array) {
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

原理图

![img2](http://upload-images.jianshu.io/upload_images/615809-771d11316d4d2892.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
// 先在有序区通过二分查找的方法找到移动元素的起始位置，
// 然后通过这个起始位置将后面所有的元素后移
function sort2(array) {
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












