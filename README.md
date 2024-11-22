document.addEventListener("DOMContentLoaded", function () {
    // Create CSS styles using JavaScript
    const style = document.createElement('style');
    style.innerHTML = `
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #2c3e50;
            margin: 0;
            flex-direction: column;
            color: white;
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
    `;
    document.head.appendChild(style);

    // Create HTML elements using JavaScript
    const body = document.body;

    // Create Canvas for the wheel
    const canvas = document.createElement('canvas');
    canvas.id = "wheelCanvas";
    body.appendChild(canvas);

    // Create the Spin Button
    const spinButton = document.createElement('button');
    spinButton.id = "spinButton";
    spinButton.textContent = "Spin the Wheel!";
    spinButton.onclick = spinWheel;
    body.appendChild(spinButton);

    // Create result display
    const resultDiv = document.createElement('div');
    resultDiv.id = "result";
    body.appendChild(resultDiv);

    // Initialize wheel variables
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
        }

        resultDiv.textContent = `Congratulations! You won ${prize}`;
        spinButton.disabled = false;
    }

    function easeOut(t, b, c, d) {
        const ts = (t /= d) * t;
        const tc = ts * t;
        return b + c * (tc + -3 * ts + 3 * t);
    }

    function spinWheel() {
        spinAngleStart = Math.random() * 10 + 10;
        spinTime = 0;
        spinTimeTotal = Math.random() * 3000 + 4000;
        spinButton.disabled = true;
        rotateWheel();
    }
});
