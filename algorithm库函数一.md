# 非修改顺序操作
## 1. for_each()
```
-功能：

for_each(begin,end,fun);
搜索的元素作为fun函数的参数执行

-示例：

#include <iostream>     // std::cout
#include <algorithm>    // std::for_each
#include <vector>       // std::vector

void myfunction (int i) {  // function:
  std::cout << ' ' << i;
}

struct myclass {           // function object type:
  void operator() (int i) {std::cout << ' ' << i;}
} myobject;

int main () {
  std::vector<int> myvector;
  myvector.push_back(10);
  myvector.push_back(20);
  myvector.push_back(30);

  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myfunction);
  std::cout << '\n';

  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myobject);
  std::cout << '\n';

  return 0;
}

Output:
myvector contains: 10 20 30
myvector contains: 10 20 30

```

## 2. find()
```
-功能
 iterator find( iterator start, iterator end, const TYPE& val );
 找到值相等的指针；

-示例：
//使用向量和向量指针
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main()
{
     int num_to_find = 9;

   vector< int > v1;
   for( int i = 0; i < 10; i++ ) {
     v1.push_back(i);
   }

   vector<int>::iterator result;    //定义向量指针
   result = find( v1.begin(), v1.end(), num_to_find );

   if( result == v1.end() ) {
     cout << "Did not find any element matching " << num_to_find << endl;
   }

   else {
     cout << "Found a matching element: " << *result << endl;
   }
    return 0;
}


//使用数组和指针
#include <iostream>
#include <algorithm>
using namespace std;
int main()
{
  int myints[] = { 10, 20, 30, 40 };
  int * p;

  p = std::find (myints, myints+4, 30);
  if (p != myints+4)
    std::cout << "Element found in myints: " << *p << '\n';
  else
    std::cout << "Element not found in myints\n";
  return 0;
}



//使用string字符串
#include <iostream>
#include <algorithm>
#include <string.h>
using namespace std;
int main()
{
     string s("1a2b3c4d5e6f7g8h9i1a2b3c4d5e6f7g8ha9i");

     string::size_type position;
     position=s.find("b3");
     if(position!=s.npos)  
     //npos表示没有找到；
         cout<<"position is : " << position << endl;
     else
         cout<<"position is not find:"<<endl;
    return 0;
}

output:
position is : 3



#include <iostream>
#include <algorithm>
using namespace std;
int main()
{
     string s("1a2b3c4d5e6f7g8h9i1a2b3c4d5e6f7g8ha9ib3");
     string::size_type position;
     position=s.find("b3");
     if(position==s.npos)
        cout<<"position is not find:"<<endl;
     else
     while(position!=s.npos)
     {
         cout<<"position is : " << position << endl;
         position=s.find("b3",position+1);
     }
    return 0;
}



output：
position is : 3
position is : 21
position is : 37




-------拓展-------
position=s.find("b3"，5);表示从下标5开始查找
position=s.rfind("b3");表示反向查找最后出现的位置
flag="acb12389efgxyz789"; 
position=flag.find_first_not_of (s); 
查找flag 中与s 第一个不匹配的位置
int find_first_of(char c, int start = 0):
查找字符串中第1个出现的c,由位置start开始。
如果有匹配，则返回匹配位置；否则，返回-1.默认情况下，start为0

```
## 3.find_if()或find_if_not
```
-功能：
它接收一个函数对象的参数作为参数，返回符合的第一个指针
find_if_not则返回第一个不符合条件的指针

-示例：
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

class EventIsIn1997 {
public:
    bool operator () (string& EventRecord)
     {
        return EventRecord.substr(11,4)=="1997";
     }
};

int main (void) {
    vector<string> Events;
    Events.push_back("07 January 1995 Draft plan of house prepared");
    Events.push_back("07 February 1996 Detailed plan of house prepared");
    Events.push_back("10 January 1997 Client agrees to job");
    Events.push_back("15 January 1997 Builder starts work on bedroom");
    Events.push_back("30 April 1997 Builder finishes work");

    vector<string>::iterator EventIterator = find_if (Events.begin(), Events.end(), EventIsIn1997());
    if (EventIterator==Events.end()) {
        cout << "Event not found in list" << endl;
    }
    else {
        cout << *EventIterator << endl;
    }
}
```

