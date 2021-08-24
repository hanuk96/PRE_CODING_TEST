```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;
public class Main {
	public static void main(String[] args) throws IOException{
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(in.readLine());
		int[][] adj = new int[N][N];
		boolean visited[] = new boolean[N];
		int[] minEdge = new int[N];
		
		StringTokenizer st = null;
		for (int i = 0; i < N; i++) {
			st = new StringTokenizer(in.readLine()," ");
			for (int i2 = 0; i2 < N; i2++) {
				adj[i][i2] = Integer.parseInt(st.nextToken());				
			}
			minEdge[i] = (int)1e9;//모든 정점에서의 cost를 무한대로 초기화
		}
		int result = 0; //최소 신장트리의 비용
		minEdge[0] = 0;//시작하는 정점의 위치 - 시작하는 위치가 바뀌어도, 신장트리의 총 비용은 바뀌지 않는다!
		
		for (int i = 0; i < N; i++) {
			//1. 신장트리에 포함되지 않은 정점중에 최소 간선비용의 정점을 찾는다
			int min = (int)1e9;
			int minNode = -1;//최소 cost 정점
			for (int j = 0; j < N; j++) {
				if(!visited[j] && min > minEdge[j]) {
					//신장트리에 포함되지 않았고, 가장 작은 cost의 간선
					min = minEdge[j];
					minNode = j;
				}
			}
			//신장트리에 포함됨을 체크해줌
			visited[minNode] = true;
			result += min;//최소 신장 거리
			
			//2. 선택된 정점을 기준으로 신장트리의 연결되지 않은 정점과의 cost 최소로 업데이트 해주기
			for (int j = 0; j < N; j++) {
				if(!visited[j] && minEdge[j] > adj[minNode][j] && adj[minNode][j] != 0) {
				//신장트리에 포함되어있지 않고, 연결이 가능(0이 아님), 최소 간선비용인지?
					minEdge[j] = adj[minNode][j];//update해주기
				}
			}
		}
		System.out.println(result);
	}
}
