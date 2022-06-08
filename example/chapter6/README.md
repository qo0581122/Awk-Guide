# Chapter6

> 章节六
>
> 主要讲解了getline的用法和BEGIN、END的执行顺序

## date.dat

```
2022年 6月 8日 星期三 19时32分10秒 CST
```

## Demo1

**getline** 主要用于获取输入流

```sh
awk '
BEGIN{
"date" | getline
print $0
}
' $*
```

执行命令 `./demo1`

输出

```
2022年 6月 8日 星期三 19时31分13秒 CST
```

## Demo2

```sh
awk '
BEGIN{
getline < "date.dat"
print $0
}
' $*
```

执行命令  `./demo2`

输出

```
2022年 6月 8日 星期三 19时32分10秒 CST
```

## Demo3

```sh
awk '
BEGIN{
"date" | getline
print $0 > "date2.dat"
}
' $*
```

执行命令 `./demo3`

生成新文件 `date2.dat`

内容

```
2022年 6月 8日 星期三 19时33分59秒 CST
```

## Demo4

```sh
awk '
BEGIN{
print "begin"
}
{
print "middle"
}
END {
print "end"
}
' date.dat
```

执行命令 `./demo4`

输出

```
begin
middle
end
```

## Demo5

```sh
awk '
BEGIN{
file = "data5.rpt"
print "begin" > file
close(file)
}
{
print "middle" >> file
}
END {
print "end" >> file
}
' date.dat
```

执行命令 `./demo5`

生成新文件`data5.rpt`

内容

```
begin
middle
end
```

