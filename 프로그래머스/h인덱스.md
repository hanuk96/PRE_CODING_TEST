```java
import java.util.*;
class Solution {
    public int solution(int[] citations) {
        int answer = 0;
        Arrays.sort(citations);
        for(int i = 0,size = citations.length; i < citations.length; i++,size--){
            if(citations[i] >= size){
                answer = size;
                break;
            }
        }
        return answer;
    }
}
