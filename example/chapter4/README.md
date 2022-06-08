## Chapter4

> 章节四
>
> 主要讲解了AGRC和AGRV的用法

## Demo1

**ARGC** 用于表示awk语法后跟着的参数数量

**ARGV** 用于储存awk语法后跟着的参数

```shell
awk '
BEGIN {
#获取命令行输入的字符，awk默认为第一个字符
for ( i = 0; i < ARGC ; i++)  print ARGV[i]
}
' $*
```

执行命令：`./demo1 1 2 3 4 5`

输出

```
awk
1
2
3
4
5
```



## Demo2

```shell
awk '
BEGIN{
#获取命令行输入的字符
#打印获取到的参数数量
#可以直接根据index获取
print ARGC
print ARGV[0]
print ARGV[1]
print ARGV[2]
}
' $*
```

执行命令：`./demo2 1 2 3 4 5`

输出

```
6
awk
1
2
```