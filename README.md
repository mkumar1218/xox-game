<!DOCTYPE html>
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
            flex-direction: column;
        }
        
        .player-inputs {
            margin-bottom: 20px;
        }

        .player-inputs label, .player-inputs input {
            margin-right: 10px;
        }

        .game-container {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
        }
        
        .cell {
            width: 100px;
            height: 100px;
            background-color: #ffffff;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
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

        button {
            margin-top: 10px;
            padding: 10px 20px;
            font-size: 1rem;
        }
    </style>
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

    <h1>XOX Game</h1>

    <div class="game-container" id="game-container"></div>
    <div class="status" id="status"></div>
    <button onclick="resetGame()">Restart Game</button>

    <script>
        const gameContainer = document.getElementById('game-container');
        const statusText = document.getElementById('status');
        const startGameButton = document.getElementById('startGame');
        const playerXInput = document.getElementById('playerX');
        const playerOInput = document.getElementById('playerO');
        
        let currentPlayer = 'X';
        let board = ['', '', '', '', '', '', '', '', ''];
        let playerXName = 'Player X';
        let playerOName = 'Player O';

        startGameButton.addEventListener('click', () => {
            playerXName = playerXInput.value || 'Player X';
            playerOName = playerOInput.value || 'Player O';
            statusText.textContent = `${playerXName}'s Turn (X)`;
            createBoard();
        });

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
                const winner = currentPlayer === 'X' ? playerXName : playerOName;
                statusText.textContent = `${winner} Wins! 🎉`;
                endGame();
            } else if (board.every(cell => cell !== '')) {
                statusText.textContent = 'Draw!';
            } else {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                const currentPlayerName = currentPlayer === 'X' ? playerXName : playerOName;
                statusText.textContent = `${currentPlayerName}'s Turn (${currentPlayer})`;
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
            playerXName = playerXInput.value || 'Player X';
            playerOName = playerOInput.value || 'Player O';
            statusText.textContent = `${playerXName}'s Turn (X)`;
            createBoard();
        }

        createBoard();
    </script>
</body>
</html>


