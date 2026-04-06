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
            background: url('data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAkGBwgHBgkIBwgKCgkLDRYPDQwMDRsUFRAWIB0iIiAdHx8kKDQsJCYxJx8fLT0tMTU3Ojo6Iys/RD84QzQ5OjcBCgoKDQwNGg8PGjclHyU3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3Nzc3N//AABEIAJQAlAMBIgACEQEDEQH/xAAcAAEAAQUBAQAAAAAAAAAAAAAABQECAwQGBwj/xAArEAEAAgEDAgUEAQUAAAAAAAAAAQIDBAURBhITITFBYQciMlFxFBUjQmL/xAAVAQEBAAAAAAAAAAAAAAAAAAAAAf/EABQRAQAAAAAAAAAAAAAAAAAAAAD/2gAMAwEAAhEDEQA/APDQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABWscyzRi5gGAX5KdqwAAAAAAAAAAAAAAAEv0307uPUm4Rotrwzkycc2n0isfIImJ4lIaakXr3eyT6t6L3fpO+ON0wxFMn43rPMShdNl7YmvILtbxHlDTZsndlyxWsczM8RDva/SLqCdj/ALnbw6/44yeD/txxyDzwXZKWx3tS8cWrMxMfK0AAAAAAAAAAAAB6x9BN92/adx12LX3rjtnrXsvb4eTsnZkp90RMfMA9n+vPVG2bjodHtuhzVzZa3m9rV9o8nisTMT5L5jJbm1uZ/cyxgz6PNGDV4c0xz4eSt+P4nl9H5fqnsN+mJzRmiM1sPb4XvE8PmlkpiveYitZmZBfrc39TrM+eI48TJa/H65nlgStdnzU03jZY4+EZkjtvMAtAAAAAAAAAAABWPVJ6bJjvj7bRHKLViZifKQdPXbo1ejmuKPv49vdqbZ01rdZmtWcVoiPdTZd6jSZK1zxPbE/lDt9t6r2jFita2alb8eiI4HU7HrNLq5w5ccxESm9s23smJvXiI95htbn1Ht+XUWy905J9u2EFrd61O4X8DSUmlJ/XrwCd3PXYL6S+PF2zFY45hw+SebzMe8pvW9uh2+uCJ5yT+U8+6CVQAAAAAAAAAAABWPVQBs4sEZ4jt/JtU2PcMkc4cFrx/wAtHT5ZxZInl6L0puFZrWJ84ByWm6V3bNk7bae1Ij1mXRRseHZNDOS8RbNMedpdzn12LDg75iIcF1bvlM+O1KSiOP3DUTmzzMzz5tRWZ5nmVFUAAAAAAAAAAAAAASmzbpbQ5om0/aiwHa7l1Lizaftpbz49HJarUWz2mZ9GuAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA//2Q==') center/contain no-repeat;
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
    <div class="message" id="message1">THANKS FOR COMING</div>
    <div class="message" id="message2">READY</div>
    <div class="scare" id="scare"></div>
    <div style="position:fixed;bottom:20px;color:#fff;z-index:999;font-size:20px;">Click anywhere to play animation!</div>
    
    <script>
        const msg1 = document.getElementById('message1');
        const msg2 = document.getElementById('message2');
        const scare = document.getElementById('scare');
        const body = document.body;

        // Preload SCREAMS (public domain horror audio)
        const screams = [
            'https://www.myinstants.com/media/sounds/scream.mp3',
            'https://www.myinstants.com/media/sounds/scream.mp3',
            'https://www.myinstants.com/media/sounds/scream.mp3'
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
            msg2.style.animation = 'glow 0.6s ease-in-out infinite alternate, bounce 0.5s ease-in-out 0.2s';
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
