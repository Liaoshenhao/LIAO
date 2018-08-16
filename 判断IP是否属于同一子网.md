# 判断网络号是否相同
## 题目：
    -子网掩码是用来判断任意两台计算机IP地址是否属于同一网络，子网掩码与IP地址结构相同，是32位二进制数，其中网络号部分全为1，主机号部分为"0", 利用子网掩码可以判断两台主机是否在同一个子网中，若两台主机的IP地址分别与它们子网掩码"与"后的结果相同，则说明两台主机在同一个子网中。
    -完善下面的函数，判断两台计算机IP地址是处于同一子网络，要求：
     输入参数：
     char *mask_addr, 格式为 255.255.255.0
     char *ip1_addr ，格式为 192.168.0.1
     char *ip2_addr , 格式为 192.168.0.1

## C++代码如下
```
#include <iostream>
#include <string.h>
#include <stdlib.h>

/***********将IP字符串转换成纯数字**************/
int getIntArray(char *addr,int *array_addr)
{
     char *taken = NULL; 
     if (addr == NULL)
     {
        return 2;
     } 
     std::cout << "------start-------" << std::endl;
     taken = strtok(addr,".");         //关键字分割字符串
     int i = 0; 
     while(taken != NULL) 
     { 
       array_addr[i] = atoi(taken);        //获取的字符转换成整型
       if(array_addr[i] > 255 || array_addr[i] < 0) 
        { 
           return 2; 
        } 
       i++; 
       taken = strtok(NULL,".");          //获取下一分割字符
     } 
     return 0;
}
 
   /******判断IP地址是否是同一子网******/
int checkNetSegment (char *mask_addr, char *ip1_addr, char *ip2_addr)
{
      char *taken = NULL;
      int aMask[4] = {0}; 
      int aip1[4] = {0}; 
      int aip2[4] = {0}; 
      int ret = 0;  

      ret = getIntArray(mask_addr,aMask);
      if (ret != 0)
      {
         std::cout << " invalid mask!" << std::endl;
         return 2;
      }
      ret = getIntArray(ip1_addr,aip1);
      if (ret != 0)
      {
        std::cout << " invalid ip1!" << std::endl;
         return 2;
      }
     ret = getIntArray(ip2_addr,aip2);
     if (ret != 0)
     {
       std::cout << " invalid ip2!" << std::endl;
       return 2;
     }
     int i = 0; 
     for (int i = 0; i < 4; i ++)
     {
       if ((aip1[i]&aMask[i]) == (aip2[i]&aMask[i])){
       continue;
     }
     std::cout << " ips are not in the same netsegment" << std::endl; 
     return 1;
    }
     std::cout << " ips are in the same netsegment" << std::endl; 
     return 0;
 }

int main()
{
     char mask[] = "255.255.255.0";
     char ip1 [] = "192.168.1.120";
     char ip2 [] = "192.168.1.128";
     int ret = checkNetSegment (mask,ip1,ip2);
     return 0;
}

```