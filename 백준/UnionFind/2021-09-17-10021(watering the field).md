---

자꾸 순열쓰지말자... 이 문제는 중복한 좌표값 구해주면 틀린다...

---
```java
import java.util.*;
class node implements Comparable<node>{
	int from;
	int to;
	int dis;
	node(int from, int to, int dis){
		this.from = from;
		this.to = to;
		this.dis = dis;
	}
	public int compareTo(node o) {
		return Integer.compare(this.dis,o.dis);
	}
}
public class Main {
	static int vertex[][];
	static int pipe[];
	static int n,c;
	static PriorityQueue<node> pq;
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
	    n = sc.nextInt();//노드의 수
	    c = sc.nextInt();//최소 파이프 길이
    	
	    pipe = new int[n+1];
	    //1.부모 초기화
	    for (int i = 1; i <= n; i++) {
			pipe[i] = i;
		}
	    
	    pq = new PriorityQueue<node>();
	    vertex = new int[n+1][2];
	    //1 ~ n번 노드까지
	    for (int i = 1; i <= n; i++) {
			int from = sc.nextInt();
			int to = sc.nextInt();
			vertex[i][0] = from;
			vertex[i][1] = to;
		}
	    //int sum[][] = new int[2][2];
	    //comb(0,sum,0,0,0);
	    for (int i = 1; i <= n - 1; i++) {
			for (int j = i+1; j <= n; j++) {
				int s = (vertex[i][0]-vertex[j][0])*(vertex[i][0]-vertex[j][0]) 
						+ (vertex[i][1]-vertex[j][1])*(vertex[i][1]-vertex[j][1]);
				if(s < c) continue;
				pq.add(new node(i,j,s));
			}
		}
	    
	    
	    int cnt = 0;
	    int result = 0;
	    while(!pq.isEmpty()) {
			node cur = pq.poll();//거리가 짧은 아이부터
			if(union(cur.from,cur.to)) {//사이클이 없음
				result += cur.dis;
				cnt++;
			}
		}
	    if(cnt == n-1)
	    	System.out.println(result);
	    else
	    	System.out.println(-1);
	}
	//두 점 골라주기
	/*public static void comb(int idx, int result[][], int flag, int from, int to){
		if(idx == 2) {
			//유클리드 계산
			int sum = (result[0][0]-result[1][0])*(result[0][0]-result[1][0]) 
					+ (result[0][1]-result[1][1])*(result[0][1]-result[1][1]);
			
			if(sum < c) return;
			
			pq.add(new node(from,to,sum));
			return;
		}
		for (int i = 1; i <= n; i++) {
			if((flag&1<<i) != 0)continue;
			result[idx][0] = vertex[i][0];
			result[idx][1] = vertex[i][1];
			if(idx == 0)
				comb(idx+1,result,flag|1<<i,i,to);
			else if(idx == 1)
				comb(idx+1,result,flag|1<<i,from,i);
		}
	}*/
	//부모 찾기
	public static int find(int a) {
		//부모가 자기자신이면 return
		if(a == pipe[a]) return a;
		else return pipe[a] = find(pipe[a]);//root 부모를 반환 
	}
	//합치기
	public static boolean union(int a, int b) {
		int pA = find(a);
		int pB = find(b);
		
		//부모가 같음 => cycle 존재
		if(pA == pB)
			return false;
		//작은놈이 부모가 되게 해주자!
		if(pA > pB) 
			pipe[pA] = pB;
		else 
			pipe[pB] = pA;
		
		return true;
	}
}
