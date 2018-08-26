# 一、partitions分割函数系列
## 1. partition()
```
-功能：
Iterator partition ( first, last,  pred)；
若满足条件则留下，左边起第一个不满足条件的和右边起第一个不满足条件的交换，第二个和第二个交换.....返回第一个不满足条件的指针

-示例
#include <iostream>     // std::cout
#include <algorithm>    // std::partition
#include <vector>       // std::vector

bool IsOdd (int i) { return (i%2)==1; }

int main () {
  std::vector<int> myvector;
  for (int i=1; i<10; ++i) myvector.push_back(i); 
  // 1 2 3 4 5 6 7 8 9

  std::vector<int>::iterator bound;
  bound = std::partition (myvector.begin(), myvector.end(), IsOdd);

  std::cout << "odd elements:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=bound; ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  std::cout << "even elements:";
  for (std::vector<int>::iterator it=bound; it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}



output:
odd elements: 1 9 3 7 5
even elements: 6 4 8 2
```

## 2. stable_partition()
```
-功能：
Iterator stable_partition ( first, last,  pred)；
作用与partition函数相同，只是这个不改变先后顺序；

-示例：
上段代码改成
bound = std::stable_partition (myvector.begin(), myvector.end(), IsOdd);

output:
odd elements: 1 3 5 7 9
even elements: 2 4 6 8

```


## 3.is_partitioned()
```
-功能：
bool is_partitioned ( first,  last,  pred)；
判断是够满足条件分割；返回false和true；

if ( std::is_partitioned(foo.begin(),foo.end(),IsOdd) )

-注意：C++11版本以上
```

## 4. partition_copy()
```
-功能：
partition_copy (first, last,result_true，result_false, pred)；
满足条件的放在true数组，不满足的放在false数组
返回一对指针，分别是true和false的尾指针；

-注意：C++11版本以上
      请预先为存放结果分配内存空间
```


# 二、sorting排序函数系列
## 1.sort()
```
-功能：
sort（first,last);
区间内元素按升序排序

sort(first,last,pre);
区间内元素按条件排序
bool myfunction (int i,int j) { return (i>j); }
若条件如上，则可实现降序排序

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::sort
#include <vector>       // std::vector

bool myfunction (int i,int j) { return (i<j); }

struct myclass {
  bool operator() (int i,int j) { return (i<j);}
} myobject;

int main () {
  int myints[] = {32,71,12,45,26,80,53,33};
  std::vector<int> myvector (myints, myints+8);               // 32 71 12 45 26 80 53 33

  std::sort (myvector.begin(), myvector.begin()+4);           
  //(12 32 45 71)26 80 53 33

  std::sort (myvector.begin()+4, myvector.end(), myfunction); // 12 32 45 71(26 33 53 80)

  std::sort (myvector.begin(), myvector.end(), myobject);    
  //(12 26 32 33 45 53 71 80)

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
myvector contains: 12 26 32 33 45 53 71 80

```

## 2. partial_sort()
```
-功能：
void partial_sort (first,middle, last);
对顺序前（middle-first）个元素进行排序，后面的元素不一定保持原顺序

void partial_sort ( first, middle, last, Compare comp);
按条件对顺序前（middle-first）个元素进行排序

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::partial_sort
#include <vector>       // std::vector

bool myfunction (int i,int j) { return (i<j); }

int main () {
  int myints[] = {9,8,7,6,5,4,3,2,1};
  std::vector<int> myvector (myints, myints+9);

  std::partial_sort (myvector.begin(), myvector.begin()+5, myvector.end());

  std::partial_sort (myvector.begin(), myvector.begin()+5, myvector.end(),myfunction);

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Possible output:
myvector contains: 1 2 3 4 5 9 8 7 6
```

## 3. partial_sort_copy()
```
-功能：
partial_sort_copy (myints, myints+9, myvector.begin(), myvector.end());
先定义myvector大小，根据大小对myints前面元素排序并拷贝过来

partial_sort_copy (myints, myints+9, myvector.begin(), myvector.end(), myfunction);

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::partial_sort_copy
#include <vector>       // std::vector

bool myfunction (int i,int j) { return (i<j); }

int main () {
  int myints[] = {9,8,7,6,5,4,3,2,1};
  std::vector<int> myvector (5);

  std::partial_sort_copy (myints, myints+9, myvector.begin(), myvector.end());

  std::partial_sort_copy (myints, myints+9, myvector.begin(), myvector.end(), myfunction);

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
myvector contains: 1 2 3 4 5
```

## 4. is_sorted() 和 is_sorted_until()
```
-功能：
bool is_sorted ( first,  last);
bool is_sorted ( first,  last,  comp);
是否按升序或者条件排好序

is_sorted_until( first,  last)；
is_sorted_until( first,  last，comp)；
返回有序元素后面无序的第一个元素的指针
可用it-begin得到有序元素个数

-注意：C++11版本
```

# 三、Binary search二分搜索函数系列
## 1.lower_bound()和upper_bound()
```
-功能：
Iterator lower_bound ( first, last, const T& val)；
返回一个非递减序列[first, last)中的第一个大于等于值val的位置。

Iterator upper_bound ( first, last, const T& val)；
返回一个非递减序列[first, last)中的第一个大于值val的位置。

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::lower_bound, std::upper_bound, std::sort
#include <vector>       // std::vector

int main () {
  int myints[] = {10,20,30,30,20,10,10,20};
  std::vector<int> v(myints,myints+8);           
  // 10 20 30 30 20 10 10 20

  std::sort (v.begin(), v.end());               
  // 10 10 10 20 20 20 30 30

  std::vector<int>::iterator low,up;
  low=std::lower_bound (v.begin(), v.end(), 20); 
  up= std::upper_bound (v.begin(), v.end(), 20); 

  std::cout << "lower_bound at position " << (low- v.begin()) << '\n';
  std::cout << "upper_bound at position " << (up - v.begin()) << '\n';

  return 0;
}



Output:
lower_bound at position 3
upper_bound at position 6


-注意：
Iterator lower_bound ( first, last, const T& val,comp)；
如果序列不是升序，而是降序，可添加comp函数i>j强调降序;
此系列默认都是升序，一定要先sort排序，也都可添加comp函数
```

## 2. equal_range()
```
-功能：
equal_range ( first,  last, const T& val)；
返回值与val相等的一对指针，即起始指针和最后一个数的下一指针；

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::equal_range, std::sort
#include <vector>       // std::vector

bool mygreater (int i,int j) { return (i>j); }

int main () {
  int myints[] = {10,20,30,30,20,10,10,20};
  std::vector<int> v(myints,myints+8);                        
  std::pair<std::vector<int>::iterator,std::vector<int>::iterator> bounds;

  std::sort (v.begin(), v.end());                              // 10 10 10 20 20 20 30 30
  bounds=std::equal_range (v.begin(), v.end(), 20);            

  std::sort (v.begin(), v.end(), mygreater);                   // 30 30 20 20 20 10 10 10
  bounds=std::equal_range (v.begin(), v.end(), 20, mygreater); 
  //此为降序，所以添加函数强调
  std::cout << "bounds at positions " << (bounds.first - v.begin());
  std::cout << " and " << (bounds.second - v.begin()) << '\n';

  return 0;
}


Output:
bounds at positions 2 and 5
```

## 3. binary_search()
```
-功能：
bool binary_search ( first,  last, const T& val);
判断序列是否存在值为val的元素，默认升序要注意
```


