# Game of life


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
