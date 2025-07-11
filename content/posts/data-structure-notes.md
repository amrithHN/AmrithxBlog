---
author: amrithmmh
category:
  - uncategorized
date: "2022-10-10T03:33:07+00:00"
draft: "true"
guid: https://armphibian.wordpress.com/?p=413
title: Data structure notes
url: /

---
important data structures are:

1. array
1. linked list
1. stack
1. queue
1. graphs
1. hashtable
1. tree

Arrays:

Array is a linear data structure and they are not dynamic . however the size can be increased by copying the array to a new array and using the new address.

```
#include<iostream.h>
int main(){
int array[10]={0,1,2,3,4,5,6,7,8,9};
for(int i=0;i<10;i++)
std::cout<<array[i]<<" ";

return 0;
}
```
