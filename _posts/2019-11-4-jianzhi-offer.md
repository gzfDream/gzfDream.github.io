---
layout: post
title:  剑指offer(第二版) C++题解
date:   2019-11-4 11:17:00 +0800
categories: 手撕代码
tags: c++ algorithm
author: gzf
excerpt: 使用C++解决剑指offer中的例题
mathjax: true
---

## 例题1 赋值运算函数
**题目描述**
> 如下是`CMyString`的声明，请为其添加赋值运算函数。

```cpp
Class CMyString{
public:
	CMyString(char *pData=NULL);
	CMyString(const CMyString& m);
	~CMyString();
private:
	char* m_pData;
}
```

**思路**
* 返回值为该类型的引用
* 出入的参数类型声明为常量引用
* 释放实例自身已有的内存
* 检查传入的参数和当前的实例是不是同一个实例

<details>
  <summary>code</summary>
  <pre><code>
  	// 解法1
  	CMyString& CMyString::operator=(const CMyString& str){
		if(&str == this)
			return *this;
		else{
			delete []m_pData;
			m_pData = NULL;

			m_pData = new char[strlen(str.m_pData)+1];
			strcpy(m_pData, str.m_pData);

			return *this;
		}
  	}
  </code></pre>

  <pre><code>
    // 解法2 分配内存前先delete释放m_pData，如果内存不足，会导致new char抛出异常。采用如下解决办法。 
  	CMyString& CMyString::operator=(const CMyString& str){
		if(&str == this)
			return *this;
		else{
			CMyString str_tmp(str);

			char *tmp=str_tmp.m_pData;
			str_tmp.m_pData=m_pData;
			m_pData = tmp;

			return *this;
		}
  	}
  </code></pre>
</details>



## 例题2 单例模式
**题目描述**
> 设计一个类，只能生成该类的一个实例。

**思路**
> 单例模式是为确保一个类只有一个实例，并为整个系统提供一个全局访问点的一种模式方法。
> 
> 单例特点：
> 
> 1.在任何情况下，单例类永远只有一个实例存在。
>
> 2.单例需要有能力为整个系统提供这一唯一实例。
>
> 请[参考](https://zhuanlan.zhihu.com/p/62014096)

<details>
  <summary>code</summary>
  <pre><code> 
    class Singleton{
    private:
    	Singleton(){}
    	static Singleton* instance;
    public:
    	static Singleton* get_instance(){
    		if(instance==NULL)
    			instance = new Singleton();
    		return instance;
    	}
 	}
 	//Singleton* Singleton::_instance = NULL;
  </code></pre>
</details>



## 例题3.1 数组中重复的数字
**题目描述**
> 在长度为n的数组中，所有的数字都在0~n-1之间。找出数组中任意重复的数字。

**示例**
>{2,3,1,0,2,5,3}
>
>输出：2或者3

**思路**
> 1. 排序或者使用哈希表
> 2. 扫描数组，将位置i的数字m与位置m处的数字比较，若相等则重复。若不相等，则交换两者的位置。重复此过程。

```cpp
      bool duplicate(const std::vector<int>& v, int* dup){
        if(v.size()==0){
          return false;
        }

        for(int i=0; i<v.size(); i++){
          if(v[i]<0 || v[i]>v.size())
            return false;
        }

        for(int i=0; i<v.size(); i++){
          while(v[i]!=i){
            if(v[i]==v[v[i]]){
              *dup = v[i];
              return true;
            }else{
              int tmp=v[v[i]];
              v[v[i]]=v[i];
              v[i]=tmp;
            }
          }
        }
        return false;
      }
```


## 例题3.2 数组中重复的数字
**题目描述**
> 在长度为n+1的数组中，所有的数字都在1~n之间。找出数组中任意重复的数字。

**示例**
>{2,3,1,4,2,5,3}
>
>输出：2或者3

**思路**
> * 创建一个n+1的数组，将原数组的数字复制到新数组对应下标的位置。空间复杂度O(n)，时间复杂度O(n)
> * 类似于二分的思想，取1~n的中间值m，遍历数组统计落在1~m中的数字的个数，如果大于m，则这一半里一定有重复的数字。时间复杂度O(nlogn)，空间复杂度O(1)

```cpp
    int duplicate(const std::vector<int>& nums, int* dup){
      itn n=nums.size();
      int left=1;
      int right=n-1;
      while(left<=right){
        int m=((right-left)>>1)+left;
        int tmp=0;
        for(int i=0; i<nums.size(); i++){
          if(nums[i]<=m)
            tmp++;
        }
        if(left==right){
          if(tmp>1)
            return left;
          else
            break;
        }
        if(tmp>m-left+1)
          right=m;
        else
          left=m+1;
      }
      return -1;
    }
```



## 例题4 二维数组中查找
**题目描述**
> 一个m x n 矩阵 matrix，该矩阵具有以下特性：
> 
> 每行的元素从左到右升序排列。
>
> 每列的元素从上到下升序排列。
>
>  编写一个高效的算法，查找其中的一个目标值 target。

**示例**
> [
>
>  [1,   4,  7, 11, 15],
>
>  [2,   5,  8, 12, 19],
>
>  [3,   6,  9, 16, 22],
>
>  [10, 13, 14, 17, 24],
>
>  [18, 21, 23, 26, 30]
>
> ]

**思路**
> 从右上角数字m开始，target如果大于m，则去掉这一行; 如果target小于m，则去掉这一列; 相等则结束。
```cpp
    bool searchMatrix(vector<vector<int> >& matrix, int target) {
        if(matrix.empty())return false;
        int m = 0;
        int n = matrix[0].size()-1;
        while(m<matrix.size()&&n>=0){
            if(matrix[m][n]==target)return true;
            else if(matrix[m][n]>target) n--;
            else if(matrix[m][n]<target) m++;
        }
        return false;
    }
```