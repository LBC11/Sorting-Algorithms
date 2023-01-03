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



