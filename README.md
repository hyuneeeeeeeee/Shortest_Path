# Shortest_Path

- #### 입력값 : 가중치 그래프 G(V,E), |V|=n, |E|=m

- #### 출력값 : 출발점 s로부터 (n-1)개의 점까지 각각 최단 거리를 저장한 배열 D



## 입력

##### 정점과 간선의 수, 출발점을 입력하고, for문을 통해 간선 사이의 거리를 입력한다.

```java
int node, line, s;
node = scanner.nextInt();      
line = scanner.nextInt();      
s = scanner.nextInt();        

graph G = new graph(node);
for (int a = 0; a < line; a++) {
	int i = scanner.nextInt();
	int j = scanner.nextInt();
	int k = scanner.nextInt();
	G.input(i, j, k);
    }
```

1.  배열 D를 ∞ 로 초기화시킨다.  

   ```java
   int[] D = new int[n + 1]; 
   for (int i = 1; i < n + 1; i++)
   	D[i] = Integer.MAX_VALUE;
   ```

2.  while(s로부터의 최단 거리가 확정되지 않은 점이 있으면) visited 배열에서 false 값으로 둔다.

   ```java
   boolean[] visited = new boolean[n + 1];
   visited[start] = true
   ```

3.  현재까지 s로부터 최단거리가 확정되지 않은 각 점 v에 대해서 최소의 D[v]의 값을 가진 점 min_index를 선택하고, 출발점 s로부터 점 min_index까지의 최단 거리 D[min_index]를 확정한다.

   ```java
   for (int j = 1; j < n + 1; j++)
   	if (!visited[i] && D[j] != Integer.MAX_VALUE) {      
   		if (D[j] < min) {
   			min = D[j];
               min_index = j;
           }
       }
   visited[min_index] = true;  
   ```

4.  s로부터 현재보다 짧은 거리로 점 min_index를 통해 우회 가능한 각 점 k에 대해서 D[k]를 갱신한다.

   ```
   for (int k = 1; k < n + 1; k++)
   	if (!visited[k] && G.weight[min_index][k] != 0)       
       	if (D[k] > D[min_index] + G.weight[min_index][k]) 
           	D[k] = D[min_index] + G.weight[min_index][k];
   ```

   

