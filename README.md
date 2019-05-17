# FinalExam
 
 For the following problems we will implement them recursively and will use Dynammic Programming as well.
 I will say "dp" in order to reference "Dynamic Programming."
 
 
# Perfect Squares 

# Problem

Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

# Example 1:
    Input: n = 12
    Output: 3 
    Explanation: 12 = 4 + 4 + 4.

# Example 2:
    Input: n = 13
    Output: 2
    Explanation: 13 = 4 + 9.


# Explanation/Solution

I created a dp array.
 This dynamic programming array stands for the least number of perfect square numbers for its index. 
 For example: dp[13]=2 stands for if you want to decompose 13 into some perfect square numbers, 
 it will contains at least two terms which are 33 and 22.

After the initialization of the dp array. 
Its fundamental to iterate through the array to fill all the corresponding indices and after looking through each iteration the formula dp[i] = 1 + min (dp[i-j*j] for j*j<=i) is being applied. In order for me to become a little bit more cleared I had to trace it with actual numbers and obtained the following examples...

If you want to get dp[13] and suppose they are filled out...

dp[13] = dp[13-1x1]+dp[1]= dp[12]+1 = 3;
dp[13] = dp[13-2x2]+dp[2x2]= dp[9]+1 = 2;
dp[13] = dp[13-3x3]+dp[3x3]= dp[4]+1 = 2;

Finally just choose the smallest one which is 2 and that will be in this case dp[13] = 2.  

The code isnt exactly complex but it is fundamental to understand the fomrula in order to be able to apply dp efficieintly.

# Code
public class Perfect_Squares() {
	
	public int NumSquares(int n){
	    int[] dp = new int[n + 1];
	    
	     for (int i = 1; i <= n; i++){
	         int min= int.MaxValue;
	         
	         for (int j = 1; j * j <= i; j++){
	             min= Math.Min(min, dp[i - j * j] + 1);
	         }
	         
	         dp[i] = min;
	     }
	     return dp[n];
	}
}

------------------------------------------------------------------------------------------------------------------

# Palindromic Substring

# Problem

Given a string, your task is to count how many palindromic substrings in this string. 
The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters. 
Example 1:
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
Example 2:
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
Note:
1.	The input string length won't exceed 1000.

# Explanation/Solution

In order to apply the solution I used two for loops. The outter loop will iterate thorugh the lengths of palindromes. While the second inner for loop is in charged of iterating through all the positions in String s. We first have to look for all lengths of palindromes starting from {1,2,3,n...n+1}. By this being said i will build odd length palindromes of previous odd length palindromes and even length palindromes off of previous even length palindromes.

For example: For a palindrome candidate of length i, we should check the palindrome of length i - 2. The result will be stored in ans and as mentioned len will check the total lenght.

# Code
public class Palindromic_Substrings {
	
    public int SubstringsCount(String s) {
        int ans = 0;
        int len =  s.length();
        
        boolean[][] dp = new boolean[len][len];
        
        for(int i = 0; i < len; i++){
        	
            for(int j = 0; j < len - i; j++){
                dp[i][j] = (i < 3 || dp[i - 2][j + 1]) && (s.charAt(j) == s.charAt(j + i));
                if(dp[i][j]) ans++;
            }
        }
        
        return ans;
    }
}
------------------------------------------------------------------------------------------------------------------
# Integer Break

# Problem

Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.
Example 1:
Input: 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
Example 2:
Input: 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
Note: You may assume that n is not less than 2 and not larger than 58.

# Explanation/Solution

