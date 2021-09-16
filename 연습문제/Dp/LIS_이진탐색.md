```java
import java.util.*;
public class Main {
	static int s, result = 0;
    public static void main(String[] args) {
    	
    	Scanner sc = new Scanner(System.in);
    	int N = sc.nextInt();
    	int[] arr = new int[N];//모든 원소의 값은 다름
    	int[] LIS = new int[N];
    	
    	for (int i = 0; i < N; i++) {
			arr[i] = sc.nextInt();
		}
    	int size = 0;// LIS에 채워지는 원소의 수(값이 있는)
    	for (int i = 0; i < N; i++) {
    		//중복값이 없으므로 무조건 탐색실패 -> -INDEX -1 의 결과가 나옴
			int idx = Math.abs(Arrays.binarySearch(LIS,0,size,arr[i])) - 1;
			LIS[idx] = arr[i];
			
			if(idx == size) size++;//덮어씌우지 않고 끝에 붙었음
		}
    	System.out.println(size);
    }
}
