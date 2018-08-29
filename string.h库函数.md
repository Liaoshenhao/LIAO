# 一、拷贝函数
**对于memory系列很多都用到&符号**
## 1.memcpy()和memmove()
```
-功能：

void * memcpy ( void * destination, const void * source, size );
将source开始的size个字符拷贝到destination开始的字符串；
这是字节拷贝，可以拷贝的类型很多，不同于strcpy的字符拷贝

void * memmove ( void * destination, const void * source, size_t num );
作用与memcpy相同，区别仅在于如果目标地址和原地址有重叠，即在同一字符串中时，应使用memmove
------------------------

  char str[] = "memmove can be very useful.";

  memmove (str+20,str+15,11);

  Output:
     memmove can be very very useful.

-------------------------

-示例：
#include <stdio.h>
#include <string.h>

struct {
  char name[40];
  int age;
} person, person_copy;

int main ()
{
  char myname[] = "Pierre de Fermat";

  /* using memcpy to copy string: */
  memcpy ( person.name, myname, strlen(myname)+1 );
  person.age = 46;

  /* using memcpy to copy structure: */
  memcpy ( &person_copy, &person, sizeof(person) );

  printf ("person_copy: %s, %d \n", person_copy.name, person_copy.age );

  return 0;
}


Output:
person_copy: Pierre de Fermat, 46 


注意： char型字符串拷贝的时候应包含'\0', 所以strlen(myname)+1；
如果用sizeof(myname)则不用+1，包含了'\0'内存单元
memcpy ( &person_copy, &person, sizeof(person) );
&是取地址符，将person的字节全部拷贝，对于string类型也是&
memcmp(&buffer1, &buffer2, sizeof(buffer1));
```

# 2. strcpy()
```
-功能：
char * strcpy ( char * destination, const char * source );
将source开始的char型字符拷贝到destination
注意一定时char型字符串，需要将'\0'拷贝

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str1[]="Sample string";
  char str2[40];
  char str3[40];
  strcpy (str2,str1);
  strcpy (str3,"copy successful");
  printf ("str1: %s\nstr2: %s\nstr3: %s\n",str1,str2,str3);
  return 0;
}


Output:

str1: Sample string
str2: Sample string
str3: copy successful
```

# 3. strncpy()
```
-功能：
char * strncpy ( char * destination, const char * source, size);
与memcpy很相像，都是拷贝N个字节到destination地址；
不同的是strncpy针对的是char型字符串

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str1[]= "To be or not to be";
  char str2[40];
  char str3[40];

  strncpy ( str2, str1, sizeof(str1) );

  strncpy ( str3, str2, 5 );
  str3[5] = '\0';  

  puts (str1);
  puts (str2);
  puts (str3);

  return 0;
}



Output:
To be or not to be
To be or not to be
To be 
```

## 二、连接函数
# 1.strcat()
```
-功能：
char * strcat ( char * destination, const char * source );
将source接在destination地址后面，只针对char类型

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[80];
  strcpy (str,"these ");
  strcat (str,"strings ");
  strcat (str,"are ");
  strcat (str,"concatenated.");
  puts (str);
  return 0;
}


Output:
these strings are concatenated.
```

# 2. strncat()
```
-功能：
char * strncat ( char * destination, const char * source, size_t num );
将source开始的n个字节拷贝到des，注意char类型；
拷贝完末尾自动添加'\0',不用手动添加

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str1[20];
  char str2[20];
  strcpy (str1,"To be ");
  strcpy (str2,"or not to be");
  strncat (str1, str2, 6);
  puts (str1);
  return 0;
}


Output:
To be or not
```

---
**如果是string类型，可以直接使用+号进行连接**

# 三、比较函数
## 1. memcmp()
```
-功能：
int memcmp ( const void * ptr1, const void * ptr2, size_t num );
将ptr1开始的和ptr2开始的N个字节进行比较；
返回  >0   ptr1大， <0   ptr2大； 0  则相等

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char buffer1[] = "DWgaOtP12df0";
  char buffer2[] = "DWGAOTP12DF0";

  int n;

  n=memcmp ( buffer1, buffer2, sizeof(buffer1) );

  if (n>0) printf ("'%s' is greater than '%s'.\n",buffer1,buffer2);
  else if (n<0) printf ("'%s' is less than '%s'.\n",buffer1,buffer2);
  else printf ("'%s' is the same as '%s'.\n",buffer1,buffer2);

  return 0;
}


Output:
'DWgaOtP12df0' is greater than 'DWGAOTP12DF0'
```

# 2. strcmp()和strncmp()
```
-功能：
int strcmp ( const char * str1, const char * str2 );
针对char类型的字符串比较；

int strncmp ( const char * str1, const char * str2, size_t);
针对char类型的前N个字节字符串比较；

返回：<0  str2大； >0  str1大 ；  0   相等；

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char key[] = "apple";
  char buffer[80];
  do {
     printf ("Guess my favorite fruit? ");
     fflush (stdout);
     scanf ("%79s",buffer);
  } while (strcmp (key,buffer) != 0);
  puts ("Correct answer!");
  return 0;
}


Output:
Guess my favourite fruit? orange
Guess my favourite fruit? apple
Correct answer!
```

