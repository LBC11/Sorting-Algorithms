# Pairwise comparison sorting

### Bubble Sort(거품 정렬)
1. 정렬되지 않은 index중 맨 앞 선택
2. 현재 index의 값보다 다음 index의 값이 크면 값을 교환하는 것을 마지막까지 반복.
3. 모든 index에서 정렬이 완료될 때까지 위의 과정 반복

```
// 정렬하려는 array
int[] array = new int[]{4,1,6,7,9,2};

for(int i = 0; i < array.length-1; i++) {
    // 정렬 되지 않은 index 만큼 반복
    for (int j=0; j < array.length-i-1; j++) {

        // j+1의 값보다 j의 값이 클 경우
        if(array[j] > array[j+1]) {

            // 둘의 값을 교환
            int temp = array[j];
            array[j] = array[j+1];
            array[j+1] = temp;
        }
    }
}

// 출력값: 1 2 4 6 7 9
for (int i : array) {
    System.out.print(i+" ");
}

```
- - -

### Selection Sort(선택 정렬)
1. 정렬되지 않은 index중 맨 앞 선택  
2. 그 앞부터 맨 끝의 index 중 가장 작은 값을 가진 index와 선택된 index의 값을 바꾼다.
3. 모든 index에서 정렬이 완료될 때까지 위의 과정 반복

```
// 정렬하려는 array
int[] array = new int[]{4,1,6,7,9,2};

for(int i = 0; i < array.length - 1; i++) {
    int min_index = i;

    // 최솟값을 갖고있는 인덱스 찾기
    for(int j = i + 1; j < array.length; j++) {
        if(array[j] < array[min_index]) {
            min_index = j;
        }
    }

    // i번째 값과 찾은 최솟값을 서로 교환
    int temp = array[min_index];
    array[min_index] = array[i];
    array[i] = temp;
}

// 출력값: 1 2 4 6 7 9
for (int i : array) {
    System.out.print(i+" ");
}

```
- - -

### Insertion sort(삽입 정렬)
1. 정렬되지 않은 index중 앞에서 두번째 선택  
2. 선택된 index의 값보다 이전 index의 값이 작다면 값을 교환하고 이전 index을 선택한다.
3. 선택된 index의 값이 이전 index의 값보다 크거나 index가 0일 때까지 2의 과정을 반복한다.
4. 모든 index에서 정렬이 완료될 때까지 위의 과정 반복한다.

```
// 정렬하려는 array
int[] array = new int[]{4,1,6,7,9,2};

// index 1부터 시작
for(int i = 1; i < array.length; i++) {

    // 선택된 index 의 값
    int target = array[i];

    // 선택된 index 의 값이 이전 index 의 값보다 크거나 index 가 0일 때
    while( i>=1 && array[i-1]>target) {
        array[i] = array[i-1];
        i--;
    }

    array[i] = target;
}

// 출력값: 1 2 4 6 7 9
for (int i : array) {
    System.out.print(i+" ");
}
```
- - -

### Shell Sort(쉘 정렬)
이전 원소를 모두 비교, 교환해야 하는 Insertion Sort(삽입 정렬)의 단점을 극복하기 위한 방식  
일정 간격(gap)을 주기로 sub array끼리 Insertion Sort 실행

1. gap 설정
2. gap으로 구분된 각 sub array에서 Insertion sort 실행
3. gap 단축
4. gap이 1이 될 때까지 위의 과정 반복

```
// 정렬하려는 array
int[] array = new int[]{4,1,6,7,9,2};

int gap = array.length;

// gap 이 1이 될 때까지 실행
while(gap > 1) {
    gap = gap/3 + 1;

    // gap 으로 구분된 각 sub array 정렬
    for(int i=0; i<gap; i++) {

        // Insertion Sort 실행
        for(int j=i+gap; j<array.length; j+=gap) {

            // 선택된 index 의 값
            int target = array[j];

            // 선택된 index 의 값이 이전 index 의 값보다 크거나 
            // index 가 각 sub array 의 초기값일 때
            while( j-gap>=i && array[j-gap]>target) {
                array[j] = array[j-gap];
                j -= gap;
            }

            array[j] = target;
        }
    }
}

// 출력값: 1 2 4 6 7 9
for (int i : array) {
    System.out.print(i+" ");
}
```
- - -

