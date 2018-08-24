# 修改顺序操作
## 1. copy()和copy_n()
```
-功能：
 copy ( first,  last,  begin);
 first和last 是需要复制的起始和终止地址，begin是目标起始地址
 copy_n(first,size,begin);
 firs 是需要复制的起始地址，begin是目标起始地址,size是复制大小

 -示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::copy
#include <vector>       // std::vector

int main () {
  int myints[]={10,20,30,40,50,60,70};
  std::vector<int> myvector (7);

  std::copy ( myints, myints+7, myvector.begin() );

  // std::copy_n ( myints, 7, myvector.begin() );

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it = myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;

  std::cout << '\n';

  return 0;
}


Output:
myvector contains: 10 20 30 40 50 60 70

----------------------
注意：
    vector<int> u(10,100);
    vector<int> v;
    copy(u.begin(),u.end(),v.begin());

    for(vector<int>::iterator it=v.begin();it!=v.end();it++)
    {
        cout<<*it<<ends;
    }
运行错误！

 vector<int> v；定义时定义时没有分配空间，copy不成功。应改为vector<int> v(u.size());
 或者使用iterator适配器copy(u.begin(),u.end(),back_inserter(v)); 

 建议使用第二种
 ```

## 2. copy_if()
```
-功能：
 copy_if ( first,  last, result,  pred)
前三项和copy一样，最后的是条件函数；
满足条件的则拷贝，返回最后result最后一项的下一地址

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::copy_if, std::distance
#include <vector>       // std::vector

int main () {
  std::vector<int> foo = {25,15,5,-5,-15};
  std::vector<int> bar (foo.size());

  // copy only positive numbers:
  auto it = std::copy_if (foo.begin(), foo.end(), bar.begin(), [](int i){return !(i<0);} );
  bar.resize(std::distance(bar.begin(),it));  
  std::cout << "bar contains:";
  for (int& x: bar) std::cout << ' ' << x;
  std::cout << '\n';

  return 0;
}


Output:
bar contains: 25 15 5

提醒：C++98编译错误
```

## 3. copy_backward()
```
-功能：
copy_backward ( myvector.begin(), myvector.begin()+5, myvector.end() );
将区间内元素依次反向拷贝到目标函数末端，即元素搬迁

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::copy_backward
#include <vector>       // std::vector

int main () {
  std::vector<int> myvector;

  // set some values:
  for (int i=1; i<=5; i++)
    myvector.push_back(i*10);          // myvector: 10 20 30 40 50

  myvector.resize(myvector.size()+3);  // allocate space for 3 more elements

  std::copy_backward ( myvector.begin(), myvector.begin()+5, myvector.end() );

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
myvector contains: 10 20 30 10 20 30 40 50
```

##  4. move()和move_backward()
```
-功能：
基本和copy一样，只是move针对于string类型，但只支持C++11以上版本

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::move_backward
#include <string>       // std::string

int main () {
  std::string elems[10] = {"air","water","fire","earth"};

  // insert new element at the beginning:
  std::move_backward (elems,elems+4,elems+5);
  elems[0]="ether";

  std::cout << "elems contains:";
  for (int i=0; i<10; ++i)
    std::cout << " [" << elems[i] << "]";
  std::cout << '\n';

  return 0;
}


Output:
elems contains: [ether] [air] [water] [fire] [earth] [] [] [] [] []
```

## 5. swap()
```
-功能：
swap ( T& a, T& b )
交换两个变量的值，无返回

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::swap
#include <vector>       // std::vector

int main () {

  int x=10, y=20;                              // x:10 y:20
  std::swap(x,y);                              // x:20 y:10

  std::vector<int> foo (4,x), bar (6,y);       // foo:4x20 bar:6x10
  std::swap(foo,bar);                          // foo:6x10 bar:4x20

  std::cout << "foo contains:";
  for (std::vector<int>::iterator it=foo.begin(); it!=foo.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
foo contains: 10 10 10 10 10 10
```

## 6. swap_ranges()
```
-功能：
Iterator swap_ranges ( first1, last1, first2)；
指定区间内元素和first2开始的元素等量交换

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::swap_ranges
#include <vector>       // std::vector

int main () {
  std::vector<int> foo (5,10);        // foo: 10 10 10 10 10
  std::vector<int> bar (5,33);        // bar: 33 33 33 33 33

  std::swap_ranges(foo.begin()+1, foo.end()-1, bar.begin());

  // print out results of swap:
  std::cout << "foo contains:";
  for (std::vector<int>::iterator it=foo.begin(); it!=foo.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  std::cout << "bar contains:";
  for (std::vector<int>::iterator it=bar.begin(); it!=bar.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
foo contains: 10 33 33 33 10
bar contains: 10 10 10 33 33
```

