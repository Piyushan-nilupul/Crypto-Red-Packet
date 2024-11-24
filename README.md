<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Customized Lucky Wheel</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 60vh;
            margin: 0;
            font-family: Arial, sans-serif;
            flex-direction: column;
            background-color: #FFFFFF; /* Background color specified */
        }

        #wheel {
            width: 250px;
            height: 250px;
            border-radius: 50%;
            border: 3px solid #333;
            position: relative;
            overflow: hidden;
            box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.1); /* Shadow enabled */
        }

        .segment {
            position: absolute;
            width: 50%;
            height: 50%;
            background-color: #f1f1f1;
            transform-origin: 100% 100%;
            clip-path: polygon(0 0, 100% 0, 100% 100%);
        }

        #spinButton {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Ba</h1> <!-- Title specified in JSON -->
    <div id="wheel"></div>
    <button id="spinButton">Spin</button>
    <audio id="spinSound" src="ticking-sound.mp3"></audio>
    <audio id="winSound" src="applause-sound-soft.mp3"></audio>

    <script>
        const wheel = document.getElementById('wheel');
        const spinButton = document.getElementById('spinButton');
        const spinSound = document.getElementById('spinSound');
        const winSound = document.getElementById('winSound');

        const segmentColors = [
            '#3369E8', '#D50F25', '#EEB211', '#009925'
        ]; // Only enabled colors used
        const segmentLabels = ['Ali', 'Beatriz', 'Charles', 'Diya', 'Eric', 'Fatima', 'Hanna'];
        const segmentCount = segmentLabels.length;
        const segmentAngle = 360 / segmentCount;
        let currentRotation = 0;

        // Sound settings
        spinSound.volume = 0.5; // 50% volume for during spin sound
        winSound.volume = 0.5;   // 50% volume for after spin sound

        // Create wheel segments
        function createSegments() {
            for (let i = 0; i < segmentCount; i++) {
                const segment = document.createElement('div');
                segment.classList.add('segment');
                segment.style.backgroundColor = segmentColors[i % segmentColors.length];
                segment.style.transform = `rotate(${i * segmentAngle}deg)`;
                wheel.appendChild(segment);

                // Add label
                const label = document.createElement('div');
                label.style.position = 'absolute';
                label.style.top = '50%';
                label.style.left = '50%';
                label.style.transform = `rotate(${segmentAngle / 2}deg) translate(-50%, -50%) rotate(-${i * segmentAngle + segmentAngle / 2}deg)`;
                label.style.fontSize = '14px';
                label.style.textAlign = 'center';
                label.style.color = '#FFFFFF'; // Text color for contrast
                label.innerText = segmentLabels[i];
                segment.appendChild(label);
            }
        }

        // Spin the wheel
        function spinWheel() {
            const randomAngle = Math.floor(Math.random() * 360) + 1080; // 1080° ensures at least 3 full spins
            currentRotation += randomAngle;
            wheel.style.transition = 'transform 10s ease-out'; // 10 seconds spin time
            wheel.style.transform = `rotate(${currentRotation}deg)`;
            
            // Play during spin sound
            spinSound.play();

            // Display result after spinning
            setTimeout(() => {
                spinSound.pause();
                spinSound.currentTime = 0;

                // Launch confetti (if implemented)
                if (true) {
                    alert('Confetti effect here!');
                }

                // Play win sound
                winSound.play();

                const winningSegmentIndex = Math.floor(((currentRotation % 360) / segmentAngle)) % segmentCount;
                const winningLabel = segmentLabels[winningSegmentIndex];

                // Display winner dialog
                alert(`Congratulations! You won: ${winningLabel}`);
            }, 10000); // Wait for the animation to complete
        }

        // Initialize segments and add event listener for spin button
        createSegments();
        spinButton.addEventListener('click', spinWheel);
    </script>
</body>
</html>
