
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>XOX Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f4f4f9;
        }
        
        .game-container {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
        }
        
        .cell {
            width: 100px;
            align-items: center;
            height: 100px;
            background-color: #ffffff;
            display: flex;
            justify-content: center;
            font-size: 2rem;old;
            cursor: pointer;
            border: 2px solid #333;
        }
        
        .cell.taken {
            pointer-events: none;
        }
        
        .status {
            margin-top: 20px;
            font-size: 1.5rem;
        }
    </style>
</head>
<body>
    <div>
        <h1>XOX Game</h1>
        <div class="game-container" id="game-container"></div>
        <div class="status" id="status"></div>
        <button onclick="resetGame()">Restart Game</button>
    </div>

    <script>
        const gameContainer = document.getElementById('game-container');
        const statusText = document.getElementById('status');
        let currentPlayer = 'X';
        let board = ['', '', '', '', '', '', '', '', ''];

        function createBoard() {
            gameContainer.innerHTML = '';
            board.forEach((cell, index) => {
                const cellElement = document.createElement('div');
                cellElement.classList.add('cell');
                cellElement.dataset.index = index;
                cellElement.addEventListener('click', handleCellClick);
                cellElement.textContent = cell;
                gameContainer.appendChild(cellElement);
            });
        }

        function handleCellClick(e) {
            const index = e.target.dataset.index;
            if (board[index] !== '') return;

            board[index] = currentPlayer;
            e.target.textContent = currentPlayer;
            e.target.classList.add('taken');

            if (checkWin()) {
                statusText.textContent = `Player ${currentPlayer} Wins!`;
                endGame();
            } else if (board.every(cell => cell !== '')) {
                statusText.textContent = 'Draw!';
            } else {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                statusText.textContent = `Player ${currentPlayer}'s Turn`;
            }
        }

        function checkWin() {
            const winPatterns = [
                [0, 1, 2], [3, 4, 5], [6, 7, 8], // rows
                [0, 3, 6], [1, 4, 7], [2, 5, 8], // columns
                [0, 4, 8], [2, 4, 6] // diagonals
            ];

            return winPatterns.some(pattern => {
                const [a, b, c] = pattern;
                return board[a] && board[a] === board[b] && board[a] === board[c];
            });
        }

        function endGame() {
            document.querySelectorAll('.cell').forEach(cell => cell.classList.add('taken'));
        }

        function resetGame() {
            board = ['', '', '', '', '', '', '', '', ''];
            currentPlayer = 'X';
            statusText.textContent = `Player ${currentPlayer}'s Turn`;
            createBoard();
        }

        createBoard();
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>XOX Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <!-- Player Name Inputs -->
    <div class="player-inputs">
        <label for="playerX">Player X Name:</label>
        <input type="text" id="playerX" placeholder="Enter Player X's Name">
        <label for="playerO">Player O Name:</label>
        <input type="text" id="playerO" placeholder="Enter Player O's Name">
        <button id="startGame">Start Game</button>
    </div>
    
    <!-- Game Title -->
    <h1>XOX Game</h1>
    
    <!-- Game Board -->
    <div class="game-board">
        <div class="row">
            <button class="cell"></button>
            <button class="cell"></button>
            <button class="cell"></button>
        </div>
        <div class="row">
            <button class="cell"></button>
            <button class="cell"></button>
            <button class="cell"></button>
        </div>
        <div class="row">
            <button class="cell"></button>
            <button class="cell"></button>
            <button class="cell"></button>
        </div>
    </div>
    
    <!-- Winner Display -->
    <p id="winnerMessage"></p>
    
    <!-- Restart Button -->
    <button id="restartGame">Restart Game</button>
    
    <script src="script.js"></script>
</body>
</html>

