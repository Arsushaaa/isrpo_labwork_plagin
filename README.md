# Плагин для удобства работы с сортировками
## cppsort - плагин с помощью которого ты можешь не писать сортировки саморучно по сотни раз! **Он содержит сортировки и структуры данных** *написанные без встроенных функций и только на iostream*, которые ты можешь вставлять к себе в код!

## Что включает в себя плагин (коды ниже):
### сортировки (и их хоткеи):
- bubbleSort - Ctrl+Shift+Alt+B
- insertionSort - Ctrl+Shift+Alt+I
- countingSortNotStable - Ctrl+Shift+Alt+9
- countingSortStable - Ctrl+Shift+Alt+0
- radixSort - Ctrl+Shift+Alt+R
- quickSort - Ctrl+Shift+Alt+Q
- mergeSort - Ctrl+Shift+Alt+M
- HeapSort - Ctrl+Shift+Alt+1

### структуры (и их хоткеи):
- HeapMin - Ctrl+Shift+Alt+2
- HeapMax - Ctrl+Shift+Alt+3
- StructAllSorts - Ctrl+Shift+Alt+A (это структура включающая в себя все сортировки и отдельные две структуры мин и макс куч) 

#### коды в плагине
##### bubbleSort
```
void sswap(int *arr, int i, int j) 
{
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
void bubbleSort(int *arr, int n) 
{
    bool swapped;
    for (int i = 0; i < n - 1; i++) 
    {
        swapped = false;
        for (int j = 0; j < n - i - 1; j++) 
        {
            if (arr[j] > arr[j + 1]) 
            {
                sswap(arr, j, j + 1);
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```
##### insertionSort
```
void insertionSort(int *arr, int n) 
{
    for (int i = 1; i < n; ++i) 
    {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) 
        {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```
