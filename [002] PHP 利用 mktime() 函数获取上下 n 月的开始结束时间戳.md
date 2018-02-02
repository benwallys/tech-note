### 利用 mktime() 函数实现需求

- 获取上/下 n 个月的开始结束时间戳

```
/**
 * 获取上/下 n 个月的开始结束时间戳
 * @param  {Number} $n n 为正数代表获取上 n 个月的，负数代表获取下 n 个月的，0 代表当月
 * @return {Array}    ['startTimeLine', 'endTimeLine']
 */
function getMonthTimeLine($n = 0)
{
    $line = time();
    $month = date('n', $line) - $n;
    $year = date('Y', $line);
    $start = mktime(0, 0, 0, $month, 1, $year);
    $lastDay = date('t', $start);
    $end = mktime(23, 59, 59, $month, $lastDay, $year);


    return ['startTimeLine'=>$start, 'endTimeLine'=>$end];
}
```

- $end 还有另外一种做法，下一个月的 1 日时间戳减 1 秒即可

```
    $end = mktime(0, 0, 0, $month + 1, 1, $year) - 1;
```
