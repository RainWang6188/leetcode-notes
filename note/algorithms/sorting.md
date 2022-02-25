# Sorting Algorithms

## 1. Bubble Sort
Bubble sort is a simple sorting algorithm. The algorithm starts at the beginning of the data set. It compares the first two elements, and if the first is greater than the second, it swaps them. It continues doing this for each pair of adjacent elements to the end of the data set. It then starts again with the first two elements, repeating until no swaps have occurred on the last pass.

The above function always runs $O(N^2)$ time even if the array is sorted. It can be optimized by stopping the algorithm if inner loop didn’t cause any swap. 

```C++
void bubbleSort(vector<int> &arr) {
    int n = arr.size();
    for(int i = 0; i < n - 1; i++) {
        for(int j = 0; j < n - i - 1; j++) {
            if(arr[j] > arr[j+1])
                swap(arr[j], arr[j+1]);
        }
    }
}
```

**Worst and Average Case Time Complexity:** $O(N^2)$
**Auxiliary Space:** $O(1)$
**Stable:** Yes
**Sorting In Place:** Yes


## 2. Insertion Sort
Insertion sort is a simple sorting algorithm that is relatively efficient for small lists and mostly sorted lists, and is often used as part of more sophisticated algorithms.

It works by taking elements from the list one by one and inserting them in their correct position into a new sorted list similar to how we put money in our wallet. In arrays, the new list and the remaining elements can share the array's space, but insertion is expensive, requiring shifting all following elements over by one. Shellsort is a variant of insertion sort that is more efficient for larger lists.

```C++
void insertionSort(vector<int> &arr) {
    for(int i = 1; i < arr.size(); i++) {
        for(int j = i; j > 0 && arr[j] < arr[j-1]; j--) {
            swap(arr[j], arr[j-1]);
        }
    }
}
```

**Worst and Average Case Time Complexity:** $O(N^2)$
**Best Case Time Complexity:** $O(N)$ if the array is already sorted
**Auxiliary Space:** $O(1)$
**Stable:** Yes
**Sorting In Place:** Yes

## 3. Shellsort
Shellsort was invented by Donald Shell in 1959. It improves upon insertion sort by moving out of order elements more than one position at a time. 

The concept behind Shellsort is that insertion sort performs in $O(kn)$ time, where k is the greatest distance between two out-of-place elements. This means that generally, they perform in $O(n^2)$, but for data that is mostly sorted, with only a few elements out of place, they perform faster. So, by first sorting elements far away, and progressively shrinking the gap between the elements to sort, the final sort computes much faster. One implementation can be described as arranging the data sequence in a two-dimensional array and then sorting the columns of the array using insertion sort.

```C++
int shellSort(int arr[], int n)
{
    // Start with a big gap, then reduce the gap
    for (int gap = n/2; gap > 0; gap /= 2)
    {
        // Do a gapped insertion sort for this gap size.
        // The first gap elements a[0..gap-1] are already in gapped order
        // keep adding one more element until the entire array is
        // gap sorted
        for (int i = gap; i < n; i += 1)
        {
            // add a[i] to the elements that have been gap sorted
            // save a[i] in temp and make a hole at position i
            int temp = arr[i];
 
            // shift earlier gap-sorted elements up until the correct
            // location for a[i] is found
            int j;           
            for (j = i; j >= gap && arr[j - gap] > temp; j -= gap)
                arr[j] = arr[j - gap];
             
            //  put temp (the original a[i]) in its correct location
            arr[j] = temp;
        }
    }
    return 0;
}
```

Time complexity of above implementation of shellsort is $O(N^2)$. In the above implementation gap is reduce by half in every iteration. There are many other ways to reduce gap which lead to better time complexity.

**Stable:** No

## 4. Selection Sort
Selection sort is an in-place comparison sort. It has $O(N^2)$ complexity, making it inefficient on large lists, and generally performs worse than the similar insertion sort. Selection sort is noted for its simplicity, and also has performance advantages over more complicated algorithms in certain situations.

The algorithm finds the minimum value, swaps it with the value in the first position, and repeats these steps for the remainder of the list. It does no more than $N$ swaps, and thus is useful where swapping is very expensive.