## 四、字符查找函数
# 1. memchr()
```
-功能：
void * memchr（const void* ptr ，int value，size_t  num）；
查找ptr开始的num个字节里value的位置，返回的void指针

注意：value所要查找的字符，虽然写的int 类型但一般是直接传一个字符：比如‘A’，‘R’等等
如果没有找到符合要求的字符，则返回NULL；

-示例：
#include <iostream>
#include<string.h>
using namespace std;
int main() 
{ 
char a[]="I love WUXI";
char *p;
p=(char *)memchr(a,'X',strlen(a));
if(p!=NULL){
cout<<"找到了X它是第"<<p-a+1<<"个字符" ;
}
else cout<<"没有找到X" ;
return 0;

}

请特别注意p=(char *)memchr(a,'X',strlen(a));
因为memchr返回的是void*，需要强制转换一下指针类型。
```

# 2. strchr()和strrchr()
```
-功能：
char * strchr ( char * str, int character );
针对char类型的字符查找函数，返回位置指针

char * strrchr ( char * str, int character );
针对char类型的反向字符查找函数，即从后面开始，返回位置指针

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] = "This is a sample string";
  char * pch;
  printf ("Looking for the 's' character in \"%s\"...\n",str);
  pch=strchr(str,'s');
  while (pch!=NULL)
  {
    printf ("found at %d\n",pch-str+1);
    pch=strchr(pch+1,'s');
  }
  return 0;
}


Output:
Looking for the 's' character in "This is a sample string"...
found at 4
found at 7
found at 11
found at 18
```

# 3. strcspn()和strspn()
```
-功能：
size_t strcspn ( const char * str1, const char * str2 );
顺序在字符串s1中搜寻与s2中字符的第一个相同字符，包括结束符'\0'，返回下标；
也就是用来计算字符串 str1 中连续有几个字符都不属于字符串str2，返回数目，完全不同则返回strlen（str1）

------------------------------------

size_t strspn( const char * str1, const char * str2 );
用来计算字符串 str1 中连续有几个字符都属于字符串 str2，返回数量
如果 str1 所包含的字符都属于str2，那么返回 str1 的长度；如果 str1 的第一个字符不属于 str2，那么返回 0。


-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] = "fcba73";
  char keys[] = "1234567890";
  int i;
  i = strcspn (str,keys);
  printf ("The first number in str is at position %d.\n",i+1);
  return 0;
}


Output:
The first number in str is at position 5


----------------------------------


#include <stdio.h>
#include <string.h>

int main ()
{
  int i;
  char strtext[] = "129th";
  char cset[] = "1234567890";

  i = strspn (strtext,cset);
  printf ("The initial number has %d digits.\n",i);
  return 0;
}


Output:
The initial number has 3 digits.

```

# 4.strpbrk()
和上一个函数功能相同，一个返回下标，一个返回位置指针
```
-功能：
 char * strpbrk (char * str1, const char * str2 );
 在字符串s1中寻找字符串s2中任何一个字符相匹配的第一个字符的位置，空字符NULL不包括在内。

 返回指向s1中第一个相匹配的字符的指针，如果没有匹配字符则返回空指针NULL。

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] = "This is a sample string";
  char key[] = "aeiou";
  char * pch;
  printf ("Vowels in '%s': ",str);
  pch = strpbrk (str, key);
  while (pch != NULL)
  {
    printf ("%c " , *pch);
    pch = strpbrk (pch+1,key);
  }
  printf ("\n");
  return 0;
}


Output:
Vowels in 'This is a sample string': i i a a e i
```

# 5. strstr()
```
-功能：
char * strstr (char * str1, const char * str2 );
从字符串str1中查找是否有字符串str2， 如果有返回str1中出现首地址的指针，如果没有，返回null。

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] ="This is a simple string";
  char * pch;
  pch = strstr (str,"simple");
  strncpy (pch,"sample",6);
  puts (str);
  return 0;
}


Output:
This is a sample string
```

# 6. strtok()
```
-功能：
char * strtok ( char * str, const char * delimiters );
按关键词分割字符串函数，delimiters为关键词；
返回分割的字符串地址，若没有了则返回NULL
第二次开始分割时使用strtok ( NULL, const char * delimiters );

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] ="- This, a sample string.";
  char * pch;
  printf ("Splitting string \"%s\" into tokens:\n",str);
  pch = strtok (str," ,.-");
  while (pch != NULL)
  {
    printf ("%s\n",pch);
    pch = strtok (NULL, " ,.-");
  }
  return 0;
}


Output:
Splitting string "- This, a sample string." into tokens:
This
a
sample
string
```

# 五、其他函数
## 1.memset()
```
-功能：
void * memset ( void * ptr, int value, size_t num );
用value在一段num内存块中填充某个给定的值，一般用来清零

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] = "almost every programmer should know memset!";
  memset (str,'-',6);
  puts (str);
  return 0;
}


Output:
------ every programmer should know memset!
```
 ## 2. strlen()
```
-功能：
size_t strlen ( const char * str );
计算char型字符串长度，不包括'\0'

-示例：
#include <stdio.h>
#include <string.h>

int main ()
{
  char szInput[256];
  printf ("Enter a sentence: ");
  gets (szInput);
  printf ("The sentence entered is %u characters long.\n",(unsigned)strlen(szInput));
  return 0;
}


Output:
Enter sentence: just testing
The sentence entered is 12 characters long.
```