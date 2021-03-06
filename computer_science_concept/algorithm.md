## 정렬
- O(n^2)
  - 선택 정렬 : 가장 작은 원소를 찾아 0번부터 올바른 위치에 배열
    ```java
    private static void selectionSort(int arr[]){
      for (int i = 0; i < arr.length - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < arr.length; j++) {
          if(arr[j] < arr[minIndex])
            minIndex = j;
        }
        swap(arr, i, minIndex);
      }
    }
    ```
  - 버블 정렬 : 가장 큰 원소를 오른쪽 끝으로 보내며 순차적으로 윈도우를 왼쪽으로 옮기며 정렬
    ```java
    private static void bubbleSort(int arr[]){
      for (int i = arr.length - 1; i > 0; i--) {
        for (int j = 0; j < i; j++) {
          if(arr[j] > arr[j + 1])
            swap(arr, j, j + 1);
        }
      }
    }
    ```
  - 삽입 정렬 : 윈도우를 0번부터 시작하여 끝가지 이동하며 지금까지 정렬된 값 중 올바른 위치에 삽입
    ```java
    private static void insertionSort(int arr[]){
      for (int i = 1; i < arr.length; i++) {
        for (int j = 0; j < i; j++) {
          if(arr[j] > arr[i]){
            int temp = arr[i];
            for (int k = i; k > j; k--) {
              arr[k] = arr[k - 1];
            }
            arr[j] = temp;
            break;
          }
        }
      }
    }
    ```
- O(nlogn)
  - 합병 정렬 : 배열을 이등분하여 각각 정렬한 후 합병
    ```java
    private static void mergeSort(int arr[]){
      internalMergeSort(arr, 0, arr.length - 1);
    }
    
    private static void internalMergeSort(int arr[], int left, int right){
      int mid = (left + right) / 2;
		
      if(left < right){
        internalMergeSort(arr, left, mid);
        internalMergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);	
      }
    }

    private static void merge(int arr[], int left, int mid, int right){
      int rs[] = new int[right - left + 1];
      int rsIndex = 0;
      int leftIndex = left;
      int rightIndex = mid + 1;
	
      while(leftIndex <= mid && rightIndex <= right){
        if(arr[leftIndex] <= arr[rightIndex])
          rs[rsIndex++] = arr[leftIndex++];
        else
          rs[rsIndex++] = arr[rightIndex++];
      }
		
      while(leftIndex <= mid)
        rs[rsIndex++] = arr[leftIndex++];
		
      while(rightIndex <= right)
        rs[rsIndex++] = arr[rightIndex++];
		
		
      for (int i = 0; i < rs.length; i++) {
        arr[left + i] = rs[i];
      }
    }
    ```
  - 퀵 정렬 : 한 원소를 pivot으로 선정하고 pivot 기준으로 2개의 파티션으로 분할한 뒤 왼쪽 파티션에는 pivot보다 작은 값, 오른쪽 파티션에는 pivot보다 크거나 같은 값을 배치하여 순환 적용
    ```java
    public static void quickSort(int[] arr){		
      internalQuickSort(arr, 0, arr.length-1);
    }
	
    private static void internalQuickSort(int[] arr, int left, int right){
      if(left > right)						
        return ;

      int p = partition(arr, left, right);      //p는 파티션이 끝난 뒤에 사용된 피봇의 인덱스
        internalQuickSort(arr, left, p-1);      //앞부분
        internalQuickSort(arr, p+1, right);     //뒷부분
      }
	
    private static int partition(int arr[], int left, int right){
      swap(arr, left, (left + right) / 2);	//pivot을 중간값과 바꿔서 평균 O(nlogn)이 되도록 함
      int pivot = arr[left];                    //맨 좌측 원소값을 pivot으로 잡음

      int p = left;                             //p는 두 파티션의 경계 인덱스

      for(int i = left + 1; i <= right; i++){   //a[i+1] ~ a[j]에 있는 모든 원소를 검사하여
        if(arr[i] < pivot){                     //a[k]가 pivot보다 작으면 
          p++;                                  //p를 1 증가시켜 a[k]를 p인덱스 범위 안으로 포함되게 함
          swap(arr, p, i);                      //a[p] 와 a[k] 위치 교환
        }
      }

      swap(arr, left, p);			//a[i]와 a[p]위치 교환
		
      return p;
    }	
    ```
  - 히프 정렬 : 정렬할 원소를 모두 공백 히프에 삽입한 뒤 루트 노드를 차례로 삭제하여 리스트 뒤에 삽입
  - 트리 정렬 : 배열을 (중복이 허용되는)이진 정렬 트리에 삽입한 뒤, 중위 우선 순회 방법으로 원소를 하나씩 검색하여 원래의 배열에 저장
