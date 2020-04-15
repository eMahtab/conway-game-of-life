# Game of life
## https://leetcode.com/problems/game-of-life

## Approach :
We can easily solve the problem once we understand the problem, basically we have to update the given board's cell values according to the 4 rules of game of life. One important thing to note is, we always have to consider the original board's values, while updating a cell's value.

One way to solve this problem would be to simply create a copy of the 2D board, so that we never loose track of the original cell values, and we will be updating the cell values in the original given board, but we will be applying the rules on the clone board's values.

## Implementation : Time => O(rows * cols) , Space => O(rows * cols)

```java
class Solution {
    final static int[][] directions = { {0,1}, {0,-1}, {1,0}, {-1,0}, {-1,1}, {1,1}, {-1,-1}, {1, -1}};
    public void gameOfLife(int[][] board) {
        if(board == null || board.length == 0)
            return;
        
        int rows = board.length;
        int cols = board[0].length;

        // Create a copy of the original board
        int[][] copyBoard = new int[rows][cols];

        // Create a copy of the original board
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                copyBoard[row][col] = board[row][col];
            }
        }
        
        
        // Iterate through board cell by cell.
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                int liveNeighbors = count(copyBoard, row, col);
                if ((copyBoard[row][col] == 1) && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    board[row][col] = 0;
                }
                if (copyBoard[row][col] == 0 && liveNeighbors == 3) {
                    board[row][col] = 1;
                }
            }
        }    
    }
    
    private int count(int[][] board, int row, int col) {
        int alive = 0;
        for(int[] direction : directions) {
            int nextRow = row + direction[0];
            int nextCol = col + direction[1];
            if(nextRow >= 0 && nextRow < board.length && nextCol >= 0 && nextCol < board[0].length &&
               board[nextRow][nextCol] == 1)
              alive++;  
        }
        return alive;
    }
}
```
## Implementation 2 : Time => O(rows * cols) , Space => O(1)
```java
class Solution {
    final static int[][] directions = { {0,1}, {0,-1}, {1,0}, {-1,0}, {-1,1}, {1,1}, {-1,-1}, {1, -1}};
    public void gameOfLife(int[][] board) {
        if(board == null || board.length == 0)
            return;
        
        int rows = board.length;
        int cols = board[0].length;
       
        // Iterate through board cell by cell.
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                int liveNeighbors = count(board, row, col);
                if ((board[row][col] == 1) && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    board[row][col] = -1;
                }
                if (board[row][col] == 0 && liveNeighbors == 3) {
                    board[row][col] = 2;
                }
            }
        }  
        // Update the board such that -1 becomes 0 and 2 becomes 1
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(board[i][j] == 1 || board[i][j] == 2)
                    board[i][j] = 1;
                else  
                    board[i][j] = 0;
            }
        }
    }
    
    private int count(int[][] board, int row, int col) {
        int alive = 0;
        for(int[] direction : directions) {
            int nextRow = row + direction[0];
            int nextCol = col + direction[1];
            if(nextRow >= 0 && nextRow < board.length && nextCol >= 0 && nextCol < board[0].length &&
               Math.abs(board[nextRow][nextCol]) == 1)
              alive++;  
        }
        return alive;
    }
}
```

## References :
1. https://leetcode.com/articles/game-of-life
2. https://www.youtube.com/watch?v=sUqYZvfZ9UE
