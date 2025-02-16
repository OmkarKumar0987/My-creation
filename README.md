<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Tic Tac Toe</h1>
        <div class="game-board">
            <div class="cell" data-index="0"></div>
            <div class="cell" data-index="1"></div>
            <div class="cell" data-index="2"></div>
            <div class="cell" data-index="3"></div>
            <div class="cell" data-index="4"></div>
            <div class="cell" data-index="5"></div>
            <div class="cell" data-index="6"></div>
            <div class="cell" data-index="7"></div>
            <div class="cell" data-index="8"></div>
        </div>
        <p class="status">Player X's turn</p>
        <button class="restart">Restart Game</button>
    </div>
    <script src="script.js"></script>
</body>
</html>body {
    font-family: 'Arial', sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: linear-gradient(135deg, #f4a261, #e76f51);
    margin: 0;
    text-align: center;
}

.container {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
}

h1 {
    margin-bottom: 10px;
    color: #264653;
}

.game-board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 5px;
    margin: 20px auto;
}

.cell {
    width: 100px;
    height: 100px;
    background: #2a9d8f;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2.5rem;
    font-weight: bold;
    color: white;
    border-radius: 5px;
    cursor: pointer;
    transition: transform 0.2s ease-in-out;
}

.cell:hover {
    transform: scale(1.05);
}

.cell.winning {
    background: #e9c46a;
    animation: bounce 0.5s ease-in-out alternate infinite;
}

@keyframes bounce {
    from { transform: scale(1); }
    to { transform: scale(1.1); }
}

.status {
    font-size: 1.2rem;
    color: #264653;
    margin: 10px 0;
}

.restart {
    background: #e63946;
    color: white;
    border: none;
    padding: 10px 20px;
    font-size: 1rem;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s;
}

.restart:hover {
    background: #d62828;
}

@media (max-width: 400px) {
    .game-board {
        grid-template-columns: repeat(3, 80px);
        grid-template-rows: repeat(3, 80px);
    }

    .cell {
        width: 80px;
        height: 80px;
        font-size: 2rem;
    }
}const cells = document.querySelectorAll(".cell");
const statusText = document.querySelector(".status");
const restartBtn = document.querySelector(".restart");
let currentPlayer = "X";
let board = ["", "", "", "", "", "", "", "", ""];
let gameActive = true;

const winPatterns = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
    [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
    [0, 4, 8], [2, 4, 6]  // Diagonals
];

cells.forEach(cell => {
    cell.addEventListener("click", () => {
        const index = cell.dataset.index;
        if (board[index] === "" && gameActive) {
            board[index] = currentPlayer;
            cell.textContent = currentPlayer;
            cell.style.pointerEvents = "none";
            checkWinner();
            switchPlayer();
        }
    });
});

function checkWinner() {
    let won = false;
    winPatterns.forEach(pattern => {
        const [a, b, c] = pattern;
        if (board[a] && board[a] === board[b] && board[a] === board[c]) {
            won = true;
            cells[a].classList.add("winning");
            cells[b].classList.add("winning");
            cells[c].classList.add("winning");
        }
    });

    if (won) {
        statusText.textContent = `Player ${currentPlayer} Wins! 🎉`;
        gameActive = false;
        return;
    }

    if (!board.includes("")) {
        statusText.textContent = "It's a Draw! 🤝";
        gameActive = false;
    }
}

function switchPlayer() {
    if (gameActive) {
        currentPlayer = currentPlayer === "X" ? "O" : "X";
        statusText.textContent = `Player ${currentPlayer}'s turn`;
    }
}

restartBtn.addEventListener("click", () => {
    board = ["", "", "", "", "", "", "", "", ""];
    cells.forEach(cell => {
        cell.textContent = "";
        cell.classList.remove("winning");
        cell.style.pointerEvents = "auto";
    });
    currentPlayer = "X";
    statusText.textContent = "Player X's turn";
    gameActive = true;
});
