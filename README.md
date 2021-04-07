# Shortest Path(G, s) #

- 입력값 : 가중치 그래프 G(V,E) , |V| = n, |E| = m

- 출력값 : 출발점 s로부터 (n-1)개의 점까지 각각 최단 거리를 저장한 배열 D

------

> Dijkstra의 최단 경로 알고리즘 : 네트워크에서 하나의 시작 정점으로부터 모든 다른 정점까지의 최단 경로를 찾는 알고리즘

- 최단 경로 : 정점 U와 정점 V를 연결하는 경로 중 간선들의 가중치 합이 최소가 되는 경로

- 집합 S : 시작 정점 v로부터의 최단 경로가 이미 발견된 정점들의 집합

- 배열 D: 시작 정점에서 집합 S에 있는 정점만을 거쳐서 다른 정점으로 가능 최단 거리를 기록하는 배열 

- 가중치 -> 가중치 인접 행렬이라고 불리는 2차원 배열에 저장

  가중치는 인접 행렬에 저장되므로 가중치 인접 행렬을 weight라고 했을 때 distance[w] = weight[v] [w] 와 같이 사용할 수 있다.

```java
class graph {
    int n;           //노드의 수
    int[][] weight;       //노드들 간의 가중치 저장 변수

    public graph(int n) {
        this.n = n;
        weight = new int[n + 1][n + 1];
    }

    public void input(int i, int j, int k) {
        weight[i][j] = k;
        weight[j][i] = k;
    }
}
```



1. 출발점 S는 자기 자신과의 거리는 0이므로 D[start] = 0 이다. 최소 거리를 저장하는 배열 D의 크기를 정점의 개수로 정하고, 배열을 무한대로 초기화시킨다.

```java
        int[] D = new int[n + 1];
        boolean[] visited = new boolean[n + 1];   

        for (int i = 1; i < n + 1; i++)
            D[i] = Integer.MAX_VALUE;

        D[start] = 0;
        visited[start] = true;  
```



2. 출발점으로부터 최단 거리가 확정되지 않은 점에 대해서 최소의 가중치 값을을 가진 점을 선택한다. 그리고 출발점으로부터 그 점에 대한 거리를 가지고 배열 D를 갱신한다. 

   (집합 S에 있지 않은 정점 중에서 가장 distance 값이 작은 정점을 S에 추가한다.)

```
        for (int i = 1; i < n + 1; i++)
            if (!visited[i] && G.weight[start][i] != 0)
                D[i] = G.weight[start][i];

        for (int i = 0; i < n - 1; i++) {
            int min = Integer.MAX_VALUE;    
            int min_index = 0;   
```



3. 새로운 정점 u가 S에 추가되면, S에 있지 않은 다른 정점들의 distance 값을 수정한다. 시작 기준점이 u로 바뀌었기 때문에, 새로 추가된 정점 u를 거쳐서 정점까지 가는 거리와 기존의 거리를 비교한다. 그 후 더 작은 거리값을 기준으로 배열 D를 갱신한다.

```java
	for (int j = 1; j < n + 1; j++)
		if (!visited[j] && D[j] != Integer.MAX_VALUE) {
			if (D[j] < min) {
				min = D[j];
			min_index = j;
			}
		}
	visited[min_index] = true;
            
	for (int k = 1; k < n + 1; k++)
		if (!visited[k] && G.weight[min_index][k] != 0)
			if (D[k] > D[min_index] + G.weight[min_index][k])
				D[k] = D[min_index] + G.weight[min_index][k];
```



------

## **시간복잡도**

> 네트워크에 n개의 정점이 있다면, Dijkstra 최단 경로 알고리즘은 주 반복문을 n번 반복하고 내부 반복문을 2n번 반복하므로 O(n^2)의 시간 복잡도를 가진다.

**WHY**?

while 루프가 (n - 1)번 반복되고, 1회 반복될 때 최소의 D[v]를 가진 점 Vmin을 찾는 것에 O(n) 시간이 걸린다.

-> 배열 D에서 최소값을 찾는 것이기 때문

위의 3번 코드에서도 Vmin에 연결된 점의 수가 최대 (n-1)개이므로, 각 D[w]를 갱신하는데 걸리는 시간은 O(n)이다.

***따라서, 시간 복잡도는 (n - 1) x {O(n) + O(n)} = O(n^2)이다.***
