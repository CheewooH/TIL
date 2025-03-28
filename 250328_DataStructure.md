# 자료구조를 활용한 데이터 관리

## 알고리즘 (Algorithm)

문제를 해결하기 위해 정해진 진행절차나 방법
컴퓨터에서 알고리즘은 어떠한 행동을 하기 위해서 만들어진 프로그램 명령어의 집합

### 알고리즘 조건
1. **입력** : 알고리즘은 0개 이상의 입력을 가져야 함
2. **출력** : 알고리즘은 최소 1개 이상의 결과를 가져야 함
3. **명확성** : 수행 과정은 모호하지 않고 정확한 수단을 제공해야 함
4. **유한성** : 수행 과정은 무한하지 않고 유한한 작업 이후에 정지해야 함
5. **효과성** : 모든 과정은 명백하게 실행 가능해야 함

## 자료구조 (Data Structure)

프로그래밍에서 데이터를 효율적으로 접근 및 수정할 수 있도록 하는 자료의 조직, 관리, 저장 방식

### 자료구조 형태
- **선형구조** : 자료 간 관계가 1 대 1인 구조
  - 배열, 연결 리스트, 스택, 큐, 덱
- **비선형구조** : 자료 간 관계가 1 대 다 혹은 다 대 다인 구조
  - 트리, 그래프

## 알고리즘 성능 평가

효율적인 문제 해결을 위해 알고리즘의 성능을 판단하는 기준이 필요하며, 시간과 공간 자원의 소비량을 측정함

### 알고리즘 평가 기준
- **시간복잡도** : 알고리즘의 수행 시간 분석
- **공간복잡도** : 알고리즘이 사용하는 메모리 크기 분석

### Big-O 표기법
알고리즘의 복잡도를 나타내는 점근 표기법으로 데이터 증가에 따른 시간 증가를 계산

## 수행 시간 분석

알고리즘의 성능은 평균의 상황과 최악의 상황을 기준으로 평가함

## 리스트 (List)

런타임 중 크기를 확장할 수 있는 배열 기반의 자료구조  
배열요소의 갯수를 특정할 수 없는 경우 사용이 용이
```cs
List<string> list = new List<string>();

// 추가
list.Add("0번 데이터");           // 맨 뒤에 추가하기 : 0(1)
list.Add("1번 데이터");
list.Add("2번 데이터");
list.Insert(1, "중간 데이터1");   // 중간에 끼워넣기  : 0(n)
list.Insert(3, "중간 데이터2");
list.Insert(5, "끝");

// 삭제 : 0(n)
list.Remove("1번 데이터");           // 똑같은거 찾아서 지우기 같은데이터가 여러개 라면 앞에서부터 찾은걸 지운다 : 0(n)
list.RemoveAt(1);                    // 특정 위치에있는 요소 지우기 : 0(n)
list.Remove("3번 데이터");           // 찾아서 지울때 없었으면 무시 ( 반환은 false)

// 접근  : 0(1)
list[0] = "데이터0";                 // 리스트는 배열로  구현되어있기 떄문에 인덱스를 통한 접근이가능하다.
string value = list[0];


// 탐색 : 0(n)
int indexOf = list.IndexOf("2번 데이터");  // 찾아서 인덱스 가져오기
bool contain = list.Contains(" ");         // 찾아서 있으면trus 없으면 false

int count = list.Count;        // 리스트의 크기 
int capacity = list.Capacity;  // 리스트의 용량

// List<T>.TrimExcess // TrimExcess()는 메모리 최적화를 위해 리스트의 용량을 현재 개수에 맞게 줄이는 메서드!
                      // 하지만 리스트를 다시 추가할 가능성이 있으면 사용하지 않는 게 더 나을 수도 있음!
```
## 연결 리스트 (Linked List)
데이터를 포함하는 노드들을 연결식으로 만든 자료구조
```cs
static void Main(string[] args)
{
    LinkedList<string> linkedList = new LinkedList<string>();

    // 추가
    LinkedListNode<string> node0 = linkedList.AddFirst("0번 데이터");           // 
    LinkedListNode<string> node1 = linkedList.AddLast("1번 데이터");
    LinkedListNode<string> node2 = linkedList.AddLast("2번 데이터");
    LinkedListNode<string> node3 = linkedList.AddFirst("3번 데이터");
    LinkedListNode<string> node4 = linkedList.AddBefore(node0, "4번 데이터");   // 노드 앞에 붙히기
    LinkedListNode<string> node5 = linkedList.AddAfter(node0, "5번 데이터");    // 노드 뒤에 붙히기

    // Console.WriteLine(node1.Value);            // "1번 데이터"
    // Console.WriteLine(node1.Next.Value);       // "2번 데이터"
    // Console.WriteLine(node1.Previous.Value);   // "5번 데이터"
    foreach (string i in linkedList)
    {
        Console.WriteLine(i);
    }

    // 삭제
    linkedList.Remove(node1);         // O(1)
    linkedList.RemoveFirst();         // O(1)
    linkedList.RemoveLast();          // O(1)
    linkedList.Remove("1번 데이터");  // O(n)

    // 접근
    // linkedList[2] : 연결리스트는 인덱스가 없다(연속적으로 저장하지않기 떄문에 인덱스 사용이 불가능하다)
    LinkedListNode<string> prevNode = node0.Previous;
    LinkedListNode<string> nextNode = node0.Next;
    LinkedListNode<string> firstNode = linkedList.First;
    LinkedListNode<string> lastNode = linkedList.Last;


    // 탐색
    LinkedListNode<string> findNode = linkedList.Find("5번 데이터");
    bool contain = linkedList.Contains("1번 데이터");

    int[] ararry = new int[8];
    for (int i = 0; i < ararry.Length; i++)
    {
        Console.WriteLine(ararry[i]);
    }

    foreach (int value in ararry)
    {
        Console.WriteLine(value);
    }

    LinkedList<int> list = new LinkedList<int>();
    for (LinkedListNode<int> node = list.First; node != null; node = node.Next)
    {
        Console.WriteLine(node.Value);
    }

    foreach (int value in list)
    {
        Console.WriteLine(value);
    }
}
```