## 7. iter_swap()
```
-功能：
void iter_swap (ForwardIterator1 a, ForwardIterator2 b)
{
  swap (*a, *b);
}
交换两个指针所指元素；

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::iter_swap
#include <vector>       // std::vector

int main () {

  int myints[]={10,20,30,40,50 };     //   myints:  10  20  30  40  50
  std::vector<int> myvector (4,99);    // myvector:  99  99  99  99

  std::iter_swap(myints,myvector.begin());     //   myints: [99] 20  30  40  50
                            // myvector: [10] 99  99  99

  std::iter_swap(myints+3,myvector.begin()+2); //   myints:  99  20  30 [99] 50
                           // myvector:  10  99 [40] 99

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
  

Output:
myvector contains: 10 99 40 99
```

## 8. transform()
```
-功能：
transform(first,last,result,op);
//first是容器的首迭代器，last为容器的末迭代器
result为存放结果的容器，op为要进行操作的一元函数对象或sturct、class。
**将区间元素进行op操作后存放到result（大小需要resize（））**

transform(first1,last1,first2,result,binary_op);
//first1是第一个容器的首迭代 器，last1为第一个容器的末迭代器，
first2为第二个容器的首迭代器，result为存放结果的容器
binary_op为要进行操作的二元函数 对象或sturct、class。
**将A和B两个区间的元素进行操作，结果存放到result**

注意：第二个重载版本必须要保证两个容器的元素个数相等才行，否则会抛出异常。

-示例：
#include <iostream>
#include <algorithm>
using namespace std;
char op(char ch)
{
      if(ch>='A'&&ch<='Z')
        return ch+32;
      else
        return ch;
}
int main()
{
     string first,second;
     cin>>first;
     second.resize(first.size());
     transform(first.begin(),first.end(),second.begin(),op);
     cout<<second<<endl;
     return 0;
}


output：
ABcd
abcd

--------------------------------

#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
void print(int &elem){cout<<elem<<" ";}
int op(int a,int b){return a*b;}
int main()
{
    vector <int> A,B,SUM;
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        int t;
        cin>>t;
        A.push_back(t);
    }
    for(int i=0;i<n;i++)
    {
        int t;
        cin>>t;
        B.push_back(t);
    }
    SUM.resize(n);
    transform(A.begin(),A.end(),B.begin(),SUM.begin(),op);
    for_each(SUM.begin(),SUM.end(),print);
    return 0;
}


output：
3
1 2 3
4 5 6
4 10 18
```

## 9. replace()和replace_if()
```
-功能：
replace ( first, last，const T& old_value, const T& new_value)；
将区间内元素的值为old的全部替换成new

replace_if (first, last, pred, const T& new_value);
将满足pred函数条件的元素全部替换成new值


-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::replace
#include <vector>       // std::vector

int main () {
  int myints[] = { 10, 20, 30, 30, 20, 10, 10, 20 };
  std::vector<int> myvector (myints, myints+8);            // 10 20 30 30 20 10 10 20

  std::replace (myvector.begin(), myvector.end(), 20, 99); // 10 99 30 30 99 10 10 99

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}



Output:
myvector contains: 10 99 30 30 99 10 10 99

-提醒：直接用数组也可以，但是数组长度需要sizeof(myints)/sizeof(int)
不能直接用strlen或者length


---------------------------

#include <iostream>     // std::cout
#include <algorithm>    // std::replace_if
#include <vector>       // std::vector

bool IsOdd (int i) { return ((i%2)==1); }

int main () {
  std::vector<int> myvector;

  // set some values:
  for (int i=1; i<10; i++) myvector.push_back(i);               // 1 2 3 4 5 6 7 8 9

  std::replace_if (myvector.begin(), myvector.end(), IsOdd, 0); // 0 2 0 4 0 6 0 8 0

  std::cout << "myvector contains:";
  for (std::vector<int>::iterator it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}


Output:
myvector contains: 0 2 0 4 0 6 0 8 0
```

## 10. replace_copy()和replace_copy_if()
```
-功能：
replace_copy (first,last, result, const T& old_value, const T& new_value)；
就是replace和copy的结合，替换之后存放到result

replace_copy_if (first, last,result, pred,const T& new_value)；
将满足条件的替换成新值存放到result


-示例：
int myints[] = { 10, 20, 30, 30, 20, 10, 10, 20 };
std::vector<int> myvector (8);
std::replace_copy (myints, myints+8, myvector.begin(), 20, 99);


-----------------------------

 std::vector<int> foo,bar;
 for (int i=1; i<10; i++) foo.push_back(i);         
    // 1 2 3 4 5 6 7 8 9
 bar.resize(foo.size());  
 std::replace_copy_if (foo.begin(), foo.end(), bar.begin(), IsOdd, 0);
    // 0 2 0 4 0 6 0 8 0
```








