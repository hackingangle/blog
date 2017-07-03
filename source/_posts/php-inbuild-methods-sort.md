title: php内置九种排序
date: 2016-03-03 11:16:25
tags: php
---

## 讲解
排序，在这里是针对`array`而言的。

让我们先来总结一下，这九种排序：

|排序方向|正常|字典v|字典k|
|:-:|:-:|:-:|:-:|
|正|sort|asort|ksort|
|逆|rsort|arsort|krsort|
|自定义|usort|uasort|uksort|

PS:`正常`丢失`字典结构`

- sort
> 可以一般的排序，如果是k-v数据，会丢失关系

``` php
<?php
$nums = [90, 83, 100, 99, 78, 61];
$nums = [
    'zhangsan' => 90,
    'lisi' => 83,
    'wanger' => 100,
    'zhanghui' => 99,
    'xuelong' => 78,
    'zhangjian' => 61,
];

var_dump($nums);
sort($nums);
var_dump($nums);

// OUTPUT
// array(6) {
//   ["zhangsan"]=>
//   int(90)
//   ["lisi"]=>
//   int(83)
//   ["wanger"]=>
//   int(100)
//   ["zhanghui"]=>
//   int(99)
//   ["xuelong"]=>
//   int(78)
//   ["zhangjian"]=>
//   int(61)
// }
// array(6) {
//   [0]=>
//   int(61)
//   [1]=>
//   int(78)
//   [2]=>
//   int(83)
//   [3]=>
//   int(90)
//   [4]=>
//   int(99)
//   [5]=>
//   int(100)
// }

// 结论：丢失了k-v关系
```

- asort
> 问：sort排序不能保持k-v关系，那有没有能保持的？
> 答：asort就可以满足了保持k-v关系，对v排序。

``` php
<?php
$nums = [90, 83, 100, 99, 78, 61];
$nums = [
    'zhangsan' => 90,
    'lisi' => 83,
    'wanger' => 100,
    'zhanghui' => 99,
    'xuelong' => 78,
    'zhangjian' => 61,
];

var_dump($nums);
asort($nums);
var_dump($nums);

// OUTPUT
// array(6) {
//   ["zhangsan"]=>
//   int(90)
//   ["lisi"]=>
//   int(83)
//   ["wanger"]=>
//   int(100)
//   ["zhanghui"]=>
//   int(99)
//   ["xuelong"]=>
//   int(78)
//   ["zhangjian"]=>
//   int(61)
// }
// array(6) {
//   ["zhangjian"]=>
//   int(61)
//   ["xuelong"]=>
//   int(78)
//   ["lisi"]=>
//   int(83)
//   ["zhangsan"]=>
//   int(90)
//   ["zhanghui"]=>
//   int(99)
//   ["wanger"]=>
//   int(100)
// }
 
// 结论：保持了k-v关系，对v排序

```

- ksort
> 问：既然asort能对v排序，有没有能对k排序的？
> 答：当然了，我们只需要比较k就行了，这就是ksort

``` php
<?php
$nums = [90, 83, 100, 99, 78, 61];
$nums = [
    'zhangsan' => 90,
    'lisi' => 83,
    'wanger' => 100,
    'zhanghui' => 99,
    'xuelong' => 78,
    'zhangjian' => 61,
];

var_dump($nums);
ksort($nums);
var_dump($nums);

// OUTPUT
// array(6) {
//   ["zhangsan"]=>
//   int(90)
//   ["lisi"]=>
//   int(83)
//   ["wanger"]=>
//   int(100)
//   ["zhanghui"]=>
//   int(99)
//   ["xuelong"]=>
//   int(78)
//   ["zhangjian"]=>
//   int(61)
// }
// array(6) {
//   ["lisi"]=>
//   int(83)
//   ["wanger"]=>
//   int(100)
//   ["xuelong"]=>
//   int(78)
//   ["zhanghui"]=>
//   int(99)
//   ["zhangjian"]=>
//   int(61)
//   ["zhangsan"]=>
//   int(90)
// }
// 结论：保持k-v关系，对k排序
```

我们已经介绍了`sort`,`asort`,`ksort`，他们的区别是`是否保留k-v关系`，是比较`k`还是比较`v`。
除此之外，一个排序算法，我们还再意，是升序还是降序。
我们这里说一下降序的三个对应：`rsort`,`arsort`,`krsort`。
- rsort
- arsort
- krsort

- usort
>问：上述我们提到了`升序`，`降序`问题，什么控制了升降？
>答：在排序时候的比较算法，对于复杂结构，我们是否可以自定义比较算法，这就是`usort`了。

``` php
<?php
/**
 * sort 分为9种
 * sort,asort,ksort
 * rsort,arsort,krsort
 * usort,uasort,uksort
 */

$users = [
    [
        'username' => 'zhangsan',
        'age' => 32,
    ],
    [
        'username' => 'lisi',
        'age' => 26,
    ],
    [
        'username' => 'wangermazi',
        'age' => 90,
    ],
    [
        'username' => 'zhaosixiaojie',
        'age' => 20,
    ]
];

function cmpAge($prefix, $suffix)
{
    // echo "compare doing:", $prefix['age'], ',', $suffix['age'], "\n";
    $attr = 'age';
    if ($prefix[$attr] > $suffix[$attr]) {
        return true;
    }
    if ($prefix[$attr] < $suffix[$attr]) {
        return false;
    }
    return 0;
}
var_dump($users);
usort($users, 'cmpAge');
var_dump($users);

//OUTPUT
//array(4) {
//   [0]=>
//   array(2) {
//     ["username"]=>
//     string(8) "zhangsan"
//     ["age"]=>
//     int(32)
//   }
//   [1]=>
//   array(2) {
//     ["username"]=>
//     string(4) "lisi"
//     ["age"]=>
//     int(26)
//   }
//   [2]=>
//   array(2) {
//     ["username"]=>
//     string(10) "wangermazi"
//     ["age"]=>
//     int(90)
//   }
//   [3]=>
//   array(2) {
//     ["username"]=>
//     string(13) "zhaosixiaojie"
//     ["age"]=>
//     int(20)
//   }
// }
// array(4) {
//   [0]=>
//   array(2) {
//     ["username"]=>
//     string(13) "zhaosixiaojie"
//     ["age"]=>
//     int(20)
//   }
//   [1]=>
//   array(2) {
//     ["username"]=>
//     string(4) "lisi"
//     ["age"]=>
//     int(26)
//   }
//   [2]=>
//   array(2) {
//     ["username"]=>
//     string(8) "zhangsan"
//     ["age"]=>
//     int(32)
//   }
//   [3]=>
//   array(2) {
//     ["username"]=>
//     string(10) "wangermazi"
//     ["age"]=>
//     int(90)
//   }
// }
// 结论：定义方法，作为相邻元素的比较方法

```

那么，`usort`是否对应了`逆序`哪？
Of course not.`usort`的比较算法，本身就控制了`是否逆序`。

我们还需要知道，是否需要对`k-v`结果保留，以及比较的方法：

- uasort
- uksort

## 总结

正序：sort, asort, ksort
逆序：rsort, arsort, krsort
控制：usort, uasort, uksort

都需要关注：`k-v`是否保留，是比较`k`，还是比较`v`
