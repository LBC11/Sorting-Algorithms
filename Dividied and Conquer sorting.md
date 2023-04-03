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
    // 물론 중간 지점을 이 code 에서 처음의 pivot 으로 지정하기는 한다.
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

        // 이 방식은 역순으로 정렬된다.
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
