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
            height: 100dvh; 
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: linear-gradient(to right, #ee9ca7, #ffdde1);
            font-family: 'Arial', sans-serif;
            overflow: hidden;
            text-align: center;
            -webkit-tap-highlight-color: transparent;
        }

        h1 {
            color: #d6336c;
            font-size: 2rem;
            margin-bottom: 20px;
            padding: 0 20px;
        }

        .buttons {
            display: flex;
            gap: 20px;
            justify-content: center;
            align-items: center;
            width: 100%;
            height: 100px;
        }

        button {
            padding: 15px 40px;
            font-size: 1.2rem;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        #yesBtn {
            background-color: #ff4d6d;
            color: white;
            z-index: 100;
        }

        #noBtn {
            background-color: #868e96;
            color: white;
            position: fixed;
            z-index: 50;
            transition: all 0.2s ease;
        }

        #feedback-msg {
            margin-top: 20px;
            font-size: 1.2rem;
            font-weight: bold;
            color: #d6336c;
            min-height: 1.5em;
        }

        /* Success Section */
        #success-container {
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: fixed;
            inset: 0;
            background: linear-gradient(to bottom, #ffdde1, #ee9ca7);
            z-index: 1000;
            padding: 20px;
        }

        .heart {
            background-color: #ff4d6d;
            height: 80px;
            width: 80px;
            transform: rotate(-45deg);
            animation: heartbeat 0.8s infinite;
            margin-bottom: 40px;
        }

        .heart:before, .heart:after {
            content: "";
            background-color: #ff4d6d;
            border-radius: 50%;
            height: 80px;
            width: 80px;
            position: absolute;
        }
        .heart:before { top: -40px; left: 0; }
        .heart:after { left: 40px; top: 0; }

        @keyframes heartbeat {
            0% { transform: scale(1) rotate(-45deg); }
            50% { transform: scale(1.2) rotate(-45deg); }
            100% { transform: scale(1) rotate(-45deg); }
        }

        .final-text {
            font-size: 1.8rem;
            line-height: 1.6;
            color: #c9184a;
            white-space: pre-line; /* Keeps line breaks */
        }
    </style>
</head>
<body>

    <div id="main-content">
        <h1>Do you want to be my Valentine?</h1>
        <div class="buttons">
            <button id="yesBtn">YES</button>
            <button id="noBtn">NO</button>
        </div>
        <p id="feedback-msg"></p>
    </div>

    <div id="success-container">
        <div class="heart"></div>
        <div class="final-text" id="finalMsg"></div>
    </div>

    <audio id="bgMusic" loop>
        <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
    </audio>

    <script>
        const yesBtn = document.getElementById('yesBtn');
        const noBtn = document.getElementById('noBtn');
        const feedbackMsg = document.getElementById('feedback-msg');
        const successContainer = document.getElementById('success-container');
        const finalMsg = document.getElementById('finalMsg');
        const audio = document.getElementById('bgMusic');

        const funnyMessages = [
            "Are you sure? 🤔", "Try again! 😜", "Not that one! 🚫", 
            "Error 404: No not found ❌", "Nice try! 😂", "Wait, what? 🤨"
        ];

        function moveButton(e) {
            if(e) e.preventDefault();
            const x = Math.random() * (window.innerWidth - noBtn.offsetWidth);
            const y = Math.random() * (window.innerHeight - noBtn.offsetHeight);
            noBtn.style.left = `${x}px`;
            noBtn.style.top = `${y}px`;
            
            feedbackMsg.innerText = funnyMessages[Math.floor(Math.random() * funnyMessages.length)];
        }

        // iPhone & Android "No" evasion
        noBtn.addEventListener('touchstart', moveButton, { passive: false });
        noBtn.addEventListener('mouseover', moveButton);

        function handleYes() {
            successContainer.style.display = 'flex';
            // Your exact requested message
            finalMsg.innerText = "💝Clever got you 😄.\nThis is the best \"yes\" ever 💝💖\nYou are his valentine now 🥰🥰";
            audio.play().catch(() => console.log("Music ready after tap"));
        }

        yesBtn.addEventListener('click', handleYes);
        yesBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            handleYes();
        });
    </script>
</body>
</html>

