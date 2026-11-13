<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tengo algo muy importante que decirte...</title>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@700&family=Montserrat:wght@300;500;700&family=Great+Vibes&display=swap" rel="stylesheet">
    <!-- Script de confetti corregido -->
    <script src="https://unpkg.com/canvas-confetti@1.6.0/dist/confetti.browser.js"></script>
    
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
