# Chapter3

> 章节三
>
> 主要讲解了split内置函数的用法，FS和RS的深入用法

## week.rpt

```

张长弓
GNUPLOT 入门




吴国强
Latex 简介
VAST-2 使用手册
mathematic 入门




李小华
awk Tutorial Guide
Regular Expression





```

## Demo1

**BEGIN** awk命令中最先执行的代码

**Split** 将字符串分割为数组的内置函数

```shell
awk '
BEGIN{
split("一. 二. 三. 四. 五. 六. 七. 八. 九.", C_Number, " ")
print length(C_Number)
}
' week.rpt
```

执行命令：`./demo1`

输出

```
9
```



## Demo2

```shell
awk '
{
#输出行号和分栏数
print NR, NF
} ' week.rpt
```

执行命令：`./demo2`

输出

```
1 0
2 1
3 2
4 0
5 0
6 0
7 0
8 1
9 2
10 2
11 2
12 0
13 0
14 0
15 0
16 1
17 3
18 2
19 0
20 0
21 0
22 0
23 0
```



## Demo3

```shell
awk '
BEGIN {
#分栏分割方式设置为\n
FS = "\n"
}
{
print NR, NF
} ' week.rpt
```

执行命令：`./demo3`

输出

```
1 0
2 1
3 1
4 0
5 0
6 0
7 0
8 1
9 1
10 1
11 1
12 0
13 0
14 0
15 0
16 1
17 1
18 1
19 0
20 0
21 0
22 0
23 0
```



## Demo4

**RS** 用于设置行的分割符号

```shell
awk '
BEGIN {
#行分割设置为空，默认合并多个\n后再分割为行
RS = ""
}
{
print NR, NF
print $0
} ' week.rpt
```

执行命令：`./demo4`

输出

```
1 3
张长弓
GNUPLOT 入门
2 7
吴国强
Latex 简介
VAST-2 使用手册
mathematic 入门
3 6
李小华
awk Tutorial Guide
Regular Expression
```

## Demo5

```shell
awk '
BEGIN {
#行分割设置为空，默认合并多个\n后再分割为行
RS = ""
FS = "\n"
}
{
print NR, NF
print $0
} ' week.rpt
```

执行命令：`./demo5`

输出

```
1 2
张长弓
GNUPLOT 入门
2 4
吴国强
Latex 简介
VAST-2 使用手册
mathematic 入门
3 3
李小华 #第1分栏
awk Tutorial Guide #第2分栏
Regular Expression #第3分栏
```

## Demo6

```shell
awk '
BEGIN {
#分栏分割设置为\n
#行分割设置为空，默认合并多个\n后再分割为行
FS = "\n"
RS = ""
split("一. 二. 三. 四. 五. 六. 七. 八. 九.", C_Number, " ")
}
{
printf("\n%s 报告人 : %s \n", C_Number[NR], $1)
for(i = 2; i <= NF; i++) printf(" %d. %s\n", i - 1, $i)
} ' week.rpt
```

执行命令：`./demo6`

输出

```
一. 报告人 : 张长弓
 1. GNUPLOT 入门

二. 报告人 : 吴国强
 1. Latex 简介
 2. VAST-2 使用手册
 3. mathematic 入门

三. 报告人 : 李小华
 1. awk Tutorial Guide
 2. Regular Expression
```

