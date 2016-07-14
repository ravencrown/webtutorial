---
title: 算法实例
category: javascriptapi
layout: page
date: 2016-06-28
modifiedOn: 2016-06-28
---

本节主要是在工作中遇到的JavaScript算法的总结，思考，后续会不断更新

## Array 

**判断变量是否是数组**

```javascript
function isArray(value) {
    return object != null && typeof object == "object" && 'splice' in object && 'join' in object;
}

function isArrayfn(value) {
    if (typeof Array.isArray === 'function') {
        return Array.isArray(value);
    } else {
        return Object.prototype.toString.call(value) === '[object Array]';
    } 
} 
```


**在一个数组里面移除指定value**

```javascript
function RemoveElement(arr, item){
    var i = 0,
        j = 0,
        len = arr.length;
    var arrFlag = isArrayFn(arr)
    if (arrFlag) {
        for (i = 0; i < len; i++) {
            if (arr[i] == item) {
                continue;
            }
            arr[j] = arr[i];
            j++;
        }
        arr = arr.slice(0, j);
        return "j: " + j + " arr: " + arr;
    } else {
        return "arr is not Array";
    }
}
```

## Number

**两个大数相加**

```javascript
function bigNumAdd(a, b) {
    a += "";
    b += "";
    var maxLen = Math.max(a.length, b.length),
        aArr = a.split("").reverse(),
        bArr = b.split("").reverse(),
        i = 0,
        addToNext = 0,
        resArr = [],
        fn,
        sn,
        sum;
    for (; i < maxLen; i++) {
        fn = parseInt(aArr[i] || 0);
        sn = parseInt(bArr[i] || 0);
        sum = fn + sn + addToNext;
        addToNext = (sum - sum % 10) / 10;
        resArr.unshift(sum % 10);
    }
    if (resArr[0] == 0) {
        resArr.splice(0, 1);
    }
    return resArr.join("");
}
```

## Sort

**快速排序**

```javascript
// 方法一
var quickSort = function(arr) {
    if (arr.length < 2) { return arr; }
        var pivotIndex = Math.floor(arr.length / 2);
        var pivot = arr[pivotIndex];
        var left = [];
        var right = [];
        for (var i = 0; i < arr.length; i++) {
            if (arr[i] < pivot) {
                left.push(arr[i]);
            } else {
                right.push(arr[i]);
            }
        }
    }
    return quickSort(left).concat([pivot], quickSort(right));
};

// 方法二

function swap(items, firstIndex, secondIndex){
    var temp = items[firstIndex];
    items[firstIndex] = items[secondIndex];
    items[secondIndex] = temp;
}

function partition(items, left, right) {
    var pivot   = items[Math.floor((right + left) / 2)],
        i       = left,
        j       = right;
    while (i <= j) {
        while (items[i] < pivot) {
            i++;
        }
        while (items[j] > pivot) {
            j--;
        }
        if (i <= j) {
            swap(items, i, j);
            i++;
            j--;
        }
    }
    return i;
}

function quickSort(items, left, right) {
    var index;
    if (items.length > 1) {
        index = partition(items, left, right);
        if (left < index - 1) {
            quickSort(items, left, index - 1);
        }
        if (index < right) {
            quickSort(items, index, right);
        }

    }
    return items;
}

// first call
var result = quickSort(items, 0, items.length - 1);


```

## String

**字符串去重一**

```javascript
/**
 * 字符串aaaabbccc的处理，处理结果为4a2b3c；
 * 其中aaaa被处理成4a，映射规则为连续的字母a的数量，
 * bb被处理为2b，ccc被处理为3c，整个字符串的处理结果为4a2b3c。
 */

function deRepeatStr(str) {
    var json = {};
    var len = str.length;
    for (var i = 0; i < len; i++) {
        if (!json[str.charAt(i)]) {
            json[str.charAt(i)] = 1;
        } else {
            json[str.charAt(i)]++;
        }
    }

    var result = "";
    for (var i in json) {
        result = result + json[i] + i;
    }
    return result;
}

/**
 * 字符串aaabbbccc处理成 abc
 */
function dropRepeatStr(str) {
    var json = {};
    var len = str.length;
    for (var i = 0; i < len; i++) {
        if (!json[str.charAt(i)]) {
            json[str.charAt(i)] = 1;
        }
    }
    var result = "";
    for (var i in json) {
        result = result + i;
    }
    return result;
}
```

**判断字符串是否是回文**

```javascript
// 回文就是指一个单词，数组，短语，从前往后从后往前都是一样的 12321.abcba
// 回文最简单的思路就是, 把元素反转后如果与原始的元素相等，那么就意味着这就是一个回文了
function isPalindrome(word) {

    if (word.length = 0) {
        return 
    }
    var arr = word.split(""),
        rword = "";
    while(arr.length > 0) {
        rword += arr.pop();
    }

    if (word == rword) {
        return true;
    } else {
        return false;
    }
}
```


## 参考链接

- LeetCode题解, [LeetCode题解](https://siddontang.gitbooks.io/leetcode-solution/content/)
- LeetCode答案查询, [LeetCode答案查询](http://www.jiuzhang.com/solutions/)
- 结构之法 算法之道, [结构之法 算法之道](http://blog.csdn.net/v_july_v)
- July, [The-Art-Of-Programming-By-July](https://github.com/julycoding/The-Art-Of-Programming-By-July)
