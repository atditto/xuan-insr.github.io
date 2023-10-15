# 0 数字与计算
Original Post: [http://git-zju-edu-cn-s.webvpn.zju.edu.cn:8001/ppl-course/mua/-/tree/main/](http://git-zju-edu-cn-s.webvpn.zju.edu.cn:8001/ppl-course/mua/-/tree/main/)

By PPL2021

## 要求
你的任务是实现一个 MUA 语言的解释器。从标准输入中读入输入程序，然后在标准输出（屏幕）中写入 MUA 程序的输出（也就是 `print` 操作产生的效果）。

你暂时不需要考虑任何输入不合法的情况。输出实数时，你可以在合理的范围内自由定义输出的小数位数。

### 提示

1. 先完整看完这个提示，再看下面的语法说明。
2. 为了挑战自己，尝试在看懂语法说明之前 **不要** 看测试样例。确信自己看懂之后，再看测试样例来验证自己有没有真的看懂。如果没有超出你预期的测试样例，奖励你一块饼干 🍪
3. 看输入样例时，可以根据自己的理解脑补一下输出，再与输出样例比对。如果一致，再来一块 🍪
4. 完全看懂测试样例之后，再开始构思怎么组织这个程序。
5. 在构思怎么组织这个程序之前，建议你简单浏览完整的 [MUA 语法说明](../grammar)。你不需要完全看懂它，但是你需要获得一些有用的信息，例如之后你可能要支持更多的基本操作、数据类型，或者对现有的数据类型做更多的操作。

## 语法说明

### 基本数据类型 | value
数字 number

- 任何值之间都以空格分隔
- 数字的字面量以 `[0~9]` 或 `'-'` 开头，不区分整数，浮点数

### 基本操作 | operation
基本形式：操作名 参数

操作名是一个名字，与参数间以空格分隔。参数可以有多个，多个参数间以空格分隔。每个操作所需的参数数量是确定的，所以不需要括号或语句结束符号。所有的操作都有返回值。

一个程序就是操作的序列。

基本操作有：

- `print <value>`：输出 value 并换行，返回这个 value
- 运算符 operator 
   - `add`, `sub`, `mul`, `div`, `mod`：`<operator> <number> <number>`

提示：

- 可以看到，这是一种「前缀」语言，也就是说，每一个表达式的操作符都出现在表达式的最开头，因此当我们在读到一个操作符之后，就知道后面还需要得到多少个表达式的值就可以了。
- **表达式** 是运算符和操作数的序列，用来指明一个计算。一个 number 也是一个表达式，一个 print 或运算操作也构成一个表达式。表达式可能有操作数，操作数也是一个表达式；所以这里有递归的思想在里面。
   - 不妨用 C++ 类比：`+` 是一个运算符，接受 2 个操作数，例如 `1 + 2`；但是这里的操作数不止可以是数，还可以是表达式，例如 `func() + 3`, `(1 * 3) + 2` 等。 
- 表达式除了有一个 **返回值** 之外，还可以有 **副作用**，例如赋值和输出。
   - 例如，C++ 中的 `i += 1;` 的副作用是将 `i` 的值 +1，而返回值是计算后的 `i` 值。
   - 在 Assignment 0 中，唯一的副作用就是 `print` 带来的输出。
- 在目前看来，一个程序由若干表达式组成。因此你的 `main` 函数干的事情可能就是一个 `while (true)` 循环，不断接受表达式。

---

## 测试样例

### Input Case
```lua
print -1.3
print add 1 2
print sub 1 3.5
print mul 2 4
print div 3 5
print mod 7 2
print add 1 mul 2 3
print div add 2 4 mul 3 9
print print add 1 2
add 1 4
```

提示：第 9 行应当有 2 个输出，第 10 行不应当有输出

### Output Case

注：数字的小数位数不重要

```
print -1.3
-1.300000
print add 1 2
3.000000
print sub 1 3.5
-2.500000
print mul 2 4
8.000000
print div 3 5
0.600000
print mod 7 2
1.000000
print add 1 mul 2 3
7.000000
print div add 2 4 mul 3 9
0.222222
print print add 1 2
3.000000
3.000000
add 1 4
```