### Heap Sort(힙 정렬)
1. 정렬이 완료되지 않은 부분을 max heap으로 정렬
2. index 0의 값(최댓값)과 마지막 index의 값 교환
3. 모든 index에서 정렬이 완료될 때까지 위의 과정 반복한다.

#### max heap의 특징
1. left child node index = parent node index * 2 + 1
2. right child node index = parent node index * 2 + 2
3. parnet node index = (child node index - 1) / 2

#### max heap으로 정렬하기
1. parent node의 값과 child node의 값 비교
2. child node의 값이 크면 값 교환
3. 교환이 일어났으면 해당 child node를 parent로 가지는 sub tree 재검사
4. 값의 교환이 일어나지 않을 때까지 위의 과정 반복

```
 // array 를 maxHeap 으로 정렬하는 함수
    static void maxHeap(int[] array, int parentIdx, int lastIdx) {

        /*
        만약 교환이 이뤄졌을 시 교환된 node 를 parent 로 두는
        sub tree 도 재검사 하기 위해 while 문 사용

        while 문의 조건이 leftChildIdx 를 기준으로 하는 이유는
        오른쪽으로 할 시 왼쪽에만 child 를 가지고 있는 경우에서
        정렬을 진행하지 않는다.
         */
        while((parentIdx * 2) + 1 <= lastIdx) {
            int leftChildIdx = 2 * parentIdx + 1;
            int rightChildIdx = 2 * parentIdx + 2;
            int largestIdx = parentIdx;

            // leftChildNode 와 parent node value 비교
            if(lastIdx >= leftChildIdx && array[leftChildIdx] >= array[largestIdx]) {
                largestIdx = leftChildIdx;
            }

            // rightChildNode 와 parent node value 비교
            if(lastIdx >= rightChildIdx && array[rightChildIdx] >= array[largestIdx]) {
                largestIdx = rightChildIdx;
            }

            // parentIdx 의 value 보다 childIdx 의 value 가 클 때
            if(parentIdx != largestIdx) {

                // value 교환
                swap(array, parentIdx, largestIdx);

                // parentIdx 갱신
                // 교환된 값을 가지는 parent 의 sub tree 를 재검사
                parentIdx = largestIdx;
            }

            // 그렇지 않으면 반복문 종료
            else return ;
        }
    }

    static void heapSort(int[] array) {

        int length = array.length;

        // array 의 원소가 0 혹은 1개이면 정렬할 필요가 없다.
        if(length < 2) {
            return ;
        }

        // 가장 마지막 child 의 parent index
        int parentIdx = getParentIdx(length - 1);

        // max heap 만들기
        for(int i= parentIdx; i>=0; i--) {
            maxHeap(array, i, length - 1);
        }

        for(int i=length-1; i>0; i--) {

            // index 0 의 값과 index 마지막 값 교환
            swap(array, 0, i);

            // 다시 max heap 으로
            maxHeap(array, 0, i-1);

        }

    }

    static int getParentIdx(int childIdx) {
        return (childIdx - 1) / 2;
    }

    static void swap(int[] array, int a, int b) {
        int temp = array[a];
        array[a] = array[b];
        array[b] = temp;
    }
    
```

- - -

# Dividied and Conquer sorting

### Merge sort(병합 정렬)
1. 나누어진 sub array의 길이가 1이 될 때까지 array를 절반으로 나눈다. (divide: 분할)
2. 인접한 sub array 끼리 정렬하여 합친다. (conqure: 정복)

