<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lucky Box Game with Crypto Ticker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
            margin: 0;
            padding: 0;
        }
        /* Ticker container styling */
        .ticker-container {
            background-color: #333;
            color: white;
            padding: 10px 0;
            overflow: hidden;
            white-space: nowrap;
            font-size: 18px;
            font-weight: bold;
        }
        .ticker {
            display: inline-block;
            animation: scroll 20s linear infinite;
        }
        /* Crypto animation */
        @keyframes scroll {
            0% {
                transform: translateX(100%);
            }
            100% {
                transform: translateX(-100%);
            }
        }
        h1 {
            margin-top: 20px;
            color: #333;
        }
        .game-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr); /* 3 columns for 3x3 grid */
            gap: 10px; /* Space between boxes */
            justify-content: center;
            margin-top: 40px;
            max-width: 200px; /* Limit the container width to make it look neat */
            margin-left: auto; /* Center align horizontally */
            margin-right: auto; /* Center align horizontally */
        }
        .box {
            width: 60px;  /* Adjusted box size */
            height: 60px;
            background-color: #007bff;
            border: 2px solid #0056b3;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            color: white;
            cursor: pointer;
            transition: transform 0.2s, background-color 0.3s;
        }
        .box:hover {
            transform: scale(1.1);
        }
        .box.lucky {
            background-color: #28a745;
            border-color: #1e7e34;
            animation: bounce 0.8s ease;
        }
        .box.unlucky {
            background-color: #dc3545;
            border-color: #c82333;
            animation: shake 0.5s ease;
        }
        .message {
            margin-top: 20px;
            font-size: 20px;
            color: #555;
        }
        #timer {
            margin-top: 10px;
            font-size: 18px;
            color: #ff0000;
        }

        /* Button styling */
        .signup-button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            color: white;
            background-color: #f0b90b; /* Binance yellow */
            border: none;
            cursor: pointer;
            text-decoration: none;
            border-radius: 5px;
        }

        .signup-button:hover {
            background-color: #d79a09; /* Darker shade on hover */
        }

        /* Bounce animation for winning */
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {
                transform: translateY(0);
            }
            40% {
                transform: translateY(-20px);
            }
            60% {
                transform: translateY(-10px);
            }
        }

        /* Shake animation for losing */
        @keyframes shake {
            0% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            50% { transform: translateX(10px); }
            75% { transform: translateX(-10px); }
            100% { transform: translateX(0); }
        }
    </style>
</head>
<body>
    <!-- Crypto ticker as headline -->
    <div class="ticker-container">
        <div class="ticker">
            🚀 Bitcoin (BTC) | 🌕 Ethereum (ETH) | 🌐 Binance Coin (BNB) | 💎 Ripple (XRP) | 
            📉 Cardano (ADA) | 🔥 Solana (SOL) | 🌀 Polkadot (DOT) | 🎯 Dogecoin (DOGE) | 
            💰 Litecoin (LTC) | 🌟 Chainlink (LINK)
        </div>
    </div>

    <h1>Lucky Box Game</h1>
    
    <!-- Game Container -->
    <div class="game-container" id="gameContainer">
        <!-- Boxes will be generated here by JavaScript -->
    </div>
    <div class="message" id="message"></div>
    <div id="timer"></div>

    <!-- Note below the boxes -->
    <h2>
        a<br>
        B<br>
        C<br>
        D
    </h2>

    <!-- Sign Up Button -->
    <a href="https://www.binance.com" target="_blank" class="signup-button">Sign Up on Binance</a>

    <script>
        const totalBoxes = 9;
        let luckyBoxIndex = Math.floor(Math.random() * totalBoxes);
        const gameContainer = document.getElementById('gameContainer');
        const messageElement = document.getElementById('message');
        const timerElement = document.getElementById('timer');

        let attempts = 0;
        const maxAttempts = 3;
        let isGameDisabled = false;
        const cooldownTime = 10 * 60 * 1000; // 10 minutes in milliseconds
        let cooldownEndTime;

        // Secret word
        const secretWord = "Congratulations! Your Secret Word is 'CryptoLuck'";

        // Function to create the boxes
        function createBoxes() {
            gameContainer.innerHTML = ''; // Clear existing boxes
            messageElement.textContent = ''; // Clear message
            timerElement.textContent = ''; // Clear timer

            for (let i = 0; i < totalBoxes; i++) {
                const box = document.createElement('div');
                box.classList.add('box');
                box.textContent = '?';
                box.addEventListener('click', () => checkBox(i, box));
                gameContainer.appendChild(box);
            }
        }

        // Function to check if the clicked box is the lucky one
        function checkBox(index, box) {
            if (isGameDisabled) return;

            if (index === luckyBoxIndex) {
                box.classList.add('lucky');
                box.textContent = '🎉';
                messageElement.textContent = 'Congratulations! You found the lucky box!';
                messageElement.style.color = 'green';
                
                // Display secret word in a popup
                alert(secretWord);
                
                // Automatically restart the game after win
                setTimeout(restartGame, 2000);
            } else {
                box.classList.add('unlucky');
                box.textContent = '❌';
                attempts++;

                if (attempts >= maxAttempts) {
                    messageElement.textContent = 'No more attempts! Please wait 10 minutes.';
                    messageElement.style.color = 'red';
                    startCooldown();
                    endGame();
                } else {
                    messageElement.textContent = `Try Again! Attempts left: ${maxAttempts - attempts}`;
                    messageElement.style.color = 'red';
                }
            }
        }

        // Function to end the game
        function endGame() {
            // Disable all boxes after the game ends
            const boxes = document.querySelectorAll('.box');
            boxes.forEach(box => {
                box.style.pointerEvents = 'none';
            });
        }

        // Start the cooldown timer
        function startCooldown() {
            isGameDisabled = true;
            cooldownEndTime = Date.now() + cooldownTime;
            updateTimer();
            const cooldownInterval = setInterval(() => {
                if (!updateTimer()) {
                    clearInterval(cooldownInterval);
                }
            }, 1000);
        }

        // Update the timer countdown
        function updateTimer() {
            const remainingTime = cooldownEndTime - Date.now();
            if (remainingTime > 0) {
                const minutes = Math.floor(remainingTime / 60000);
                const seconds = Math.floor((remainingTime % 60000) / 1000);
                timerElement.textContent = `Cooldown: ${minutes}m ${seconds}s remaining`;
                return true;
            } else {
                timerElement.textContent = '';
                isGameDisabled = false;
                attempts = 0;
                createBoxes(); // Restart the game
                return false;
            }
        }

        // Restart game function
        function restartGame() {
            luckyBoxIndex = Math.floor(Math.random() * totalBoxes); // Choose a new lucky box
            attempts = 0; // Reset attempts
            createBoxes(); // Re-create the boxes
        }

        // Initialize game
        createBoxes();
    </script>
</body>
</html>
