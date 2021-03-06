# Game of life
## https://leetcode.com/problems/game-of-life

## Approach :
We can easily solve the problem once we understand the problem, basically we have to update the given board's cell values according to the 4 rules of game of life. One important thing to note is, we always have to consider the original board's values, while updating a cell's value.

One way to solve this problem would be to simply create a copy of the 2D board, so that we never loose track of the original cell values, and we will be updating the cell values in the original given board, but we will be applying the rules on the clone board's values.

## Implementation 1 : Time => O(rows * cols) , Space => O(rows * cols)

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
Can we do it without making a copy of the original board, lets think about it. Why we needed to copy the original board, because we didn't wanted to loose the original cell values as we update the values in the original board.

Can we do it without copying the original board, the answer is yes.
```
The state change happens only in two cases :
1. A live cell dies because of under-population or over-population, so state changes from 1 to 0 (1 -> 0)
2. A dead cell becomes alive, if it have exactly three live neighbors, so state changes from 0 to 1 (0 -> 1)

We can update the board as long as we don't loose the original cell value `board[row][column]`. 
So how can we do that. How can we update the board's cell value, while at the same time 
we should be able to figure out what was the original value at `board[row][column]`

If the state change happens because of first case, we will set the board's cell value to -1. 
So when we see a cell value of -1 we know that this cell was initially alive but later died.

If the state change happens because of second case, we will set the board's cell value to 2. 
So when we see a cell value of 2 we know that this cell was initially dead but later became alive.

And lastly we update the cell values so that -1 becomes 0 and 2 becomes 1.
```

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
