```
import java.util.Scanner;

public class ShortestPath {
    static final int N=10;
    static final int MAX=10000; //10000을 무한대라고 가정
    static int[] D=new int[N];
    public static int[] dijsktra(int[][] distance){
        int u,w;

        D[0]=0;
        for(int i=1;i<N;i++)
            D[i]=MAX;  //나머지 거리는 무한대로 초기화

        boolean[] T=new boolean[N]; // 최단거리가 확정된 점들의 집합
        T[0]=true;

        for(int i=1;i<N;i++){  //초기화
            D[i]=distance[0][i];
            T[i]=false;
        }

        for(int i=0;i<N-1;i++){
            u=choose(D,N,T);
            T[u]=true;
            for(w=1;w<N;w++){
                if(!T[w])
                    if(D[u]+distance[u][w]<D[w])
                        D[w]=D[u]+distance[u][w];
            }
        }
        return D;
    }
    public static int choose(int[] D,int N,boolean[] T){
        int i,min,minpos;
        min=MAX;
        minpos=-1;
        for(i=1;i<N;i++)
            if(D[i]<min && !T[i]){
                min=D[i];
                minpos=i;
            }
        return minpos;
    }

    public static void main(String[] args) {
        Scanner sc=new Scanner(System.in);
        int[][] distance=new int[N][N];
        for(int i=0;i<N;i++)
            for(int j=i+1;j<N;j++)
                distance[i][j]=distance[j][i]=MAX; //다 무한대로 초기화

        for(int i=0;i<N;i++)
            for(int j=i+1;j<N;j++)
                distance[i][j]=distance[j][i]=sc.nextInt();

        D=dijsktra(distance);
        for(int i=0;i<N;i++){
            System.out.print(D[i]+" ");
        }
    }
}
```