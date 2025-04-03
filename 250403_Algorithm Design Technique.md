# 알고리즘 설계기법 (Algorithm Design Technique)
## 탐색 (Searching)
### <순차 탐색>
- 자료구조에서 순차적으로 찾고자 하는 데이터를 탐색
- 시간복잡도 - O(n)
```cs
public static int LinearSearch(int[] array, int target)
{
    for (int i = 0; i < array.Length; i++)
    {
        if (array[i] == target)
        {
            return i;
        }
    }
    return -1;
}
```
### <이진 탐색>
- 정렬이 되어있는 자료구조에서 2분할을 통해 데이터를 탐색
- 단, 이진 탐색은 정렬이 되어 있는 자료에만 적용 가능
- 시간복잡도 - O(logn)
```cs
public static int BinarySearch(int[] array, int target)
{
    int low = 0;
    int high = array.Length - 1;
    while (low <= high) // 찾을때까지 || 없을때까지
    { // 중간위치를 mid 로 잡는다
        int mid = (low + high) / 2;

        if (array[mid] > target) // mid 가 타겟보다 클때
        {
            high = mid - 1; // 오른쪽 값들은 무시 -> high를 mid 한칸 앞으로 옮긴다
        }
        else if (array[mid] < target) // mid가 타겟보다 작을때
        {
            low = mid + 1; // 왼쪽 값들은 무시 -> low를 mid 한칸 뒤로 옮긴다
        }
        else  // if  중간값이 찾고자 하는 값이랑 같은 경우 
        {
            return mid; // 값을 찾았다
        }
    }
    return -1;
}
```
### <깊이 우선 탐색 (Depth-First Search)>
- 그래프의 분기를 만났을 때 최대한 깊이 내려간 뒤,
- 분기의 탐색을 마쳤을 때 다음 분기를 탐색
- 스택을 통해 구현
```cs
public static void DFS(bool[,] graph, int start, out bool[] visited, out int[] parents)
{
    int size = graph.GetLength(0); // 정점의 갯수
    visited = new bool[size];
    parents = new int[size];

    for (int i = 0; i < size; i++)
    {
        visited[i] = false;
        parents[i] = -1;      // 모든 정점의 부모를 -1로 초기화 (출발 정점 제외)
    }
    // 함수 호출 스택을 쓰는 방법
    SearchNode(graph, start, visited, parents);
}

private static void SearchNode(bool[,] graph, int vertex, bool[] visited, int[] parents)
{
    int size = graph.GetLength(0);
    visited[vertex] = true;

    for (int i = 0; i < size; i++)
    {
        if (graph[vertex, i] == true &&       // 현재 정점과 연결된 정점이며
                visited[i] == false)        // 아직 방문하지 않은 정점이면
        {
            parents[i] = vertex;
            SearchNode(graph, i, visited, parents);
        }
    }
}
```
### <너비 우선 탐색 (Breadth-First Search)>
- 그래프의 분기를 만났을 때 모든 분기들을 탐색한 뒤,
- 다음 깊이의 분기들을 탐색
- 큐를 통해 탐색
```cs
public static void BFS(bool[,] graph, int start, out bool[] visited, out int[] parents)
{
    int size = graph.GetLength(0); // 정점의 갯수
    visited = new bool[size];
    parents = new int[size];

    for (int i = 0; i < size; i++)
    {
        visited[i] = false;
        parents[i] = -1;      // 모든 정점의 부모를 -1로 초기화 (출발 정점 제외)
    }

    Queue<int> queue = new Queue<int>();
    queue.Enqueue(start);    // 시작 정점을 큐에 추가
    visited[start] = true;   // 시작 정점을 방문 표시

    while (queue.Count > 0)
    {
        int next = queue.Dequeue();  // 큐에서 다음으로 탐색할 정점을 확인한다;

        for (int vertex = 0; vertex < size; vertex++) // 정점들을 반복하며
        {
            if (graph[next, vertex] == true &&       // 현재 정점과 연결된 정점이며
                visited[vertex] == false)            // 아직 방문하지 않은 정점이면
            {
                queue.Enqueue(vertex);  // 큐에 추가하여 탐색 대기열에 넣음
                visited[vertex] = true; // 방문 표시
                parents[vertex] = next; // 부모 정점 기록 (어디에서 왔는지)

            }
        }
    }
}
```
### <다익스트라 알고리즘>
- 특정한 노드에서 출발하여 다른 노드까지 가는 각각의최단 경로를 구하는 알고리즘
1. 방문하지 않은 노드중에서 가장 가까운 노드를 선택한 후,
2. 선택한 노드를 거쳐서 더 짧아지는 경로가 있는 경우 대체
```cs
const int INF = 99999;
public static void Dijkstra(int[,] graph, int start, out bool[] visited, out int[] parents, out int[] cost)
{
    int size = graph.GetLength(0);
    visited = new bool[size];
    parents = new int[size];
    cost = new int[size];
    for (int i = 0; i < size; i++)
    {
        visited[i] = false;
        parents[i] = -1;
        cost[i] = -1;
    }
    cost[start] = 0;

    for (int i = 0; i < size; i++)
    {
        // 1.방문하지 않은 정점중에서 가장 가까운 정점 선택
        int minIndex = -1;
        int minCost = INF;
        for (int j = 0; j < size; j++)
        {

            if (visited[j] == false &&        // 방문한적 없으며
                cost[j] < minCost)           // 가장 가까운 정점
            {
                minIndex = j;
                minCost = cost[j];
            }
        }
        if (minIndex < 0)
            break;
        visited[minIndex] = true;
        // 2. 직접 연결된 거리보다 거쳐서 더 짧아지면 대체
        for (int j = 0; j < size; j++)
        {
            // cost[j]           : 목적지까지 직접 연결된 거리   (AB)
            // cost[minIndex]    : 중간점까지의 거리             (AC)
            // graph[minIndex,j] : 중간점부터 목적지까지 거리    (CB)
            if (cost[j] > cost[minIndex] + graph[minIndex,j])
            {
                cost[j] = cost[minIndex] + graph[minIndex, j];
                parents[j] = minIndex;
            }
        }
    }
}
```
## 배운것
데이터가 정렬만 되어있다면 이진탐색이 장점이 크고 정렬이 안되어있다면  
순차 탐색을 쓰지만 데이터가 너무 적다면 순차탐색이 더 좋을때도 있다.  
깊이 우선 탐색(DFS)는 모든 노드를 방문하거나 특정 조건을 만족하는 경로를 찾을 때 사용되고  
가중치가 있든 없든 상관 없이 최단 경로를 보장하지않고  
너비 우선 탐색 (BFS)는 가중치가 없다면 최단 경로를 보장하고  
가중치가 있다면 다익스트라를 이용해 최단 경로를 찾는다.