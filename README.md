<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }

        .game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        .status {
            font-size: 1.5rem;
            font-weight: bold;
            color: #444;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            background-color: #333;
        }

        .cell {
            width: 100px;
            height: 100px;
            background-color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 3rem;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .cell:hover {
            background-color: #eee;
        }

        .cell.x { color: #ff4757; }
        .cell.o { color: #2ed573; }

        .controls {
            display: flex;
            gap: 10px;
        }

        button {
            padding: 10px 20px;
            font-size: 1rem;
            border: none;
            border-radius: 5px;
            background-color: #3498db;
            color: white;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        button:hover {
            background-color: #2980b9;
        }

        .scores {
            display: flex;
            gap: 20px;
            margin-top: 10px;
        }

        .score {
            font-size: 1.2rem;
            color: #555;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Stopwatch</h1>
        <div class="status" id="status">00:00:00</div>
        <div class="timer-display">
            <span id="display">00:00:00</span>
        </div>
        <div class="controls">
            <button id="start">Start</button>
            <button id="pause">Pause</button>
            <button id="reset">Reset</button>
            <button id="lap">Lap</button>
        </div>
        <div class="laps" id="laps">
            <h3>Lap Times</h3>
            <ul id="lap-times"></ul>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const display = document.getElementById('display');
            const startButton = document.getElementById('start');
            const pauseButton = document.getElementById('pause');
            const resetButton = document.getElementById('reset');
            const lapButton = document.getElementById('lap');
            const lapTimes = document.getElementById('lap-times');

            let startTime;
            let elapsedTime = 0;
            let timerInterval;
            let isRunning = false;

            function formatTime(ms) {
                let date = new Date(ms);
                let minutes = date.getUTCMinutes();
                let seconds = date.getUTCSeconds();
                let milliseconds = date.getUTCMilliseconds();

                return (
                    String(minutes).padStart(2, '0') + ':' +
                    String(seconds).padStart(2, '0') + '.' +
                    String(Math.floor(milliseconds/10)).padStart(2, '0')
                );
            }

            function startTimer() {
                if (!isRunning) {
                    startTime = Date.now() - elapsedTime;
                    timerInterval = setInterval(function printTime() {
                        elapsedTime = Date.now() - startTime;
                        display.textContent = formatTime(elapsedTime);
                    }, 10);
                    isRunning = true;
                }
            }

            function checkGameResult() {
                const winningConditions = [
                    [0, 1, 2], [3, 4, 5], [6, 7, 8], // rows
                    [0, 3, 6], [1, 4, 7], [2, 5, 8], // columns
                    [0, 4, 8], [2, 4, 6]             // diagonals
                ];

                let roundWon = false;

                for (let condition of winningConditions) {
                    const [a, b, c] = condition;
                    if (boardState[a] && boardState[a] === boardState[b] && boardState[a] === boardState[c]) {
                        roundWon = true;
                        break;
                    }
                }

                if (roundWon) {
                    status.textContent = `Player ${currentPlayer} wins!`;
                    gameActive = false;
                    
                    // Update scores
                    if (currentPlayer === 'X') {
                        xScore++;
                        xScoreElement.textContent = xScore;
                    } else {
                        oScore++;
                        oScoreElement.textContent = oScore;
                    }
                    return;
                }

                // Check for draw
                if (!boardState.includes('')) {
                    status.textContent = 'Game ended in a draw!';
                    gameActive = false;
                    return;
                }

                // Switch player
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                status.textContent = `Player ${currentPlayer}'s turn`;
            }

            resetButton.addEventListener('click', resetGame);
            newGameButton.addEventListener('click', newGame);

            function resetGame() {
                boardState = ['', '', '', '', '', '', '', '', ''];
                currentPlayer = 'X';
                gameActive = true;
                status.textContent = `Player ${currentPlayer}'s turn`;
                
                const cells = document.querySelectorAll('.cell');
                cells.forEach(cell => {
                    cell.textContent = '';
                    cell.classList.remove('x', 'o');
                });
            }

            function newGame() {
                resetGame();
                xScore = 0;
                oScore = 0;
                xScoreElement.textContent = '0';
                oScoreElement.textContent = '0';
            }
        });
    </script>
</body>
</html>
