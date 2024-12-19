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

        .celebration {
            display: none; /* Hidden by default, displayed after win */
            text-align: center;
            margin-top: 20px;
        }

        .celebration img {
            max-width: 100%;
            height: auto;
        }

        .celebration h2 {
            font-size: 2rem;
            color: #ff5722;
        }
    </style>
</head>
<body>
    <h1>XOX Game</h1>
    <div class="game-container" id="game-container"></div>
    <div class="status" id="status"></div>
    <button onclick="resetGame()">Restart Game</button>

    <!-- Celebration Section -->
    <div class="celebration" id="celebration">
        <h2>Congratulations, <span id="winner-name">Player X</span> Wins!</h2>
        <img src="game-celebration.png" alt="Winner Celebration Image">
    </div>

    <script>
        const gameContainer = document.getElementById('game-container');
        const statusText = document.getElementById('status');
        const celebrationDiv = document.getElementById('celebration');
        const winnerNameText = document.getElementById('winner-name');
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
                endGame(currentPlayer);
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

        function endGame(winner) {
            document.querySelectorAll('.cell').forEach(cell => cell.classList.add('taken'));
            celebrationDiv.style.display = 'block'; // Show the celebration image and message
            winnerNameText.textContent = `Player ${winner}`; // Display the winner's name
        }

        function resetGame() {
            board = ['', '', '', '', '', '', '', '', ''];
            currentPlayer = 'X';
            statusText.textContent = `Player ${currentPlayer}'s Turn`;
            createBoard();
            celebrationDiv.style.display = 'none'; // Hide celebration message on reset
        }

        createBoard();
    </script>
</body>
</html>


