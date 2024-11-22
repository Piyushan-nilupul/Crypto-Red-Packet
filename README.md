<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Winning Spin Wheel with Countdown</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #2c3e50;
            margin: 0;
            flex-direction: column;
            color: white;
            position: relative;
        }

        #wheelCanvas {
            background-color: #34495e;
            border-radius: 50%;
            margin-bottom: 20px;
            max-width: 90vmin;
            max-height: 90vmin;
        }

        #spinButton {
    padding: 10px 20px;
    font-size: 18px;
    background-color: #27ae60;
    border: none;
    color: white;
    cursor: pointer;
    border-radius: 5px;
    position: absolute;
    bottom: 20px; /* Position it just below the wheel */
    left: 50%; /* Position it horizontally in the center */
    transform: translateX(-50%); /* Offset it horizontally to perfectly center */
}

        #spinButton:disabled {
            background-color: #95a5a6;
        }

        #result {
            margin-top: 20px;
            font-size: 24px;
            font-weight: bold;
            text-align: center;
        }

        #countdown {
            font-size: 18px;
            margin-bottom: 10px;
            text-align: center;
        }
    </style>
</head>
<body>
    <div id="countdown"></div>
    <canvas id="wheelCanvas"></canvas>
    <button id="spinButton" onclick="spinWheel()">Spin the Wheel!</button>
    <div id="result"></div>

    <script>
        const segments = [
            "Prize 1 (Click to Copy Secret Code)",
            "Prize 2",
            "Prize 3",
            "Prize 4",
            "Prize 5",
            "Prize 6",
            "Prize 7",
            "Prize 8"
        ];

        const colors = [
            "#f1c40f", "#e74c3c", "#3498db", "#2ecc71",
            "#9b59b6", "#e67e22", "#1abc9c", "#34495e"
        ];

        let startAngle = 0;
        let arcSize = (2 * Math.PI) / segments.length;
        let spinTimeout = null;
        let spinAngleStart = 10;
        let spinTime = 0;
        let spinTimeTotal = 0;
        let spinCount = 0;
        const maxSpins = 3;
        const cooldownTime = 60 * 60 * 1000; // 1 hour in milliseconds
        let cooldownTimeout = null;
        let countdownInterval = null;
        const secretCode = "SecretCode123";
        const canvas = document.getElementById("wheelCanvas");
        const ctx = canvas.getContext("2d");

        // Responsive canvas size setup
        function resizeCanvas() {
            const size = Math.min(window.innerWidth, window.innerHeight) * 0.8;
            canvas.width = size;
            canvas.height = size;
            drawWheel();
        }

        window.addEventListener('resize', resizeCanvas);
        window.addEventListener('load', resizeCanvas);

        function drawWheel() {
            const radius = canvas.width / 2;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < segments.length; i++) {
                const angle = startAngle + i * arcSize;
                ctx.fillStyle = colors[i];
                ctx.beginPath();
                ctx.moveTo(radius, radius);
                ctx.arc(radius, radius, radius, angle, angle + arcSize);
                ctx.lineTo(radius, radius);
                ctx.fill();
                ctx.save();
                ctx.fillStyle = "white";
                ctx.translate(
                    radius + Math.cos(angle + arcSize / 2) * (radius - 50),
                    radius + Math.sin(angle + arcSize / 2) * (radius - 50)
                );
                ctx.rotate(angle + arcSize / 2 + Math.PI / 2);
                ctx.fillText(segments[i], -ctx.measureText(segments[i]).width / 2, 0);
                ctx.restore();
            }
        }

        function rotateWheel() {
            const spinAngle = spinAngleStart - easeOut(spinTime, 0, spinAngleStart, spinTimeTotal);
            startAngle += (spinAngle * Math.PI) / 180;
            drawWheel();
            spinTime += 30;
            if (spinTime >= spinTimeTotal) {
                stopRotateWheel();
            } else {
                spinTimeout = setTimeout(rotateWheel, 30);
            }
        }

        function stopRotateWheel() {
            const degrees = (startAngle * 180) / Math.PI + 90;
            const arcd = (arcSize * 180) / Math.PI;
            const index = Math.floor((360 - (degrees % 360)) / arcd);
            const prize = segments[index];

            if (prize.startsWith("Prize 1")) {
                navigator.clipboard.writeText(secretCode)
                    .then(() => {
                        alert("Secret Code copied to clipboard!");
                    })
                    .catch(err => {
                        console.error("Error copying secret code: ", err);
                        alert("Failed to copy the secret code.");
                    });
            }

            document.getElementById("result").textContent = `Congratulations! You won ${prize}`;
            document.getElementById("spinButton").disabled = false;

            // Spin counter
            spinCount++;
            if (spinCount >= maxSpins) {
                disableSpinButton();
            }
        }

        function easeOut(t, b, c, d) {
            const ts = (t /= d) * t;
            const tc = ts * t;
            return b + c * (tc + -3 * ts + 3 * t);
        }

        function spinWheel() {
            if (spinCount >= maxSpins) {
                alert("You have used all your spins. Please wait an hour.");
                return;
            }

            spinAngleStart = Math.random() * 10 + 10;
            spinTime = 0;
            spinTimeTotal = Math.random() * 3000 + 4000;
            document.getElementById("spinButton").disabled = true;
            rotateWheel();
        }

        function disableSpinButton() {
            document.getElementById("spinButton").disabled = true;
            startCountdown(cooldownTime);

            // Set a cooldown to reset spins after 1 hour
            cooldownTimeout = setTimeout(() => {
                spinCount = 0;
                document.getElementById("spinButton").disabled = false;
                clearInterval(countdownInterval);
                document.getElementById("countdown").textContent = "You can spin again!";
            }, cooldownTime);
        }

        // Countdown timer
        function startCountdown(duration) {
            let timeLeft = duration / 1000; // convert milliseconds to seconds
            countdownInterval = setInterval(() => {
                const minutes = Math.floor(timeLeft / 60);
                const seconds = timeLeft % 60;
                document.getElementById("countdown").textContent = `Next spin in: ${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
                timeLeft--;

                if (timeLeft < 0) {
                    clearInterval(countdownInterval);
                }
            }, 1000);
        }

        drawWheel();
    </script>
</body>
</html>
