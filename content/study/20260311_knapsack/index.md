---
title: 'Knapsack Problem'
date: '2026-03-11T21:05:49+08:00'
# weight: 1
# aliases: ["/first"]
tags: ["Algorithm"]
author: "PaperMoon"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
# description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
# disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
hiddenInHomeList: true
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
math: true
cover:
    image: "https://picsum.photos/800/400" # image path/url
    alt: "test-caption" # alt text
    caption: "test" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/MilknoCandy/milknocandy.github.io/tree/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
<!-- set id for section manually -->
## 0-1 Knapsack Problem {#01knapsack-ref}
{{< box default >}}
**Description:**
There are $N$ items and one knapsack with a maximum capacity of $V$. Each item can be selected at most once (i.e., either take it or leave it). The $i\text{-th}$ item has a volume of $v_i$​ and a value of $w_i$​. Your task is to select a subset of items to put into the knapsack such that:
1. The total volume of the selected items does not exceed the knapsack's capacity V.
2. The total value of the selected items is maximized.

Output the maximum possible total value achievable under these constraints.

**Input Format:**
- Line 1: Two integers $N$ and $V$, separated by a space, representing the number of items and the capacity of the knapsack, respectively.
- Next $N$ lines: Each line contains two integers $v_i$​ and $w_i$​, separated by a space, representing the volume and value of the $i\text{-th}$ item.

**Output Format:**
Output a single integer, the maximum total value of items that can be packed into the knapsack.

**Constraints:**<br>
$$0\lt N,V \le 1000\\ 0\lt v_i, w_i \le 1000$$

**Sample Input:**
```bash
4 5
1 2
2 4
3 4
4 5
```
**Sample Output:**
```bash
8
```
{{< /box >}}

As can be seen from the problem statement, our goal is to determine the optimal subset of items to pack into the knapsack to maximize the total value. Since each item can be selected at most once (the 0-1 knapsack constraint), the core idea is to iteratively determine the optimal selection when considering the $i\text{-th}$ item. This decision-making process can be formalized using the following dynamic programming recurrence relation:
$$
\text{dp}[i][j] = \max(\text{dp}[i-1][j], \text{dp}[i-1][j-v_i]+w_i)\tag{1-1}
$$
In the iteration where we decide whether to include the i-th item, we compare the total value obtained by including this item against the optimal value achieved without considering it (i.e., the optimal solution for the first $i\text{−1}$ items). This is a classic dynamic programming problem, and the corresponding implementation code is provided below:

{{< code-tabs labels="C++: 1-D sequence, C++: 2-D matrix, Python: 1-D sequence" >}}
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() 
{
    int n, m;   
    cin >> n >> m;
    vector<int> f(m+1)
    int v, w;
    
    for(int i = 1; i <= n; i++) {
        cin >> v >> w;
        // Reverse iteration (core: j >= v[i])
        for(int j = m; j >= v; j--) {
            f[j] = max(f[j], f[j - v] + w);
        }
    }

    cout << f[m] << endl;
    return 0;
}
```
---
```cpp
#include<bits/stdc++.h>

using namespace std;

const int MAXN = 1005;
int v[MAXN];    // 体积
int w[MAXN];    // 价值 

int main() 
{
    int n, m;   
    cin >> n >> m;
    vector<vector<int>> f(n+1, vector<int>(m+1, 0));
    for(int i = 1; i <= n; i++) 
        cin >> v[i] >> w[i];

    for(int i = 1; i <= n; i++) 
        for(int j = 1; j <= m; j++)
        {
            //  if current bag can't pack i-th goods, then the optimal value is equal to the summation of the first i-1 goods
            if(j < v[i]) 
                f[i][j] = f[i - 1][j];
            // can pack this, then check this
            else    
                f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);
        }           

    cout << f[n][m] << endl;

    return 0;
}
```
---
```python
def main():
    num_N, val_V = map(int, input().split())
    solutions = [0] * (val_V + 1)
    
    # 提前绑定max为局部变量（微小提速，不影响简洁性）
    max_func = max
    
    for _ in range(num_N):  # 无需i计数，用_更简洁
        w, v = map(int, input().split())
        # 逆序枚举，保留你的核心逻辑
        for j in range(val_V, w - 1, -1):
            solutions[j] = max_func(solutions[j], solutions[j - w] + v)
    
    print(solutions[val_V])
    
if __name__ == '__main__':
    main()
```
{{< /code-tabs >}}

## Complete Knapsack Problem
{{< box default >}}
**Description:**
There are $N$ items and a knapsack with capacity $V$. Each item can be selected any number of times. The $i\text{-th}$ item has volume $v_i$ and value $w_i$. Select items to maximize total value, with total volume $\le V$. Output the maximum value.

**Input Format:**
- Line 1: Two integers $N, V$ (number of items, knapsack capacity).
- Next $N$ lines: Each line has two integers $v_i$, $w_i$ (volume and value of the $i\text{-th}$ item).

**Output Format:**
Output a single integer (maximum total value).

**Constraints:**
$$0\lt N,V \le 1000\\ 0\lt v_i, w_i \le 1000$$

**Sample Input:**
```bash
4 5
1 2
2 4
3 4
4 5
```
**Sample Output:**
```bash
10
```
{{< /box >}}
We can add one for-loop to the code from [0-1 Knapsack Problem](#01knapsack-ref).
{{< code-tabs labels="C++: Think for Unbounded KP" >}}
```cpp
#include<bits/stdc++.h>

using namespace std;

const int MAXN = 1005;
int v[MAXN];    // 体积
int w[MAXN];    // 价值 

int main() 
{
    int n, m;   
    cin >> n >> m;
    vector<vector<int>> f(n+1, vector<int>(m+1, 0));
    for(int i = 1; i <= n; i++) 
        cin >> v[i] >> w[i];

    for(int i = 1; i <= n; i++) 
        for(int j = 1; j <= m; j++)
        {
            for(int k = 0; k*v[i]<=j; k++){
                f[i][j] = max(f[i][j], f[i - 1][j - k*v[i]] + k*w[i]);
            }
        }           

    cout << f[n][m] << endl;

    return 0;
}
```
{{< /code-tabs >}}
Let's deduct this, find out how we update:
$$
\scriptsize
\begin{aligned}
\text{dp}[i][j] &= \max(\text{dp}[i-1][j], \text{dp}[i-1][j-v_i]+w_i, \text{dp}[i-1][j-2\cdot v_i]+2\cdot w_i, ...)\\
\text{dp}[i][j-v] &= \max(\text{dp}[i-1][j-v], \text{dp}[i-1][j-2\cdot v_i]+2\cdot w_i, \text{dp}[i-1][j-3\cdot v_i]+3\cdot w_i, ...)
\tag{1-2}
\end{aligned}
$$