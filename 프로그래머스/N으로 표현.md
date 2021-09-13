```java
class Solution {
	static int answer = (int)1e9;
    public int solution(int N, int number) {
        dp(0,0,N,number);
        if(answer == (int)1e9) answer = -1;
        return answer;
    }
    public static void dp(int cnt, int num, int n, int number){
    	if(cnt > 8) {
            return;
    	}
    	if(num == number) {
    		answer = Math.min(answer, cnt);
    		return;
    	}
    	
    	int newNumber = 0;
    	for (int i = 0; i < 8; i++) {
    		newNumber = newNumber * 10 + n; 
            
			dp(cnt+1+i,num + newNumber,n,number);
			dp(cnt+1+i,num - newNumber,n,number);
			dp(cnt+1+i,num / newNumber,n,number);
			dp(cnt+1+i,num * newNumber,n,number);
		}
    }
}
