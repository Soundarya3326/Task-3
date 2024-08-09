#include <iostream>
using namespace std;

char board[3][3];
char currentPlayer;

void initializeBoard() {
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            board[i][j] = ' ';
        }
    }
}

void displayBoard() {
    cout << "  0 1 2\n";
    for (int i = 0; i < 3; ++i) {
        cout << i << " ";
        for (int j = 0; j < 3; ++j) {
            cout << board[i][j];
            if (j < 2) cout << "|";
        }
        cout << "\n";
        if (i < 2) cout << "  -+-+-\n";
    }
}

bool isMoveValid(int row, int col) {
    return row >= 0 && row < 3 && col >= 0 && col < 3 && board[row][col] == ' ';
}

void playerMove() {
    int row, col;
    cout << "Player " << currentPlayer << ", enter your move (row and column): ";
    cin >> row >> col;

    while (!isMoveValid(row, col)) {
        cout << "Invalid move. Try again: ";
        cin >> row >> col;
    }

    board[row][col] = currentPlayer;
}

bool checkWin() {
    // Check rows and columns
    for (int i = 0; i < 3; ++i) {
        if ((board[i][0] == currentPlayer && board[i][1] == currentPlayer && board[i][2] == currentPlayer) ||
            (board[0][i] == currentPlayer && board[1][i] == currentPlayer && board[2][i] == currentPlayer)) {
            return true;
        }
    }

    // Check diagonals
    if ((board[0][0] == currentPlayer && board[1][1] == currentPlayer && board[2][2] == currentPlayer) ||
        (board[0][2] == currentPlayer && board[1][1] == currentPlayer && board[2][0] == currentPlayer)) {
        return true;
    }

    return false;
}

bool checkDraw() {
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            if (board[i][j] == ' ') {
                return false;
            }
        }
    }
    return true;
}

void switchPlayer() {
    currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
}

int main() {
    char playAgain;

    do {
        initializeBoard();
        currentPlayer = 'X';
        bool gameWon = false;
        bool gameDraw = false;

        while (!gameWon && !gameDraw) {
            displayBoard();
            playerMove();
            gameWon = checkWin();
            gameDraw = checkDraw();
            if (!gameWon && !gameDraw) {
                switchPlayer();
            }
        }

        displayBoard();

        if (gameWon) {
            cout << "Player " << currentPlayer << " wins!\n";
        } else if (gameDraw) {
            cout << "The game is a draw!\n";
        }

        cout << "Do you want to play again? (y/n): ";
        cin >> playAgain;
    } while (playAgain == 'y');

    return 0;
}
