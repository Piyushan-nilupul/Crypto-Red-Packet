<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lucky Wheel</title>
    <style>
        /* CSS for the Lucky Wheel */
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
            font-family: Arial, sans-serif;
            margin: 0;
        }

        .container {
            text-align: center;
        }

        .wheel {
            width: 300px;
            height: 300px;
            border-radius: 50%;
            border: 16px solid #4CAF50;
            position: relative;
            overflow: hidden;
            transition: transform 4s cubic-bezier(0.5, 0.1, 0.3, 1);
        }

        .segment {
            position: absolute;
            width: 50%;
            height: 50%;
            background-color: #f9c74f;
            text-align: center;
            line-height: 150px; /* Adjust line height based on wheel size */
            font-size: 24px;
            color: white;
            transform-origin: 100% 100%; /* Key for making the segments fit correctly */
        }

        /* Adjust the rotation for each segment */
        .segment:nth-child(1) { transform: rotate(0deg) skewY(-45deg); background-color: #f94144; }
        .segment:nth-child(2) { transform: rotate(45deg) skewY(-45deg); background-color: #f3722c; }
        .segment:nth-child(3) { transform: rotate(90deg) skewY(-45deg); background-color: #f9844a; }
        .segment:nth-child(4) { transform: rotate(135deg) skewY(-45deg); background-color: #f9c74f; }
        .segment:nth-child(5) { transform: rotate(180deg) skewY(-45deg); background-color: #90be6d; }
        .segment:nth-child(6) { transform: rotate(225deg) skewY(-45deg); background-color: #43aa8b; }
        .segment:nth-child(7) { transform: rotate(270deg) skewY(-45deg); background-color: #577590; }
        .segment:nth-child(8) { transform: rotate(315deg) skewY(-45deg); background-color: #f9c74f; }

        #spinButton {
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
            margin-top: 20px;
        }

        #result {
            font-size: 20px;
            font-weight: bold;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="wheel" class="wheel">
            <div class="segment">1</div>
            <div class="segment">2</div>
            <div class="segment">3</div>
            <div class="segment">4</div>
            <div class="segment">5</div>
            <div class="segment">6</div>
            <div class="segment">7</div>
            <div class="segment">8</div>
        </div>
        <button id="spinButton">Spin the Wheel!</button>
        <div id="result"></div>
    </div>

    <script>
        // JavaScript to make the wheel spin
        document.getElementById('spinButton').addEventListener('click', spinWheel);

        function spinWheel() {
            const wheel = document.getElementById('wheel');
            const randomDegree = Math.floor(Math.random() * 360 + 3600); // Spin multiple times (10+ full rotations)
            wheel.style.transition = 'transform 4s cubic-bezier(0.5, 0.1, 0.3, 1)'; // Smooth transition for 4 seconds
            wheel.style.transform = `rotate(${randomDegree}deg)`;
            
            // Calculate the result after the spin completes
            const resultDegree = randomDegree % 360; // Get the final degree after multiple rotations
            setTimeout(() => {
                const segmentIndex = Math.floor(resultDegree / 45); // 8 segments (each 45 degrees)
                document.getElementById('result').innerText = `You landed on segment ${segmentIndex + 1}!`;

                // Reset the transition for the next spin
                wheel.style.transition = 'none';
            }, 4000); // Wait for the spin to finish (4 seconds)
        }
    </script>
</body>
</html>
