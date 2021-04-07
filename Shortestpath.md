```
import java.util.Scanner;

class graph {
    int n;
    int[][] w;

    public graph(int n) {
        this.n = n;
        w = new int[n + 1][n + 1];
    }

    public void input(int i, int j, int k) {
        w[i][j] = k;
        w[j][i] = k;
    }
}

public class ShortestPath {
    public static int[] shortestpath(graph G, int s, int n) {
        int[] D = new int[n + 1];    //출발점 s로부터 점 v까지의 거리를 저장하는데 사용할 배열 D
        for (int i = 1; i <= n; i++) D[i] = Integer.MAX_VALUE;    //각 점 v에 대해 D[v]=∞ 초기화
        D[s] = 0;

        int[] check = new int[n + 1];    //각 점의 방문여부를 확인할 배열 check 선언
        check[s] = 1;

        for (int i = 1; i < n; i++)
            if (G.w[s][i] != 0) D[i] = G.w[s][i];     //출발점 s로부터 간선이 존재하는 점 v에 대해 가중치를 배열 D에 저장

        for (int i = 1; i < n; i++) {    //반복문 n-1회 수행
            int vmin = Integer.MAX_VALUE;
            for (int j = 1; j < n + 1; j++)
                if (check[j] == 0)     //최단거리가 확정되지 않은 점
                    vmin = D[j] < vmin ? j : vmin;    //D[v]가 최소인 점 vmin을 선택
            check[vmin] = 1;

            for (int j = 1; j < n + 1; j++)
                if (check[j] == 0 && G.w[vmin][j] != 0)    //최단거리가 확정되지 않았으며 간선이 존재하는 점
                    if (D[vmin] + G.w[vmin][j] < D[j])    //vmin을 거쳐감으로서 거리가 짧아지는 점 w
                        D[j] = D[vmin] + G.w[vmin][j];   //D[w] 갱신

        }

        return D;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int node, line;
        node = scanner.nextInt();
        line = scanner.nextInt();

        graph G = new graph(node);
        for (int a = 0; a < line; a++) {
            int i, j, k;
            i = scanner.nextInt();
            j = scanner.nextInt();
            k = scanner.nextInt();
            G.input(i, j, k);
        }

        int[] D;
        D = shortestpath(G, 1, node);
        for (int i = 1; i < node + 1; i++)
            System.out.print(D[i] + " ");
        
        scanner.close();
    }
}
```