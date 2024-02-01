import random

def create_board(rows, cols, bombs):
    board = [[' ' for _ in range(cols)] for _ in range(rows)]

    # Place bombs randomly on the board
    bomb_positions = random.sample(range(rows * cols), bombs)
    for pos in bomb_positions:
        row = pos // cols
        col = pos % cols
        board[row][col] = 'B'

    return board

def print_board(board):
    for row in board:
        print(' '.join(row))
    print()

def count_adjacent_bombs(board, row, col):
    count = 0
    for i in range(row - 1, row + 2):
        for j in range(col - 1, col + 2):
            if 0 <= i < len(board) and 0 <= j < len(board[0]) and board[i][j] == 'B':
                count += 1
    return count

def reveal_board(board, row, col, visited):
    if 0 <= row < len(board) and 0 <= col < len(board[0]) and (row, col) not in visited:
        visited.add((row, col))
        if board[row][col] == ' ':
            for i in range(row - 1, row + 2):
                for j in range(col - 1, col + 2):
                    reveal_board(board, i, j, visited)

def play_minesweeper():
    rows = 5
    cols = 5
    bombs = 24

    board = create_board(rows, cols, bombs)
    revealed_cells = set()

    while True:
        print_board(board)

        row = int(input("Enter row (0 to {}): ".format(rows - 1)))
        col = int(input("Enter column (0 to {}): ".format(cols - 1)))

        if board[row][col] == 'B':
            print("Game Over! You hit a bomb.")
            break
        else:
            bombs_nearby = count_adjacent_bombs(board, row, col)
            board[row][col] = str(bombs_nearby) if bombs_nearby > 0 else ' '
            revealed_cells.add((row, col))
            reveal_board(board, row, col, revealed_cells)

        if len(revealed_cells) == (rows * cols) - bombs:
            print("Congratulations! You've cleared the minefield.")
            break

if __name__ == "__main__":
    play_minesweeper()