- O(n^1.25)
  - 쉘 정렬 : 원소의 비교 연산과 먼 거리의 이동을 줄이기 위해 몇 개의 서브리스트로 나누어 삽입 정렬을 반복 수행
    - 간격(interval)이 작을수록 짧은 거리를 이동하고, 원소의 이동이 적다
- O(n)
  - 기수 정렬 : 정렬할 원소의 키값을 나타내는 숫자의 자리수를 기초로 정렬하는 기법
  
## 탐색
- 이진 탐색
  ```java  
  import java.io.BufferedWriter;
  import java.io.IOException;
  import java.io.OutputStreamWriter;

  public class BinarySearch {
    //재귀로 구현
    private static int binSearch(int arr[], int left, int right, int value){
      if(left > right)
        return -1;
		
      int mid = (left + right) / 2;
		
      if(arr[mid] == value)
        return mid;
      if(arr[mid] < value)
        return binSearch(arr, mid + 1, right, value);
      if(arr[mid] > value)
        return binSearch(arr, left, mid - 1, value);
		
      return -1;	//This will not occured
    }

    //반복으로 구현
    private static int binSearch(int arr[], int value){
      int left = 0;
      int right = arr.length - 1;
		
      while(left <= right){
        int mid = (left + right) / 2;
			
        if(arr[mid] == value)
          return mid;
        if(arr[mid] < value)
          left = mid + 1;
        if(arr[mid] > value)
          right = mid - 1;
      }
		
      return -1;
    }
	
    public static void main(String[] args) throws IOException {
      BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
      int arr[] = {1,2,3,4,5,6,7,8,9};
		
      bw.write("-----recursive binSearch()-----\n");
      for (int i = 0; i < arr.length + 2; i++) {
        bw.write(i + " : " + binSearch(arr, 0, arr.length - 1, i) + "\n");
      }
		
      bw.write("-----binSearch() without recursive call-----\n");
      for (int i = 0; i < arr.length + 2; i++) {
        bw.write(i + " : " + binSearch(arr, i) + "\n");
      }
		
      bw.close();
    }
  }
  ```
 
## 최단 경로
- 하나의 정점에서 다른 모든 정점까지의 최단 경로 : Dijkstra 알고리즘
- 음의 가중치가 허용된 최단 경로 : Bellman and Ford 알고리즘
- 모든 정점 쌍의 최단 경로 : allShortestPath 알고리즘
- 이행적 폐쇄 행렬 : 가중치 없는 방향 그래프에서 경로의 존재하는지 표현
  
## 위상 순서(Topological order)
- AOV(Activity on vertex) 네트워크 : 작업간의 선후 관계를 나타내는 방향 그래프
  - 정점들은 서로 선행자와 후속자의 관계로 이루어짐
- 위상 순서 : 방향 그래프에서 두 정점 i와 j에 대해, i가 j의 선행자이면 반드시 i가 j보다 먼저 나오는 정점의 순차 리스트

## 임계 경로(Critical path)
- AOE(Activity on edge) 네트워크 : 프로젝트의 스케쥴을 표현하는 DAG(Directed acyclic graph, 사이클이 없는 방향 그래프)
  - 정점 : 프로젝트 수행을 위한 공정 단계
  - 간선 : 공정들의 선후 관계와 각 공정의 작업 소요 시간
