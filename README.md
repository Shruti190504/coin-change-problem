BY RECURSION:

class Solution {
    
    public int countWays(int coins[], int sum) {
        return count(coins.length, coins, sum);
    }

    int count(int n, int coins[], int sum) {
        if (sum == 0) return 1; // Base case: Found a valid combination
        if (n == 0 || sum < 0) return 0; // No more coins or negative sum
        
        int include = count(n, coins, sum - coins[n - 1]); // Use current coin
        int exclude = count(n - 1, coins, sum); // Skip current coin
        
        return include + exclude;
    }
}

BY MEMOIZATION:

class Solution {
    
    public int count(int coins[], int sum) {
        int memo[][] = new int[coins.length + 1][sum + 1];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        return func(coins.length, coins, sum, memo);
    }

    int func(int n, int arr[], int target, int memo[][]) {
        if (target == 0) return 1;
        if (n == 0 || target < 0) return 0;
        if (memo[n][target] != -1) return memo[n][target];

        int pos1 = func(n, arr, target - arr[n - 1], memo);
        int pos2 = func(n - 1, arr, target, memo);
        memo[n][target] = pos1 + pos2;
        return memo[n][target];
    }
}

BY TABULATION:

class Solution {
    
    public int countWays(int coins[], int sum) {
        int n = coins.length;
        int dp[][] = new int[n + 1][sum + 1];

        // Base case: There is one way to make sum 0 (by taking no coins)
        for (int i = 0; i <= n; i++) {
            dp[i][0] = 1;
        }

        // Fill DP table
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= sum; j++) {
                int exclude = dp[i - 1][j]; // Not taking the coin
                int include = (j >= coins[i - 1]) ? dp[i][j - coins[i - 1]] : 0; // Taking the coin
                dp[i][j] = exclude + include;
            }
        }

        return dp[n][sum]; // Final answer
    }
}
