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

## 栈

特殊的列表，栈内的元素只能通过列表的一端访问，栈顶后入先出(LIFO,last-in-first-out)的数据结构  

javascript提供可操作的方法, 入栈 push, 出栈 pop，但是pop会移掉栈中的数据  

- 实现一个栈的实现类
- 底层存数数据结构采用 数组
- 因为pop是删除栈中数据，所以需要实现一个查找方法 peek
- 实现一个清理方法 clear
- 栈内元素总量查找 length
- 查找是否还存在元素 empty

```javascript
function Stack(){
    this.dataStore = []
    this.top    = 0;
    this.push   = push
    this.pop    = pop
    this.peek   = peek
    this.length = length;
}

function push(element){
    this.dataStore[this.top++] = element;
}

function peek(element){
    return this.dataStore[this.top-1];
}

function pop(){
    return this.dataStore[--this.top];
}

function clear(){
    this.top = 0
}

function length(){
    return this.top
}
```

利用栈实现阶乘

```javascript
// 方法一
function fact(n) {
    var s = new Stack()
    while (n > 1) {
        //[5,4,3,2]
        s.push(n--);
    }
    var product = 1;
    while (s.length() > 0) {
        product *= s.pop();
    }
    return product;
}

// 方法二

function fact(n) {

    var arr = [],
        product = 1;
    while (n > 1) {
        arr.push(n--);
    }

    while (arr.length > 0) {
        product *= arr.pop();
    }

    return product;
}
```

## 队列

队列是只允许在一端进行插入操作，另一个进行删除操作的线性表，队列是一种先进先出（First-In-First-Out，FIFO）的数据结构  

队列本来也是一种特殊的线性表，在JavaScript我们可以直接使用数组实现这样的一个设计，数组的push()方法可以在数组末尾加入元素，shift()方法则可删除数组的第一个元素。  

```javascript
function Queue() {
    this.dataStore = [];
    this.enqueue   = enqueue;
    this.dequeue   = dequeue;
    this.first     = first;
    this.end       = end;
    this.toString  = toString;
    this.empty     = empty;
}

// enqueue()方法向队尾添加一个元素
function enqueue(element) {
    this.dataStore.push(element);
}

// dequeue()方法删除队首的元素：
function dequeue() {
    return this.dataStore.shift();
}

// 可以使用如下方法读取队首和队尾的元素

function first() {
    return this.dataStore[0];
}

function end() {
    return this.dataStore[this.dataStore.length - 1];
}

// toString()方法显示队列内的所有元素 

function toString() {
    var retStr = "";
    for (var i = 0; i < this.dataStore.length; ++i) {
        retStr += this.dataStore[i] + "\n";
    }
    return retStr;
}

// 需要一个方法判断队列是否为空 
function empty() {
    if (this.dataStore.length == 0) {
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
