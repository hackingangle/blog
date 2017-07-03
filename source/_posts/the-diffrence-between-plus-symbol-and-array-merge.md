title: 数组的USAGE-`+`合并数组与array_merge的区别
date: 2017-03-22 11:08:13
tags: php
---

先上代码：

``` php
<?php
/**
 * 合并数组测试
 * + 或 array_merge
 */

$arrMock1 = [
    [
        'uname' => 'hackingangle',
        'age' => 28,
        'sex' => 'male',
    ],
];

$arrMock2 = [
    [
        'uname' => 'zhangsan',
        'age' => 18,
        'sex' => 'female',
        'asf' => '123'
    ],
    [
        'asdf' => 'a',
    ]
];

$merge1 = (array) $arrMock1 + (array) $arrMock2;
var_dump($merge1);

$merge2 = array_merge($arrMock1, $arrMock2);
var_dump($merge2);
```

### +
如果key相同，则不会覆盖，直接把后面的同名key做舍弃

```
array(2) {
  [0]=>
  array(3) {
    ["uname"]=>
    string(12) "hackingangle"
    ["age"]=>
    int(28)
    ["sex"]=>
    string(4) "male"
  }
  [1]=>
  array(1) {
    ["asdf"]=>
    string(1) "a"
  }
}
```

### array_merge
不区分是否key相同，直接进行合并，重新对key进行递增处理

```
array(3) {
  [0]=>
  array(3) {
    ["uname"]=>
    string(12) "hackingangle"
    ["age"]=>
    int(28)
    ["sex"]=>
    string(4) "male"
  }
  [1]=>
  array(4) {
    ["uname"]=>
    string(8) "zhangsan"
    ["age"]=>
    int(18)
    ["sex"]=>
    string(6) "female"
    ["asf"]=>
    string(3) "123"
  }
  [2]=>
  array(1) {
    ["asdf"]=>
    string(1) "a"
  }
}
```
