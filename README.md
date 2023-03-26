import random

# Initialize the game board as a 4x4 grid filled with zeros
board = [[0]*4 for _ in range(4)]

# Function to place a random tile with the value 2 or 4 on the board
def add_tile():
    empty_cells = [(i,j) for i in range(4) for j in range(4) if board[i][j] == 0]
    if empty_cells:
        i,j = random.choice(empty_cells)
        board[i][j] = 2 if random.random() < 0.9 else 4

# Function to merge tiles in a given row or column
def merge(line):
    merged_line = [0]*4
    idx = 0
    for tile in line:
        if tile != 0:
            if merged_line[idx] == 0:
                merged_line[idx] = tile
            elif merged_line[idx] == tile:
                merged_line[idx] *= 2
                idx += 1
            else:
                idx += 1
                merged_line[idx] = tile
    return merged_line

# Function to move all tiles in the chosen direction and merge adjacent tiles with the same value
def move(direction):
    global board
    if direction == 'left':
        board = [merge(row) for row in board]
    elif direction == 'right':
        board = [row[::-1] for row in board]
        board = [merge(row)[::-1] for row in board]
    elif direction == 'up':
        board = [[board[j][i] for j in range(4)] for i in range(4)]
        board = [merge(row) for row in board]
        board = [[board[j][i] for j in range(4)] for i in range(4)]
    elif direction == 'down':
        board = [[board[j][i] for j in range(4)] for i in range(4)]
        board = [row[::-1] for row in board]
        board = [merge(row)[::-1] for row in board]
        board = [[board[j][i] for j in range(4)] for i in range(4)]

# Function to check if the game is over (i.e., no more moves can be made)
def is_game_over():
    for i in range(4):
        for j in range(4):
            if board[i][j] == 0:
                return False
            if j < 3 and board[i][j] == board[i][j+1]:
                return False
            if i < 3 and board[i][j] == board[i+1][j]:
                return False
    return True

# Function to check if the user has won (i.e., reached 2048)
def has_won():
    for i in range(4):
        for j in range(4):
            if board[i][j] == 2048:
                return True
    return False

# Main game loop
while True:
    # Place two random tiles with the values 2 or 4 on the board
    add_tile()
    add_tile()
    
    # Display the game board
    print('\n'.join(['\t'.join([str(cell) for cell in row]) for row in board]))
    
    # Get user input for direction
    direction = input("Enter direction (left/right/up/down): ").lower().strip()
    
    # Move tiles in the chosen direction and merge adjacent tiles with the same value
    move(direction)
    
    # Check if any tiles
