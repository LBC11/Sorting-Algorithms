# Distributed Sorting

### Counting sort
1. 정렬하고자 하는 수의 최댓값을 구한다.
2. 최댓값 만큼의 크기를 가지는 array A를 준비한다. (array[max+1])
3. A에 각 index 크기 만큼의 value를 가지는 원소의 개수를 구한다. (counting array)
4. 3에서 구한 counting array의 각 value를 누적합으로 변환한다.
5. counting array에서 각 (value - 1)은 정렬 되었을 때 해당 index의 시작 위치를 알려준다.
(ex. A[5] = 9 -> 정렬된 array에서 index 8의 value는 5이다.)

``` 

static int[] counting;
static int[] sorted;

public static void countingSort(int[] array) {

    // counting array init
    counting = new int[max + 1];

    // counting sort 시작
    countingSort(array, array.length);

    // 메모리 할당 해제
    counting = null;

}

private static void countingSort(int[] array, int length) {

    // sorted array init
    sorted = new int[length];

    // 과정 3의 counting array 채우기
    for (int i : array) {
        counting[i]++;
    }

    // 과정 4의 counting array 누적합으로 변환
    for (int i = 1; i < counting.length; i++) {
        counting[i] += counting[i - 1];
    }

    // 과정 5의 counting array 를 이용한 정렬 시작
    // 누적합을 이용하는 방식이기에 정렬된 부분은 역순으로 값이 저장된다.
    // 이러한 이슈를 해결하기 위해 index 마지막부터 시작했다.
    for (int i = length-1; i>=0; i--) {
        int value = --counting[array[i]];
        sorted[value] = array[i];
    }

    // array 로 정렬된 결과 옮기기
    for (int i = 0; i < array.length; i++) {
        array[i] = sorted[i];
    }

    // 메모리 할당 해제
    sorted = null;
}

``` 

- - -
    
### Bucket sort
1. 컴퓨터의 성능을 고려하여 한번에 정렬하고자 하는 수의 비율을 정합니다.
2. 비율에 따른 bucket n개를 준비합니다. (ex. 비율이 10% 이면 n=10)
3. 원소의 크기에 따라 해당하는 bucket에 집어넣습니다. (ex. 98% 의 크기를 가진 원소는 10번째 bucket에)
4. 각 bucket에서 quick sort를 실행합니다.

``` 

/*
숫자의 범위 0 ~ 99
bucket 은 하나당 10 %씩, 총 10개의 bucket 을
수의 개수는 150개이고 균일한 분포를 가졌다고 가정
 */
 
// 정렬에 사용할 bucket 
static int[][] buckets;

public static void bucketSort(int[] array) {

    // buckets init
    buckets = new int[10][15];

    // counting sort 시작
    bucketSort(array, array.length);

    // 메모리 할당 해제
    buckets = null;

}

private static void bucketSort(int[] array, int length) {

    // bucket 에 나누어 담는 과정
    for (int i : array) {

        // n번째 bucket 에 저장
        int n = i/10;

        // 마지막 index 의 value 는 몇번째까지
        // 값이 저장되었는지 알려주는 pointer 로 사용
        // 마지막까지 배열을 채우게 되면 자연스럽게
        // pointer 정보도 삭제됨
        int pointer = buckets[n][15]++;
        buckets[n][pointer] = i;
    }

    for (int[] bucket : buckets) {

        // 각 bucket 에서 퀵 정렬 실행
        // quickSort code 는 위에 있어 생략
        quickSort_middle(bucket, 0, bucket.length - 1);
    }

    // array 의 pointer
    int i = 0;

    // array 로 정렬된 결과 옮기기 
    for(int[] bucket : buckets) {
        for (int num : bucket) {
            array[i++] = num;
        }
    }
}

``` 
- - -

### Radix sort
1. 데이터 중 가장 큰 숫자의 자릿수를 구합니다.
2. 가장 작은 자릿수부터 해당 자릿수만을 보고 counting sort를 진행합니다.
3. 위 3의 과정을 모든 자릿수에서 반복합니다.

``` 

static int[] counting;
static int[] sorted;

public static void RadixSort(int[] array) {

    // 원소의 개수가 0 혹은 1개이면 정렬할 필요가 없다.
    if (array.length < 2) return;

    int length = array.length;

    // max value length 구하기
    int max = Arrays.stream(array).max().orElse(0);

    // radix sort 시작
    RadixSort(array, length, max);
}

private static void RadixSort(int[] array, int length, int max) {

    // 각 자릿수에서 counting sort 실행
    for (int i = 1; i < max; i *= 10) {
        countingSort(array, length, i);
    }
}

private static void countingSort(int[] array, int length, int exp) {

    // memory 할당
    counting = new int[10];
    sorted = new int[length];

    // 과정 3의 counting array 채우기
    for (int i : array) {

        // num 에서 해당 자릿수만 추출
        int idx = (i / exp) % 10;
        counting[idx]++;
    }

    // 과정 4의 counting array 누적합으로 변환
    for (int i = 1; i < counting.length; i++) {
        counting[i] += counting[i - 1];
    }

    // 과정 5의 counting array 를 이용한 정렬 시작
    // 누적합을 이용하는 방식이기에 정렬된 부분은 역순으로 값이 저장된다.
    // 이러한 이슈를 해결하기 위해 index 마지막부터 시작했다.
    for (int i = length - 1; i >= 0; i--) {

        // num 의 해당 자릿수
        int idx = (array[i] / exp) % 10;

        int value = --counting[idx];
        sorted[value] = array[i];
    }

    // array 로 정렬된 결과 옮기기
    for (int i = 0; i < length; i++) {
        array[i] = sorted[i];
    }

    // memory 해제
    counting = null;
    sorted = null;
}

``` 