##### countingSortNotStable
```
void countingSortNotStable(int *arr, int size) {
    if (size <= 0) return;
    int maxVal = arr[0];
    int minVal = arr[0];
    for (int i = 1; i < size; i++) 
    {
        if (arr[i] > maxVal) maxVal = arr[i];
        if (arr[i] < minVal) minVal = arr[i];
    }
    int range = maxVal - minVal + 1;
    int* count = new int[range];
    for (int i = 0; i < range; i++) count[i] = 0;
    for (int i = 0; i < size; i++) count[arr[i] - minVal]++;
    int index = 0;
    for (int i = 0; i < range; i++) 
    {
        while (count[i] > 0) 
        {
            arr[index++] = i + minVal;
            count[i]--;
        }
    }
    delete[] count;
}
```
##### countingSortStable
```
void countingSortStable(int *arr, int size) 
{
    if (size <= 0) return;
    int maxVal = arr[0];
    int minVal = arr[0];
    for (int i = 1; i < size; i++) 
    {
        if (arr[i] > maxVal) maxVal = arr[i];
        if (arr[i] < minVal) minVal = arr[i];
    }
    int range = maxVal - minVal + 1;
    int* count = new int[range];
    for (int i = 0; i < range; i++) count[i] = 0;
    for (int i = 0; i < size; i++) count[arr[i] - minVal]++;
    for (int i = 1; i < range; i++) count[i] += count[i - 1];
    int* output = new int[size];
    for (int i = size - 1; i >= 0; i--) 
    {
        output[count[arr[i] - minVal] - 1] = arr[i];
        count[arr[i] - minVal]--;
    }
    for (int i = 0; i < size; i++) arr[i] = output[i];
    delete[] count;
    delete[] output;
}
```
##### radixSort
```
void countingSortForRadix(int *arr, int size, int exp) 
{
    int* output = new int[size];
    int count[10] = {0};

    for (int i = 0; i < size; i++)
        count[(arr[i] / exp) % 10]++;

    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    for (int i = size - 1; i >= 0; i--) 
    {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    for (int i = 0; i < size; i++)
        arr[i] = output[i];

    delete[] output;
}

void radixSortNonNegative(int *arr, int size) 
{
    if (size <= 0) return;

    int maxVal = arr[0];
    for (int i = 1; i < size; i++)
        if (arr[i] > maxVal) maxVal = arr[i];

    for (int exp = 1; maxVal / exp > 0; exp *= 10)
        countingSortForRadix(arr, size, exp);
}

void radixSort(int *arr, int size) 
{
    if (size <= 0) return;

    int* neg = new int[size];
    int* pos = new int[size];
    int negCount = 0, posCount = 0;

    for (int i = 0; i < size; i++) 
    {
        if (arr[i] < 0)
            neg[negCount++] = -arr[i];
        else
            pos[posCount++] = arr[i];
    }

    radixSortNonNegative(neg, negCount);
    radixSortNonNegative(pos, posCount);

    int index = 0;
    for (int i = negCount - 1; i >= 0; i--)
        arr[index++] = -neg[i];
    for (int i = 0; i < posCount; i++)
        arr[index++] = pos[i];

    delete[] neg;
    delete[] pos;
}
```
##### quickSort
```
void sswap(int *arr, int i, int j) 
{
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

void quickSort(int *arr, int l, int r) 
{
    if (l >= r) return;
    int n = r - l + 1;

    int pivot = arr[l + rand() % n];

    int i = l, j = r;

    while (i <= j) 
    {
        while (arr[i] < pivot) i++;
        while (arr[j] > pivot) j--;

        if (i <= j) 
        {
            sswap(arr, i, j);
            i++;
            j--;
        }
    }

    quickSort(arr, l, j);
    quickSort(arr, i, r);
}
```
##### mergeSort
```
void merge(int *arr, int l, int m, int r) 
{
    int n1 = m - l + 1;
    int n2 = r - m;

    int* L = new int[n1];
    int* R = new int[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[l + i];
    for (int j = 0; j < n2; j++) R[j] = arr[m + 1 + j];

    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) 
    {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
    }

    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];

    delete[] L;
    delete[] R;
}

void mergeSort(int *arr, int l, int r) 
{
    if (l >= r) return;
    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}
```
##### heapSort 
```
void sswap(int *arr, int i, int j) 
{
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

void heapifyDown(int *arr, int size, int i)
{
    while (true)
    {
        int left = 2*i + 1;
        int right = 2*i + 2;
        int smallest = i;
        if (left < size && arr[left] < arr[smallest])
        {
            smallest = left;
        }
        if (right < size && arr[right] < arr[smallest])
        {
            smallest = right;
        }
        if (smallest == i)
        {
            break;
        }
        sswap(arr, i, smallest);
        i = smallest;
    }
}

void heapBuild(int *arr, int size)
{
    for (int i = (size / 2) - 1; i >= 0; --i)
    {
        heapifyDown(arr, size, i);
    }
}

void heap_sort(int *arr, int n)
{
    heapBuild(arr,n);
    for (int i = n-1; i >= 0; --i)
    {
        sswap(arr, 0, i);
        heapifyDown(arr, i, 0);
    }
}
```
##### HeapMin
```
struct HeapMin
{
    int* arr;
    int size;

    HeapMin(int n) 
    {
        arr = new int[n];
        size = 0;
    }

    ~HeapMin() 
    {
        delete[] arr;
    }

    void sswap(int el1, int el2)
    {
        int _ = arr[el1];
        arr[el1] = arr[el2];
        arr[el2] = _;
    }

    bool isEmpty()
    {
        return size == 0;
    }

    void insert(int num)
    {
        arr[size] = num;
        heapifyUp(size);
        size++;
    }

    int extractMin()
    {
        int top = arr[0];
        sswap(0, size - 1);
        size--;
        heapifyDown(0);
        return top;
    }

    int getMin()
    {
        return arr[0];
    }

    void heapifyDown(int i)
    {
        while (true)
        {
            int left = 2*i + 1;
            int right = 2*i + 2;
            int smallest = i;

            if (left < size && arr[left] < arr[smallest])
            {
                smallest = left;
            }
            if (right < size && arr[right] < arr[smallest])
            {
                smallest = right;
            }
            if (smallest == i)
            {
                break;
            }
            sswap(i, smallest);
            i = smallest;
        }
    }

    void heapifyUp(int i)
    {
        while (i > 0)
        {
            int parent = (i - 1) / 2;
            if (arr[parent] > arr[i])
            {
                sswap(parent, i);
                i = parent;
            }
            else break;
        }
    }

    void heapBuild()
    {
        for (int i = (size / 2) - 1; i >= 0; --i)
        {
            heapifyDown(i);
        }
    }
};
```
##### HeapMax
```
struct HeapMax
{
    int* arr;
    int size;

    HeapMax(int n) 
    {
        arr = new int[n];
        size = 0;
    }

    ~HeapMax() 
    {
        delete[] arr;
    }

    void sswap(int el1, int el2)
    {
        int _ = arr[el1];
        arr[el1] = arr[el2];
        arr[el2] = _;
    }

    bool isEmpty()
    {
        return size == 0;
    }

    void insert(int num)
    {
        arr[size] = num;
        heapifyUp(size);
        size++;
    }

    int extractMax()
    {
        int top = arr[0];
        sswap(0, size - 1);
        size--;
        heapifyDown(0);
        return top;
    }

    int getMax()
    {
        return arr[0];
    }

    void heapifyDown(int i)
    {
        while (true)
        {
            int left = 2 * i + 1;
            int right = 2 * i + 2;
            int largest = i;

            if (left < size && arr[left] > arr[largest])
            {
                largest = left;
            }
            if (right < size && arr[right] > arr[largest])
            {
                largest = right;
            }
            if (largest == i)
            {
                break;
            }
            sswap(i, largest);
            i = largest;
        }
    }

    void heapifyUp(int i)
    {
        while (i > 0)
        {
            int parent = (i - 1) / 2;
            if (arr[parent] < arr[i])
            {
                sswap(parent, i);
                i = parent;
            }
            else break;
        }
    }

    void heapBuild()
    {
        for (int i = (size / 2) - 1; i >= 0; --i)
        {
            heapifyDown(i);
        }
    }
};
```
##### structAllSorts
```
struct SortingAlgorithms {
    static void sswap(int *arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    struct BubbleSort {
        static void sort(int *arr, int n) {
            bool swapped;
            for (int i = 0; i < n - 1; i++) {
                swapped = false;
                for (int j = 0; j < n - i - 1; j++) {
                    if (arr[j] > arr[j + 1]) {
                        sswap(arr, j, j + 1);
                        swapped = true;
                    }
                }
                if (!swapped) break;
            }
        }
    };

    struct InsertionSort {
        static void sort(int *arr, int n) {
            for (int i = 1; i < n; ++i) {
                int key = arr[i];
                int j = i - 1;
                while (j >= 0 && arr[j] > key) {
                    arr[j + 1] = arr[j];
                    j--;
                }
                arr[j + 1] = key;
            }
        }
    };

    struct CountingSort {
        struct NotStable {
            static void sort(int *arr, int size) {
                if (size <= 0) return;
                int maxVal = arr[0];
                int minVal = arr[0];
                for (int i = 1; i < size; i++) {
                    if (arr[i] > maxVal) maxVal = arr[i];
                    if (arr[i] < minVal) minVal = arr[i];
                }
                int range = maxVal - minVal + 1;
                int* count = new int[range];
                for (int i = 0; i < range; i++) count[i] = 0;
                for (int i = 0; i < size; i++) count[arr[i] - minVal]++;
                int index = 0;
                for (int i = 0; i < range; i++) {
                    while (count[i] > 0) {
                        arr[index++] = i + minVal;
                        count[i]--;
                    }
                }
                delete[] count;
            }
        };
        struct Stable {
            static void sort(int *arr, int size) {
                if (size <= 0) return;
                int maxVal = arr[0];
                int minVal = arr[0];
                for (int i = 1; i < size; i++) {
                    if (arr[i] > maxVal) maxVal = arr[i];
                    if (arr[i] < minVal) minVal = arr[i];
                }
                int range = maxVal - minVal + 1;
                int* count = new int[range];
                for (int i = 0; i < range; i++) count[i] = 0;
                for (int i = 0; i < size; i++) count[arr[i] - minVal]++;
                for (int i = 1; i < range; i++) count[i] += count[i - 1];
                int* output = new int[size];
                for (int i = size - 1; i >= 0; i--) {
                    output[count[arr[i] - minVal] - 1] = arr[i];
                    count[arr[i] - minVal]--;
                }
                for (int i = 0; i < size; i++) arr[i] = output[i];
                delete[] count;
                delete[] output;
            }
        };
    };

    struct RadixSort {
    private:
        static void countingSortForRadix(int *arr, int size, int exp) {
            int* output = new int[size];
            int count[10] = {0};

            for (int i = 0; i < size; i++)
                count[(arr[i] / exp) % 10]++;

            for (int i = 1; i < 10; i++)
                count[i] += count[i - 1];

            for (int i = size - 1; i >= 0; i--) {
                int digit = (arr[i] / exp) % 10;
                output[count[digit] - 1] = arr[i];
                count[digit]--;
            }

            for (int i = 0; i < size; i++)
                arr[i] = output[i];

            delete[] output;
        }

        static void radixSortNonNegative(int *arr, int size) {
            if (size <= 0) return;

            int maxVal = arr[0];
            for (int i = 1; i < size; i++)
                if (arr[i] > maxVal) maxVal = arr[i];

            for (int exp = 1; maxVal / exp > 0; exp *= 10)
                countingSortForRadix(arr, size, exp);
        }

    public:
        static void sort(int *arr, int size) {
            if (size <= 0) return;

            int* neg = new int[size];
            int* pos = new int[size];
            int negCount = 0, posCount = 0;

            for (int i = 0; i < size; i++) {
                if (arr[i] < 0)
                    neg[negCount++] = -arr[i];
                else
                    pos[posCount++] = arr[i];
            }

            radixSortNonNegative(neg, negCount);
            radixSortNonNegative(pos, posCount);

            int index = 0;
            for (int i = negCount - 1; i >= 0; i--)
                arr[index++] = -neg[i];
            for (int i = 0; i < posCount; i++)
                arr[index++] = pos[i];

            delete[] neg;
            delete[] pos;
        }
    };

    struct QuickSort {
        static void sort(int *arr, int l, int r) {
            if (l >= r) return;
            int n = r - l + 1;

            int pivot = arr[l + rand() % n];
            int i = l, j = r;

            while (i <= j) {
                while (arr[i] < pivot) i++;
                while (arr[j] > pivot) j--;

                if (i <= j) {
                    sswap(arr, i, j);
                    i++;
                    j--;
                }
            }

            sort(arr, l, j);
            sort(arr, i, r);
        }
    };

    struct MergeSort {
    private:
        static void merge(int *arr, int l, int m, int r) {
            int n1 = m - l + 1;
            int n2 = r - m;

            int* L = new int[n1];
            int* R = new int[n2];

            for (int i = 0; i < n1; i++) L[i] = arr[l + i];
            for (int j = 0; j < n2; j++) R[j] = arr[m + 1 + j];

            int i = 0, j = 0, k = l;
            while (i < n1 && j < n2) {
                if (L[i] <= R[j]) arr[k++] = L[i++];
                else arr[k++] = R[j++];
            }

            while (i < n1) arr[k++] = L[i++];
            while (j < n2) arr[k++] = R[j++];

            delete[] L;
            delete[] R;
        }

    public:
        static void sort(int *arr, int l, int r) {
            if (l >= r) return;
            int m = l + (r - l) / 2;
            sort(arr, l, m);
            sort(arr, m + 1, r);
            merge(arr, l, m, r);
        }
    };

    struct HeapSort {
    private:
        static void heapifyDown(int *arr, int size, int i) {
            while (true) {
                int left = 2*i + 1;
                int right = 2*i + 2;
                int smallest = i;
                
                if (left < size && arr[left] < arr[smallest]) {
                    smallest = left;
                }
                if (right < size && arr[right] < arr[smallest]) {
                    smallest = right;
                }
                if (smallest == i) {
                    break;
                }
                sswap(arr, i, smallest);
                i = smallest;
            }
        }

        static void heapBuild(int *arr, int size) {
            for (int i = (size / 2) - 1; i >= 0; --i) {
                heapifyDown(arr, size, i);
            }
        }

    public:
        static void sort(int *arr, int n) {
            heapBuild(arr, n);
            for (int i = n-1; i >= 0; --i) {
                sswap(arr, 0, i);
                heapifyDown(arr, i, 0);
            }
        }
    };
};

struct HeapMin {
    int* arr;
    int size;
    int capacity;

    HeapMin(int n) {
        arr = new int[n];
        size = 0;
        capacity = n;
    }

    ~HeapMin() {
        delete[] arr;
    }

    void sswap(int el1, int el2) {
        int temp = arr[el1];
        arr[el1] = arr[el2];
        arr[el2] = temp;
    }

    bool isEmpty() {
        return size == 0;
    }

    void insert(int num) {
        if (size >= capacity) return;
        arr[size] = num;
        heapifyUp(size);
        size++;
    }

    int extractMin() {
        if (isEmpty()) return -1;
        int top = arr[0];
        sswap(0, size - 1);
        size--;
        heapifyDown(0);
        return top;
    }

    int getMin() {
        return isEmpty() ? -1 : arr[0];
    }

private:
    void heapifyDown(int i) {
        while (true) {
            int left = 2*i + 1;
            int right = 2*i + 2;
            int smallest = i;

            if (left < size && arr[left] < arr[smallest]) {
                smallest = left;
            }
            if (right < size && arr[right] < arr[smallest]) {
                smallest = right;
            }
            if (smallest == i) {
                break;
            }
            sswap(i, smallest);
            i = smallest;
        }
    }

    void heapifyUp(int i) {
        while (i > 0) {
            int parent = (i - 1) / 2;
            if (arr[parent] > arr[i]) {
                sswap(parent, i);
                i = parent;
            } else break;
        }
    }
};

struct HeapMax
{
    int* arr;
    int size;

    HeapMax(int n) 
    {
        arr = new int[n];
        size = 0;
    }

    ~HeapMax() 
    {
        delete[] arr;
    }

    void sswap(int el1, int el2)
    {
        int _ = arr[el1];
        arr[el1] = arr[el2];
        arr[el2] = _;
    }

    bool isEmpty()
    {
        return size == 0;
    }

    void insert(int num)
    {
        arr[size] = num;
        heapifyUp(size);
        size++;
    }

    int extractMax()
    {
        int top = arr[0];
        sswap(0, size - 1);
        size--;
        heapifyDown(0);
        return top;
    }

    int getMax()
    {
        return arr[0];
    }

    void heapifyDown(int i)
    {
        while (true)
        {
            int left = 2 * i + 1;
            int right = 2 * i + 2;
            int largest = i;

            if (left < size && arr[left] > arr[largest])
            {
                largest = left;
            }
            if (right < size && arr[right] > arr[largest])
            {
                largest = right;
            }
            if (largest == i)
            {
                break;
            }
            sswap(i, largest);
            i = largest;
        }
    }

    void heapifyUp(int i)
    {
        while (i > 0)
        {
            int parent = (i - 1) / 2;
            if (arr[parent] < arr[i])
            {
                sswap(parent, i);
                i = parent;
            }
            else break;
        }
    }

    void heapBuild()
    {
        for (int i = (size / 2) - 1; i >= 0; --i)
        {
            heapifyDown(i);
        }
    }
};
```