To solve this I check whether you have to observe for all poosible integer breaks and then be able to decide the which would be the one that maximes the result. Since for n = even we can divide it into pairs as for example (4,6),(3,7),(5,5) etc, which can further be divided like (4,6) -> ((2,2),(3,3)) . Many sub-problems will have to be analyzed in order to keep track of the bigger problem. Also since (4,6) -> ((2,2),(3,3)) and (2,8)->(2,(2,(3,3))) hence we are solving repeatively for (2) and (3) were it has an overlapping property.
In order to construct the solution we need a base condition were dp[1] = 1, we also need a condition that will terminate the program whenever n <= 0 and finally the most important step the recursive step int val = Math.max((n-i)*i,Math.max((n-i)*termCond(i),Math.max(termCond(n-i)*i,termCond(n-i)*termCond(i)

# Code

public class Integer_Break {
    
    int[] dp;
    
    public int termCond(int n){ //This condition will terminate the program.
	    if(n <= 0)
	    	return 1;

        if(dp[n] > 0) //This would be the memorization of the computer or program.
            return dp[n];
        
        // This will check all the possible solutions recursively
        int max = 0;
        for(int i=1;i<n;i++){
            int val = Math.max((n-i)*i,Math.max((n-i)*termCond(i),Math.max(termCond(n-i)*i,termCond(n-i)*termCond(i))));
            if(val > max)
                max = val;
        }
        dp[n] = max;
        return max;
    }
    public int integerBreak(int n) {
        
        if(n == 0)
            return 1;
        
        dp = new int[n+1];
        dp[1] = 1;
        dp[0] = 0;
        return termCond(n);
    }
}
------------------------------------------------------------------------------------------------------------------
# Minimum Falling Path Sum

# Problem

2 - Minimum Falling Path Sum

Given a square array of integers A, we want the minimum sum of a falling path through A.
A falling path starts at any element in the first row, and chooses one element from each row.  The next row's choice must be in a column that is different from the previous row's column by at most one.
Example 1:
Input: [[1,2,3],[4,5,6],[7,8,9]]
Output: 12
Explanation: 
The possible falling paths are:
•	[1,4,7], [1,4,8], [1,5,7], [1,5,8], [1,5,9]
•	[2,4,7], [2,4,8], [2,5,7], [2,5,8], [2,5,9], [2,6,8], [2,6,9]
•	[3,5,7], [3,5,8], [3,5,9], [3,6,8], [3,6,9]
The falling path with the smallest sum is [1,4,7], so the answer is 12.
 Note:
1.	1 <= A.length == A[0].length <= 100
2.	-100 <= A[i][j] <= 100

# Explanation/ Solution

Apply 3 calls from the each element of the first row... as we can consider.

This tree will recursively form the first elements on the first row.
In order to make it more effective I used DP.
In this approach I created a storage array and save computed minimum sum of each col so that we dont have to calculate it again.

                                                  ( 0 , 0 )                          
                                                 /    |     \
		          (- Base case--> first row) (1, -1) (1, 0) (1, 1)
                                                    /  |  \                     
			 (2, -1) (2, 0)(2, 1) ( + Base Case - returns last row elements)



# Code

public static int MinSum(int[][] matrix){
		int ans = Integer.MAX_VALUE;
		int[][] res = new int[matrix.length][matrix[0].length]; // This array will store the result.
		
		for (int col = 0; col < matrix[0].length; col++) { // operation for the elements within first row
			ans = Math.min(ans, helper(matrix, 0, col,res)); 
		}
		return ans;

	
	// DP
	public static int helper(int[][] matrix, int row, int col, int[][] res){
	
			//Base case
			if (row >= matrix.length || col >= matrix[0].length || col < 0) {
				return Integer.MAX_VALUE;
			}
			
			//When at last row return the specified result.
			if (row == matrix.length - 1) {
				return matrix[row][col];
			}
			
			//If answer has been seen return the result
			if(res[row][col] != 0){
				return res[row][col];
			}
	
			int res = 0; //To store the minimum sum of the current Fall
			int prev = 0, curr = 0, next = 0;
			prev = helper(matrix, row + 1, col - 1,res);
			curr = helper(matrix, row + 1, col,res); 
			next = helper(matrix, row + 1, col + 1,res); 
			res = Math.min(prev, Math.min(curr, next)) + matrix[row][col]; 
	
			res[row][col] = res; //Stores the result that was calculated.
			return res;
	
		
	}
}

------------------------------------------------------------------------------------------------------------------
# Minimum ASCII Delete Sum for Two Strings

# Problem

Given two strings s1, s2, find the lowest ASCII sum of deleted characters to make two strings equal.

# Example 1:
    Input: s1 = "sea", s2 = "eat"
    Output: 231
    Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
    Deleting "t" from "eat" adds 116 to the sum.
    At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.
# Example 2:

    Input: s1 = "delete", s2 = "leet"
    Output: 403
    Explanation: Deleting "dee" from "delete" to turn the string into "let",
    adds 100[d]+101[e]+101[e] to the sum.  Deleting "e" from "leet" adds 101[e] to the sum.
    At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403.
    If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.

Note: 
        •  0 < s1.length, s2.length <= 1000. 
        •  All elements of each string will have an ASCII value in [97, 122].


# Explanation/Solution
 
 I will create a dp array that will be the answer for the Strings s1, s2.
 By this being said if one of the input Strings is empty, the answer on this case will be the sum of ascii of the other String. When s1[i] == s2[j], we have dp[i][j] = dp[i+1][j+1] as we can ignore these two characters.
 Or if it is the opposite were s1[i] != s2[j], we will have to delete at least one of them since we have the previous one. in adittion dp[i][j] will be the mimimun of both of the deleted options. This is considered to be a great dp practice and storing all the values of previous Subproblems as the one just mentioned above. 
    
 # Code
    
 public int minimumDeleteSum(String s1, String s2) {
        
        int[][] dp = new int[s1.length() + 1][s2.length() + 1];
        
        for (int i = 1; i <= s1.length(); i++) {
            dp[i][0] += dp[i - 1][0] + s1.charAt(i - 1);
        }
        
        for (int j = 1; j <= s2.length(); j++) {
            dp[0][j] = dp[0][j - 1] + s2.charAt(j - 1);
        }
        
        for (int i = 1; i < dp.length; i++) {
            for (int j = 1; j < dp[0].length; j++) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    int d1 = dp[i - 1][j] + s1.charAt(i - 1);
                    int d2 = dp[i][j - 1] + s2.charAt(j - 1);
                    dp[i][j] = Math.min(d1, d2);
                }
            }
        }
        return dp[s1.length()][s2.length()];
    }
}