## 4. adjacent_find()
```
-功能：
it = adjacent_find (myvector.begin(), myvector.end());
it = adjacent_find (myvector.begin(), myvector.end(), myfunction);
用来查找连续两个相等的或者符合方法的,返回第一个数的指针

-示例：
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

bool myfunction (int i, int j)
{
    return (i==j*2);
}
int main()
{
    int myints[] = {10,20,30,30,40,20,10,20};

    vector<int> myvector (myints,myints+8);
    vector<int>::iterator it;

    it = adjacent_find (myvector.begin(), myvector.end());

    if (it!=myvector.end())
        cout << "the first consecutive repeated elements are: " << *it << endl;

    it = adjacent_find (myvector.begin(), myvector.end(), myfunction);

    if (it!=myvector.end())
        cout << "the second consecutive repeated elements are: " << *it << endl;

    return 0;
}


output：
the first consecutive repeated elements are: 30
the second consecutive repeated elements are: 40

```

## 5. count()或count_if()
```
-功能:
count(ivec.begin() , ivec.end() , searchValue)；
count_if(ivec.begin() , ivec.end() , myfunction)；
用来查找与searchValue相等的或者符合条件的,返回个数

-示例：
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int main()
{
    int ival , searchValue;
    vector<int> ivec;
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        cin>>ival;
        ivec.push_back(ival);
    }
    cin>>searchValue;
    cout<<count(ivec.begin() , ivec.end() , searchValue)
        <<"  elements in the vector have value "
        <<searchValue<<endl;

    return 0;
}



--------------------------------------------

#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
struct student
{
    string name;
    int score;
};
bool compare(student a)
{
    return 90<a.score;
}
int main()
{
    int n;
    cin>>n;
    vector<student> V;
    for(int i=0;i<n;i++)
    {
        student temp;
        cin>>temp.name>>temp.score;
        V.push_back(temp);
    }
    cout<<count_if(V.begin(),V.end(),compare)<<endl;
    return 0;
}

```

## 6. mismatch()
```
-功能：
mismatch(first1,last1,first2,compare)
/first1为第一个容器的首迭代器，last1为第一个容器的末迭代器，first2为第二个容器的首指针，compare为比较函数（可省略）。
求出两个序列相异的或不满足比较函数的第一个元素。

-示例：
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main()
{
    vector <int> A,B;
    int n,m;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        int t;
        cin>>t;
        A.push_back(t);
    }
    cin>>m;
    for(int i=0;i<m;i++)
    {
        int t;
        cin>>t;
        B.push_back(t);
    }
    pair <vector <int> ::iterator ,vector <int> ::iterator > bound;
    bound=mismatch(A.begin(),A.end(),B.begin());
    cout<<*(bound.first)<<" "<<*(bound.second)<<endl;
    return 0;
}


output：
5
1 2 3 4 5
6
1 2 3 5 4 6
4 5

-------------------------------

/********还支持不同类型的序列比较**************/
#include <iostream>     // std::cout
#include <algorithm>    // std::mismatch
#include <vector>       // std::vector

bool mypredicate (int i, int j) {
  return (i==j);
}

int main () {
  std::vector<int> myvector;
  for (int i=1; i<6; i++) myvector.push_back (i*10);
   // myvector: 10 20 30 40 50

  int myints[] = {10,20,80,320,1024};              
  //   myints: 10 20 80 320 1024

  std::pair<std::vector<int>::iterator,int*> mypair;

  mypair = std::mismatch (myvector.begin(), myvector.end(), myints);
  std::cout << "First mismatching elements: " << *mypair.first;
  std::cout << " and " << *mypair.second << '\n';

  ++mypair.first; ++mypair.second;

  mypair = std::mismatch (mypair.first, myvector.end(), mypair.second, mypredicate);
  std::cout << "Second mismatching elements: " << *mypair.first;
  std::cout << " and " << *mypair.second << '\n';

  return 0;
}

Output:
First mismatching elements: 30 and 80
Second mismatching elements: 40 and 320

```

