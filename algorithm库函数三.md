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

# 四、Merge合并函数系列
*此系列元素都需要先进行sort排序，方可正确执行*
## 1. Merge()
```
-功能：
Iterator merge (first1, last1, first2, last2, result)；
将内容1与内容2合并并放在result并升序顺序
注意要求内容1和内容2都是升序

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::merge, std::sort
#include <vector>       // std::vector

int main () {
  int first[] = {5,10,15,20,25};
  int second[] = {50,40,30,20,10};
  std::vector<int> v(10);

  std::sort (first,first+5);
  std::sort (second,second+5);
  std::merge (first,first+5,second,second+5,v.begin());

  std::cout << "The resulting vector contains:";
  for (std::vector<int>::iterator it=v.begin(); it!=v.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
The resulting vector contains: 5 10 10 15 20 20 25 30 40 50
```

# 2. inplace_merge()
```
-功能：
void inplace_merge (first,middle,last);
将同一容器的两段升序内容重新整体升序排序；

void inplace_merge ( first, middle, last, Compare comp);
若两段内容为降序，则可通过comp函数说明

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // inplace_merge,sort,copy
#include <vector>       // std::vector
bool comp(int i ,int j)
{return i>j;}
int main () {
  int first[] = {50,40,30,20,10};
  int second[] = {10,20,30,40,50};
  std::vector<int> v(10);
  std::vector<int>::iterator it;

  std::sort (second,second+5,comp);

  it=std::copy (first, first+5, v.begin());
     std::copy (second,second+5,it);

  std::inplace_merge (v.begin(),v.begin()+5,v.end(),comp);

  std::cout << "The resulting vector contains:";
  for (it=v.begin(); it!=v.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
The resulting vector contains: 50 50 40 40 30 30 20 20 10 10
```

## 3. includes()
```
-功能：
bool includes ( first1,  last1, first2, last2)
判断序列1是否包含有序列2的全部元素，需要升序排列

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::includes, std::sort

int main () {
  int container[] = {5,10,15,20,25,30,35,40,45,50};
  int continent[] = {40,30,20,10};

  std::sort (container,container+10);
  std::sort (continent,continent+4);

  // using default comparison:
  if ( std::includes(container,container+10,continent,continent+4) )
    std::cout << "container includes continent!\n";
  
  return 0;
}

```

## 4. set_union()
```
-功能：
Iterator set_union (first1, last1,first2, last2, result);
将两个升序序列去除重复元素后排序放于result，可添加comp变为降序

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::set_union, std::sort
#include <vector>       // std::vector

int main () {
  int first[] = {5,10,15,20,25};
  int second[] = {50,40,30,20,10};
  std::vector<int> v(10);                      
  std::vector<int>::iterator it;

  std::sort (first,first+5);     //  5 10 15 20 25
  std::sort (second,second+5);   // 10 20 30 40 50

  it=std::set_union (first, first+5, second, second+5, v.begin());
  // 5 10 15 20 25 30 40 50  0  0
  v.resize(it-v.begin());                      
  // 5 10 15 20 25 30 40 50

  std::cout << "The union has " << (v.size()) << " elements:\n";
  for (it=v.begin(); it!=v.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
The union has 8 elements:
 5 10 15 20 25 30 40 50
 ```

 ## 5. set_intersection()
 ```
 -功能：
 Iterator set_intersection (first1,last1,first2,last2, result);
 将两个升序序列中的重复元素提取出来排序放在result，可添加comp函数

 -示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::set_intersection, std::sort
#include <vector>       // std::vector

int main () {
  int first[] = {5,10,15,20,25};
  int second[] = {50,40,30,20,10};
  std::vector<int> v(10);                     
  std::vector<int>::iterator it;

  std::sort (first,first+5);     //  5 10 15 20 25
  std::sort (second,second+5);   // 10 20 30 40 50

  it=std::set_intersection (first, first+5, second, second+5, v.begin());
  // 10 20 0  0  0  0  0  0  0  0
  v.resize(it-v.begin());                      
  // 10 20

  std::cout << "The intersection has " << (v.size()) << " elements:\n";
  for (it=v.begin(); it!=v.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
The intersection has 2 elements:
 10 20
 ```

 ## 6.set_difference()和set_symmetric_difference()
 ```
 -功能：
 Iterator set_difference (first1,last1,first2,last2,result);
 将两个升序序列中的第一个序列独有元素提取出来排序放在result，可添加comp函数

 set_symmetric_difference()则是将两个序列中的独有元素都提取出来排序

 -示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::set_difference, std::sort
#include <vector>       // std::vector

int main () {
  int first[] = {5,10,15,20,25};
  int second[] = {50,40,30,20,10};
  std::vector<int> v(10);                     
  std::vector<int>::iterator it;

  std::sort (first,first+5);     //  5 10 15 20 25
  std::sort (second,second+5);   // 10 20 30 40 50

  it=std::set_difference (first, first+5, second, second+5, v.begin());
  //  5 15 25  0  0  0  0  0  0  0
  v.resize(it-v.begin());                      
  //  5 15 25

  std::cout << "The difference has " << (v.size()) << " elements:\n";
  for (it=v.begin(); it!=v.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
The difference has 3 elements:
 5 15 25
 ```