```

// 정렬 과정에서 필요한 임시 공간
private static int[] temp;

/*
private 와 public 을 따로 구현한 이유
1. 내구의 구현방식을 외부에 노출시키지 않음
2. private mergeSort 에서 temp array 를 init 하는데
   이를 재귀로 돌리게 될 경우 문제가 발생한다.
 */
public static void mergeSort(int[] array) {

    // 임시 공간 init
    temp = new int[array.length];

    // merge sort 실행
    mergeSort(array, 0, array.length-1);

    // 불필요해진 memory 해제
    // temp 를 static 으로 선언 해놓았기에 GC가 자동으로
    // 해제해주기 어려워 null 을 대입하는 방식 사용
    temp = null;
}

private static void mergeSort(int[] array, int left, int right) {

    // 범위 내의 원소의 개수가 0 혹은 1개면 정렬할 필요가 없다.
    if(left >= right) return;

    // 중간 지점
    int mid  = (left+right)/2;

    mergeSort(array, left, mid);
    mergeSort(array, mid+1, right);

    merge(array, left, mid, right);
}

private static void merge(int[] array, int left, int mid, int right) {

    int l = left; // left sub array 시작점
    int r = mid +1; // right sub array 시작점
    int idx = left; // temp 의 index

    while(l <= mid && r <= right) {

        // 각 sub array 의 value 를 비교한 후 더 작은 값을
        // 가진 value 가 temp array 에 들어간다.

        // left array 값이 더 클 경우
        if(array[l] >= array[r]) {
            temp[idx] = array[r];
            idx++;
            r++;
        }

        // right array 값이 더 클 경우
        else{
            temp[idx] = array[l];
            idx++;
            l++;
        }
    }

    // left 가 먼저 옮겨진 경우
    // right 에서 나머지 부분을 옮겨 적는다.
    if(l > mid) {
        while(r <= right) {
            temp[idx] = array[r];
            idx++;
            r++;
        }
    }

    // right 가 먼저 옮겨진 경우
    // left 에서 나머지 부분을 옮겨 적는다.
    else {
        while(l <= mid) {
            temp[idx] = array[l];
            idx++;
            l++;
        }
    }

    // 정렬된 부분을 다시 array 로 옮겨 담는다.
    for(int i=left; i<=right; i++) {
        array[i] = temp[i];
    }

}
    
```    

- - -
### Quick Sort(퀵 정렬)
1. array에서 중간 숫자를 pivot으로 선택한다.
2. pivot을 기준으로 왼쪽에서 pivot 보다 작은 수를 오른쪽에서 pivot보다 큰 수를 찾는다.
3. 양 방향에서 찾은 두 수를 교환한다.
4. 양 방향의 탐색 위치가 엇갈리지 않을 때까지 2~3 과정을 반복한다.
5. 엇갈린 지점을 기준으로 두개의 sub array로 나누고 각각의 array에서 위의 과정을 반복 (Divide: 분할)
6. 나누어진 sub array의 길이가 1이 될 때까지 위의 과정 반복
7. 인접한 sub array끼리 합친다. (Conquar: 정복) 

```  

static void quickSort_middle(int[] array, int left, int right) {

    // 범위 내의 원소의 개수가 0 혹은 1개면 정렬할 필요가 없다.
    if (left >= right) return;

    // array 를 나누는 기준
    // merge sort 는 mid 가 기준이 되었다면
    // quick sort 는 양방향에서 엇갈리는 지점이 기준이 된다.
    // 물론 중간 지점을 처음 이 code 에서 처음의 pivot 으로 지정하기는 한다.
    // 이 외에도 가장 왼쪽을 기준으로 하는 방법, 오른쪽을 기준으로 하는 방법이 있다.
    // 중간지점을 선택하는 방식이 성능면에서 우수하여 이를 선택했다.
    int pivot = divide(array, left, right);

    quickSort_middle(array, left, pivot);
    quickSort_middle(array, pivot + 1, right);

    }

    private static int divide(int[] array, int left, int right) {

    int l = left; // 왼쪽 방향의 시작 지점
    int r = right; // 오른쪽 방향의 시작 지점
    int pivot = array[(left + right) / 2]; // 처음 정하는 pivot

    // 양 방향의 pointer 가 엇갈릴 떄까지 계속한다.
    while (true) {

        // 이 방식은 역수능로 정렬된다.
        // pivot 보다 작거나 같은 원소를 찾는다.
        while (array[l] > pivot) {
            l++;
        }

        // pivot 보다 크거나 같은 위치의 원소를 찾는다.
        // 동시에 r은 l보다 크거나 같아야 한다.
        while (pivot > array[r] && r > l) {
            r--;
        }

        // 양방향에서 엇갈리는 경우
        // 엇갈리는 지점을 다음 array 를 나누는
        // 기준(pivot)으로 한다.
        if (l >= r) {
            return r;
        }

        // 두 위치의 value 를 교환한다.
        swap(array, l, r);
    }

    }

    static void swap(int[] array, int a, int b) {
    int temp = array[a];
    array[a] = array[b];
    array[b] = temp;
    }
    
```  

- - -

# Distributed sorting

### Bucket sort

### Counting sort

### Radix sort

# Hybrid sorting

### Tim sort
