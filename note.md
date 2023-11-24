# important methods
## others
- 四舍五入
```C
double x=1.34;
int a=(int)(x+0.5);//加0.5后类型转化
```

## 排序sort
### inserting sort
- 基础代码
  - 递归写法 *从大到小*
   ```c
  void sort(int begin,int end,int a[]){
    if(begin<end){
      sort (begin,end-1,a);
      int x=a[end];//the element need to de inserted
      int i;
      for(i=end-1;i>=begin;i--){
        if(a[i]>x){
          a[i+1]=a[i];
        }else{
          a[i+1]=x;
          break;
        }
      }
      //保证当x最小的情况可行
      if(i==begin-1){
        a[begin]=x;
      }
    }   
  }
  ```

   - 循环写法 *从小到大*
  ```c
  int j;
  for(int i=1;i<=end;i++){//i表示已经排好序的元素
    int x=a[i]// the element to be inserted
    for(j=i-1;j>=0;j--){
      if(a[j]>x){
        a[j+1]=a[j];
      }else{
        a[j+1]=x;
        break;
      }
    }
    if(j==-1){
      a[0]=x;
    }
  }
  ```

- 时间效率为O(n^2)  
### hash sort 哈希排序
### selection sort 选择排序
- 每次找到最大的，放在最后一位

```c
for(int j=1;j<len;j++){
  int index=0;
  for(int i=1;i<=len-j;i++){
// find the max
    if(a[i]>a[index]){
      index=i;
    }
// put the max to the end of the array
  int t=a[index];a[index]=a[len-j];a[len-j]=t;
  }
}
```

- 时间效率为O（n^2）

### 冒泡排序
- 基础代码

  ```C
  for(int j=1;j<=len;j++){//j表示已经排好的数据
      for(int i=1;i<len-j+1;i++){
          if(a[i]<a[i-1]){
                int t=a[i];
                a[i]=a[i-1];
                a[i-1]=t;
            }//最大的数依次冒泡
        }
    } 
  ```
- 优化
  ```C
  int lastloc=len-1;//lastloc之后的数组已经排好了
  int loc=-1;
  for(int j=0;j<len;j++){
    loc=-1;
    for(int i=1;i<=lastloc;i++){
        if(a[i]<a[i-1]){
            int t=a[i];
            a[i]=a[i-1];
            a[i-1]=t;
            loc=i;//记下最后一次交换的下标，其之后不在需要计算
        }
    }
    lastloc=loc;
    if(loc==-1){//loc==-1则已经排好
        break;
    }        
  } 
  ```
  
- 效率为O(n^2)
### merge sort 归并搜索
- 基础函数merge，将两个数组排好序发在剩下的数组里

```C
//数组a的长度为len1,数组b的长度为len2，将a,b数组排好序合并入c数组（a,b已排好序）
void merge(int a[],int b[],int c[],int len1,int len2){
  int i=0,j=0,k=0;
  while(i<len1||j<len2){
    if(j==len2||i<len1&&a[i]<b[j]){
      c[k++]=a[i++];
    }else{
      c[k++]=b[j++];
    }
  }
}
```

- 递归写法
```c
void merge(int a[],int begin, int mid,int end ){
  int i=begin,j=mid+1,k=0;
  //定义好len很关键，方便算出n中元素数量
  int len=end-begin+1;
  int b[len];
  while(k<=len-1){
    if(j>end||i<=mid&&a[i]<a[j]){
      b[k++]=a[i++];
    }else{
      b[k++]=a[j++];
    }
  }
  //将b中排好序的拷贝回a
  for(int m=0;m<len;m++){
    a[begin+m]=b[m];
  }
}
void sort(int a[],int begin,int end){
    if(begin<end){
        int mid=(begin+end)/2;
      //这里只能用mid+1,否则会栈溢出  
      //split
        sort(a,begin,mid);
        sort(a,mid+1,end);
      //merge  
        merge(a,begin,mid,end);
      }
}
```
- 迭代写法
    - **思想是从小到大，从两个一组逐步扩大**
```c
void merge(int a[],int begin, int mid,int end ){
  int i=begin,j=mid+1,k=0;
  //定义好len很关键，方便算出n中元素数量
  int len=end-begin+1;
  int b[len];
  while(k<=len-1){
    if(j>end||i<=mid&&a[i]<a[j]){
      b[k++]=a[i++];
    }else{
      b[k++]=a[j++];
    }
  }
  //将b中排好序的拷贝回a
  for(int m=0;m<len;m++){
    a[begin+m]=b[m];
  }
}
int main(){
  for(int k=1;k<len;k*=2){//k表示已经排好的个数
    for(int i=0;i<len;i=i+2*k){
//传送对应的begin mid end 下标，注意不要出现重叠
//min是为了防止下标越界
      merge(a,i,min(i+k-1,len-1),min(i+2*k-1,len-1));
    }
  }
}
```

- 时间复杂度为O(n*logn)

### quick sort快排
- **partition**
     - 确定一个哨兵 pivot
     - 使左边的都比pivot小，右边都比pivot大

- lumoto partition
```c
int i=begin,j=begin;
int pivot=a[end];
for(j=begin;j<=end;j++){
  if(a[j]<pivot){
//保证i与j之间的数比pivot小
    swap(a[i],a[j]);
    i++;
  }
}
swap(a[i],a[end]);
return i;
```

- hoare partition
```c
int i=begin-1;
int j=end+1;
int pivot=a[(end+begin)/2];
while(i<j){
  if(a[i]<pivot) i++;
  if(a[j]>pivot) j++;
  swap(a[i],a[j]);
}
return j;
```

- 总的算法，递归
```c
if(begin<end){
  int p=quiksort(a,begin,end);
  quicksort(a,begin,p-1);
  quicksort(a,p+1,end);
}
```