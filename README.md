<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Creepster&family=Bangers&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #000;
            font-family: 'Bangers', cursive;
            overflow: hidden;
            cursor: none;
        }
        
        .message {
            font-size: 8rem;
            color: #fff;
            text-align: center;
            text-shadow: 0 0 20px #ff00ff, 0 0 40px #ff00ff;
            animation: glow 1s ease-in-out infinite alternate;
            opacity: 0;
        }
        
        @keyframes glow {
            from { text-shadow: 0 0 20px #ff00ff, 0 0 40px #ff00ff; }
            to { text-shadow: 0 0 30px #00ffff, 0 0 60px #00ffff; }
        }
        
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-30px); }
            60% { transform: translateY(-15px); }
        }
        
        .scare {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(1);
            width: 400px;
            height: 400px;
            background: url('https://i.imgur.com/0Z5qJ2L.jpg') center/contain no-repeat;
            opacity: 0;
            z-index: 1000;
            transition: all 0.1s;
        }
        
        .scare.active {
            transform: translate(-50%, -50%) scale(1.2);
            opacity: 1;
        }
        
        body.dark { background: #000; }
        body.bright { background: #fff; }
        
        @media (max-width: 768px) {
            .message { font-size: 4rem; }
            .scare { width: 300px; height: 300px; }
        }
    </style>
</head>
<body>
    <div class="message" id="message1">WELCOME</div>
    <div class="message" id="message2">GOT U TWICE</div>
    <div class="scare" id="scare"></div>
    <div style="position:fixed;bottom:20px;color:#fff;z-index:999;font-size:20px;">Click anywhere to play animation!</div>
    
    <script>
        const msg1 = document.getElementById('message1');
        const msg2 = document.getElementById('message2');
        const scare = document.getElementById('scare');
        const body = document.body;

        // Preload SCREAMS (public domain horror audio)
        const screams = [
            'https://www.soundjay.com/misc/sounds/ghost-1.mp3',
            'https://freesound.org/data/previews/153/153380_2673929-lq.mp3',
            'https://freesound.org/data/previews/245/245645_4101564-lq.mp3'
        ].map(url => {
            const audio = new Audio(url);
            audio.preload = 'auto';
            audio.volume = 1.0;
            return audio;
        });

        // Sequence timing
        setTimeout(() => {
            msg1.style.animation = 'glow 1s ease-in-out infinite alternate, bounce 1s ease-in-out';
            msg1.style.opacity = '1';
        }, 500);

        setTimeout(() => {
            msg1.style.opacity = '0';
            msg2.style.opacity = '1';
            msg2.style.animation = 'glow 0.8s ease-in-out infinite alternate, bounce 0.8s ease-in-out 0.2s';
        }, 3000);

        setTimeout(() => {
            msg2.style.opacity = '0';
            document.querySelector('div[style*="Click"]').style.display = 'none';
        }, 5500);

        // TRAP: Click/Space = Fullscreen + SCREAMS + SCARE
        const triggerScare = () => {
            document.documentElement.requestFullscreen().catch(() => {});
            scare.classList.add('active');
            
            // TRIPLE SCREAM ASSAULT (staggered)
            screams.forEach((audio, i) => {
                setTimeout(() => {
                    audio.currentTime = 0;
                    audio.play().catch(() => {});
                }, i * 80);
            });
            
            // Screen flicker HELL (faster)
            let flicker = 0;
            const flickerInt = setInterval(() => {
                flicker++;
                body.className = flicker % 2 === 0 ? 'dark' : 'bright';
                if (flicker > 250) clearInterval(flickerInt);
            }, 35);
            
            // Remove listener after trigger
            document.removeEventListener('click', triggerScare);
            document.removeEventListener('keydown', spaceTrigger);
        };

        const spaceTrigger = (e) => {
            if (e.key === ' ') {
                e.preventDefault();
                triggerScare();
            }
        };

        document.addEventListener('click', triggerScare);
        document.addEventListener('keydown', spaceTrigger);
    </script>
</body>
</html>
