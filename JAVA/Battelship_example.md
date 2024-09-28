```java
public class Main {
    public static void main(String[] args) { // Main method
        int size = 10;
        int[][] grid = new int[size][size];
        funExperiment(grid, size);
        printArray(grid, size);
    }

    public static void printArray(int[][] array, int size) {
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                System.out.print(array[i][j] + " ");
            }
            System.out.println();
        }
    }

    public static void funExperiment(int[][] grid, int size) {
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                if (i != 0) {
                    grid[i - 1][j]++; // Top middle

                    if (j != 0) {
                        grid[i - 1][j - 1]++; // top left
                    }

                    if (j != size - 1) {
                        grid[i - 1][j + 1]++; // top right
                    }
                }

                if (i != size - 1) {
                    grid[i + 1][j]++; // bottom middle
                    if (j != 0) {
                        grid[i + 1][j - 1]++; // bottom left
                    }
                    if (j != size - 1) {
                        grid[i + 1][j + 1]++; // bottom right
                    }

                }

                if (j != 0) {
                    grid[i][j - 1]++; // middle left
                }
                if (j != size - 1) {
                    grid[i][j + 1]++; // middle right
                }
            }
        }

    }
}
```
