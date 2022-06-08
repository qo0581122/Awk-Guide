# Chapter5

> 章节五
>
> 主要讲解了for循环的用法

## reg.dat

学员名称，课程名称

```
Mary O.S. Arch. Discrete
Steve D.S Algorithm Arch.
Wang Discrete Graphics O.S.
Lisa Graphics A.I.
Lily Discrete Algorithm
```

## Demo1

```shell
#统计每门课程的数量
awk '
{
for(i=2;i<=NF;i++) Number[$i]++
}
END {
for(course in Number) printf("%s %d\n", course, Number[course])
}
' $*
```

执行命令：`./demo1 reg.dat`

输出

```
D.S 1
Discrete 3
O.S. 2
A.I. 1
Graphics 2
Arch. 2
Algorithm 2
```

## Demo2

```shell
#统计学生学习课程的数量
awk '
{
for(i=1;i<=NR;i++) Student[$1] = NF - 1
}
END {
for(std in Student) printf("%s %d\n", std, Student[std])
}
' $*
```

执行命令：`./demo2 reg.dat`

输出

```
Steve 3
Mary 3
Lisa 2
Lily 2
Wang 3
```



