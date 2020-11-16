NPuzzleSolver
=============
by Zachary Cava

A Javascript solver to the classical 15-Puzzle game, abstracted to take any nxn size grid finding solutions through pattern matching rather than graph search. This pattern matching gives us incredible speed up allowing solutions to puzzles as big as 20x20 to be found nearly instantly. A more detailed description of the algorithm can be found below.
This was written to be part of a testing suite that had to test the solveability of student's programmed 15 puzzles.

How to Use
==========

To use the solver you must create a 2D array describing the maze with an empty string holding the spot for the empty space. So take this example 3x3 grid:
```
     +---+---+---+
     | 1 | 2 | 3 |
     +---+---+---+
     | 4 | 5 | 6 |
     +---+---+---+
     | 7 | 8 |   |
     +---+---+---+
```
The array representation would be equal to:
```javascript
var array = [[ 1, 2, 3], [4, 5, 6], [7, 8, ""]];
```
With this array representation simply create a new instance of the puzzle solver and pass the array as an argument:
```javascript
var solver = new NPuzzleSolver(array);
```
Then to get the solutions call solve():
```javascript
var solution = solver.solve();
```
If there is a solution, solve will return an array of moves that will lead to a solution. If no solution is found, null is returned instead.

Algorithm Details
=================
The algorithm works by reducing the puzzle from nxn to 1x1 solving each size in between to generate the smaller puzzle within.

Given an nxn size grid the algorithm will position blocks 1 to n - 2 into their appropriate place in the row, the final two blocks are placed such that block n - 1 is in position n in the first row, and the block n is in the second row at position n. Then the blocks are rotated into position.
There is the possibility that n - 1 and n will be in the reverse order and this is handled by doing a 2x3 rotation set (detailed later) to fix the ordering. But now the first row will be complete.

The algorithm will the proceed to solve the first column in a very similar way, position 1 to n - 2 into the appropriate place and then place n - 1 at position n in the first column and block n at position n in the second column and perform a right rotation. The same order error can occur and a mirrored set of moves fixes this problem as well.

Now however we have completed the first row and first column and successfully reduced the problem to a (n - 1)x(n - 1) square. So we rinse and repeat while n > 2. When n == 2 only the row is solved and the last block is flipped with the empty space as needed. If an order error occurs at this stage the puzzle is unsolveable and the intial configuration was an invalid one.

Notes
=====
I am working to clean up the code and make it more efficient, but as it is now it is rather quick and met the needs for our grading scripts.
