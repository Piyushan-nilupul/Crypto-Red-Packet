<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crypto Red Packet</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
        }

        h1 {
            color: #ff4500;
        }

        #countdown {
            font-size: 24px;
            color: #ff4500;
            margin-bottom: 20px;
        }

        #wheel {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-gap: 10px;
            margin: 20px auto;
            max-width: 400px;
        }

        .box {
            background-color: #ddd;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            font-size: 16px;
            cursor: pointer;
        }

        #spinButton {
            padding: 10px 20px;
            margin-top: 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 18px;
        }

        #spinButton:disabled {
            background-color: grey;
            cursor: not-allowed;
        }

        #note {
            font-size: 12px;
            color: grey;
            margin-top: 10px;
        }

        #signupButton {
            padding: 10px 20px;
            margin-top: 20px;
            background-color: #008CBA;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>Crypto Red Packet</h1>
    <div id="countdown">Countdown: 01:00:00</div>

    <div id="wheel">
        <div class="box" id="box1">Secret Word</div>
        <div class="box">Box 2</div>
        <div class="box">Box 3</div>
        <div class="box">Box 4</div>
        <div class="box">Box 5</div>
        <div class="box">Box 6</div>
        <div class="box">Box 7</div>
        <div class="box">Box 8</div>
    </div>

    <button id="spinButton" onclick="spinWheel()">Spin</button>
    <div id="note">*You have 3 chances to spin. After 3 spins, you must wait 1 hour.</div>
    <button id="signupButton">Sign Up</button>

    <script>
        let spinCount = 0;
        let maxSpins = 3;
        let countdownInterval;

        function spinWheel() {
            if (spinCount < maxSpins) {
                const randomBoxIndex = Math.floor(Math.random() * 8) + 1;
                const selectedBox = document.getElementById('box' + randomBoxIndex);

                if (selectedBox) {
                    alert('You selected: ' + selectedBox.textContent);

                    // If the selected box contains the secret word, copy it to clipboard
                    if (selectedBox.id === 'box1') {
                        copyToClipboard(selectedBox.textContent);
                        alert('Secret word copied to clipboard!');
                    }

                    spinCount++;
                }

                // Disable the button after 3 spins
                if (spinCount >= maxSpins) {
                    document.getElementById('spinButton').disabled = true;
                    startCountdown(3600); // 1 hour in seconds
                }
            }
        }

        function copyToClipboard(text) {
            const tempInput = document.createElement('input');
            document.body.appendChild(tempInput);
            tempInput.value = text;
            tempInput.select();
            document.execCommand('copy');
            document.body.removeChild(tempInput);
        }

        function startCountdown(seconds) {
            let remainingTime = seconds;
            countdownInterval = setInterval(() => {
                if (remainingTime <= 0) {
                    clearInterval(countdownInterval);
                    document.getElementById('spinButton').disabled = false;
                    spinCount = 0;
                    document.getElementById('countdown').textContent = 'Countdown: 01:00:00';
                } else {
                    const hours = Math.floor(remainingTime / 3600);
                    const minutes = Math.floor((remainingTime % 3600) / 60);
                    const secs = remainingTime % 60;
                    document.getElementById('countdown').textContent = `Countdown: ${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
                    remainingTime--;
                }
            }, 1000);
        }
    </script>
</body>
</html>
