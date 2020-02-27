---
title: 程序设计思维与实践 Week2 作业
date: 2020-02-27 09:30:30
categories: 简单且枯燥
tags:
---

## A - Maze

#### 题目描述

东东有一张地图，想通过地图找到妹纸。地图显示，0表示可以走，1表示不可以走，左上角是入口，右下角是妹纸，这两个位置保证为0。既然已经知道了地图，那么东东找到妹纸就不难了，请你编一个程序，写出东东找到妹纸的最短路线。

<!-- more -->

#### 解题思路与代码

bfs处理出层次，然后倒推回去就可以，dfs很好写。

```cpp
#include<cstdio>
#include<iostream>
#include<cstring>
#include<string>
#include<algorithm>

using namespace std;
int pic[10][10];
pair<int, int> q[50];
inline void bfs(){
    int h = 1, t = 0;
    q[++t] = make_pair(1, 1);
    pic[1][1] = 2;
    while (h <= t)
    {
        int x = q[h].first, y = q[h++].second, k = pic[x][y];
        if (x < 5 && !pic[x + 1][y]) q[++t] = make_pair(x + 1, y), pic[x + 1][y] = k + 1;
        if (x > 1 && !pic[x - 1][y]) q[++t] = make_pair(x - 1, y), pic[x - 1][y] = k + 1;
        if (y < 5 && !pic[x][y + 1]) q[++t] = make_pair(x, y + 1), pic[x][y + 1] = k + 1;
        if (y > 1 && !pic[x][y - 1]) q[++t] = make_pair(x, y - 1), pic[x][y - 1] = k + 1;
    }
}
inline void dfs(int x, int y){
    if (x == 1 && y == 1) {
        printf("(0, 0)\n");
        return;
    }
    if (x > 1 && pic[x - 1][y] == pic[x][y] - 1) dfs(x - 1, y);
    else if (y > 1 && pic[x][y - 1] == pic[x][y] - 1) dfs(x, y - 1);
    else if (x < 5 && pic[x + 1][y] == pic[x][y] - 1) dfs(x + 1, y);
    else dfs(x, y + 1);
    printf("(%d, %d)\n", x - 1, y - 1);
}
int main(){
    for (int i = 1; i <= 5; ++i)
        for (int j = 1; j <= 5; ++j)
            scanf("%d", &pic[i][j]);
    bfs();
    dfs(5, 5);
    // system("pause");
}
```

## B - Pour Water

#### 问题描述

倒水问题， "fill A" 表示倒满A杯，"empty A"表示倒空A杯，"pour A B" 表示把A的水倒到B杯并且把B杯倒满或A倒空。

#### Input

输入包含多组数据。每组数据输入 A, B, C 数据范围 0 < A <= B 、C <= B <=1000 、A和B互质。

#### Output

你的程序的输出将由一系列的指令组成。这些输出行将导致任何一个罐子正好包含C单位的水。每组数据的最后一行输出应该是“success”。输出行从第1列开始，不应该有空行或任何尾随空格。

#### Sample 

##### input

```
2 7 5 
2 7 4
```
##### Output

```
fill B
pour B A
success 
fill A
pour A B
fill A
pour A B
success
```

#### 解题思路

假设A一定比B容量大，要求一定在A杯中得到C容量的水，那么A中的水一定由B中很好次倒过去得到，每次B可以向A中导入B容量的水，则我们只需要将C分为两部分：$⌊\dfrac{C}{B}⌋\times B$ 与$C\bmod B$，因此我们现在只需要在A中得到 $C\bmod B$ 就可以解决问题。

要取得 $C\bmod B$，我们发现一个比较显然的迭代操作是假设B中现有容量为E的水，将E倒入A中，用B将A倒满，则B中会剩下$B-(A-E)\bmod B$的水，思考操作的可行性，将式子化一下：
$$
B-(A-E)\bmod B\\
=((B-A+E)\bmod B) \bmod B
$$
即每次迭代事实上是在做$E-A$ 的操作，因为A、B互质，所以这样一定能迭代到B剩余系下的每一个数字，所以方法可行。

#### 代码

```cpp
#include<cstdio>
#include<iostream>
#include<cstring>
#include<string>
#include<algorithm>

using namespace std;

int A, B, C;
char a, b;
inline void getMod(int tmp){
    printf("pour %c %c\n", b, a);
    while(tmp < A){
        tmp += B;
        printf("fill %c\n", b);
        printf("pour %c %c\n", b, a);
    }
    printf("empty %c\n", a);
}
inline void getC(){
    int tmp = C % B;
    printf("pour %c %c\n", b, a);
    while(tmp != C){
        tmp += B;
        printf("fill %c\n", b);
        printf("pour %c %c\n", b, a);
    }
}
int main(){
    
    while(~scanf("%d%d%d", &A, &B, &C)){
        int tmp = 0;
        if (A > B) a = 'A', b = 'B';
        else swap(A, B), a = 'B', b = 'A';
        while(tmp != C%B){
            getMod(tmp);
            tmp = B - (A - tmp) % B;
        }
        getC();
        printf("success\n");
    }
}
```

