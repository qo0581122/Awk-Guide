# Chapter7

> 章节七
>
> 总结上述章节的用法

## Demo1

```sh
awk '
BEGIN{
print "ID Number Arrival Time" > "today_rpt1"
print "======================" > "today_rpt1"
close("today_rpt1")
}
{printf("%s %s\n", $1, $2) >> "today_rpt1"}
' arr.dat
```

执行命令 `./demo`

生成新文件`today_rpt1`

内容

```
ID Number Arrival Time
======================
1034 7:26
1025 7:27
1101 7:32
1006 7:45
1012 7:46
1028 7:49
1051 7:51
1029 7:57
1042 7:59
1008 8:01
1052 8:05
1005 8:12
```

## Demo2

```sh
awk '
BEGIN{
#调用date 获取时间
#getline把date获取到到时间赋值为$0
"date" | getline
print "Today is ", $2, $3 > "today_rpt2"
print "===========" > "today_rpt2"
print "ID Number Arrival Time" > "today_rpt2"
print "===========" > "today_rpt2"
close("today_rpt2")
}
#sort -k 1 把printf后到输出按第一列进行排列 并把结果追加到today_rpt2末尾
{printf("%s %s\n", $1, $2) | "sort -k 1 >> today_rpt2"}
' arr.dat
```

执行命令 `./demo2`

生成新文件`today_rpt2`

输出

```
Today is  6月 8日
===========
ID Number Arrival Time
===========
1005 8:12
1006 7:45
1008 8:01
1012 7:46
1025 7:27
1028 7:49
1029 7:57
1034 7:26
1042 7:59
1051 7:51
1052 8:05
1101 7:32
```

## Demo3

```sh
awk  '
BEGIN {
#按正则去分割分栏
FS = "[ \t:]+"
#获取当前时间
"date" | getline
#把当前时间输出到文件
print $0 > "today_rpt3"
print "Today is ", $2, $3 > "today_rpt3"
print "=======================" > "today_rpt3"
print "ID Number Arrival Time" > "today_rpt3"
close("today_rpt3")
}
{
#自定义函数HM_to_M
arrival = HM_to_M($2, $3)
#支持三目运算符
printf("%s %s:%s %s\n", $1, $2, $3, arrival > 480 ? "*" : "") | "sort -k 1 >> today_rpt3"
total += arrival
}
END {
close("today_rpt3")
printf("Average arrival time: %d:%d\n", total/NR/60, (total/NR) % 60) >> "today_rpt3"
}
function HM_to_M (hour, min) {
return hour*60 + min
}
' arr.dat
```

执行命令 `./demo3`

生成新文件 `today_rpt3`

内容

```
2022年 6月 8日 星期三 19时55分56秒 CST
Today is  6月 8日
=======================
ID Number Arrival Time
Average arrival time: 7:49
1005 8:12 *
1006 7:45
1008 8:01 *
1012 7:46
1025 7:27
1028 7:49
1029 7:57
1034 7:26
1042 7:59
1051 7:51
1052 8:05 *
1101 7:32
```

## Demo4

### 6月late.dat

```
1012 0
1034 0
1005 9
1051 0
1006 1
1052 2
1008 1
1101 0
1025 1
1028 0
1029 2
1042 0
```

### 执行文件

```sh
awk '
BEGIN {
#定义变量
Sys_Sort = "sort -k 1 >> today_rpt4"
Result = "today_rpt4"
#重新设置分栏分割符，支持正则表达式
FS = "[ \t:]+"
#获取当前时间
"date" | getline
#输出当前时间到文件
print $0 > Result
print "Today is ", $2, $3 > Result
print "================" > Result
print "ID Number Arrival Time" > Result
close(Result)
#获取文件内容
late_file = $2"late.dat"
late_file2 = $2"late2.dat"
while( getline < late_file > 0) cnt[$1] = $2
close( late_file)
}
{
arrival = HM_to_M($2, $3)
if (arrival > 480) {
mark = "*"
cnt[$1]++
}else {
mark = " "
}
message = cnt[$1] ? cnt[$1] " times" : ""
printf("%s %d:%d %s %s\n", $1, $2, $3, mark, message) | Sys_Sort
total += arrival
}
END {
#关闭sort -k 1 >> today_rpt4， 可以把尚未输出的内容输出到文件中
close(Sys_Sort)
printf("Average arrival time : %d:%d\n", total/NR/60, (total/NR)%60) >> Result
close(Result)
for(any in cnt)
print any, cnt[any] > late_file2
close(late_file2)
}
#自定义函数
function HM_to_M(hour, min) {
return 60*hour+min
}
```

执行命令 `./demo4`

生成新文件 `today_rpt4` `6月late2.dat`

today_rpt4 内容

```
2022年 6月 8日 星期三 20时17分34秒 CST
Today is  6月 8日
================
ID Number Arrival Time
1005 8:12 * 10 times
1006 7:45   1 times
1008 8:1 * 2 times
1012 7:46
1025 7:27   1 times
1028 7:49
1029 7:57   2 times
1034 7:26
1042 7:59
1051 7:51
1052 8:5 * 3 times
1101 7:32
Average arrival time : 7:49
```

6月late2.dat 内容

```
1012 0
1034 0
1005 10
1006 1
1051 0
1052 3
1008 2
1101 0
1025 1
1028 0
1029 2
1042 0
```