## 7. equal（）
```
-功能：
bool equal (  first1,  last1, first2，compare )；
判断两个序列是否相等或者是否满足比较函数，相等返回true；
//支持不同序列类型

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::equal
#include <vector>       // std::vector

bool mypredicate (int i, int j) {
  return (i==j);
}

int main () {
  int myints[] = {20,40,60,80,100};               //   myints: 20 40 60 80 100
  std::vector<int>myvector (myints,myints+5);     // myvector: 20 40 60 80 100

  if ( std::equal (myvector.begin(), myvector.end(), myints) )
    std::cout << "The contents of both sequences are equal.\n";
  else
    std::cout << "The contents of both sequences differ.\n";

  myvector[3]=81;                                

  if ( std::equal (myvector.begin(), myvector.end(), myints, mypredicate) )
    std::cout << "The contents of both sequences are equal.\n";
  else
    std::cout << "The contents of both sequences differ.\n";

  return 0;
}


Output:
The contents of both sequences are equal.
The contents of both sequence differ.

```

## 8. is_permutation()
```
-功能：
bool is_permutation (foo.begin(), foo.end(), bar.begin())
两个序列含有元素相同，return true；

-示例：

#include <iostream>     // std::cout
#include <algorithm>    // std::is_permutation
#include <array>        // std::array

int main () {
  std::array<int,5> foo = {1,2,3,4,5};
  std::array<int,5> bar = {3,1,4,5,2};

  if ( std::is_permutation (foo.begin(), foo.end(), bar.begin()) )
    std::cout << "foo and bar contain the same elements.\n";

  return 0;
}

Output:
foo and bar contain the same elements

-提醒：这个我运行时会出错，貌似要C++11以上的编译器才可以
```

## 9. search()
```
-功能：
Iterator search (  first1, last1, first2, last2，compare(可省略))
查找序列2在序列1的位置，返回指针


#include <iostream>     // std::cout
#include <algorithm>    // std::search
#include <vector>       // std::vector

bool mypredicate (int i, int j) {
  return (i==j);
}

int main () {
  std::vector<int> haystack;

  //  haystack: 10 20 30 40 50 60 70 80 90
  for (int i=1; i<10; i++) haystack.push_back(i*10);

  // using default comparison:
  int needle1[] = {40,50,60,70};
  std::vector<int>::iterator it;
  it = std::search (haystack.begin(), haystack.end(), needle1, needle1+4);

  if (it!=haystack.end())
    std::cout << "needle1 found at position " << (it-haystack.begin()) << '\n';
  else
    std::cout << "needle1 not found\n";

  // using predicate comparison:
  int needle2[] = {20,30,50};
  it = std::search (haystack.begin(), haystack.end(), needle2, needle2+3, mypredicate);

  if (it!=haystack.end())
    std::cout << "needle2 found at position " << (it-haystack.begin()) << '\n';
  else
    std::cout << "needle2 not found\n";

  return 0;
}

Output:
needle1 found at position 3
needle2 not found

```

## 10. search_n()
```
-功能：
Iterator search_n ( first,  last, Size count, const T& val)；
Iterator search_n ( first, last, Size count, const T& val，compare)；
寻找首次连续出现count次val的位置；
寻找首次连续count次满足函数的位置，val作为第二个参数

-示例：
#include <iostream>     // std::cout
#include <algorithm>    // std::search_n
#include <vector>       // std::vector

bool mypredicate (int i, int j) {
  return (i==j);
}

int main () {
  int myints[]={10,20,30,30,20,10,10,20};
  std::vector<int> myvector (myints,myints+8);

  std::vector<int>::iterator it;

  // using default comparison:
  it = std::search_n (myvector.begin(), myvector.end(), 2, 30);

  if (it!=myvector.end())
    std::cout << "two 30s found at position " << (it-myvector.begin()) << '\n';
  else
    std::cout << "match not found\n";

  // using predicate comparison:
  it = std::search_n (myvector.begin(), myvector.end(), 2, 10, mypredicate);

  if (it!=myvector.end())
    std::cout << "two 10s found at position " << int(it-myvector.begin()) << '\n';
  else
    std::cout << "match not found\n";

  return 0;
}

Output:
Two 30s found at position 2
Two 10s found at position 5
```



