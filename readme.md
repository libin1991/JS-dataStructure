# JavaScript相关的数据结构
<!-- TOC -->

- [JavaScript相关的数据结构](#javascript相关的数据结构)
    - [列表](#列表)
        - [列表的抽象数据类型定义](#列表的抽象数据类型定义)
        - [实现列表类](#实现列表类)
        - [`append` 给列表添加一个元素](#append-给列表添加一个元素)
        - [`find` 从列表中查找某一个元素](#find-从列表中查找某一个元素)
        - [`remove` 从列表中删除一个元素](#remove-从列表中删除一个元素)
        - [`length` 列表中有多少个元素](#length-列表中有多少个元素)
        - [`toString` 显示列表中的元素](#tostring-显示列表中的元素)
        - [`insert` 向列表中插入一个元素](#insert-向列表中插入一个元素)
        - [`clear` 清空列表中所有的元素](#clear-清空列表中所有的元素)
        - [`contains` 判断给定值是否在列表中](#contains-判断给定值是否在列表中)
        - [遍历列表](#遍历列表)
        - [使用迭代器访问列表](#使用迭代器访问列表)
    - [栈](#栈)
        - [对栈的操作](#对栈的操作)
        - [栈的实现](#栈的实现)
        - [使用Stack类](#使用stack类)
            - [1. 数制间的相互转换](#1-数制间的相互转换)
            - [2. 判断是否为回文](#2-判断是否为回文)
            - [3. 递归演示](#3-递归演示)
    - [队列](#队列)
        - [对队列的操作](#对队列的操作)
        - [用一个数组实现队列](#用一个数组实现队列)
        - [使用队列：方块舞的舞伴分配问题](#使用队列方块舞的舞伴分配问题)

<!-- /TOC -->


## 列表

列表是一组有序数据，每个列表中的数据项成为元素。

不包含任何元素的列表称为**空列表**，列表中包含元素的个数成为列表的**length**,在内部实现上，用一个变量**listSize**保存列表中元素的个数。可以在列表末尾**append**一个元素，也可以在一个给定元素后或列表的起始位置**insert**一个元素，使用**remove**方法从列表中删除元素，使用**clear**方法清空列表中所有的元素。使用**toString**方法显示列表中的所有元素，使用**getElement**显示当前元素。使用**next**可以从当前元素移动到下一元素，使用**prev**可以移动到当前元素的前一个元素。使用**moveTo**直接移动到指定位置。**dataStore**表示列表的当前位置。

### 列表的抽象数据类型定义
|属性/方法|描述|
|:---|:---|
|listSize|列表的元素个数|
|pos|列表的当前元素|
|length|返回列表中元素的个数|
|clear|清空列表中的所有元素|
|toString|返回列表的字符串形式|
|getElement|返回当前元素的位置|
|insert|在现有元素后插入新元素|
|append|在列表末尾添加新元素|
|remove|从列表中删除元素|
|front|将列表的当前位置移动到第一个元素|
|end|将列表的当前位置移动到最后一个元素|
|prev|将当前位置后移一位|
|next|将当前位置前移一位|
|hasNext|判断后一位|
|hasPrev|判断前一位|
|currPos|返回列表的当前位置|
|moveTo|将当前位置移动到指定位置|

### 实现列表类

定义一个列表

```javascript
function List() {
    this.listSize = 0;
    this.pos = 0;
    this.dataStore = [];  //  初始化一个空数组来保存列表元素
    this.clear = clear;
    this.find = find;
    this.toString = toString;
    this.insert = insert;
    this.append = append;
    this.remove = remove;
    this.front = front;
    this.end = end;
    this.prev = prev;
    this.next = next;
    this.hasNext;
    this.hasPrev;
    this.length = length;
    this.currPos = currPos;
    this.moveTo = moveTo;
    this.getElement = getElement;
    this.contains = contains;
}
```

### `append` 给列表添加一个元素

给列表的下一个位置添加一个新元素，这个位置刚好等于变量`listSize`的值：当前元素就位后，`liseSize`加一

```javascript
function append(element) {
    this.dataStore[this.listSize++] = element;
}
```

### `find` 从列表中查找某一个元素

通过对数组对象`dataStore`进行迭代，查找指定元素，如果找到该元素，返回该元素所在的位置，否则返回`-1`。

```javascript
function find(element) {
    for (var i = 0; i < this.dataStore.length; i++) {
        if (this.dataStore[i] == element) {
            return i;
        }
    }
    return -1;
}
```

### `remove` 从列表中删除一个元素

`remove`方法使用`find`方法返回的位置对数组`dataStore`进行截取，数组改变后，将变量`listSize`的值减一，以反映列表的最新长度。如果元素删除成功，则返回`true`，否则返回`false`。

```javascript
function remove(element) {
    var fountAt = this.find(element);
    if (fountAt > -1) {
        this.dataStore.splice(fountAt, 1);
        --this.listSize;
        return true;
    }
    return  false;
}
```

### `length` 列表中有多少个元素

```javascript
function length() {
    return this.liseSize;
}
```

### `toString` 显示列表中的元素

严格来说，该方法返回的是一个数组，而不是一个字符串。

```javascript
function toString() {
    return this.dataStore;
}
```

### `insert` 向列表中插入一个元素

使用`find()`方法找到元素的位置后，使用`splice()`方法将新元素插入到该位置之后，然后将变量`listSize`加一并返回`true`，表明插入成功。

```javascript
function insert(element, after) {
    var insertPos = this.find(after);
    if (insertPos > -1) {
        this.dataStore.splice(insertPos + 1, 0, element);
        ++this.listSize;
        return true;
    }
    return false;
}
```

### `clear` 清空列表中所有的元素

使用`delete`操作符删除数组`dataStore`，接着在下一行创建一个空数组，最后将`listSize`和`pos`的值都设为1，表明这是一个新的空列表。

```javascript
function clear() {
    delete this.dataStore;
    this.dataStore.length = 0;
    this.listSize = this.pos = 0;
}
```

### `contains` 判断给定值是否在列表中

```javascript
function contains(elment) {
    for (var i = 0; i < this.dataStore.length; i++) {
        if (this.dataStore[i] == element) {
            return true;
        }
    }
    return  false;
}
```

### 遍历列表

```javascript
function front() {
    this.pos = 0;
}

function end() {
    this.pos = this.listSize -1;
}

function prev() {
    --this.pos;
}

function next() {
    if (this.pos < this.listSize) {
        ++this.pos;
    }
}

function currPos() {
    return this.pos;
}

function moveTo(positon) {
    this.pos = position;
}

function getElement() {
    return this.dataStore[this.pos];
}

function hasNext() {
    return this.pos < this.listSize;
}

function hasPrev() {
    return this.pos >= 0;
}
```

### 使用迭代器访问列表

优点：
1. 访问列表元素时不必担心底层的数据存储结构；
2. 当为列表添加一个元素时，索引的值就不对了，此时只用更新列表，而不用更新迭代器；
3. 可以用不同类型的数据存储方式实现cList类，迭代器为访问列表里的元素提供了一种统一的方式。

例如:

从前向后遍历列表：

```javascript
for (names.front(); names.hasNext();names.next()) {
    console.log(names.getElment());
}
```

从后向前遍历列表 

```javascript
for (names.end(); names.hasNext();names.prev()) {
    console.log(names.getElment());
}
```

## 栈

栈是一种高效的数据结构，因为数据只能在栈顶添加或删除，所以这样的操作很快，而且容易实现。

栈是一种**先入后出** *LIFO(last-in-first-out)*的数据结构。

### 对栈的操作

对栈的两种主要操作：将元素压入栈；将元素弹出栈。

入栈使用`push()`方法，出栈使用`pop()`方法。

`pop()`方法虽然可以访问栈顶的元素，但是调用该方法后，栈顶元素也从栈中被永久的删除了。`peek()`方法则只返回栈顶元素，而不删除它。

`clear()`方法清除栈内所有元素，`length`属性记录栈内元素的个数。

### 栈的实现

```javascript
function Stack() {
    this.dataStore = [];  // 保存栈内元素
    this.top = 0;  // 记录栈顶位置
    this.push = push;  // 压入栈
    this.pop = pop;  // 弹出栈
    this.peek = peek;  // 返回栈顶元素
    this.length = length;  // 栈内元素的长度
    this.clear = clear;  // 清空栈
}
```

`push()`方法：当向栈中压入一个新元素时，需要将其保存在数组中变量top所对应的位置，然后将top值加一，让其指向数组中下一个空位置。

```javascript
function push(element) {
    this.dataStore[this.top++] = element;
}
```

`pop()`方法：返回栈顶的元素，同时将变量`top`的值减一。

```javascript
function pop() {
    return this.dataStore[--this.top];
}
```

`peek()`方法：返回数组的第top-1个位置的元素，即栈顶元素。如果这个栈是空的，则返回`undefined`。

```javascript
function peek() {
    return this.dataStore[this.top - 1];
}
```

`length()`方法：通过返回变量top的值，返回栈内元素的个数。

```javascript
function length() {
    return this.top;
}
```

`clear()`方法：将变量top的值设为0，清空栈。

```javascript
function clear() {
    this.top = 0;
}
```

完整的Stack为：

```javascript
function Stack() {
    this.dataStore = [];  // 保存栈内元素
    this.top = 0;  // 记录栈顶位置
    this.push = push;  // 压入栈
    this.pop = pop;  // 弹出栈
    this.peek = peek;  // 返回栈顶元素
    this.length = length;  // 栈内元素的长度
    this.clear = clear;  // 清空栈
}
function push(element) {
    this.dataStore[this.top++] = element;
}
function pop() {
    return this.dataStore[--this.top];
}
function peek() {
    return this.dataStore[this.top - 1];
}
function length() {
    return this.top;
}
function clear() {
    this.top = 0;
}
```

### 使用Stack类

#### 1. 数制间的相互转换

假设将数字n转换为以b为基数的数字，实现转换的算法如下：

1. 最高位为n%b， 将此位压入栈。
2. 使用n/b代替n。
3. 重复步骤1和2，直到n等于0，且没有余数。
4. 持续将栈内元素弹出，直到栈空，一次将这些元素排列，就得到转换后的字符串形式。

实现算法如下：

```javascript
function mulBase(num, base) {
    var s = new Stack();
    do {
        s.push(num % base);
        num = Math.floor(num /= base);
    } while(num > 0);
    var converted = '';
    while(s.length() > 0) {
        converted += s.pop()
    }
    return converted;
}
```

例如：

将二进制转换为十进制；将十进制转换为八进制。

```javascript
function Stack() {
    this.dataStore = [];  // 保存栈内元素
    this.top = 0;  // 记录栈顶位置
    this.push = push;  // 压入栈
    this.pop = pop;  // 弹出栈
    this.peek = peek;  // 返回栈顶元素
    this.length = length;  // 栈内元素的长度
    this.clear = clear;  // 清空栈
}
function push(element) {
    this.dataStore[this.top++] = element;
}
function pop() {
    return this.dataStore[--this.top];
}
function peek() {
    return this.dataStore[this.top - 1];
}
function length() {
    return this.top;
}
function clear() {
    this.top = 0;
}
function mulBase(num, base) {
    var s = new Stack();
    do {
        s.push(num % base);
        num = Math.floor(num /= base);
    } while(num > 0);
    var converted = '';
    while(s.length() > 0) {
        converted += s.pop()
    }
    return converted;
}
var numBase = 10;
var num = 32;
var base = 2;
var result = mulBase(num, base);
console.log(`${numBase}进制数${num}转换为${base}进制数为${result}`);
```

#### 2. 判断是否为回文

```javascript
function Stack() {
    this.dataStore = [];  // 保存栈内元素
    this.top = 0;  // 记录栈顶位置
    this.push = push;  // 压入栈
    this.pop = pop;  // 弹出栈
    this.peek = peek;  // 返回栈顶元素
    this.length = length;  // 栈内元素的长度
    this.clear = clear;  // 清空栈
}
function push(element) {
    this.dataStore[this.top++] = element;
}
function pop() {
    return this.dataStore[--this.top];
}
function peek() {
    return this.dataStore[this.top - 1];
}
function length() {
    return this.top;
}
function clear() {
    this.top = 0;
}

// 主要函数：判断是否为回文
function isPalindrome(word) {
    var s = new Stack();
    for (var i = 0; i < word.length; ++i) {
        s.push(word[i]);
    }
    var rword = '';
    while(s.length() > 0) {
        rword += s.pop();
    }
    if(word == rword) {
        return true;
    } else {
        return false;
    }
}

var word = 'arra';
if(isPalindrome(word)) {
    console.log(`${word} is a palindrome.`);
} else {
    console.log(`${word} is not a palindrome`);
}
```

#### 3. 递归演示

阶乘函数

```javascript
function fact(n) {
    var s = new Stack();
    while(n > 1) {
        s.push(n--);
    }
    var product = 1;
    while(s.length() > 0) {
        product *= s.pop();
    }
    return product;
}

console.log(fact(5));
```


## 队列

队列是一种列表，队列只能在队尾插入元素，在队首删除元素。

队列是一种**先进先出**的数据结构。队列被用在很多地方：提交操作系统执行的一系列进程、打印任务池等，一些仿真系统用队列来模拟银行或杂货店里排队的顾客。

### 对队列的操作

队列的两种操作时：向队列中插入新元素和删除队列中的元素。插入元素也叫*入队*，删除的操作也叫*出队*。

入队操作在队尾插入新元素，出队操作删除队头的元素。读取队头的元素为`peek()`，该操作返回队头的元素，但不把它从队列中删除。可以使用`length`得知队列中有多少个元素，使用`clear()`方法可以清空队列中的元素。

### 用一个数组实现队列

```javascript
function Queue() {
    this.dataStore = [];
    this.enqueue = enqueue;
    this.dequeue = dequeue;
    this.front = front;
    this.back = back;
    this.toString = toString;
    this.empty = empty;
}
```

`enqueue()`方法：向队尾添加一个元素

```javascript
function enqueue(element) {
    this.dataStore.push(element);
}
```

`dequeue()`方法：删除队首元素 

```javascript
function dequeue() {
    return this.dataStore.shift();
}
```

`front()`方法：读取队首元素

```javascript
function front() {
    return this.dataStore[0];
}
```

`back()`方法：读取队尾元素

```javascript
function back() {
    return this.dataStore[this.dataStore.length - 1];
}
```

`toString()`方法：显示队列内的所有元素

```javascript
function toString() {
    var retStr = '';
    for(var i = 0; i < this.dataStore.length; ++i) {
        retStr += this.dataStore[i] + '\n';
    }
    return retStr;
}
```

`empty()`方法：判断队列是否为空

```javascript
function empty() {
    if(this.dataStore.length == 0) {
        return true;
    } else {
        return false;
    }
}
```

### 使用队列：方块舞的舞伴分配问题

题意描述：当男男女女来到舞池，他们按照自己的性别排成两队。当舞池中有地方空出来时，选两个队列中的第一人组成舞伴，他们身后的人要各自向移动一步，变成新的队首。当一对舞伴迈入舞池时，主持人会大声喊出他们的名字。当一对舞伴走出舞池，且两排队伍中有任意一队没有人时，支持人也会把这个情况告诉大家。

```javascript
function Queue() {
    this.dataStore = [];
    this.enqueue = enqueue;
    this.dequeue = dequeue;
    this.front = front;
    this.back = back;
    this.toString = toString;
    this.empty = empty;
}
function enqueue(element) {
    this.dataStore.push(element);
}
function dequeue() {
    return this.dataStore.shift();
}
function front() {
    return this.dataStore[0];
}
function back() {
    return this.dataStore[this.dataStore.length - 1];
}
function toString() {
    var retStr = '';
    for(var i = 0; i < this.dataStore.length; ++i) {
        retStr += this.dataStore[i] + '\n';
    }
    return retStr;
}
function empty() {
    if(this.dataStore.length == 0) {
        return true;
    } else {
        return false;
    }
}
function Dancer(name, sex) {
    this.name = name;
    this.sex = sex;
}
function getDancers(males,females) {
    var names = read('dancers.txt').split('\n');  // 读取‘dancers.txt’内的人员名单
    for(var i = 0; i < names.length; ++i) {
        names[i] = names[i].trim();
    }
    for(var i = 0; i < names.length; ++i) {
        var dancer = names[i].split('');
        var sex = dancer[0];
        var name = dancer[1];
        if (sex == 'F') {
            femaleDancers.enqueue(new Dancer(name, sex));
        } else {
            maleDancers.enqueue(new Dancer(name, sex));
        }
    }
}

function dance(males, females) {
    console.log('The dance parents are: ');
    while(!females.empty() && !males.empty()) {
        person = females.dequeue();
        console.log('female dancer is ' + person.name);
        person = males.dequeue();
        console.log('and the male dancer is ' + person.name);
    }
}
```

未更新完。。。