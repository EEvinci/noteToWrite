# PAT甲级备考

[TOC]



## 基础数据结构题

题目有1004，1020，1021，1028，1032，1107等。

涉及到的有**队列**，**栈**，[**链表**](https://www.zhihu.com/search?q=链表&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})，**二叉树**，**并查集**。

发现自己并不熟悉这些，那么应该花几天的时间学习**c++STL**相关操作，**二叉树前中后遍历**。

会**针对结构体使用algorithm头文件的排序**，对**STL**的**vector**，**stack**，**queue**，**map**，**set**相关操作和[**迭代器**](https://www.zhihu.com/search?q=迭代器&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})足够熟悉，否则强烈建议题主花上一些时间学习。

## 模拟题目

范围较广，可以锻炼思维，增强码代码能力。

题目有但不限于这些：1005，1006，1008，1009，1011，1015，1024，1035，1042，1043，1048，1065。

需要学会**贪心思想**，[深度优先搜索](https://www.zhihu.com/search?q=深度优先搜索&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})和**广度优先搜索**，[**进制转换**](https://www.zhihu.com/search?q=进制转换&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})，**筛素数**，**字符串处理**，[**二分查找**](https://www.zhihu.com/search?q=二分查找&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})。

涉及到一些**算法**，更**高级一点的数据结构**，**数学**，[**动态规划**](https://www.zhihu.com/search?q=动态规划&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})知识。

**动态规划**较为晦涩，初学者需要较多时间才能掌握。例如1007，**最大子串和**就是经典的一题。



**数据结构方面**学习优美的[**树状数组**](https://www.zhihu.com/search?q=树状数组&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})，**AVL**。

例如**1057**需要用到**树状数组**的**快速求和进行二分查找**，**1066**使用**AVL进行模拟**，**AVL的旋转思想**对Splay（展开）这样飘逸数据结构的学习是必不可少的。



**算法方面**在题库里主要涉及到**图论算法**。

如1003，1046，1106，主要是**最短路算法**和**深度优先搜索**的应用。

1053有**多叉树的储存和遍历**。

**图的储存**学会使用**矩阵**和[**vector**](https://www.zhihu.com/search?q=vector&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})两种方式。

**最短路算法**较多，不建议全都学会，但一定要对其中一种足够熟悉，并且对**矩阵图**和[**vector图**](https://www.zhihu.com/search?q=vector图&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})都会写。



**数学方面**主要是学会**筛素数**，求**gcd**，**lcm**，**O(sqrt(n))的找约数**，**素因子**，**会用约数和定理**，[**约数个数定理**](https://www.zhihu.com/search?q=约数个数定理&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A77585143})，c++的话还有**大数的模拟**。

## 考纲

考纲里要求掌握的算法为：

- 哈希映射、
- [并查集](https://www.zhihu.com/search?q=并查集&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A488422617})、
- 最短路径、
- 拓扑排序、
- [关键路径](https://www.zhihu.com/search?q=关键路径&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A488422617})、
- 贪心、
- [深度优先搜索](https://www.zhihu.com/search?q=深度优先搜索&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A488422617})、
- 广度优先搜索、
- 回溯剪枝等。

**哈希映射**：一般会用**map**和**unordered_map**就好。此外，比如说题目固定了关键字为4个大写字母，那么可以把关键字看成26进制数，这样就能把字符串转换为int型数据处理，提高程序运行速度。更新：当然基本概念也不能忘，解决哈希冲突的**基础方法**要理解，包括[开放地址法](https://www.zhihu.com/search?q=开放地址法&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A488422617})（线性探测、平方探测）和[链地址法](https://www.zhihu.com/search?q=链地址法&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A488422617})（开放定址法）。

## 1、数据结构

可以用STL系列
栈：1051
堆：1098
队列：1014、1056
链表：1032、1052、1074、1097、1133
并查集：1107、1114、1118
树状数组：1057
树：1004、1053、1079、1090、1094、1102、1106
二叉树：1020、1043、1064、1066、1086、1099、1110、1115、1119、1127、1135、1147、1151、1155

## 2、基础算法

复习一下基础系列
模拟：1002、1009、1017、1026、1042、1046、1065、1105、1153
排序：1012、1016、1025、1028、1055、1062、1075、1080、1083、1113、1125、1141
字符串处理：1001、1005、1023、1024、1035、1060、1061、1073、1077、1082、1108、1140、1150、1152
二分查找：1010、1044、1085
查找元素：1006、1011、1036
分数模拟：1081、1088
贪心：1033、1037、1038、1067、1070

## 3、图论相关

多背一背模板系列
最短路径：1003、1018、1030、1072、1087、1111
深度优先搜索DFS：1013、1021、1034、1103、1130、1131、1134
广度优先搜索BFS：1076、1091
记忆化搜索：1007、1040、1045、1068、1101
其他的图论：1123、1126、1142
拓扑排序：1146