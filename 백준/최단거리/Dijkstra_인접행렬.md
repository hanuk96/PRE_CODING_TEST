```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;
class node implements Comparable<node>{
	int idx;
	int cost;
	node(int idx,int cost){
		this.idx = idx;
		this.cost = cost;
	}
	
	@Override
	public int compareTo(node o) {
		return this.cost - o.cost;
	}
}
public class Main {
	static int n, e;
	static int map[][];
	static int d[];
	public static void main(String[] args)throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String str = br.readLine();
		String command[] = str.split(" ");
		
		n = Integer.parseInt(command[0]);
		e = Integer.parseInt(command[1]);
		
		str = br.readLine();
		int start = Integer.parseInt(str);
		
		map = new int[n][n];//인접행렬
		d = new int[n];//최단거리
		
		//초기화
		//d -> 모두 무한대로
		//map -> 자기자신은 0, 나머지는 무한대
		for (int i = 0; i < n; i++) {
			d[i] = (int)1e9;
			for (int j = 0; j < n; j++) {
				map[i][j] = (int)1e9;
				if(i == j) map[i][j] = 0;
			}
		}
		for (int i = 0; i < e; i++) {
			String info = br.readLine();
			String split[] = info.split(" ");
			
			int from = Integer.parseInt(split[0]);
			int to = Integer.parseInt(split[1]);
			int cost = Integer.parseInt(split[2]);
			
			map[from-1][to-1] = cost;
		}
		
		PriorityQueue<node> pq = new PriorityQueue<node>();//최저 cost의 노드를 들고다닐 priority queue
		
		//Dijkstra
		d[start-1] = 0;//맨 start의 최단거리는 0이다
		pq.add(new node(start-1,0));//맨처음 시작 노드의 정보를 담음
		
		while(!pq.isEmpty()) {//어차피 모든 노드들은 다 연결되어있기때문에, 
			node cur = pq.poll();//최단거리의 노드 뽑기
			int cost = cur.cost;
			int nowIdx = cur.idx;
			//이미 처리된적이 있다? 무시
			if(d[nowIdx] < cost) continue;
			for (int i = 0; i < n; i++) {
				//map의 from / to, 자기와 인접한 노드찾기
				if(map[nowIdx][i] != 0 && map[nowIdx][i] != (int)1e9) {
					int changeCost = d[nowIdx] + map[nowIdx][i];
					if(changeCost < d[i]) {
						d[i] = changeCost;
						pq.offer(new node(i,changeCost));
					}
				}
			}
		}
		for (int result:d) {
			if(result == (int)1e9) { System.out.println("INF"); continue;}
			System.out.println(result);
		}
	}		
}
