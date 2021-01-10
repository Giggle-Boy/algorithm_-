## 插入排序
- 对于插入排序来说，可以将整个数组进行排序，但是我更倾向于创建一个**超集**，即可以固定到某一个区间的插入排序。ps:其实是为了后面的归并排序的方便调用。
- 说明：**参数均为数组的下标**
```c++
template<class T>
void insertSort(T *array, int head, int tail) {
    for (int i = head + 1; i < tail + 1; ++i) {
        T emt = array[i];
        int j = i - 1;
        while (j >= head && emt < array[j]) {   // 切记这里j一定和head比较，而不是简单意义上的0
            array[j + 1] = array[j];
            --j;
        }
        array[j + 1] = emt;
    }
}
```
### 优化插入排序(通过二分法查找)
```c++
template<class T>
void insertSortThroughBinSearch(T *array, int head, int tail) {
    for (int i = head + 1; i < tail + 1; ++i) {
        // 获取待插入元素
        T emt = array[i];
        
        // 二分法查找待插入数据在数组中的位置
        int left = 0;
        int right = i - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (emt < array[mid]) {
                right = mid - 1;
            } else if (emt > array[mid]) {
                left = mid + 1;
            }
        }
        
        // left就是需要插入数据的位置
        int j = i - 1;
        while (j >= left) {
            array[j + 1] = array[j];
            --j;
        }
        array[j + 1] = emt;
    }
}
```
## 归并排序的实现
```c++
// 辅助数组，用于存储子数组
int *assist = new int[100];

void merge(int *array, int head, int tail){
    for(int fakeHead = head; fakeHead <= tail; ++fakeHead)
        assist[fakeHead] = array[fakeHead];

    int mid = (head + tail) / 2;
    int fakeMid = mid + 1;
    int fakeHead = head;

    while (fakeHead <= mid && fakeMid <= tail)
        if (assist[fakeHead] >= assist[fakeMid])
            array[head++] = assist[fakeMid++];
        else
            array[head++] = assist[fakeHead++];

    while (fakeHead <= mid)    array[head++] = assist[fakeHead++];

    while (fakeMid <= tail)    array[head++] = assist[fakeMid++];

}

void mergeSort(int *array, int head, int tail){
    if(head < tail){
        int mid = (head + tail) / 2;
        mergeSort(array, head, mid);
        mergeSort(array, mid + 1, tail);
        merge(array, head,tail);
    }
}
```
## 归并排序的进一步优化
- 虽然归并排序的最坏情况运行时间为&theta;(nlgn)，而插入排序的最坏情况运行时间为&theta;(n^2),但是在数组长度较小时，插入排序在许多机器上的运行速度反而优于归并排序(摘自《算法导论》)。*通常是在 n>30 左右后归并排序才开始打败插入排序！*
- 判断在归并过程中数组的长度是否达到某一个值，如果没有达到使用插入排序，反之使用原归并函数，所以将这个长度作为一个参数传入函数中即可。

代码实现
```c++
// 将部分子程序转换为直接插入排序
void mergeThroughInsertSort(int *array, int head, int tail, int turn){
    if(tail - head < turn){
        insertSort(array, head, tail);
    }else{
        merge(array,head,tail);
    }
}

// 优化之后的归并排序
void mergeSortAndInsertSort(int *array, int head, int tail, int turn){
    if(head < tail){
        int mid = (head + tail) / 2;
        mergeSortAndInsertSort(array, head, mid,turn);
        mergeSortAndInsertSort(array, mid + 1, tail, turn);
        mergeThroughInsertSort(array,head,tail, turn);
    }
}
```
## 实验结果
- 在数据数量为一百万，且数组长度大于30才调用归并函数情况下，经过一百次重复的测试。普通归并排序所消耗的平均时间为：**0.168527**。优化之后的归并排序所消耗的时间为：**0.165244**。
- 千万：2.02344 亿：21.8738
- 千万：2.0677 亿：21.847
- 优化和不优化的结果貌似不是非常的明显，但是归并排序还是非常的快。对于上亿的数据表现来说还是很不错的。空间消耗，即使上亿的数据，也是可以接受的。
