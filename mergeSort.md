# algorithm_-
another repository
## mergeSort implement
```c++
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
