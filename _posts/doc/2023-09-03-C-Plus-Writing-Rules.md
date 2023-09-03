---
tags:
  - GTD/Source
Status: Doing
LatestKeyDate: None
Comments: 
share: true
title: C++代码规范
sharetitle: 2023-09-03-C-Plus-Writing-Rules
sharepath: doc
---

# C++代码规范

## 零散的规范

1. 建议不用using namespace std
2. 文件名采用小写英文单词+下划线组成

## 变量命名规范

1. 变量名需有实际物理含义，采用英文单词或通用缩写命名，使用大驼峰命名法（如：`MyVariable`）或下划线命名法（如：`My_Variable`），可以大驼峰命名法为主，必要时辅以下划线命名法。
2. 变量名前需要加该变量的数据类型缩写（如：`double dMyVariable`），常用的数据类型缩写见下表：

| 数据类型名称 | 数据类型 | 缩写 |
|---|---|---|
| 整型 | int | i |
| 无符号整型 | unsigned int | ui |
| 浮点型 | double | d |
| 布尔型 | bool | b |
| 字符型 | char | c |
| 字符串 | string | s |
| 结构体 | struct | str |
| 类的对象 | class | o |
| vector对象 | vector | vct |
| map对象 | map | map |

3. 特殊变量的命名规则
	1. 通用常量可采用宏定义`#define <VARIABLE> <VALUE>`，需在`stdafx.h`中定义，变量名全部为大写字母。
	2. 声明结构体时，结构体名称全大写（如：`struct MYSTRUCT`）
	3. 类的数据成员（即类的属性）需加前缀`m_`（如：`int m_iMyProperty`）
	4. 局部使用的临时变量可加后缀`tmp`
	5. 涉及到角度的变量命名时需增加后缀，角度值为`_deg`，弧度值为`_rad`
	6. 变量的单位默认为国际单位，如果不是国际单位需在变量名后增加相应后缀，如单位为毫秒的变量需增加后缀`_ms`

## 函数定义规范

1. 函数名需有实际物理含义，采用英文单词或通用缩写命名，使用下划线命名法、同时每个单词的首字母小写。
2. 函数的输出为多个变量时，优先考虑采用一个可通用的结构体，否则采用在形参中传递地址的方式。例如：

```cpp
struct MYSTRUCT 
{
	double dTime;
	int iFlag;
	std::vector<double> vctData;
}

//采用一个可通用的结构体作为输出
MYSTRUCT my_function_struct_output(double dInput1, double dInput2)
{
	......
	MYSTRUCT strOutput;
	strOutput.dTime = ...;
	strOutput.iFlag = ...;
	strOutput.vctData.pushback(...);
	return strOutput;
}

//在形参中传递地址作为输出
bool my_function_address_output(double dInput1, double dInput2, double *dOutput1, double *dOutput2)
{
	......
	*dOutput1 = ...;
	*dOutput2 = ...;
	return true;
}

//使用示例
MYSTRUCT strData = {};
double dMyInput1 = 0;
double dMyInput2 = 0;
strData = my_function_struct_output(dMyInput1, dMyInput2);//采用一个可通用的结构体作为输出

double dMyOutput1 = 0;
double dMyOutput2 = 0;
my_function_address_output(dMyInput1, dMyInput2, &dMyOutput1, &dMyOutput2);//在形参中传递地址作为输出
```

3. 在排序函数参数时， 将所有输入参数放在所有输出参数之前。

## 类定义规范

1. 类名采用大驼峰命名法，同时加前缀`C`，例如`CDataReader`。
2. 数据成员和成员函数的顺序：数据成员和成员函数分别集中放置，其中数据成员放在成员函数之前。
3. public成员和private成员的顺序：public成员放在private成员之前。

## 头文件引用规范

