```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;
class edge implements Comparable<edge>{
	int start, end, cost;
	edge(int start, int end, int cost){
		this.cost = cost;
		this.start = start;
		this.end = end;
	}
	@Override
	public int compareTo(edge o) {
		//return this.cost - o.cost 간선의 부호가 모두 같을때
		return Integer.compare(this.cost,o.cost);//부호가 다를수도 있으므로
	}
}
public class Main {
	static int parents[];
	static int V,E;
	static edge[] edgeList;
	public static void main(String[] args) throws IOException {
		Scanner sc = new Scanner(System.in);
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(in.readLine()," ");
		V = Integer.parseInt(st.nextToken());
		E = Integer.parseInt(st.nextToken());
		
		//간선리스트 작성
		 edgeList = new edge[E];
		 for (int i = 0; i < E; i++) {
			st=new StringTokenizer(in.readLine()," ");
			int start = Integer.parseInt(st.nextToken());
			int end = Integer.parseInt(st.nextToken());
			int cost = Integer.parseInt(st.nextToken());
			edgeList[i] = new edge(start,end,cost);
		 }
		 Arrays.sort(edgeList);
		 make();
		 
		 int cnt = 0;//쓰인 간선의 개수 V-1 체크할거
		 int result = 0;//cost의 합
		 for(edge e : edgeList) {
			 if(union(e.start, e.end)) {
				 result += e.cost;
				 if(++cnt == V-1) break;//신장트리 완성
			 }
		 }
		 System.out.println(result);
	}
	static void make() {
		parents = new int[V];
		for (int i = 0; i < V; i++) {
			parents[i] = i;
		}
	}
	static int find(int a) {
		if(a == parents[a]) return a;
		return parents[a] = find(parents[a]);
	}
	static boolean union(int a, int b) {
		int aRoot = find(a);
		int bRoot = find(b);
		if(aRoot != bRoot) {
			parents[bRoot] = aRoot;
			return true;
		}
		else return false; 
	}
}
