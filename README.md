<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Guess the Number</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f8efd4;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
        }
        .container {
            background: linear-gradient(135deg, #ff9a9e, #fad0c4);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
            width: 90%;
            max-width: 350px;
        }
        h2 {
            color: #4a148c;
        }
        input, button {
            width: 100%;
            margin: 5px 0;
            padding: 12px;
            font-size: 18px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
        button {
            cursor: pointer;
            color: white;
            background: linear-gradient(135deg, #ff7eb3, #ff758c);
            border: none;
        }
        button:hover {
            background: linear-gradient(135deg, #ff5370, #d50000);
        }
        #result, #cnt {
            font-weight: bold;
        }
        .fullscreen {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background-color: #fff3e0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            font-weight: bold;
            color: #4a148c;
            padding: 20px;
            text-align: center;
        }
        @keyframes confetti {
            0% { transform: translateY(0); }
            100% { transform: translateY(100vh); }
        }
        .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            top: 0;
            border-radius: 50%;
            animation: confetti linear infinite;
        }
    </style>
</head>
<body>
    <div class="container" id="setup">
        <h2>Guess the Number</h2>
        <p>Enter a range:</p>
        <input type="number" id="low" placeholder="Enter lower bound">
        <input type="number" id="high" placeholder="Enter upper bound">
        <button onclick="game()">Start Game</button>
    </div>

    <div class="container" id="game" style="display: none;">
        <p>Guess the number (Range: <span id="range"></span>)</p>
        <input type="number" id="guess" placeholder="Enter your guess" onkeydown="if(event.key==='Enter') check()">
        <button onclick="check()">Guess</button>
        <p id="result"></p>
        <p id="cnt"></p>
    </div>

    <div id="fullscreen" class="fullscreen" style="display: none;"></div>

    <script>
        let rn, cnt, small, big;

        function game() {
            small = parseInt(document.getElementById('low').value);
            big = parseInt(document.getElementById('high').value);
            if (isNaN(small) || isNaN(big) || small >= big) {
                alert("Enter a valid range where the lower bound is smaller than the upper bound.");
                return;
            }
            rn = Math.floor(Math.random() * (big - small + 1)) + small;
            cnt = 0;
            document.getElementById('game').style.display = 'block';
            document.getElementById('setup').style.display = 'none';
            document.getElementById('range').innerText = `${small} - ${big}`;
            document.getElementById('result').innerText = '';
            document.getElementById('cnt').innerText = '';
        }

        function check() {
            let guess = parseInt(document.getElementById('guess').value);
            if (isNaN(guess)) {
                alert("Enter a valid number.");
                return;
            }
            cnt++;
            if (guess < rn) {
                document.getElementById('result').innerText = `The number is bigger than ${guess}`;
            } else if (guess > rn) {
                document.getElementById('result').innerText = `The number is smaller than ${guess}`;
            } else {
                res();
            }
        }

        function res() {
            let fullscreenDiv = document.getElementById('fullscreen');
            fullscreenDiv.innerHTML = `
                <p>You found it in ${cnt} moves!</p>
                <button onclick="regame()">Play Again</button>
                <button onclick="exitGame()">Exit</button>
            `;
            fullscreenDiv.style.display = 'flex';
            document.getElementById('game').style.display = 'none';
            startConfetti();
        }

        function regame() {
            document.getElementById('setup').style.display = 'block';
            document.getElementById('fullscreen').style.display = 'none';
            document.getElementById('low').value = "";
            document.getElementById('high').value = "";
            small = undefined;
            big = undefined;
            stopConfetti();
        }

        function exitGame() {
            document.getElementById('fullscreen').innerHTML = '<p>Thanks for playing!</p>';
            stopConfetti();
        }

        function startConfetti() {
            for (let i = 0; i < 50; i++) {
                let confetti = document.createElement("div");
                confetti.classList.add("confetti");
                confetti.style.left = Math.random() * 100 + "vw";
                confetti.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
                confetti.style.animationDuration = Math.random() * 3 + 2 + "s";
                confetti.style.top = `${Math.random() * -20}px`;
                confetti.style.width = `${Math.random() * 10 + 5}px`;
                confetti.style.height = confetti.style.width;
                document.body.appendChild(confetti);
                setTimeout(() => confetti.remove(), 5000);
            }
        }

        function stopConfetti() {
            document.querySelectorAll('.confetti').forEach(el => el.remove());
        }
    </script>
</body>
</html>
