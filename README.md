<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Be My Valentine?</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            height: 100vh;
            /* Use dvh for better mobile browser support (address bar handling) */
            height: 100dvh; 
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: linear-gradient(to right, #ee9ca7, #ffdde1);
            font-family: 'Arial', sans-serif;
            overflow: hidden;
            text-align: center;
            -webkit-tap-highlight-color: transparent; /* Removes blue box on iPhone tap */
        }

        h1 {
            color: #d6336c;
            font-size: 2rem;
            margin-bottom: 20px;
            padding: 0 20px;
            z-index: 10;
        }

        .buttons {
            display: flex;
            gap: 20px;
            margin-top: 20px;
            z-index: 10;
            position: relative;
        }

        button {
            padding: 15px 40px;
            font-size: 1.2rem;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.1s;
        }

        #yesBtn {
            background-color: #28a745;
            color: white;
            z-index: 10;
        }

        #noBtn {
            background-color: #dc3545;
            color: white;
            position: fixed; /* Changed to FIXED for better movement */
            z-index: 5; /* Lower z-index so it doesn't overlap Yes button weirdly */
            /* Initial position relative to the flex container won't apply here, 
               so we let JS set the initial spot or center it manually */
            left: 50%;
            top: 60%;
            transform: translate(-50%, -50%);
        }

        #feedback-msg {
            margin-top: 30px;
            font-size: 1.5rem;
            font-weight: bold;
            color: #d6336c;
            min-height: 2em;
            z-index: 10;
            pointer-events: none; /* Allows clicks to pass through text */
        }

        /* Success Message & Heart */
        #success-container {
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.9);
            z-index: 100;
        }

        .heart {
            background-color: #ff4d6d;
            display: inline-block;
            height: 50px;
            margin: 0 10px;
            position: relative;
            top: 0;
            transform: rotate(-45deg);
            width: 50px;
            animation: heartbeat 1.2s infinite;
        }

        .heart:before,
        .heart:after {
            content: "";
            background-color: #ff4d6d;
            border-radius: 50%;
            height: 50px;
            position: absolute;
            width: 50px;
        }

        .heart:before { top: -25px; left: 0; }
        .heart:after { left: 25px; top: 0; }

        @keyframes heartbeat {
            0% { transform: scale(1) rotate(-45deg); }
            50% { transform: scale(1.3) rotate(-45deg); }
            100% { transform: scale(1) rotate(-45deg); }
        }

        .big-msg {
            font-size: 2.5rem;
            color: #d6336c;
            margin-top: 50px;
            padding: 20px;
        }
    </style>
</head>
<body>

    <div id="main-content">
        <h1>Do you want to be my Valentine?</h1>
        <div class="buttons">
            <button id="yesBtn">Yes</button>
            <button id="noBtn">No</button>
        </div>
        <p id="feedback-msg"></p>
    </div>

    <div id="success-container">
        <div class="heart"></div>
        <h1 class="big-msg">Clever got you! ❤️</h1>
    </div>

    <audio id="bgMusic" loop>
        <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
    </audio>

    <script>
        const yesBtn = document.getElementById('yesBtn');
        const noBtn = document.getElementById('noBtn');
        const feedbackMsg = document.getElementById('feedback-msg');
        const successContainer = document.getElementById('success-container');
        const audio = document.getElementById('bgMusic');

        // Initial setup to place the NO button correctly on load
        // We do this so it doesn't jump immediately
        const buttonsContainer = document.querySelector('.buttons');
        const rect = buttonsContainer.getBoundingClientRect();
        
        // Slightly offset the NO button initially so it looks centered
        // but acts as a fixed element
        noBtn.style.left = (rect.left + rect.width / 2 + 60) + 'px';
        noBtn.style.top = (rect.top + rect.height / 2) + 'px';


        const funnyMessages = [
            "Nice try!", "Too slow!", "I'm fast!", "Can't catch me!", 
            "Nope!", "Try again!", "Missed me!", "Wrong answer!"
        ];

        function moveButton(e) {
            // CRITICAL FOR IPHONE: Stop the touch from turning into a click
            if(e) {
                e.preventDefault();
                e.stopPropagation();
            }

            const winWidth = window.innerWidth;
            const winHeight = window.innerHeight;
            
            // Random position within safe bounds (10% padding)
            const newX = Math.random() * (winWidth - 100) + 20;
            const newY = Math.random() * (winHeight - 100) + 20;

            noBtn.style.left = `${newX}px`;
            noBtn.style.top = `${newY}px`;
            noBtn.style.transform = 'none'; // Reset initial translate

            // Update text
            const randomMsg = funnyMessages[Math.floor(Math.random() * funnyMessages.length)];
            feedbackMsg.innerText = randomMsg;
            feedbackMsg.style.color = getRandomColor();
        }

        function getRandomColor() {
            const colors = ['#d6336c', '#ff0000', '#800080', '#0000FF'];
            return colors[Math.floor(Math.random() * colors.length)];
        }

        // --- IMPROVED EVENT LISTENERS FOR IPHONE ---

        // 1. TouchStart: Fires immediately when finger touches screen
        noBtn.addEventListener('touchstart', moveButton, { passive: false });

        // 2. MouseOver: For PC
        noBtn.addEventListener('mouseover', moveButton);

        // 3. Click: Fallback if they somehow manage to click
        noBtn.addEventListener('click', moveButton);

        // YES Button Action
        yesBtn.addEventListener('click', () => {
            successContainer.style.display = 'flex';
            audio.play().catch(e => console.log("Audio needs user interaction first"));
        });

        // Add touch support for Yes button to ensure snappy response on iPhone
        yesBtn.addEventListener('touchstart', (e) => {
            e.preventDefault(); // Stop ghost clicks
            successContainer.style.display = 'flex';
            audio.play().catch(e => console.log("Audio needs user interaction first"));
        });

    </script>
</body>
</html>