# 五、min/max最大最小函数系列
## 1. min()/max()
```
-功能：
const T& max/min (const T& a, const T& b)
求最大最小值，双目运算，对象可以是整型、字符型、浮点型等

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::min

int main () {
  std::cout << "min(1,2)==" << std::min(1,2) << '\n';
  std::cout << "min('a','z')==" << std::min('a','z') << '\n';
  std::cout << "min(3.14,2.72)==" << std::min(3.14,2.72) << '\n';

  std::cout << "max(1,2)==" << std::max(1,2) << '\n';
  std::cout << "max('a','z')==" << std::max('a','z') << '\n';
  std::cout << "max(3.14,2.73)==" << std::max(3.14,2.73) << '\n';
  return 0;
}
```

## 2. min_element()和max_element()
```
-功能：
Iterator min/max_element ( first, last )；
返回一个序列最小值/最大值元素的指针

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::min_element, std::max_element

bool myfn(int i, int j) { return i<j; }

struct myclass {
  bool operator() (int i,int j) { return i<j; }
} myobj;

int main () {
  int myints[] = {3,7,2,5,6,4,9};

  // using default comparison:
  std::cout << "The smallest element is " << *std::min_element(myints,myints+7) << '\n';
  std::cout << "The largest element is "  << *std::max_element(myints,myints+7) << '\n';

  // using function myfn as comp:
  std::cout << "The smallest element is " << *std::min_element(myints,myints+7,myfn) << '\n';
  std::cout << "The largest element is "  << *std::max_element(myints,myints+7,myfn) << '\n';

  // using object myobj as comp:
  std::cout << "The smallest element is " << *std::min_element(myints,myints+7,myobj) << '\n';
  std::cout << "The largest element is "  << *std::max_element(myints,myints+7,myobj) << '\n';

  return 0;
}


Output:
The smallest element is 2
The largest element is 9
The smallest element is 2
The largest element is 9
The smallest element is 2
The largest element is 9
```

# 六、heap堆操作
```
make_heap 
在容器范围内，就地建堆，保证最大值在所给范围的最前面，其他值的位置不确定

pop_heap 
将堆顶(所给范围的最前面)元素移动到所给范围的最后，并且将新的最大值置于所给范围的最前面

push_heap 
当已建堆的容器范围内有新的元素插入末尾后，应当调用push_heap将该元素插入堆中。

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::make_heap, std::pop_heap, std::push_heap, std::sort_heap
#include <vector>       // std::vector

int main () {
  int myints[] = {10,20,30,5,15};
  std::vector<int> v(myints,myints+5);

  std::make_heap (v.begin(),v.end());
  std::cout << "initial max heap   : " << v.front() << '\n';

  std::pop_heap (v.begin(),v.end()); 
  v.pop_back();
  std::cout << "max heap after pop : " << v.front() << '\n';
  //30处在v.end()处，被抛弃了

  v.push_back(99); std::push_heap (v.begin(),v.end());
  std::cout << "max heap after push: " << v.front() << '\n';

  std::sort_heap (v.begin(),v.end());

  std::cout << "final sorted range :";
  for (unsigned i=0; i<v.size(); i++)
    std::cout << ' ' << v[i];

  std::cout << '\n';

  return 0;
}



Output:
initial max heap   : 30
max heap after pop : 20
max heap after push: 99
final sorted range : 5 10 15 20 99
```