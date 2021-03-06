# 算法

### 复杂度

##### 时间复杂度

- **big O**

  > 找出N个数中的最大值	O(n)
  >
  > 找出一个有序数组中是否存在某个数	1.O(N) 2.O(log N)
  >
  > 找出两个有序不重复数组的公共部分	1.O(N*M) 2.O(N\*log M) 3.O(N+M)

##### 空间复杂度

> 除去输入输出，算法执行所需要的额外空间
>
>
> 例：将输入数组 [1234567] 输出为 [6712345]，使用逆序可使空间复杂度为O(1)	

##### 最优解

> 满足时间复杂度最优的情况下使空间复杂度最优

### 排序

##### 稳定性

> 排序完成后原数组中相同数字的顺序未发生改变·

##### 冒泡排序

> 时间复杂度为O(N^2)，空间复杂度为O(1)，可以做到稳定性

```java
public int[] bubbleSort(int[] arr){
    if(arr==null || arr.length<2){
        return arr;
    }
    
    boolean flag = false;
    for(int end=arr.length-1; end>0; end--){
        flag = true;
        for(int i=0; i<end; i++){
            if(arr[i]>arr[i+1]){
                swap(arr, i, i+1);
                flag = false;
            }
        }
        if(flag)
            break;
    }
    return arr;
}
```

> - 对数器
> - 贪心算法

##### 选择排序

> 时间复杂度为O(N^2)，空间复杂度为O(1)，不能做到稳定性

```java
public static void selectSort(int[] arr) {
    if(arr == null || arr.length < 2)
        return;
    selectSort(arr, 0, arr.length - 1);
}

public static void selectSort(int[] arr, int L, int R) {
    int min = 0;
    for(int i = L; i < R; i++) {
        min = i;
        for(int j = i + 1; j <= R; j++) {
            if(arr[min] > arr[j]) {
                min = j;
            }
        }
        swap(arr, i , min);
    }
}
```



##### 插入排序

> 时间复杂度为O(N^2)，最优为O(N)，空间复杂度为O(1)，可以做到稳定性

```java 
public void insertionSort(int[] arr){
    if(arr==null || arr.length<2)
        return;
    
    for(int i=1; i<arr.length; i++){
        for(int j=i-1; j>=0 && arr[j]>arr[j+1]; j--){
            swap(arr, j, j+1);
        }
    }
}
```

##### 归并排序

- 递归

  > base case

- Master公式

![master](../images/master.jpg "Master")

- 复杂度

> 时间复杂度为O(N*log N)，空间复杂度为O(N)（归并排序内部缓存法论文可做到空间复杂度为O(1)），可以做到稳定性

```java
public static void mergeSort(int[] arr) {
    if(arr == null || arr.length < 2)
        return ;
    sortProcess(arr, 0, arr.length) ;
}

private static void sortProcess (int[] arr, int L, int R){
    //base case
    if (L == R)
        return arr[L] ;
    int mid = L + ((R-L) >> 1) ;
    sortProcess(arr, L, mid);
    sortProcess(arr, mid+1, R);
    merge(arr, L, mid, R);
}

private static void merge(int[] arr, int L, int mid, int R){
    int[] help = new int[R-L+1];
  	int i = 0;
    int p1 = L;
    int p2 = mid+1;
    
    while(p1 <= mid && p2 <= R){
        help[i++] = arr[p1] <= arr[p2] ? arr[p1++] : arr[p2++];
    }
    
    //p1越界
    while(p2 <= R){
        help[i++] = arr[p2++];
    }
    //p2越界
    while(p1 <= mid){
        help[i++] = arr[p1++];
    }
    
    for(i = 0; i < help.length; i++)
        arr[L+i] = help[i];
}
```

- 练习

  > 求最小和。[4 8 6 2 1 9 7 3]
  >
  > 求降序对。[4 8 6 2 1 9 7 3]

##### 快速排序

> 经典快排思路：以数组最后一位为基准，将小于它的数放入小于区，剩下的则为大于等于它的数
>
> 随机快排：随机选择一个数为基准

- 荷兰国旗问题

> 时间复杂度最差为为O(N^2)，长期期望为O(N*log N)，空间复杂度最优为O(log N)，很难做到稳定性（论文级别可以-01 stable sort）

```java
public static void quickSort(int[] arr, int L, int R) {
    if(L < R){
        swap(arr, (int)Math.Random()*(R-L+1), R);
     	int[] p = partition(arr, L, R);
        quickSort(arr, L, p[0] - 1);
        quickSort(arr, P[1] + 1, R);
    }
}

public static int[] partition(int[] arr, int L, int R){
    int less = L - 1;
    int bigger = R;
    while(L < bigger){
        if(arr[L] < arr[R]){
            swap(arr, ++less, L++);
        }else if(arr[L] > arr[R]){
            swap(arr, --bigger, L)
        }else{
            L++;
        }
    }
    swap(arr, more, R)
    return new int[]{less+1, more};
}
```

##### 堆排序

- 完全二叉树

  > 堆在空间上是一棵完全二叉树

- 大根堆、小根堆

  > 大根堆—每棵树的中的最大值都为它的父节点

> 建立大根堆的时间复杂度为O(N)，调整大根堆的时间复杂度为O(N*log N)，空间复杂度为O(1)
>
> 将数组转换为堆，则左节点索引为i\*2+1，右节点为i\*2+2，父节点为(i-1)/2

```java
public static void heapSort(int[] arr) {
    if(arr == null || arr.length < 2) {
        return;
    }
    
    for(int i = 0; i < arr.length; i++){
        heapInsert(arr, i);
    }
    
    int size = arr.length;
    swap(arr, 0, --size);
    while(size > 0){
    	heapify(arr, 0, size);
        swap(arr, 0, --size);
    }
}

public static void heapInsert(int[] arr, int index) {
    while(arr[index] > arr[(index - 1) / 2]){
        swap(arr, index, (index - 1) / 2);
        index = (index - 1) / 2;
    }
}

             
public static void heapify(int[] arr, int index, int size) {
    int left = index * 2 +1;      
    while(left < size){
        int largest = left + 1 < size && arr[left] < arr[left + 1] ? left + 1 : left;
        largest = arr[largest] > arr[index] ? largest : index;
        if(largest == index){
            break;
        }
        swap(arr, largest, index);
        index = largest;
        left = index * 2 + 1;
    }
}
```



### 数据结构

##### 红黑树

###### 平衡二叉树

- 所有左节点都小于等于父节点的值
- 所有右节点都大于等于父节点的值

###### 红黑树

- 所有节点都是红色或者黑色
- 根节点只能是黑色
- 所有叶子都是空的黑色节点
- 每个红色节点的子节点只能为黑色
- 任意节点到其叶子的所有路径拥有相同数目的黑色节点

###### 自平衡策略

- 变色

- 左旋转

  逆时针旋转红黑树，使其右节点变为新的父节点，父节点变为左子节点

- 右旋转

  顺时针旋转红黑树，使其左节点变为新的父节点，父节点变为右子节点