1. 将引用的C++标准库中的头文件全部放置与`stdafx.h`中。
2. 如果要引用本项目编写的类，则仅在用到的头文件中引用。
3. 所有头文件都应含有`#define`保护来防止头文件被多重包含，命名格式为`<PROJECT>_<PATH>_<FILE>_H_`。示例代码如下：

```cpp
//对于Meta工程User路径下的Container.h文件，有以下代码
#ifndef META_USER_CONTAINER_H_
#define META_USER_CONTAINER_H_
...//此处为头文件中的其他代码
#endif //META_USER_CONTAINER_H_（此处注释需保留）
```

## 代码风格规范

1. 代码中适当位置添加空格
	1. 赋值运算符`=`、关系运算符（`>`、`<`等）与其两侧的变量间需加空格，例如：`dVariable1 = dVariable2;`
	2. 定义函数时，其多个形参间需以空格隔开，例如：`my_function(double dInput1, double dInput2)`
2. 代码注释
	1. 结构体、类中成员（包括数据成员及成员函数）声明时（即在头文件`.h`中），需在其后方添加用于说明该变量用途及单位的注释。
	2. 函数定义时须在其上方添加用于说明该函数用途、输入输出变量单位的注释。
	3. 使用`//`来写注释，多行注释也应遵循该原则，尽量避免使用`/* */`。


## 代码安全性规范

1. 每个函数的分支都应有返回值，否则如果函数走到无返回值的分支，其结果是未知的。
2. `switch`语句应包含`defult`分支，以处理各种预期外的情况，保证所有情况下均有操作。
3. `if...else...`语句需完整，不应出现只有`if`分支的情况，如果`else`分支无相应操作则留空。
4. 类中的数据成员（属性）一般应设为`private`，对数据成员的查询和修改需通过相应的接口函数实现。
5. 类中各数据成员的初始化需要由独立的`Initialize`函数完成，避免在构造函数中直接进行。
6. 结构体应在第一次使用到该结构体的类的头文件（`.h`文件）中定义，同时该定义应在类定义的前面。
7. 定义标准库函数`vector`或`map`时，需在定义完成后执行一次`clear`函数，以保证容器初始化为空。
8. 使用标准库函数`vector`或`map`的迭代器时，需判断该迭代器是否溢出。示例代码如下：

```cpp
std::vector<double> vctMyVector;
std::vector<double>::iterator iter_Data;
iter_Data = vctMyVector.begin();
if( iter_Data != vctMyVector.end() )
{
	...//当迭代器指向vector中存在的位置时，执行相关操作
}
else
{
	...//如果迭代器已越界则执行相应的保护措施
}
```

## 代码保护措施

> 为保证程序正常运行，需严格审查代码中是否涉及到以下情形，对涉及到以下情形的代码需严格按照示例代码的写法进行编写。

### 浮点数相等的判断

判断两个浮点数是否相等时，不可直接用关系运算符`==`或`!=`，需要判断两个浮点数是否足够接近。示例代码如下：

```cpp
double dVariable1;
double dVariable2;
//判断两浮点数是否相等，并将判断结果赋值给布尔变量bEqualFlag
bool bEqualFlag;
if( fabs( dVariable1-dVariable2 ) < 1e-10 )
{
	bEqualFlag = true;
}
else
{
	bEqualFlag = false;
}
```

### 除零保护

当 _变量_ 做分母时，需保证任何情况加该变量都不会为零，因此单独编写一个函数用于除零保护。示例代码如下：

```cpp
//除零保护函数，可直接复制
double Double0Protect( double dInput )
{
	double dMinBound = 1e-8;
	int ix_sign = (dInput<0)?-1:(dInput>=0);
	double dOutput = 0;
	if( fabs(dInput) < dMinBound )
	{
		dOutput = dMinBound*ix_sign;
	}
	else
	{
		dOutput = dInput;
	}
	return dOutput;
}

//除零保护用法示例
double dVariable1;
double dVariable2;
double dResult;
dResult = dVariable1/Double0Protect(dVariable2);
```