- 임계 경로 : 시작점에서 완료점까지 시간이 가장 많이 걸리는 경로
- 공정 P의 조기 완료 시간(EC, Earliest completion time) : 시작점부터 공정 P까지의 최장 경로 길이
- 공정 P의 완료 마감 시간(LC, Latest completion time) : 전체 프로젝트의 최단 완료 시간에 영향을 주지 않고, 공정 P가 여유를 가지고 지연하여 완료할 수 있는 시간
- 공정 P의 임계도(criticality) : EC(i)와 LC(i)의 시간 차이
- 임계 작업(critical activity) : 임계 경로 상에 있는 작업들

## 해슁(Hashing)
- reference : (1)[GeeksforGeeks 설명(set2)](https://www.geeksforgeeks.org/hashing-set-2-separate-chaining/) (2)[GeeksforGeeks 설명(set3)](https://www.geeksforgeeks.org/hashing-set-3-open-addressing/) (3)[step by step diagrams](https://www.cse.cuhk.edu.hk/irwin.king/_media/teaching/csc2100b/tu6.pdf)
- 해쉬 함수(Hash function)
  - key를 0 ~ TableSize -1 까지 매핑하는 함수
  ```java
  int hashfunc(int integer_key){
    return integer_key % HASHTABLEZIE;
  }
  ```
- 충돌 해결법(handling collisions)
  - separate chaing(closed addressing, 체이닝)
    - 해쉬 테이블의 각 셀을 linked list로 연결하는 방법
    - 장점 : 구현이 간단, 해쉬 테이블이 계속해서 확장 가능, 해쉬 함수나 요소를 읽는데 덜 민감, 삽입 삭제의 규모를 모를 때 유용
    - 단점 : 체이닝의 캐쉬 성능이 떨어짐, 공간이 낭비됨(일부 해쉬 테이블은 사용되지 않음), 체인이 길어지면 탐색시간이 선형( O(n))으로 증가, 링크에 추가 공간을 사용
    - 시간 복잡도 : 탐색, 삽입, 삭제 시간 = O(1)
      - 단, 체이닝이 길어질 경우 시간 복잡도는 증가할 수 있다.
  - open addressing(개방 주소법)
    - key의 개수보다 table의 개수가 더 많아야 함
    - 각 table은 state라는 상태를 가져야 한다
      - state = lazy delete(삭제될 상태) or empty(빈 상태) or occupied(값을 가진 상태)
    - 연산
      - 삽입 : 빈 공간을 찾을 때 까지 추가 해싱. 빈 공간이 발견되면 그 위치에 삽입
      - 탐색 : 빈 공간의 key가 찾고자 하는 key와 일치하는 동안 계속 탐색
      - 삭제 : 실제 데이터를 삭제하지 않고, 마크 비트만 deleted로 바꿈
        - insert 연산시 deleted 상태의 슬롯에는 데이터 삽입 가능
	- search 연산시 state가 lazy delete라면 해당 슬롯은 무시
    - 중복 해슁 기법(probing, 충돌시 지속적으로 hashing 적용) : 충돌 발생시 다음 해쉬를 결정하는 방법
      - 선형 탐색법(Linear probing)
        ```
        실패시 다음 라인 수행
        (hash(x) + 0) mod TableSize 
        (hash(x) + 1) mod TableSize
        (hash(x) + 2) mod TableSize
        ...
        ```
      - 제곱 탐색법(Quadratic probing)
        ```
        실패시 다음 라인 수행
        (hash(x) + 0^2) mod TableSize 
        (hash(x) + 1^2) mod TableSize
        (hash(x) + 2^2) mod TableSize
        ...
        ```
      - 이중 해슁(Double Hashing)
        ```
        실패시 다음 라인 수행
        (hash(x) + 1 * hash2(x)) mod TableSize 
        (hash(x) + 2 * hash2(x)) mod TableSize
        (hash(x) + 3 * hash2(x)) mod TableSize
        ...
      - 무작위 탐색법(Random probing)
  
# 알고리즘 기법
- [KMP 알고리즘](https://bowbowbow.tistory.com/6) : 문자열 검색을 빠르게 하는 알고리즘
