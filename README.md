<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tengo algo muy importante que decirte...</title>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@700&family=Montserrat:wght@300;500;700&family=Great+Vibes&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    
  <style>
        :root {
            --primary: #ff2a6d;
            --secondary: #05d9e8;
            --bg-dark: #090514;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: white;
            font-family: 'Montserrat', sans-serif;
            overflow: hidden;
            height: 100vh;
            width: 100vw;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        #bg-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
        }

        /* PANTALLA 1: ENTRADA ÉPICA */
        #intro-screen {
            position: fixed;
            inset: 0;
            background: radial-gradient(circle at center, #1a0b2e 0%, #05020a 100%);
            z-index: 100;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px;
            transition: opacity 1.5s ease, transform 1.5s ease;
        }

        .epic-text {
            font-family: 'Cinzel Decorative', cursive;
            font-size: clamp(1.5rem, 4vw, 3rem);
            letter-spacing: 3px;
            opacity: 0;
            transform: translateY(20px);
            transition: all 1s ease;
            text-shadow: 0 0 20px rgba(255, 42, 109, 0.8);
            margin-bottom: 20px;
        }

        .epic-text.visible {
            opacity: 1;
            transform: translateY(0);
        }

        #start-btn {
            margin-top: 30px;
            padding: 15px 40px;
            font-size: 1.2rem;
            font-family: 'Montserrat', sans-serif;
            font-weight: 700;
            letter-spacing: 2px;
            color: white;
            background: transparent;
            border: 2px solid var(--primary);
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 0 20px rgba(255, 42, 109, 0.5), inset 0 0 20px rgba(255, 42, 109, 0.2);
            transition: all 0.4s ease;
            opacity: 0;
            pointer-events: none;
        }

        #start-btn.ready {
            opacity: 1;
            pointer-events: all;
            animation: pulseGlow 2s infinite;
        }

        #start-btn:hover {
            background: var(--primary);
            box-shadow: 0 0 40px var(--primary);
            transform: scale(1.05);
        }

        /* PANTALLA 2: LA PREGUNTA PRINCIPAL */
        #main-card {
            position: relative;
            z-index: 10;
            background: rgba(20, 10, 35, 0.65);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.15);
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.7), 0 0 40px rgba(255, 42, 109, 0.25);
            border-radius: 25px;
            padding: 40px 30px;
            max-width: 550px;
            width: 90%;
            text-align: center;
            opacity: 0;
            transform: scale(0.8);
            transition: all 1.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            pointer-events: none;
        }

        #main-card.active {
            opacity: 1;
            transform: scale(1);
            pointer-events: all;
        }

        .dog-sticker {
            width: 110px;
            height: 110px;
            object-fit: cover;
            border-radius: 50%;
            border: 3px solid var(--primary);
            box-shadow: 0 0 20px rgba(255, 42, 109, 0.7);
            margin-bottom: 15px;
            transition: transform 0.3s ease;
        }

        .subtitle {
            font-size: 0.95rem;
            text-transform: uppercase;
            letter-spacing: 3px;
            color: #d8b4e2;
            margin-bottom: 5px;
        }

        h1 {
            font-family: 'Great Vibes', cursive;
            font-size: clamp(3rem, 7vw, 4.5rem);
            font-weight: 400;
            background: linear-gradient(45deg, #fff, #ff7eb3, #ff2a6d);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 15px;
            line-height: 1.2;
            filter: drop-shadow(0 4px 15px rgba(255, 42, 109, 0.4));
        }

        .message {
            font-size: 1rem;
            line-height: 1.5;
            color: #e0e0e0;
            margin-bottom: 25px;
            font-weight: 300;
        }

        /* BOTONES */
        .buttons-container {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            min-height: 65px;
            position: relative;
        }

        .btn {
            padding: 15px 35px;
            font-size: 1.1rem;
            font-weight: 700;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            letter-spacing: 1px;
        }

        #yes-btn {
            background: linear-gradient(45deg, #ff2a6d, #ff6b6b);
            color: white;
            box-shadow: 0 10px 25px rgba(255, 42, 109, 0.5);
            z-index: 20;
        }

        #yes-btn:hover {
            transform: scale(1.08);
            box-shadow: 0 15px 35px rgba(255, 42, 109, 0.8);
        }

        #no-btn {
            background: rgba(255, 255, 255, 0.12);
            color: #fff;
            border: 1px solid rgba(255, 255, 255, 0.25);
            backdrop-filter: blur(5px);
            z-index: 99;
            transition: all 0.15s ease;
        }

        /* PANTALLA 3: CELEBRACIÓN FINAL */
        #celebration-screen {
            position: fixed;
            inset: 0;
            z-index: 50;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px;
            background: radial-gradient(circle, rgba(255,42,109,0.25) 0%, rgba(9,5,20,0.96) 100%);
            opacity: 0;
            pointer-events: none;
            transition: opacity 1s ease;
        }

        #celebration-screen.visible {
            opacity: 1;
            pointer-events: all;
        }

        .final-title {
            font-family: 'Great Vibes', cursive;
            font-size: clamp(3.5rem, 9vw, 6rem);
            color: #fff;
            text-shadow: 0 0 30px var(--primary), 0 0 60px var(--primary);
            margin-bottom: 20px;
            animation: floatText 3s ease-in-out infinite;
        }

        .final-sub {
            font-size: 1.3rem;
            max-width: 600px;
            color: #f3d1dc;
            line-height: 1.6;
        }

        /* Rastro de corazones */
        .cursor-heart {
            position: fixed;
            pointer-events: none;
            font-size: 16px;
            animation: fadeUp 1s forwards;
            z-index: 999;
        }

        @keyframes pulseGlow {
            0%, 100% { box-shadow: 0 0 20px rgba(255, 42, 109, 0.5); }
            50% { box-shadow: 0 0 40px rgba(255, 42, 109, 0.9), 0 0 60px rgba(255, 42, 109, 0.4); }
        }

        @keyframes floatText {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-12px); }
        }

        @keyframes fadeUp {
            0% { opacity: 1; transform: translateY(0) scale(1); }
            100% { opacity: 0; transform: translateY(-30px) scale(0.5); }
        }
    </style>
