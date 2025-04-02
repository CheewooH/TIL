# 알고리즘 설계기법 (Algorithm Design Technique)
## 알고리즘 조건
1. 입력 : 알고리즘은 0개 이상의 입력을 가져야 함
2. 출력 : 알고리즘은 최소 1개 이상의 결과를 가져야 함
3. 명확성 : 수행 과정은 모호하지 않고 정확한 수단을 제공해야 함
4. 유한성 : 수행 과정은 무한하지 않고 유한한 작업 이후에 정지해야 함
5. 효과성 : 모든 과정은 명백하게 실행 가능해야 함
```cs

```
## 재귀 (Recursion)
어떠한 것을 정의할 때 자기 자신을 참조하는 것  
함수를 정의할 때 자기자신을 이용하여 표현하는 방법
### <재귀함수 조건>
1. 함수내용 중 자기자신함수를 다시 호출해야함
2. 종료조건이 있어야 함
### <재귀함수 장점>
1. 코드로 표현하기 어려운 경우도 직관적이고 처리가 가능
2. 분할정복을 통한 반절 계산이 가능해서 효율이 높아지게 구현이 가능
### <재귀함수 사용>
Factorial : 정수를 1이 될 때까지 차감하며 곱한 값
```cs
x! = x * (x-1)!;  
1! = 1;  
ex) 5! = 5 * 4! 
       = 5 * 4 * 3!  
       = 5 * 4 * 3 * 2!  
       = 5 * 4 * 3 * 2 * 1!  
       = 5 * 4 * 3 * 2 * 1  
```
## 정렬 (Sorting)

### <선택정렬>
데이터 중 가장 작은 값부터 하나씩 선택하여 정렬  
시간복잡도 -  O(n²)  
공간복잡도 -  O(1)
```cs
public static void SelectionSort(int[] array)
{
    for (int i = 0; i < array.Length; i++)
    {
        int minIndex = i;
        for (int j = i; j < array.Length; j++)
        {
            if (array[j] < array[minIndex])
            {
                minIndex = j;
            }
        }
        Swap(array, i, minIndex);
    }
}
public static void Swap(int[] array, int left, int right)
{
    int temp = array[left];
    array[left] = array[right];
    array[right] = temp;
}
```
### <삽입정렬>
데이터를 하나씩 꺼내어 정렬된 자료 중 적합한 위치에 삽입하여 정렬  
시간복잡도 -  O(n²)  
공간복잡도 -  O(1)  
안정정렬   -  O
```cs
public static void InsertionSort(int[] array)
{
    for (int i = 1; i < array.Length; i++)
    {
        for (int j = i; j > 0; j--)
        {
            if (array[j - 1] > array[j])
            {
                Swap(array, j - 1, j);
            }
            else
            {
                break;
            }
        }
    }
}
```
### <버블정렬>
서로 인접한 데이터를 비교하여 정렬  
시간복잡도 -  O(n²)  
공간복잡도 -  O(1)  
안정정렬   -  O
```cs
public static void BubbleSort(int[] array)
{
    for (int i = 1; i < array.Length; i++) // 처음부터 끝까지 반복
    {
        for (int j = 0; j < array.Length - i; j++) // 하나씩 골라서 하는 작업을 반복
        {
            if (array[j] > array[j + 1])  // 서로 인접한 두 데이터를 비교해서 더 크면
            {
                Swap(array, j, j + 1);
            }
        }
    }
}
```
### <병합정렬>
데이터를 2분할하여 정렬 후 병합  
시간복잡도 -  O(nlogn)  
공간복잡도 -  O(n)
```cs
public static void MergeSort(int[] array) => MergeSort(array, 0, array.Length - 1);
public static void MergeSort(int[] array, int start, int end)
{
    if (start == end)
        return;

    int mid = (start + end) / 2;
    MergeSort(array, start, mid);
    MergeSort(array, mid + 1, end);
    Merge(array, start, mid, end);
}

public static void Merge(int[] array, int start, int mid, int end)
{
    List<int> sortedList = new List<int>();
    int leftIndex = start;
    int rightIndex = mid + 1;

    while (leftIndex <= mid && rightIndex <= end)
    {
        if (array[leftIndex] < array[rightIndex])
        {
            sortedList.Add(array[leftIndex]);
            leftIndex++;
        }
        else
        {
            sortedList.Add(array[rightIndex]);
            rightIndex++;
        }
    }

    if (leftIndex <= mid)
    {
        while (leftIndex <= mid)
        {
            sortedList.Add(array[leftIndex]);
            leftIndex++;
        }
    }
    else // if (rightIndex <= end)
    {
        while (rightIndex <= end)
        {
            sortedList.Add(array[rightIndex]);
            rightIndex++;
        }
    }

    for (int i = 0; i < sortedList.Count; i++)
    {
        array[start + i] = sortedList[i];
    }
}
```
### <퀵정렬>
하나의 피벗을 기준으로 작은값과 큰값을 2분할하여 정렬  
최악의 경우(피벗이 최소값 또는 최대값)인 경우 시간복잡도가 O(n²)  
시간복잡도 -  평균 : O(nlogn)   최악 : O(n²)  
공간복잡도 -  O(1)  
안정정렬   -  X
```cs
public static void QuickSort(int[] array) => QuickSort(array, 0, array.Length - 1);
public static void QuickSort(int[] array, int start, int end)
{
    if (start >= end)
        return;

    int pivot = start;
    int left = pivot + 1;
    int right = end;


    while (left <= right) // 왼쪽과 오른쪽이 교차할때까지 반복
    {
        while (array[pivot] >= array[left] && left < right) // 왼쪽 은 pivot보다 크거나 같고 오른쪽보다 작을때까지
        {
            left++;
        }
        while (array[pivot] < array[right] && left <= right) // 오른쪽은 pivot보다 
        {
            right--;
        }

        if (left < right) // 왼쪽과 오른쪽이 교차 안했다면 
        {
            Swap(array, left, right); // 왼쪽과 오른쪽 값을 교체
        }
        else
        {
            Swap(array, pivot, right); // pivot과 오른쪽을 교체
            break;
        }
    }

    QuickSort(array, start, right - 1);
    QuickSort(array, right + 1, end);
}
```
## 느낀점
오늘 배우고 나서 레퍼런스로 5가지 알고리즘의 컨셉 동작방법을 슈도코드로 만들고  
코드작성까지 해봤는데 코드작성이 처음부터 막히는 순간이 슈도 코드작성으로  
뭔가 길이 열리는 느낌을 받아서 슈도코드 또는 플로우 차트로 만들어서 설계를 하고  
코드를 추가하는 연습을 해야겠다고 느꼈다.