```C++
void selectionSort(vector<int> &arr) {
    int n = arr.size();
    for(int i = 0; i < n - 1; i++) {
        int min_index = i;
        for(int j = i + 1; j < n; j++) {
            if(arr[j] < arr[min_index])
                min_index = j;
        }
        swap(arr[i], arr[min_index]);
    }
}
```
**Time Complexity:** $O(N^2)$ as there are two nested loops.
**Auxiliary Space:** $O(1)$ 
The good thing about selection sort is it never makes more than O(n) swaps and can be useful when memory write is a costly operation. 
**Stable:** No.
**Sorting In Place:** Yes

## 5. Merge Sort

```C++
void merge(vector<int> &arr, int left, int mid, int right) {
    const int subArrayOne = mid - left + 1;
    const int subArrayTwo = right - mid;

    vector<int> leftArray(subArrayOne, 0);
    vector<int> rightArray(subArrayTwo, 0);

    for(int i = 0; i < subArrayOne; i++)
        leftArray[i] = arr[left+i];
    for(int i = 0; i < subArrayTwo; i++)
        rightArray[i] = arr[mid+1+i];
    
    int leftArrayIndex = 0;
    int rightArrayIndex = 0;
    int mergedArrayIndex = left;

    while(leftArrayIndex < subArrayOne && rightArrayIndex < subArrayTwo) {
        if(leftArray[leftArrayIndex] <= rightArray[rightArrayIndex])
            arr[mergedArrayIndex] = leftArray[leftArrayIndex++];
        else
            arr[mergedArrayIndex] = rightArray[rightArrayIndex++];
        mergedArrayIndex++;
    }

    while(leftArrayIndex < subArrayOne) {
        arr[mergedArrayIndex++] = leftArray[leftArrayIndex++];
    }

    while(rightArrayIndex < subArrayTwo) {
        arr[mergedArrayIndex++] = rightArray[rightArrayIndex++];
    }
}

void mergeSort(vector<int> &arr, int begin, int end) {
    if(begin >= end)
        return;
    
    int mid = begin + ((end - begin) >> 1);
    mergeSort(arr, 0, mid);
    mergeSort(arr, mid + 1, end);
    merge(arr, 0, mid, end);
}
```
**Time Complexity:** $\theta(N\log N)$ in all 3 cases (worst, average and best) 
**Auxiliary Space:** $O(N)$
**Stable:** Yes
**Sorting In Place:** No

## 6. Heapsort
Using heap to sort the array.

```C++
void heapSort(vector<int> &arr) {
    auto cmp = [&](int &a, int &b) {
        return a > b;
    };

    //priority_queue<int, vector<int>, greater<int>> pq;
    priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
    for(auto & val : arr)
        pq.push(val);
    
    for(int i = 0; i < arr.size(); i++) {
        arr[i] = pq.top();
        pq.pop();
    }
}
```

## 7. Quicksort
Quicksort is a divide and conquer algorithm which relies on a partition operation: to partition an array, an element called a **pivot** is selected. All elements smaller than the pivot are moved before it and all greater elements are moved after it. This can be done efficiently in linear time and in-place. The lesser and greater sublists are then recursively sorted. 

```C++
int partition(vector<int> &arr, int start, int end) {
    int pivot = arr[start];

    int counter = 0;
    for(int i = start + 1; i <= end; i++) {
        if(arr[i] <= pivot) 
            counter++;
    }

    int pivotIndex = start + counter;
    swap(arr[pivotIndex], arr[start]);

    int i = start, j = end;
    while(i < pivotIndex && j > pivotIndex) {
        while(arr[i] < pivot)
            i++;
        
        while(arr[j] > pivot)
            j--;
        
        if(i < pivotIndex && j > pivotIndex)
            swap(arr[i++], arr[j--]);
    }

    return pivotIndex;
}

void quickSort(vector<int> &arr, int start, int end) {
    if(start >= end)
        return;
    
    int p = partition(arr, start, end);

    quickSort(arr, start, p);
    quickSort(arr, p+1, end);
}
```

 On average, the algorithm takes $O(n\log {n})$ comparisons to sort $n$ items. In the worst case, it makes $O(n^{2})$ comparisons.

**Stable:** No