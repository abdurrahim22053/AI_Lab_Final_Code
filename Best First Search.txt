import copy
import heapq
N = 3
row = [0, 0, -1, 1]
col = [-1, 1, 0, 0]
class p8_board:
    def __init__(self,board,x,y):
        self.board=board
        self.x=x
        self.y=y
        self.h=manhatten(board)
    def __lt__(self,other):
        return self.h<other.h
    
def manhatten(board):
    distance = 0
    for i in range(N):
        for j in range(N):
            val = board[i][j]
            if val!=0:
                goal_x = (val -1)//N
                goal_y = (val-1)%N
                distance+= abs(goal_x - i)+abs(goal_y - j)
    return distance
def is_solvable(board):
    flat = []
    for row in board:
        for num in row:
            if num!=0:
                flat.append(num)
    inversion = 0
    for i in range(len(flat)):
        for j in range(i+1,len(flat)):
            if flat[i]>flat[j]:
                inversion+=1            
    return inversion%2==0

def is_goal(board):
    goal =[[1, 2, 3],
         [4, 5, 6],
         [7, 8, 0]] 
    return goal == board

def print_board(board):
    for r in board:
        print(r)

def is_valid(x,y):
    return 0<=x<N and 0<=y<N

def solve(start,x,y):
    if not is_solvable(start):
        print("Puzzle is not solveable")
        return

    pq = []
    visited = set()
    heapq.heappush(pq,p8_board(start,x,y))
    while pq:
        current = heapq.heappop(pq)
        board_tuple = tuple(tuple(r) for r in current.board)

        if board_tuple in visited:
            continue
        visited.add(board_tuple)

        if is_goal(current.board):
            print("Solution found")
            print_board(current.board)
            return
        for i in range(4):
            new_x = current.x+row[i]
            new_y = current.y+col[i]
            if is_valid(new_x,new_y):
                new_board = copy.deepcopy(current.board)
                new_board[current.x][current.y],new_board[new_x][new_y] = new_board[new_x][new_y],new_board[current.x][current.y]
                heapq.heappush(pq,p8_board(new_board,new_x,new_y))  
    print("No solution found ")        

start = [[1, 2, 3],
         [4, 5, 6],
         [7, 0, 8]]
x,y=2,1
solve(start,x,y)