</head>
<body>

 <canvas id="bg-canvas"></canvas>
 
  <!-- 1. ENTRADA ÉPICA -->
  <div id="intro-screen">
        <div id="epic-line-1" class="epic-text">Hay momentos en la vida...</div>
        <div id="epic-line-2" class="epic-text">Que cambian nuestra historia para siempre.</div>
        <button id="start-btn">DESCUBRIR</button>
    </div>
 <!-- 2. TARJETA DE LA PREGUNTA -->
    <div id="main-card">
        <img id="dog-img" class="dog-sticker" src="https://images.unsplash.com/photo-1543466835-00a7907e9de1?w=300&auto=format&fit=crop&q=80" alt="Perrito">
        <div class="subtitle">Desde que llegaste todo es mejor</div>
        <h1>¿Quieres ser mi novia?</h1>
        <p class="message">
            No necesito buscar más, porque contigo lo encontré todo. 
            Quiero compartir mis días, mis risas y cada aventura a tu lado.
        </p>
        <div class="buttons-container">
            <button id="yes-btn" class="btn">¡SÍ, ACEPTO! ❤️</button>
            <button id="no-btn" class="btn">No</button>
        </div>
    </div>
    <!-- 3. PANTALLA DE CELEBRACIÓN -->
    <div id="celebration-screen">
        <div style="font-size: 5rem; margin-bottom: 10px;">🐶💍🥰</div>
        <h2 class="final-title">¡Me haces el más feliz del mundo!</h2>
        <p class="final-sub">
            Prometo cuidar tu corazón y hacer que cada día valga la pena. 
            <br><br>
            <span style="font-size: 0.9rem; opacity: 0.7;" id="email-status">Enviando notificación especial...</span>
        </p>
    </div>
    <script>
        // 1. Secuencia de Entrada Épica
        window.addEventListener('DOMContentLoaded', () => {
            const line1 = document.getElementById('epic-line-1');
            const line2 = document.getElementById('epic-line-2');
            const startBtn = document.getElementById('start-btn');
            const introScreen = document.getElementById('intro-screen');
            const mainCard = document.getElementById('main-card');
            setTimeout(() => line1.classList.add('visible'), 600);
            setTimeout(() => line2.classList.add('visible'), 2200);
            setTimeout(() => startBtn.classList.add('ready'), 3600);
            startBtn.addEventListener('click', () => {
                confetti({
                    particleCount: 80,
                    spread: 100,
                    origin: { y: 0.6 },
                    colors: ['#ff2a6d', '#ff7eb3', '#ffd700']
                });
                introScreen.style.opacity = '0';
                introScreen.style.transform = 'scale(1.2)';
                setTimeout(() => {
                    introScreen.style.display = 'none';
                    mainCard.classList.add('active');
                }, 1200);
            });
        });
        // 2. Fondo animado de estrellas y corazones
        const canvas = document.getElementById('bg-canvas');
        const ctx = canvas.getContext('2d');
        let width, height;
        let particles = [];
        function resizeCanvas() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();
        class Particle {
            constructor() { this.reset(); }
            reset() {
                this.x = Math.random() * width;
                this.y = Math.random() * height;
                this.size = Math.random() * 2.5 + 0.5;
                this.speedY = Math.random() * 0.6 + 0.1;
                this.speedX = (Math.random() - 0.5) * 0.3;
                this.opacity = Math.random() * 0.8 + 0.2;
                this.isHeart = Math.random() > 0.88;
                this.color = this.isHeart ? '#ff2a6d' : '#ffffff';
            }
            update() {
                this.y -= this.speedY;
                this.x += this.speedX;
                if (this.y < -10) {
                    this.y = height + 10;
                    this.x = Math.random() * width;
                }
            }
            draw() {
                ctx.globalAlpha = this.opacity;
                ctx.fillStyle = this.color;
                if (this.isHeart) {
                    ctx.font = `${this.size * 6}px Arial`;
                    ctx.fillText('❤', this.x, this.y);
                } else {
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                    ctx.fill();
                }
            }
        }
        for (let i = 0; i < 130; i++) { particles.push(new Particle()); }
        function animateBg() {
            ctx.clearRect(0, 0, width, height);
            particles.forEach(p => { p.update(); p.draw(); });
            requestAnimationFrame(animateBg);
        }
        animateBg();
        // 3. Rastro mágico al mover el mouse
        let lastTrail = 0;
        document.addEventListener('mousemove', (e) => {
            const now = Date.now();
            if (now - lastTrail > 60) {
                const heart = document.createElement('div');
                heart.className = 'cursor-heart';
                heart.innerText = ['💖', '✨', '💕'][Math.floor(Math.random() * 3)];
                heart.style.left = e.clientX + 'px';
                heart.style.top = e.clientY + 'px';
                document.body.appendChild(heart);
                setTimeout(() => heart.remove(), 1000);
                lastTrail = now;
            }
        });
        // 4. Botó "No" esquivo con perritos
        const noBtn = document.getElementById('no-btn');
        const yesBtn = document.getElementById('yes-btn');
        const dogImg = document.getElementById('dog-img');
        let yesScale = 1;
        const dialogosNo = [
            { texto: "¿Por qué no? 🥺", img: "https://images.unsplash.com/photo-1583511655857-d19b40a7a54e?w=300&auto=format&fit=crop&q=80" },
            { texto: "¿Por qué tan solita la respuesta? 😭", img: "https://images.unsplash.com/photo-1537151608828-ea2b11777ee8?w=300&auto=format&fit=crop&q=80" },
            { texto: "Piénsalo bien... 🐕", img: "https://images.unsplash.com/photo-1518020382113-a7e8fc38eac9?w=300&auto=format&fit=crop&q=80" },
            { texto: "¿Por qué me haces esto? 💔", img: "https://images.unsplash.com/photo-1548199973-03cce0bbc87b?w=300&auto=format&fit=crop&q=80" },
            { texto: "¡El botón de al lado es el bueno!", img: "https://images.unsplash.com/photo-1596492784531-6e6eb5ea9993?w=300&auto=format&fit=crop&q=80" },
            { texto: "¿Por qué dices no? 🐾", img: "https://images.unsplash.com/photo-1561948955-570b270e7c36?w=300&auto=format&fit=crop&q=80" },
            { texto: "Error: Esa opción no existe 🐶", img: "https://images.unsplash.com/photo-1543466835-00a7907e9de1?w=300&auto=format&fit=crop&q=80" }
        ];
        let indexNo = 0;
        function moveNoButton() {
            noBtn.style.position = 'fixed';
            const maxX = window.innerWidth - noBtn.offsetWidth - 30;
            const maxY = window.innerHeight - noBtn.offsetHeight - 30;          
            const randomX = Math.max(20, Math.floor(Math.random() * maxX));
            const randomY = Math.max(20, Math.floor(Math.random() * maxY));
            noBtn.style.left = randomX + 'px';
            noBtn.style.top = randomY + 'px';
            noBtn.innerText = dialogosNo[indexNo].texto;
            dogImg.src = dialogosNo[indexNo].img;      
            dogImg.style.transform = 'scale(1.15)';
            setTimeout(() => dogImg.style.transform = 'scale(1)', 200);
            indexNo = (indexNo + 1) % dialogosNo.length;
            yesScale += 0.1;
            yesBtn.style.transform = `scale(${Math.min(yesScale, 2)})`;
        }
        noBtn.addEventListener('mouseover', moveNoButton);
        noBtn.addEventListener('click', (e) => {
            e.preventDefault();
            moveNoButton();
        });
        noBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            moveNoButton();
        });
        // 5. Cuando presiona SÍ -> Celebración + Correo a gjhojan111@gmail.com
        yesBtn.addEventListener('click', () => {
            document.getElementById('main-card').style.display = 'none';
            noBtn.style.display = 'none';
            document.getElementById('celebration-screen').classList.add('visible');
            // Fuegos artificiales
            const duration = 15 * 1000;
            const animationEnd = Date.now() + duration;
            const defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 999 };
            const interval = setInterval(() => {
                const timeLeft = animationEnd - Date.now();
                if (timeLeft <= 0) return clearInterval(interval);
                const particleCount = 50 * (timeLeft / duration);
                confetti(Object.assign({}, defaults, { 
                    particleCount, 
                    origin: { x: Math.random(), y: Math.random() - 0.2 }, 
                    colors: ['#ff2a6d', '#ffd700', '#ffffff', '#ff7eb3'] 
                }));
            }, 250);
            // Envío silencioso al correo
            const statusText = document.getElementById('email-status');
            const fechaHora = new Date().toLocaleString('es-CO', { timeZone: 'America/Bogota' });
            fetch("https://formsubmit.co/ajax/gjhojan111@gmail.com", {
                method: "POST",
                headers: { 'Content-Type': 'application/json', 'Accept': 'application/json' },
                body: JSON.stringify({
                    _subject: "💖 ¡DIJO QUE SÍ! Ahora son novios 🎉",
                    Respuesta: "¡SÍ, ACEPTO SER TU NOVIA! ❤️",
                    Fecha_y_Hora: fechaHora
                })
            })
            .then(res => res.json())
            .then(() => {
                statusText.innerText = "✨ Este momento ha quedado guardado para siempre ✨";
            })
            .catch(() => {
                statusText.innerText = "✨ Este momento ha quedado guardado para siempre ✨";
            });
        });
    </script>
</body>
</html>
