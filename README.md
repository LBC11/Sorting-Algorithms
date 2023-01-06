# Sorting-Algorithms

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
1. 



