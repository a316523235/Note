### 说明
利用动态规划的思想寻找给定的加权图中多源点之间最短路径的算法

资料：

https://blog.csdn.net/yuewenyao/article/details/81021319

https://www.geeksforgeeks.org/floyd-warshall-algorithm-dp-16/

### 实现代码
``` php
<?php
$arr = [
    [0, 2, 6, 4],
    [999, 0, 3, 999],
    [7, 999, 0, 1],
    [5, 999, 12, 0]
];


function floyd(&$arr) {
    $len = count($arr);
    $n = 0;

    for (; $n < $len; $n++) {
        for ($i = 0; $i < $len; $i++) {
            for ($j = 0; $j < $len; $j++) {
                if($arr[$i][$n] + $arr[$n][$j] < $arr[$i][$j]) {
                    $arr[$i][$j] = $arr[$i][$n] + $arr[$n][$j];
                }
            }
        }
    }

    for (; $n < $len; $n++) {
        for ($i = 0; $i < $len; $i++) {
            for ($j = 0; $j < $len; $j++) {
                if($arr[$i][$n] + $arr[$n][$j] < $arr[$i][$j]) {
                    $arr[$i][$j] = $arr[$i][$n] + $arr[$n][$j];
                }
            }
        }
    }
}

floyd($arr);
print_r($arr);

```



