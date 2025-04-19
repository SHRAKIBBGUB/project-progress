# project-progress code

import time

class SudokuSolver:
    def __init__(self, board):
        self.board = board
        self.steps = 0
    
    def print_board(self):
     
        for i in range(9):
            if i % 3 == 0 and i != 0:
                print("-"*21)
            for j in range(9):
                if j % 3 == 0 and j != 0:
                    print("| ", end="")
                print(f"{self.board[i][j] or '.':2}", end="")
            print()

    def find_empty(self):
       
        for i in range(9):
            for j in range(9):
                if self.board[i][j] == 0:
                    return i, j
        return None

    def is_valid(self, num, pos):
    
        row, col = pos
        
       
        if num in self.board[row]:
            return False
            
      
        if num in [self.board[i][col] for i in range(9)]:
            return False
            
       
        box_x = col // 3
        box_y = row // 3
        for i in range(box_y*3, box_y*3 + 3):
            for j in range(box_x*3, box_x*3 + 3):
                if self.board[i][j] == num:
                    return False
        return True

    def solve(self):
      
        self.steps += 1
        empty = self.find_empty()
        
        if not empty:
            return True
            
        row, col = empty
        
        for num in range(1, 10):
            if self.is_valid(num, (row, col)):
                self.board[row][col] = num
                
                if self.solve():  
                    return True
                    
                self.board[row][col] = 0  
                
        return False

def load_puzzle(filename):
    
    with open(filename) as f:
        return [[int(num) for num in line.split()] for line in f]

def main():
    print("SUDOKU SOLVER")
    print("="*30)
    

    try:
        board = load_puzzle("puzzle.txt")
    except:
        print("default puzzle...")
        board = [
            [5,3,0,0,7,0,0,0,0],
            [6,0,0,1,9,5,0,0,0],
            [0,9,8,0,0,0,0,6,0],
            [8,0,0,0,6,0,0,0,3],
            [4,0,0,8,0,3,0,0,1],
            [7,0,0,0,2,0,0,0,6],
            [0,6,0,0,0,0,2,8,0],
            [0,0,0,4,1,9,0,0,5],
            [0,0,0,0,8,0,0,7,9]
        ]
    
    solver = SudokuSolver(board)
    
    print("\nOriginal Puzzle:")
    solver.print_board()
    
    print("\nSolving...")
    start_time = time.time()
    if solver.solve():
        print(f"\nSolved in {solver.steps} steps ({time.time()-start_time:.2f}s):")
        solver.print_board()
    else:
        print("\nNo solution exists!")

if __name__ == "__main__":
    main()
