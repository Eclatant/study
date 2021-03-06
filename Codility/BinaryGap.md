# BinaryGap

Find longest sequence of zeros in binary representation of an integer.

## Task description

A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.

For example, number 9 has binary representation 1001 and contains a binary gap of length 2. The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3. The number 20 has binary representation 10100 and contains one binary gap of length 1. The number 15 has binary representation 1111 and has no binary gaps.

Write a function:

```java
class Solution { public int solution(int N); }
```

that, given a positive integer N, returns the length of its longest binary gap. The function should return 0 if N doesn't contain a binary gap.

For example, given N = 1041 the function should return 5, because N has binary representation 10000010001 and so its longest binary gap is of length 5.

Assume that:

* N is an integer within the range [1..2,147,483,647].

Complexity:

* expected worst-case time complexity is O(log(N));
* expected worst-case space complexity is O(1).

## Solution

### Frist

* Programming language: Java
* Task score: 73%
* Analysis summary: The following issues have been detected, timeout errors.
* Code

```java
import java.util.*;

class Solution {
    public int solution(int N) {
        int max = 0;
        int n, num, size, gap;
        List<Integer> idxs = new ArrayList<>();
        
        while (N > 0) {
            n = -1;
            num = 1;
            
            while (N >= num) {
                n++;
                num *= 2;
            }  
            
            N = N - num / 2;
            idxs.add(n);
            
            size = idxs.size();
            if (size > 1) {
                gap = idxs.get(size - 2) - idxs.get(size - 1) - 1;
                if (gap > max)
                    max = gap;
            }
        }
        
        return max;
    }
}
```

### Second

* Programming language: Java
* Task score: 100%
* Analysis summary: The solution obtained perfect score.
* Code

```java
class Solution {
    public int solution(int N) {
        int max = 0;
        int oneCnt = 0;
        int zeroCnt = 0;

        while (N > 0) {
            if (N % 2 == 0) {
                if (oneCnt > 0)
                    zeroCnt++;
            } else {
                oneCnt++;
            }
            
            if (oneCnt == 2) {
                if (max < zeroCnt)
                    max = zeroCnt;
                oneCnt = 1;
                zeroCnt = 0;
            }

            N = N / 2;
        }

        return max;
    }
}
```

### Third

* Programming language: Java
* Task score: 100%
* Analysis summary: The solution obtained perfect score.
* Code

```java
class Solution {
    public int solution(int N) {
        return solution(N, 0, 0, 0);
    }
    
    public int solution(int N, int max, int zeroCnt, int oneCnt) {
        if (N == 0) {
            return max;
        } else if (N % 2 == 0) {
            return solution(N / 2, max, (oneCnt > 0) ? zeroCnt + 1 : zeroCnt, oneCnt);   
        } else if (++oneCnt == 2) {
            return solution(N / 2, (max > zeroCnt) ? max : zeroCnt, 0, 1);
        } else {
            return solution(N / 2, max, zeroCnt, oneCnt);
        }
    }
}
```

## Comment

정답을 위해 끼워 맞추는 식으로 해결하려는 느낌이 든다. 문제의 규칙을 찾기 위한 시간 투자가 필요할 듯 하다.