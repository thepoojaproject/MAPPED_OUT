
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minimal Compass</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, sans-serif;
            background: #121212;
            color: white;
            height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            text-align: center;
            overflow: hidden;
        }
        
        .compass {
            width: 240px;
            height: 240px;
            border-radius: 50%;
            background: #1e1e1e;
            position: relative;
            margin: 30px 0;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }
        
        .needle {
            width: 2px;
            height: 100px;
            background: #ff3b30;
            position: absolute;
            top: 50%;
            left: 50%;
            transform-origin: 50% 100%;
            transform: translateX(-50%) translateY(-100%);
            transition: transform 0.1s ease-out;
        }
        
        .center {
            width: 12px;
            height: 12px;
            background: #1e1e1e;
            border: 2px solid #ff3b30;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        
        .direction {
            font-size: 24px;
            font-weight: 600;
            margin-bottom: 10px;
        }
        
        .degrees {
            font-size: 18px;
            color: #aaa;
            margin-bottom: 20px;
        }
        
        .lock-btn {
            background: #333;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            transition: background 0.2s;
        }
        
        .lock-btn:hover {
            background: #444;
        }
        
        .lock-btn.active {
            background: #ff3b30;
        }
        
        .footer {
            position: absolute;
            bottom: 20px;
            color: #666;
            font-size: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .heart {
            display: inline-block;
            margin: 0 5px;
            color: #ff3b30;
            animation: pulse 1.5s ease-in-out infinite;
        }
        
        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.2);
            }
            100% {
                transform: scale(1);
            }
        }
    </style>
</head>
<body>
    <div class="direction" id="direction">N</div>
    <div class="degrees" id="degrees">0°</div>
    
    <div class="compass">
        <div class="needle" id="needle"></div>
        <div class="center"></div>
    </div>
    
    <button class="lock-btn" id="lockBtn">Lock</button>
    
    <div class="footer">Made with <span class="heart">💖</span> By Armeen</div>

    <script>
        const needle = document.getElementById('needle');
        const directionDisplay = document.getElementById('direction');
        const degreesDisplay = document.getElementById('degrees');
        const lockBtn = document.getElementById('lockBtn');
        
        let isLocked = false;
        let currentHeading = 0;
        
        function updateCompass(heading) {
            if (isLocked) return;
            
            const rotation = 360 - heading;
            needle.style.transform = `translateX(-50%) translateY(-100%) rotate(${rotation}deg)`;
            degreesDisplay.textContent = `${Math.round(heading)}°`;
            
            // Update direction
            const directions = ['N', 'NE', 'E', 'SE', 'S', 'SW', 'W', 'NW'];
            const index = Math.round(heading / 45) % 8;
            directionDisplay.textContent = directions[index];
        }
        
        function toggleLock() {
            isLocked = !isLocked;
            lockBtn.textContent = isLocked ? 'Unlock' : 'Lock';
            lockBtn.classList.toggle('active', isLocked);
        }
        
        // Device orientation handling
        if (window.DeviceOrientationEvent) {
            window.addEventListener('deviceorientation', (event) => {
                if (event.alpha !== null) {
                    currentHeading = event.alpha;
                    updateCompass(currentHeading);
                }
            });
        } else {
            directionDisplay.textContent = 'N/A';
            degreesDisplay.textContent = 'Not supported';
        }
        
        lockBtn.addEventListener('click', toggleLock);
    </script>
</body>
</html>	