### 연결 리스트 종류
1. **단방향 연결 리스트** : 노드가 다음 노드를 참조
2. **양방향 연결 리스트** : 노드가 이전/다음 노드를 참조
3. **환형 연결 리스트** : 마지막 노드가 첫 번째 노드를 참조

## 스택 (Stack)
- 후입선출(LIFO, Last In First Out) 구조의 자료구조
- `Push`(추가), `Pop`(제거), `Peek`(다음 데이터 확인, 꺼내진 않음)
```cs
 static void Main(string[] args)
 {
     Stack<int> stack = new Stack<int>();

     for (int i = 0; i < 5; i++)
     {      // push : 추가 O(1), 최악(용량이 가득찼을때) O(n) 
         stack.Push(i);                          // 입력순서 : 0, 1, 2, 3, 4
     }
                          // Peek : 다음요소 확인만
     Console.WriteLine(stack.Peek());            // 최상단 : 4

     for (int i = 0; i < 3; i++)
     {                        // Pop : 꺼내기
         Console.WriteLine(stack.Pop());         // 출력순서 : 4, 3, 2
     }

     for (int i = 5; i < 10; i++)
     {
         stack.Push(i);                          // 입력순서 : 5, 6, 7, 8, 9
     }

     while (stack.Count > 0)
     {
         Console.WriteLine(stack.Pop());         // 출력순서 : 9, 8, 7, 6, 5, 1, 0
     }
     // 있으면 꺼낸다
     stack.TryPop(out int pop);
 }
```

## 큐 (Queue)
선입선출(FIFO, First In First Out) 구조의 자료구조  
입력된 순서대로 처리해야 하는 상황에 이용
- `Enqueue`(데이터 추가), `Dequeue`(데이터 제거), `Peek`(다음 데이터 확인, 꺼내진 않음)
```cs
static void Main(string[] args)
{
    Queue<int> queue = new Queue<int>();

    for (int i = 0; i < 5; i++)
    {
        queue.Enqueue(i);                       // 입력순서 : 0, 1, 2, 3, 4
    }

    Console.WriteLine(queue.Peek());            // 다음순서 : 0

    for (int i = 0; i < 3; i++)
    {
        Console.WriteLine(queue.Dequeue());     // 출력순서 : 0, 1, 2
    }

    Console.WriteLine(queue.Peek());            // 다음순서 : 3

    for (int i = 5; i < 10; i++)
    {
        queue.Enqueue(i);                       // 입력순서 : 5, 6, 7, 8, 9
    }

    Console.WriteLine(queue.Peek());            // 다음순서 : 3

    while (queue.Count > 0)
    {
        Console.WriteLine(queue.Dequeue());     // 출력순서 : 3, 4, 5, 6, 7, 8, 9
    }
}
```
