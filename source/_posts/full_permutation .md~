title: "递归实现全排列"
date: 2015-04-01 10:15:20
tags:
  - 面试题
  - 全排列
  - 算法
categories:
  - 原创
  - 算法系列
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-13/35334370.jpg
---

全排列的讲解及代码实现
<!-- excerpt -->

**全排列的含义**
拿 `123` 举例，`123` 的全排列一共 `3！` , 即6种，分别是：

- `123` &emsp; `132` &emsp; *规律是1开头，然后是2和3的全排列*
- `213` &emsp; `231` &emsp; *规律是2开头，然后是1和3的全排列*
- `312` &emsp; `321` &emsp; *规律是3开头，然后是1和2的全排列*

由上不难发现，全排列其实就是每个元素分别开头，然后其余的元素全排列。很显然这是一个递归的规律。

**代码**
```C
#include <iostream>   
using namespace std;
int n = 0;
  
void swap(char *a ,char *b)
{
    int m ;
    m = *a;
    *a = *b;
    *b = m;
}

void perm(char list[],int k, int m )
{
    int i;
    if(k == m)
    {
        for(i = 0 ; i <= m ; i++)  
        {  
            cout<<" "<<list[i];  
        }  
        cout<<endl;  
        n++;  
    }
    
    for(i = k ; i <=m;i++)
    {
        swap(&list[k],&list[i]);  
        perm(list,k+1,m);  
        swap(&list[k],&list[i]);  
    }
}
  
int main()
{
    char list[] ="12345";
    perm(list,0,4);
    cout<<"total:"<<n<<"/n";
    return 0;
}
```
***
(EOF)
