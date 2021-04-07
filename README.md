# Shortest_Path

Shortest Path 문제는 주어진 가중치 그래프에서 어느 한 출발점에서 또 다른 도착점까지의 최단경로를 찾는 문제이다.
다익스트라(Dijkstra) 알고리즘을 이용해 최단 경로를 찾는다.


> #### 다익스트라 알고리즘
주어진 출발점에서 시작해 출발점으로부터 최단 거리가 확정되지 않은 점들 중에서 출발점으로부터 가장 가까운 점을 추가하고, 그 점의 최단 거리를 확정한다.

-------


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

각각의 노드들과 노드들 간의 가중치를 저장할 `graph` 클래스이다.
정점의 수와 가중치, 출발점 등은 `scanner`를 이용해 입력받도록 한다.


------

```java
public static int[] dijkstra(graph G, int start, int n) {
        int[] D = new int[n + 1];          //최단 거리 저장 변수
        boolean[] visited = new boolean[n + 1];     //해당 노드를 방문했는지 체크할 변수

        //시작점부터 각 노드까지의 모든 거리 무한대로 초기화
        for (int i = 1; i < n + 1; i++)
            D[i] = Integer.MAX_VALUE;

        //시작 노드 값 초기화
        D[start] = 0;
        visited[start] = true;

        //연결 노드 D 갱신
        for (int i = 1; i < n + 1; i++)
            if (!visited[i] && G.weight[start][i] != 0)      //방문하지 않았고 시작점에서 i까지의 가중치가 존재한다면, 거리 i에 시작점에서 i까지의 가중치 저장
                D[i] = G.weight[start][i];

        for (int i = 0; i < n - 1; i++) {
            int min = Integer.MAX_VALUE;    //가장 작은 값 기억 변수
            int min_index = 0;     //가장 작은 값의 위치 기억 변수

            //최소값 찾기
            for (int j = 1; j < n + 1; j++)
                if (!visited[j] && D[j] != Integer.MAX_VALUE) {       //방문하지 않았고 거리를 갱신한 노드들 중에서 가장 가까운 거리와 가장 가까운 노드를 구하기
                    if (D[j] < min) {
                        min = D[j];
                        min_index = j;
                    }
                }
            visited[min_index] = true;  //위의 반복문을 통해 도출된 가장 가까운 노드에 방문 표시

            for (int k = 1; k < n + 1; k++)
                if (!visited[k] && G.weight[min_index][k] != 0)      //방문하지 않았고 min_index와의 가중치가 존재하는 노드라면 (min_index에서 연결되어있어야 함)
                    if (D[k] > D[min_index] + G.weight[min_index][k]) //지금 그 노드가 가지고 있는 거리 값이 min_index와 가중치를 더한 값보다 크다면 최단거리 갱신
                        D[k] = D[min_index] + G.weight[min_index][k];
        }

        return D;
    }
```

최단 거리를 저장할 배열 `D`와 각 노드들을 방문했는지 체크할 배열 `visited`를 선언하고 시작노드 값을 초기화한다.
`D`의 시작점부터 각 노드까지의 모든 거리를 무한대로 초기화한다.

최단 거리가 확정되지 않았으며 시작점에서 i까지의 가중치가 존재하는 노드들에 대해 시작점에서 i까지의 가중치를 `D`에 저장한다.

`for` 반복문을 n-1회 수행하며 각 노드들의 최단 거리를 확정한다.

가장 작은 값을 기억할 변수 `min`과 가장 작은 값의 위치를 기억할 변수 `min_index`를 각각 무한대, 0으로 초기화 한다.
`for` 반복문을 통해 방문하지 않았고 거리를 갱신한 노드들 중에서 가장 가까운 거리와 가장 가까운 노드를 구한다.
반복문을 통해 도출된 가장 가까운 노드에는 방문했음을 표시한다.

방문하지 않았던 노드들 가운데 `min_index`와의 가중치가 존재하는 노드들에 대해서 현재 해당 노드가 가지고 있는 `D` 값이 `min_index`를 거쳐 감으로서 더 작아진다면 최단거리를 갱신한다.

최종적으로 확정된 각 노드들의 최단 거리를 저장한 `D`를 `return`한다.


------

전체 코드는 다음과 같다.
```java
import java.util.Scanner;

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

public class ShortestPath {
    public static int[] dijkstra(graph G, int start, int n) {
        int[] D = new int[n + 1];          //최단 거리 저장 변수
        boolean[] visited = new boolean[n + 1];     //해당 노드를 방문했는지 체크할 변수

        //시작점부터 각 노드까지의 모든 거리 무한대로 초기화
        for (int i = 1; i < n + 1; i++)
            D[i] = Integer.MAX_VALUE;

        //시작 노드 값 초기화
        D[start] = 0;
        visited[start] = true;

        //연결 노드 D 갱신
        for (int i = 1; i < n + 1; i++)
            if (!visited[i] && G.weight[start][i] != 0)      //방문하지 않았고 시작점에서 i까지의 가중치가 존재한다면, 거리 i에 시작점에서 i까지의 가중치 저장
                D[i] = G.weight[start][i];

        for (int i = 0; i < n - 1; i++) {
            int min = Integer.MAX_VALUE;    //가장 작은 값 기억 변수
            int min_index = 0;     //가장 작은 값의 위치 기억 변수

            //최소값 찾기
            for (int j = 1; j < n + 1; j++)
                if (!visited[j] && D[j] != Integer.MAX_VALUE) {       //방문하지 않았고 거리를 갱신한 노드들 중에서 가장 가까운 거리와 가장 가까운 노드를 구하기
                    if (D[j] < min) {
                        min = D[j];
                        min_index = j;
                    }
                }
            visited[min_index] = true;  //위의 반복문을 통해 도출된 가장 가까운 노드에 방문 표시

            for (int k = 1; k < n + 1; k++)
                if (!visited[k] && G.weight[min_index][k] != 0)      //방문하지 않았고 min_index와의 가중치가 존재하는 노드라면 (min_index에서 연결되어있어야 함)
                    if (D[k] > D[min_index] + G.weight[min_index][k]) //지금 그 노드가 가지고 있는 거리 값이 min_index와 가중치를 더한 값보다 크다면 최단거리 갱신
                        D[k] = D[min_index] + G.weight[min_index][k];
        }

        return D;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int node, line, s;
        node = scanner.nextInt();      //정점의 수
        line = scanner.nextInt();      //간선의 수
        s = scanner.nextInt();         //출발점 s

        graph G = new graph(node);
        for (int a = 0; a < line; a++) {
            int i = scanner.nextInt();
            int j = scanner.nextInt();
            int k = scanner.nextInt();
            G.input(i, j, k);
        }

        int[] D = new int [node + 1];
        D = dijkstra(G, s, node);

        for (int i = 1; i < node + 1; i++)
            System.out.print(D[i] + " ");

        scanner.close();
    }
}
```