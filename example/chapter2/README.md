## Chapter2

> 章节二
>
> 主要讲解了如何将过滤和打印相结合，以及过滤符号的作用域

## Demo1

```shell
awk '
#过滤出只含有Dan的行
/Dan/ {
print $0
print $2, $3
}
' $*
```

执行命令

`./demo emp.dat`

输出

```
A341 Dan 110 214
Dan 110
```

## Demo2

```shell
awk '
#命令1 只输出含有Dan的行
/Dan/  {print $0}
#命令2 输出所有
{
print "第二个打印" $0
}
' $*
```

执行命令

`./demo2 emp.dat`

输出

```
第二个打印A123 Jenny 80 210
A341 Dan 110 214
第二个打印A341 Dan 110 214
第二个打印P158 Max 130 209
第二个打印P148 John 125 220
第二个打印A333 May 90 200
```

## Demo3

```shell
awk '
#对A开头的行 第三分栏的数值*1.05
/^A.*/ {
$3 *= 1.05
}
#针对所有行
{
printf("if语句前：%s %s %d\n", $1, $2, $3)
if($3 < 100) {
$3 = 100
}
printf("%s %s %d\n", $1, $2, $3)
}
' $*
```

执行命令

`./demo3 emp.dat`

输出

```
if语句前：A123 Jenny 84
A123 Jenny 100
if语句前：A341 Dan 115
A341 Dan 115
if语句前：P158 Max 130
P158 Max 130
if语句前：P148 John 125
P148 John 125
if语句前：A333 May 94
A333 May 100
```

## Demo4

```shell
awk '
BEGIN {
FS = " "
}
#只输出A开头的行
/^A.*/ {
$3 *= 1.05
printf("if语句前：%s %s %d\n", $1, $2, $3)
if($3 < 100) {
$3 = 100
}
printf("%s %s %d\n", $1, $2, $3)
}
' $*
```

执行

`./demo4 amp.dat`

输出

```
if语句前：A123 Jenny 84
A123 Jenny 100
if语句前：A341 Dan 115
A341 Dan 115
if语句前：A333 May 94
A333 May 100
```



## 总结



```
第一种：
/^A.*/ {

}
第二种：
/^A.*/
{
{
执行出来的是两种结果，第一种是合成一段代码来执行，第二种默认分为两段代码来执行
```



