<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Piedra, Papel o Tijeras</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-color: #f1f1f1;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .game-container {
            text-align: center;
            background-color: #fff;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 6px 18px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 100%;
        }

        h1 {
            margin-bottom: 30px;
            color: #333;
            font-size: 2rem;
        }

        .choices {
            display: flex;
            justify-content: space-around;
            margin-bottom: 30px;
        }

        .choice {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            background-color: #e0e0e0;
            border-radius: 10px;
            width: 120px;
            cursor: pointer;
            transition: transform 0.3s, background-color 0.3s;
            border: none;
        }

        .choice:hover {
            background-color: #f8c42d;
            transform: translateY(-5px);
        }

        .choice:active {
            transform: translateY(2px);
        }

        .icon {
            width: 50px;
            height: 50px;
            margin-bottom: 10px;
        }

        span {
            font-size: 18px;
            font-weight: 600;
            color: #333;
        }

        #result {
            font-size: 24px;
            margin-top: 30px;
            font-weight: bold;
            color: #333;
        }

        #computer-choice {
            font-size: 20px;
            margin-top: 10px;
            color: #777;
        }

        .win {
            color: #28a745; /* Verde para ganaste */
        }

        .lose {
            color: #dc3545; /* Rojo para perdiste */
        }

        .draw {
            color: #17a2b8; /* Azul para empate */
        }

        #statistics {
            margin-top: 20px;
            font-size: 18px;
        }

        .stat {
            margin-top: 10px;
        }

        .button {
            padding: 10px 20px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 20px;
        }

        .button:hover {
            background-color: #0056b3;
        }

    </style>
</head>
<body>
    <div class="game-container">
        <h1>Juego Piedra, Papel o Tijeras</h1>
        <div class="choices">
            <button class="choice" id="rock">
                <img src="https://img.icons8.com/ios-filled/50/000000/rock.png" alt="Piedra" class="icon">
                <span>Piedra</span>
            </button>
            <button class="choice" id="paper">
                <img src="https://img.icons8.com/ios-filled/50/000000/paper.png" alt="Papel" class="icon">
                <span>Papel</span>
            </button>
            <button class="choice" id="scissors">
                <img src="https://img.icons8.com/ios-filled/50/000000/scissors.png" alt="Tijeras" class="icon">
                <span>Tijeras</span>
            </button>
        </div>
        <div id="result"></div>
        <div id="computer-choice"></div>

        <div id="statistics">
            <div class="stat" id="wins">Victorias: 0</div>
            <div class="stat" id="losses">Derrotas: 0</div>
            <div class="stat" id="draws">Empates: 0</div>
            <div class="stat" id="streak">Racha más grande de victorias: 0</div>
        </div>

        <button class="button" id="restart">Volver a jugar</button>
    </div>

    <script>
        const choices = ['rock', 'paper', 'scissors'];
        const resultDiv = document.getElementById('result');
        const computerChoiceDiv = document.getElementById('computer-choice');
        const winsDiv = document.getElementById('wins');
        const lossesDiv = document.getElementById('losses');
        const drawsDiv = document.getElementById('draws');
        const streakDiv = document.getElementById('streak');
        const restartButton = document.getElementById('restart');

        let wins = 0;
        let losses = 0;
        let draws = 0;
        let streak = 0;
        let maxStreak = 0;

        const getComputerChoice = () => {
            const randomIndex = Math.floor(Math.random() * 3);
            return choices[randomIndex];
        }

        const getResult = (playerChoice, computerChoice) => {
            if (playerChoice === computerChoice) {
                draws++;
                return { result: "¡Es un empate!", class: 'draw' };
            }
            if (
                (playerChoice === 'rock' && computerChoice === 'scissors') ||
                (playerChoice === 'paper' && computerChoice === 'rock') ||
                (playerChoice === 'scissors' && computerChoice === 'paper')
            ) {
                wins++;
                streak++;
                if (streak > maxStreak) maxStreak = streak;
                return { result: "¡Ganaste!", class: 'win' };
            }
            losses++;
            streak = 0;
            return { result: "¡Perdiste!", class: 'lose' };
        }

        const handleChoice = (playerChoice) => {
            const computerChoice = getComputerChoice();
            computerChoiceDiv.textContent = `La computadora eligió: ${computerChoice.charAt(0).toUpperCase() + computerChoice.slice(1)}`;
            const { result, class: resultClass } = getResult(playerChoice, computerChoice);
            resultDiv.textContent = result;
            resultDiv.className = resultClass;

            updateStatistics();
        }

        const updateStatistics = () => {
            winsDiv.textContent = `Victorias: ${wins}`;
            lossesDiv.textContent = `Derrotas: ${losses}`;
            drawsDiv.textContent = `Empates: ${draws}`;
            streakDiv.textContent = `Racha más grande de victorias: ${maxStreak}`;
        }

        const resetGame = () => {
            wins = 0;
            losses = 0;
            draws = 0;
            streak = 0;
            maxStreak = 0;
            updateStatistics();
            resultDiv.textContent = '';
            computerChoiceDiv.textContent = '';
        }

        document.getElementById('rock').addEventListener('click', () => handleChoice('rock'));
        document.getElementById('paper').addEventListener('click', () => handleChoice('paper'));
        document.getElementById('scissors').addEventListener('click', () => handleChoice('scissors'));

        restartButton.addEventListener('click', resetGame);
    </script>
</body>
</html>
