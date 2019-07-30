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
//        print_r("第{$n}轮\n");
        for ($i = 0; $i < $len; $i++) {
            for ($j = 0; $j < $len; $j++) {
//                print_r("\n\n");
//                show($arr, $i, $j, $n);   //显示流程图
                if($arr[$i][$n] + $arr[$n][$j] < $arr[$i][$j]) {
                    $arr[$i][$j] = $arr[$i][$n] + $arr[$n][$j];
                }
            }
        }
    }
}

/**
 * 显示流程图
 * @param $arr
 * @param int $target_i
 * @param int $target_j
 * @param int $target_n
 */
function show($arr, $target_i = 0, $target_j = 0, $target_n = 0) {
    $len = count($arr);
    for ($i = 0; $i < $len; $i++) {
        for ($j = 0; $j < $len; $j++) {
            $temp = "...   ";
            if($i === $target_i && $j == $target_j) {
                $temp[1] = 'X';
            }

            if(($i ===$target_i && $j === $target_n)) {
                $temp[0] = 'O';
            }

            if(($i ===$target_n && $j === $target_j)) {
                $temp[2] = 'O';
            }
            print_r($temp);
        }
        print_r("\n");
    }
}



floyd($arr);
print_r($arr);

//show($arr);

```



