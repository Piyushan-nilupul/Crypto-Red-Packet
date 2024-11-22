<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crypto Red Packet Spin Wheel</title>
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
            font-family: Arial, sans-serif;
        }

        #banner {
            font-size: 28px;
            font-weight: bold;
            margin-bottom: 20px;
            color: #f39c12;
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
            bottom: 50px;  /* Position button at the bottom of the wheel */
        }

        #spinButton:disabled {
            background-color: #95a5a6;
        }

        #result, #timer {
            margin-top: 20px;
            font-size: 24px;
            font-weight: bold;
            text-align: center;
        }
    </style>
</head>
<body>
    <div id="banner">Crypto Red Packet</div>
    <canvas id="wheelCanvas"></canvas>
    <button id="spinButton" onclick="spinWheel()">Spin the Wheel!</button>
    <div id="result"></div>
    <div id="timer"></div>

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
        let lastSpinTime = 0;

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

        const secretCode = "SecretCode123";

        function drawWheel() {
            const radius = canvas.width / 2;
            ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear canvas before drawing
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
                if (confirm("You won Prize 1! Click OK to copy the secret code.")) {
                    navigator.clipboard.writeText(secretCode)
                        .then(() => {
                            alert("Secret Code copied to clipboard!");
                        })
                        .catch(err => {
                            console.error("Error copying secret code: ", err);
                            alert("Failed to copy the secret code.");
                        });
                }
            } else {
                alert("Oops, try again!");
            }

            document.getElementById("result").textContent = `Congratulations! You won ${prize}`;
            document.getElementById("spinButton").disabled = false;
            spinCount++;
            lastSpinTime = Date.now();
            updateCooldownTimer();
        }

        function easeOut(t, b, c, d) {
            const ts = (t /= d) * t;
            const tc = ts * t;
            return b + c * (tc + -3 * ts + 3 * t);
        }

        function spinWheel() {
            if (spinCount >= maxSpins) {
                const currentTime = Date.now();
                if (currentTime - lastSpinTime < cooldownTime) {
                    alert("You have used all spins. Please wait for 1 hour.");
                    return;
                }
            }

            spinAngleStart = Math.random() * 10 + 10;
            spinTime = 0;
            spinTimeTotal = Math.random() * 3000 + 4000;
            document.getElementById("spinButton").disabled = true;
            rotateWheel();
        }

        // Countdown Timer
        function updateCooldownTimer() {
            const remainingTime = cooldownTime - (Date.now() - lastSpinTime);
            if (remainingTime <= 0) {
                document.getElementById("timer").textContent = "You can spin again!";
                return;
            }
            const minutes = Math.floor(remainingTime / 1000 / 60);
            const seconds = Math.floor((remainingTime / 1000) % 60);
            document.getElementById("timer").textContent = `Time remaining: ${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
            setTimeout(updateCooldownTimer, 1000);
        }

        drawWheel();
    </script>
</body>
</html>
