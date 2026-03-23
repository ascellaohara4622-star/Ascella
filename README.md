<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>For My Ko Ko — A Love Letter Across Pages</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400;500;600;700&family=Quicksand:wght@300;400;500;600;700&family=Playfair+Display:ital@0;1&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-dark: #050a18;
            --hydrangea-blue: #a2c2ff;
            --hydrangea-pink: #f8bbd0;
            --sakura-pink: #ffb7c5;
            --star-white: #ffffff;
            --text-main: #e6f1ff;
            --gold: #ffd700;
            --stamp-red: #e34234;
            --vinyl-purple: #8b5cf6;
            --deep-lavender: #9b87f5;
            --sunset-orange: #ff9f4a;
            --sunset-purple: #c96efd;
            --recipe-cream: #fdf8ed;
            --recipe-brown: #4a3b2a;
            --recipe-gold: #F7C873;
            --recipe-pink: #FFD9E6;
            --recipe-lavender: #C5B9E6;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background-color: var(--bg-dark);
            color: var(--text-main);
            font-family: 'Quicksand', sans-serif;
            overflow-x: hidden;
            scroll-behavior: smooth;
        }

        canvas {
            position: fixed;
            top: 0;
            left: 0;
            z-index: 1;
            pointer-events: none;
        }

        section {
            position: relative;
            z-index: 5;
            min-height: 100vh;
        }

        #page-lock {
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            z-index: 10;
            transition: opacity 1.5s ease;
        }

        .constellation {
            font-family: 'Dancing Script', cursive;
            font-size: 3rem;
            color: var(--hydrangea-blue);
            text-shadow: 0 0 15px rgba(162, 194, 255, 0.6);
            margin-bottom: 20px;
            opacity: 0;
            animation: glowIn 4s forwards;
        }

        @media (max-width: 600px) {
            .constellation { font-size: 2rem; }
        }

        .passcode-container {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            padding: 30px 20px;
            border-radius: 30px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        input {
            background: transparent;
            border: none;
            border-bottom: 2px solid var(--hydrangea-blue);
            color: white;
            font-size: 1.5rem;
            text-align: center;
            width: 180px;
            outline: none;
            letter-spacing: 5px;
            margin: 20px 0;
        }

        #page-timeline {
            display: none;
            padding: 60px 15px;
        }

        .welcome-msg { text-align: center; max-width: 600px; margin: 0 auto 50px; line-height: 1.8; }

        .timeline-container {
            position: relative;
            max-width: 800px;
            margin: 0 auto;
        }

        .timeline-line {
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            width: 2px;
            height: 100%;
            background: linear-gradient(to bottom, transparent, var(--hydrangea-blue), var(--hydrangea-pink), transparent);
        }

        .star-point {
            position: absolute;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 14px;
            height: 14px;
            background: white;
            border-radius: 50%;
            box-shadow: 0 0 10px white;
            cursor: pointer;
            z-index: 10;
            transition: 0.3s;
        }

        .star-point.glow-bright {
            background: var(--gold);
            box-shadow: 0 0 25px var(--gold), 0 0 35px rgba(255, 215, 0, 0.8);
            width: 24px;
            height: 24px;
        }

        .star-point:hover { transform: translate(-50%, -50%) scale(1.3); }

        .memory-box {
            position: absolute;
            width: 42%;
            background: rgba(255, 255, 255, 0.03);
            padding: 15px;
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            opacity: 0;
            transition: 0.5s;
            pointer-events: none;
            font-size: 0.9rem;
        }

        .memory-box.active { opacity: 1; pointer-events: auto; }

        .left { right: calc(50% + 20px); text-align: right; }
        .right { left: calc(50% + 20px); text-align: left; }
        .center-spec { 
            width: 80%;
            text-align: center; 
            left: 10%;
            right: 10%;
            background: rgba(255, 215, 0, 0.15);
            border-color: var(--gold);
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
            backdrop-filter: blur(4px);
        }

        .date-label { color: var(--hydrangea-blue); font-weight: bold; margin-bottom: 5px; font-size: 0.8rem; }

        #page-birthday {
            display: none;
            padding: 60px 20px;
            text-align: center;
            max-width: 700px;
            margin: 0 auto;
        }

        .bday-title { font-family: 'Dancing Script', cursive; font-size: 2.5rem; color: var(--hydrangea-pink); margin-bottom: 10px; opacity: 0; transform: translateY(30px); animation: fadeInUpLine 0.8s ease forwards; }
        .bday-content p { margin-bottom: 20px; line-height: 1.7; font-size: 1rem; opacity: 0; transform: translateY(20px); transition: 1s; }

        #page-4 {
            display: none;
            padding: 60px 20px;
        }

        .wishes-header { text-align: center; padding: 20px 20px 30px; }
        .wishes-script-title { font-family: 'Dancing Script', cursive; font-size: 2.2rem; color: var(--gold); text-shadow: 0 0 10px rgba(255, 215, 0, 0.5); }
        .wishes-intro { font-style: italic; opacity: 0.8; margin-top: 10px; font-size: 0.9rem; max-width: 500px; margin-left: auto; margin-right: auto; }
        
        .wishes-container { max-width: 600px; margin: 0 auto; padding-bottom: 40px; }
        
        .wish-item {
            margin: 30px 0;
            padding: 15px;
            font-size: 0.95rem;
            line-height: 1.5;
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s ease;
            border-left: 2px solid rgba(255, 215, 0, 0.3);
            background: rgba(255, 255, 255, 0.02);
            border-radius: 0 15px 15px 0;
        }

        .wish-item.reveal { opacity: 1; transform: translateY(0); }
        .wish-num { font-weight: bold; color: var(--gold); margin-right: 8px; font-size: 0.9rem; }
        .wish-icon { margin-right: 8px; filter: drop-shadow(0 0 3px gold); }

        #page-5 {
            display: none;
            padding: 60px 20px;
        }

        .reasons-header { text-align: center; padding: 20px 20px 30px; }
        .reasons-title { font-family: 'Dancing Script', cursive; font-size: 2.2rem; color: var(--hydrangea-blue); text-shadow: 0 0 10px rgba(162, 194, 255, 0.5); }
        
        .reasons-container {
            position: relative;
            max-width: 650px;
            margin: 0 auto;
            padding: 20px 0 60px;
        }
        
        .constellation-line {
            position: absolute;
            left: 25px;
            top: 0;
            width: 2px;
            height: 0%;
            background: linear-gradient(to bottom, var(--gold), var(--hydrangea-pink));
            transition: height 0.5s ease;
            z-index: 1;
        }
        
        .reason-item {
            position: relative;
            margin: 20px 0 20px 50px;
            padding: 12px 15px;
            opacity: 0;
            transform: translateX(-20px);
            transition: all 0.6s ease;
            border-left: 2px solid rgba(255, 215, 0, 0.3);
            font-size: 0.95rem;
        }
        
        .reason-star {
            position: absolute;
            left: -38px;
            top: 15px;
            width: 10px;
            height: 10px;
            background: var(--gold);
            border-radius: 50%;
            box-shadow: 0 0 8px var(--gold);
            opacity: 0;
            transition: opacity 0.5s;
        }
        
        .reason-item.revealed {
            opacity: 1;
            transform: translateX(0);
        }
        
        .reason-item.revealed .reason-star { opacity: 1; }

        #page-always {
            display: none;
            min-height: 100vh;
            padding: 60px 20px;
            text-align: center;
            background: radial-gradient(circle at center, #0a0f2a, #030617);
        }
        
        .always-container {
            max-width: 550px;
            margin: 0 auto;
            padding: 40px 20px;
        }
        
        .always-subtitle {
            font-size: 0.9rem;
            letter-spacing: 1px;
            opacity: 0.6;
            margin-bottom: 30px;
        }
        
        .always-star-btn {
            width: 100px;
            height: 100px;
            background: radial-gradient(circle, var(--gold), #ffaa00);
            border-radius: 50%;
            margin: 40px auto;
            cursor: pointer;
            box-shadow: 0 0 30px rgba(255, 215, 0, 0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 20px rgba(255, 215, 0, 0.5); }
            50% { transform: scale(1.05); box-shadow: 0 0 40px rgba(255, 215, 0, 0.8); }
            100% { transform: scale(1); box-shadow: 0 0 20px rgba(255, 215, 0, 0.5); }
        }
        
        .always-message {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            padding: 25px;
            border-radius: 20px;
            border: 1px solid rgba(255, 215, 0, 0.3);
            margin: 25px 0;
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.6s ease;
            line-height: 1.7;
            font-size: 0.95rem;
        }
        
        .always-message.show {
            opacity: 1;
            transform: translateY(0);
        }
        
        .always-footer {
            margin-top: 40px;
            font-size: 0.85rem;
            opacity: 0.7;
        }

        /* Red String Page Styles */
        #page-redstring {
            background: radial-gradient(ellipse at center, #1a1410 0%, #0a0806 100%);
            min-height: 100vh;
            padding: 60px 20px;
            position: relative;
            overflow-x: hidden;
        }

        .redstring-container {
            max-width: 800px;
            margin: 0 auto;
            text-align: center;
        }

        .redstring-header {
            margin-bottom: 40px;
        }

        .redstring-title {
            font-family: 'Dancing Script', cursive;
            font-size: 2.5rem;
            color: #ff6666;
            text-shadow: 0 0 20px rgba(255, 102, 102, 0.5);
            margin-bottom: 15px;
        }

        .redstring-intro {
            font-size: 0.9rem;
            font-style: italic;
            opacity: 0.8;
            max-width: 500px;
            margin: 0 auto;
            line-height: 1.6;
        }

        .redstring-subtitle {
            font-size: 0.9rem;
            color: #ff9999;
            margin-top: 20px;
            font-weight: 500;
        }

        .shield-container {
            position: relative;
            width: 100%;
            max-width: 450px;
            margin: 40px auto;
            min-height: 550px;
        }

        .shield-svg {
            width: 100%;
            height: auto;
            filter: drop-shadow(0 0 15px rgba(255, 68, 68, 0.3));
        }

        .knots-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .knot-point {
            position: absolute;
            width: 24px;
            height: 24px;
            background: radial-gradient(circle, #ff6666, #cc3333);
            border-radius: 50%;
            cursor: pointer;
            transform: translate(-50%, -50%);
            transition: all 0.3s ease;
            box-shadow: 0 0 10px rgba(255, 102, 102, 0.8);
            animation: knotPulse 2s infinite;
            z-index: 10;
        }

        .knot-point:hover {
            transform: translate(-50%, -50%) scale(1.2);
            box-shadow: 0 0 20px rgba(255, 102, 102, 1);
        }

        .knot-point.revealed {
            background: radial-gradient(circle, #ffaa66, #ff6666);
            box-shadow: 0 0 20px rgba(255, 170, 102, 0.8);
        }

        .knot-point.special {
            background: radial-gradient(circle, #ffaa66, #ff8844);
            animation: specialKnotPulse 1s infinite;
        }

        @keyframes knotPulse {
            0%, 100% {
                transform: translate(-50%, -50%) scale(1);
                opacity: 0.9;
            }
            50% {
                transform: translate(-50%, -50%) scale(1.1);
                opacity: 1;
            }
        }

        @keyframes specialKnotPulse {
            0%, 100% {
                transform: translate(-50%, -50%) scale(1);
                box-shadow: 0 0 10px #ffaa66;
            }
            50% {
                transform: translate(-50%, -50%) scale(1.2);
                box-shadow: 0 0 25px #ffaa66;
            }
        }

        /* Memory Card Popup */
        .memory-card {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.9);
            width: 320px;
            max-width: 85vw;
            background: linear-gradient(135deg, #fff9ef, #fff4e6);
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3), 0 0 0 1px rgba(255,102,102,0.3);
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .memory-card.show {
            opacity: 1;
            visibility: visible;
            transform: translate(-50%, -50%) scale(1);
        }

        .memory-card-icon {
            font-size: 2.5rem;
            text-align: center;
            margin-bottom: 15px;
        }

        .memory-card-title {
            font-family: 'Dancing Script', cursive;
            font-size: 1.5rem;
            color: #cc5555;
            text-align: center;
            margin-bottom: 15px;
            border-bottom: 2px solid #ffcccc;
            padding-bottom: 8px;
        }

        .memory-card-line {
            font-size: 1rem;
            color: #aa6f4e;
            font-style: italic;
            text-align: center;
            margin-bottom: 15px;
            font-weight: 500;
        }

        .memory-card-truth {
            font-size: 0.85rem;
            color: #5a3e2a;
            line-height: 1.6;
            text-align: center;
        }

        .memory-card-close {
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 1.2rem;
            color: #cc5555;
            cursor: pointer;
        }

        /* Thread animation */
        @keyframes threadGlow {
            0%, 100% {
                stroke-opacity: 0.6;
                stroke-width: 2;
            }
            50% {
                stroke-opacity: 1;
                stroke-width: 3;
            }
        }

        .thread-segment {
            animation: threadGlow 3s infinite;
        }

        .thread-segment.active {
            animation: threadGlow 0.5s infinite;
        }

        /* Ending Message */
        .shield-complete-message {
            background: rgba(255, 240, 220, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 40px;
            margin: 30px 0;
            border: 2px solid #ff8888;
            animation: fadeInUp 0.6s ease;
        }

        .shield-complete-message p {
            margin: 15px 0;
            line-height: 1.8;
            color: #5a3e2a;
        }

        .final-promise {
            font-size: 1.2rem;
            font-weight: bold;
            color: #cc5555;
            margin-top: 25px !important;
        }

        .heart-red {
            font-size: 2rem;
            margin-top: 20px;
            animation: heartBeat 1.5s infinite;
        }

        .redstring-footer {
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 102, 102, 0.3);
            font-style: italic;
            font-size: 0.9rem;
            opacity: 0.8;
        }

        #page-countdown {
            display: none;
            padding: 60px 20px;
        }

        .countdown-header { text-align: center; padding: 20px 20px 30px; }
        .countdown-title { font-family: 'Dancing Script', cursive; font-size: 2.2rem; color: var(--gold); text-shadow: 0 0 10px rgba(255, 215, 0, 0.5); }
        .countdown-intro { font-style: italic; opacity: 0.8; margin-top: 10px; font-size: 0.9rem; }

        .flight-path-container {
            position: relative;
            max-width: 650px;
            margin: 0 auto;
            padding: 20px 0 60px;
        }

        .dotted-path {
            position: absolute;
            left: 35px;
            top: 0;
            width: 2px;
            height: 0%;
            background: repeating-linear-gradient(to bottom, rgba(255, 255, 255, 0.6), rgba(255, 255, 255, 0.6) 8px, transparent 8px, transparent 16px);
            transition: height 0.5s ease;
            z-index: 1;
        }

        .milestone-item {
            position: relative;
            margin: 25px 0 25px 70px;
            padding: 15px 20px;
            opacity: 0;
            transform: translateX(-25px);
            transition: all 0.6s ease;
            border-left: 2px solid rgba(255, 215, 0, 0.4);
            background: rgba(255, 255, 255, 0.02);
            border-radius: 0 15px 15px 0;
            font-size: 0.9rem;
        }

        .milestone-item.revealed {
            opacity: 1;
            transform: translateX(0);
        }

        .flight-dot {
            position: absolute;
            left: -48px;
            top: 20px;
            width: 12px;
            height: 12px;
            background: var(--gold);
            border-radius: 50%;
            box-shadow: 0 0 10px var(--gold);
            opacity: 0;
            transition: opacity 0.5s ease;
        }

        .milestone-item.revealed .flight-dot {
            opacity: 1;
            animation: landingGlow 0.5s ease;
        }

        @keyframes landingGlow {
            0% { transform: scale(0); }
            50% { transform: scale(1.3); }
            100% { transform: scale(1); }
        }

        .milestone-number {
            display: inline-block;
            font-weight: bold;
            color: var(--gold);
            background: rgba(255, 215, 0, 0.15);
            padding: 2px 8px;
            border-radius: 20px;
            margin-right: 10px;
            font-size: 0.8rem;
        }

        .milestone-icon { font-size: 1.2rem; margin-right: 8px; display: inline-block; filter: drop-shadow(0 0 3px gold); }
        .milestone-text { font-size: 0.9rem; display: inline; }
        .milestone-detail { margin-top: 6px; font-size: 0.8rem; opacity: 0.7; padding-left: 30px; }

        .phase-header {
            position: relative;
            margin: 30px 0 15px 30px;
            padding: 8px 15px;
            background: rgba(162, 194, 255, 0.1);
            border-radius: 25px;
            display: inline-block;
            font-family: 'Dancing Script', cursive;
            font-size: 1.1rem;
            color: var(--hydrangea-blue);
        }

        .final-plane {
            text-align: center;
            margin: 40px 0 20px;
            opacity: 0;
            transition: all 0.6s ease;
        }

        .final-plane.show { opacity: 1; }

        .countdown-ending {
            text-align: center;
            margin-top: 40px;
            padding: 25px;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 25px;
            font-size: 0.9rem;
        }

        #page-tokens {
            display: none;
            padding: 60px 20px;
        }

        .tokens-header { text-align: center; padding: 20px 20px 30px; }
        .tokens-title { font-family: 'Dancing Script', cursive; font-size: 2.2rem; color: var(--gold); text-shadow: 0 0 10px rgba(255, 215, 0, 0.5); }
        .tokens-subtitle { font-size: 0.9rem; opacity: 0.7; margin-top: 8px; }

        .category-section { max-width: 1200px; margin: 30px auto; }
        .category-title { font-family: 'Dancing Script', cursive; font-size: 1.5rem; text-align: center; margin-bottom: 20px; display: inline-block; width: auto; padding-bottom: 5px; border-bottom: 2px solid; }
        .category-title.distance { border-bottom-color: var(--hydrangea-blue); color: var(--hydrangea-blue); }
        .category-title.reunion { border-bottom-color: var(--sakura-pink); color: var(--sakura-pink); }
        .category-title.silly { border-bottom-color: var(--gold); color: var(--gold); }

        .tickets-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
        }

        .ticket-card {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.08), rgba(255, 255, 255, 0.02));
            border-radius: 16px;
            padding: 18px;
            position: relative;
            transition: all 0.3s ease;
            border: 1px solid rgba(255, 255, 255, 0.1);
            opacity: 0;
            transform: translateY(20px);
        }

        .ticket-card.animate-in {
            opacity: 1;
            transform: translateY(0);
        }

        .ticket-card.hk-style { border-left: 4px solid var(--hydrangea-blue); }
        .ticket-card.japan-style { border-left: 4px solid var(--sakura-pink); }
        .ticket-card.gold-style { border-left: 4px solid var(--gold); }

        .ticket-card.redeemed { opacity: 0.6; }
        .ticket-card.redeemed .stamp-overlay { display: flex; }

        .stamp-overlay {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) rotate(-15deg);
            background: rgba(227, 66, 52, 0.95);
            color: white;
            font-size: 0.8rem;
            font-weight: bold;
            padding: 5px 12px;
            border-radius: 40px;
            border: 2px solid white;
            display: none;
            z-index: 10;
            white-space: nowrap;
            flex-direction: column;
            text-align: center;
        }

        .ticket-stamp-date { font-size: 0.6rem; display: block; }

        .ticket-icon { font-size: 1.8rem; margin-bottom: 10px; filter: drop-shadow(0 0 3px gold); }
        .ticket-title { font-size: 1rem; font-weight: bold; margin-bottom: 5px; color: var(--gold); }
        .ticket-description { font-size: 0.8rem; opacity: 0.8; line-height: 1.4; margin-bottom: 12px; }

        .redeem-btn {
            background: transparent;
            border: 1px solid var(--gold);
            color: var(--gold);
            padding: 6px 15px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 0.75rem;
            transition: all 0.3s ease;
        }

        .redeem-btn:hover { background: rgba(255, 215, 0, 0.2); }
        .redeem-btn:disabled { opacity: 0.5; cursor: not-allowed; }

        .coupon-counter {
            text-align: center;
            margin: 30px 0;
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 40px;
            font-size: 1rem;
        }

        .counter-number { font-size: 1.8rem; font-weight: bold; color: var(--gold); margin: 0 5px; }

        .closing-note {
            text-align: center;
            margin-top: 40px;
            padding: 30px;
            background: linear-gradient(135deg, rgba(162, 194, 255, 0.1), rgba(255, 183, 197, 0.1));
            border-radius: 30px;
        }

        .reset-tokens-btn {
            background: transparent;
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 6px 18px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 0.75rem;
            margin-top: 15px;
        }

        #page-playlist {
            display: none;
            padding: 60px 20px;
        }

        .playlist-header { text-align: center; padding: 20px 20px 30px; }
        .playlist-title { font-family: 'Dancing Script', cursive; font-size: 2.2rem; color: var(--vinyl-purple); text-shadow: 0 0 10px rgba(139, 92, 246, 0.5); }
        .playlist-intro { font-style: italic; opacity: 0.8; margin-top: 8px; font-size: 0.9rem; }

        .song-list {
            max-width: 700px;
            margin: 0 auto;
            padding: 20px;
        }

        .song-item {
            padding: 20px;
            margin: 20px 0;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.08), rgba(0, 0, 0, 0.3));
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s ease;
            display: flex;
            align-items: flex-start;
            gap: 20px;
            backdrop-filter: blur(5px);
        }

        .song-item:hover {
            transform: translateY(-5px);
            border-color: var(--vinyl-purple);
            box-shadow: 0 10px 25px rgba(139, 92, 246, 0.2);
        }

        .song-icon {
            font-size: 2.5rem;
            color: var(--vinyl-purple);
            flex-shrink: 0;
        }

        .song-content {
            flex: 1;
        }

        .song-title {
            font-weight: 600;
            color: var(--gold);
            margin-bottom: 8px;
            font-size: 1.1rem;
        }

        .song-desc {
            font-style: italic;
            color: rgba(255, 255, 255, 0.8);
            font-size: 0.85rem;
            margin-bottom: 12px;
            line-height: 1.5;
        }

        .song-link {
            color: var(--vinyl-purple);
            text-decoration: none;
            font-weight: 500;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s ease;
            padding: 6px 12px;
            background: rgba(139, 92, 246, 0.15);
            border-radius: 25px;
            font-size: 0.8rem;
        }

        .song-link:hover {
            color: var(--gold);
            transform: translateX(5px);
            background: rgba(139, 92, 246, 0.3);
        }

        .playlist-ending {
            text-align: center;
            margin-top: 40px;
            padding: 25px;
            background: rgba(139, 92, 246, 0.1);
            border-radius: 30px;
            font-size: 0.9rem;
            border: 1px solid rgba(139, 92, 246, 0.3);
        }

        .recipe-page, .letters-page, .poems-page {
            background: radial-gradient(ellipse at center, #2a2418 0%, #1a1610 100%);
        }
        
        .recipe-container, .letters-container {
            max-width: 900px;
            margin: 0 auto;
            padding: 60px 30px;
            background: rgba(253, 248, 237, 0.95);
            border-radius: 40px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
            color: var(--recipe-brown);
            font-family: 'Quicksand', sans-serif;
            position: relative;
            overflow: hidden;
        }
        
        .recipe-title, .letters-title {
            font-family: 'Dancing Script', cursive;
            font-size: 3rem;
            text-align: center;
            color: var(--recipe-gold);
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 10px;
            animation: fadeInUp 0.8s ease;
        }
        
        .recipe-subtitle, .letters-subtitle {
            text-align: center;
            font-style: italic;
            color: #b87c4f;
            margin-bottom: 40px;
            font-size: 1rem;
            border-bottom: 2px dashed var(--recipe-gold);
            display: inline-block;
            width: auto;
            margin-left: auto;
            margin-right: auto;
            padding-bottom: 8px;
        }
        
        .ingredient-list, .method-list {
            list-style: none;
            padding: 0;
        }
        
        .ingredient-list li, .method-list li, .note-item {
            margin: 15px 0;
            padding: 12px 20px;
            background: rgba(197, 185, 230, 0.15);
            border-radius: 20px;
            opacity: 0;
            transform: translateX(-20px);
            animation: slideInLeft 0.5s ease forwards;
            border-left: 4px solid var(--recipe-gold);
        }
        
        .ingredient-list li:nth-child(1) { animation-delay: 0.1s; }
        .ingredient-list li:nth-child(2) { animation-delay: 0.2s; }
        .ingredient-list li:nth-child(3) { animation-delay: 0.3s; }
        .ingredient-list li:nth-child(4) { animation-delay: 0.4s; }
        .ingredient-list li:nth-child(5) { animation-delay: 0.5s; }
        .ingredient-list li:nth-child(6) { animation-delay: 0.6s; }
        .ingredient-list li:nth-child(7) { animation-delay: 0.7s; }
        .ingredient-list li:nth-child(8) { animation-delay: 0.8s; }
        .ingredient-list li:nth-child(9) { animation-delay: 0.9s; }
        .ingredient-list li:nth-child(10) { animation-delay: 1s; }
        
        .method-list li { animation: slideInRight 0.5s ease forwards; }
        .method-list li:nth-child(1) { animation-delay: 0.1s; }
        .method-list li:nth-child(2) { animation-delay: 0.2s; }
        .method-list li:nth-child(3) { animation-delay: 0.3s; }
        .method-list li:nth-child(4) { animation-delay: 0.4s; }
        .method-list li:nth-child(5) { animation-delay: 0.5s; }
        .method-list li:nth-child(6) { animation-delay: 0.6s; }
        .method-list li:nth-child(7) { animation-delay: 0.7s; }
        .method-list li:nth-child(8) { animation-delay: 0.8s; }
        
        .notes-section {
            margin: 40px 0;
            padding: 20px;
            background: rgba(255, 217, 230, 0.3);
            border-radius: 25px;
        }
        
        .note-item {
            background: rgba(247, 200, 115, 0.2);
            transform: rotate(var(--rotate, -0.5deg));
            transition: all 0.3s ease;
        }
        
        .note-item:hover {
            transform: translateY(-5px) rotate(0deg);
            box-shadow: 0 8px 20px rgba(0,0,0,0.1);
        }
        
        .secret-ingredient {
            text-align: center;
            padding: 30px;
            background: linear-gradient(135deg, rgba(247, 200, 115, 0.3), rgba(255, 217, 230, 0.3));
            border-radius: 30px;
            margin: 30px 0;
            position: relative;
            overflow: hidden;
        }
        
        .secret-ingredient::before {
            content: "✨";
            position: absolute;
            font-size: 100px;
            opacity: 0.1;
            bottom: -20px;
            right: -20px;
            pointer-events: none;
        }
        
        .secret-line {
            font-size: 1.1rem;
            margin: 15px 0;
            opacity: 0;
            animation: fadeInUp 0.6s ease forwards;
        }
        
        .secret-line:nth-child(1) { animation-delay: 0.1s; }
        .secret-line:nth-child(2) { animation-delay: 0.3s; }
        .secret-line:nth-child(3) { animation-delay: 0.5s; }
        .secret-line:nth-child(4) { animation-delay: 0.7s; }
        .secret-line:nth-child(5) { animation-delay: 0.9s; }
        
        .recipe-closing {
            text-align: center;
            margin-top: 40px;
            padding: 20px;
            border-top: 2px solid var(--recipe-gold);
        }
        
        .heart-pulse {
            display: inline-block;
            animation: heartBeat 1.5s infinite;
            font-size: 1.5rem;
        }
        
        @keyframes heartBeat {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }
        
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes slideInLeft {
            from {
                opacity: 0;
                transform: translateX(-30px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        @keyframes slideInRight {
            from {
                opacity: 0;
                transform: translateX(30px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        .letter-card {
            background: rgba(255, 255, 255, 0.9);
            padding: 25px;
            margin: 25px 0;
            border-radius: 20px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
            border-left: 8px solid var(--recipe-gold);
            transition: all 0.3s ease;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUp 0.6s ease forwards;
        }
        
        .letter-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.15);
        }
        
        .letter-title {
            font-family: 'Dancing Script', cursive;
            font-size: 1.5rem;
            color: var(--recipe-gold);
            margin-bottom: 15px;
        }
        
        .letter-signature {
            text-align: right;
            margin-top: 20px;
            font-style: italic;
            color: #b87c4f;
        }
        
        .empty-envelope {
            text-align: center;
            padding: 40px;
            background: repeating-linear-gradient(45deg, rgba(247,200,115,0.1), rgba(247,200,115,0.1) 10px, rgba(255,217,230,0.1) 10px, rgba(255,217,230,0.1) 20px);
            border-radius: 20px;
            border: 2px dashed var(--recipe-gold);
            margin: 30px 0;
        }
        
        .poem-page {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 60px 20px;
        }
        
        .poem-card {
            max-width: 700px;
            margin: 0 auto;
            padding: 50px 40px;
            border-radius: 40px;
            text-align: center;
            position: relative;
            backdrop-filter: blur(15px);
            transition: all 0.3s ease;
        }
        
        .poem-card h1 {
            font-family: 'Dancing Script', cursive;
            font-size: 2.5rem;
            margin-bottom: 20px;
        }
        
        .poem-card .poem-date {
            font-size: 0.9rem;
            opacity: 0.7;
            margin-bottom: 40px;
            letter-spacing: 2px;
        }
        
        .poem-card .poem-lines {
            font-size: 1.1rem;
            line-height: 2;
            margin: 30px 0;
            font-style: italic;
        }
        
        .poem-card .poem-lines p {
            margin: 15px 0;
        }
        
        #page-poem13 {
            background: linear-gradient(135deg, #f5e6d3 0%, #f0d8b0 100%);
        }
        #page-poem13 .poem-card {
            background: rgba(255, 245, 220, 0.92);
            color: #8b5a2b;
            box-shadow: 0 25px 50px rgba(0,0,0,0.2);
            animation: warmGlow 3s infinite alternate;
        }
        @keyframes warmGlow {
            0% { box-shadow: 0 25px 50px rgba(139, 90, 43, 0.2); }
            100% { box-shadow: 0 35px 70px rgba(255, 215, 0, 0.4); }
        }
        #page-poem13 h1 { color: #c97e2c; text-shadow: 2px 2px 4px rgba(0,0,0,0.1); }
        #page-poem13 .poem-lines { animation: fadeInUp 1s ease; }
        
        #page-poem14 {
            background: linear-gradient(135deg, #c9e4ff 0%, #a8c9e8 100%);
        }
        #page-poem14 .poem-card {
            background: rgba(255, 255, 255, 0.9);
            color: #2c5a7a;
            box-shadow: 0 25px 50px rgba(0,0,0,0.15);
            animation: floatGlow 4s ease-in-out infinite;
        }
        @keyframes floatGlow {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-8px); }
        }
        #page-poem14 h1 { color: #3a7ca5; }
        #page-poem14 .poem-lines { animation: slideInRight 0.8s ease; }
        
        #page-poem15 {
            background: linear-gradient(135deg, #ffd9e6 0%, #ffb7c5 100%);
        }
        #page-poem15 .poem-card {
            background: rgba(255, 245, 250, 0.92);
            color: #b1546b;
            box-shadow: 0 25px 50px rgba(0,0,0,0.15);
            animation: gentlePulse 3s infinite alternate;
        }
        @keyframes gentlePulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.01); }
        }
        #page-poem15 h1 { color: #d46b85; }
        #page-poem15 .poem-lines { animation: fadeInUp 0.8s ease; }
        
        #page-poem16 {
            background: linear-gradient(135deg, #1a3b5c 0%, #0e2a44 100%);
        }
        #page-poem16 .poem-card {
            background: rgba(30, 50, 70, 0.88);
            color: #b8e1fc;
            box-shadow: 0 25px 50px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.2);
            animation: waveGlow 5s ease-in-out infinite;
        }
        @keyframes waveGlow {
            0%, 100% { border-color: rgba(184, 225, 252, 0.3); }
            50% { border-color: rgba(184, 225, 252, 0.8); }
        }
        #page-poem16 h1 { color: #9ac4e0; }
        #page-poem16 .poem-lines { animation: slideInLeft 0.9s ease; }
        
        #page-poem17 {
            background: radial-gradient(ellipse at center, #2a1b3d 0%, #1a0f2a 100%);
        }
        #page-poem17 .poem-card {
            background: rgba(45, 30, 65, 0.9);
            color: #e8d0ff;
            box-shadow: 0 25px 50px rgba(0,0,0,0.4), 0 0 30px rgba(255, 215, 0, 0.3);
            border: 1px solid rgba(255, 215, 0, 0.5);
            animation: starTwinkle 2s infinite alternate;
        }
        @keyframes starTwinkle {
            0% { box-shadow: 0 25px 50px rgba(0,0,0,0.4), 0 0 20px rgba(255, 215, 0, 0.2); }
            100% { box-shadow: 0 25px 50px rgba(0,0,0,0.4), 0 0 40px rgba(255, 215, 0, 0.5); }
        }
        #page-poem17 h1 { color: #f0b27a; text-shadow: 0 0 10px rgba(255,215,0,0.5); }
        #page-poem17 .poem-lines { animation: fadeInUp 1s ease; }
        
        @keyframes glowIn { from { opacity: 0; filter: blur(10px); } to { opacity: 1; filter: blur(0); } }

        .continue-btn {
            display: block;
            margin: 40px auto;
            background: transparent;
            border: 1px solid var(--hydrangea-blue);
            color: white;
            padding: 10px 25px;
            border-radius: 25px;
            cursor: pointer;
            transition: 0.3s;
            font-size: 0.9rem;
        }
        .continue-btn:hover { background: rgba(162, 194, 255, 0.1); }

        .ending-line { text-align: center; margin-top: 50px; font-style: italic; opacity: 0.9; line-height: 1.8; font-size: 1rem; }
        
        .mixing-bowl {
            text-align: center;
            font-size: 3rem;
            margin-bottom: 20px;
            animation: stir 2s infinite ease-in-out;
            display: inline-block;
            width: 100%;
        }
        
        @keyframes stir {
            0%, 100% { transform: rotate(0deg); }
            25% { transform: rotate(-10deg); }
            75% { transform: rotate(10deg); }
        }
        
        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
        }
        
/* Page 18: Tree of Us - Updated with transparent theme */
.tree-page {
    background: transparent;
    min-height: 100vh;
    padding: 60px 20px;
    position: relative;
}

.tree-page::before {
    content: "";
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: radial-gradient(ellipse at center, rgba(10, 15, 30, 0.85), rgba(3, 6, 15, 0.92));
    z-index: -1;
    pointer-events: none;
}

.tree-container {
    max-width: 900px;
    margin: 0 auto;
    position: relative;
}

.tree-title {
    font-family: 'Dancing Script', cursive;
    font-size: 3rem;
    text-align: center;
    color: var(--gold);
    margin-bottom: 20px;
    text-shadow: 0 0 20px rgba(255, 215, 0, 0.5);
    background: linear-gradient(135deg, #ffd966, #ffaa66);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

.tree-subtitle {
    text-align: center;
    font-style: italic;
    color: #aaccff;
    margin-bottom: 50px;
    font-size: 1rem;
    text-shadow: 0 0 10px rgba(100, 150, 255, 0.3);
}

.tree-section {
    position: relative;
    margin: 60px 0;
    padding: 30px;
    background: rgba(10, 20, 40, 0.35);
    backdrop-filter: blur(12px);
    border-radius: 30px;
    border: 1px solid rgba(255, 215, 0, 0.15);
    opacity: 0;
    transform: translateY(30px);
    transition: all 0.8s ease;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.tree-section.visible {
    opacity: 1;
    transform: translateY(0);
}

.tree-section:hover {
    border-color: rgba(255, 215, 0, 0.3);
    background: rgba(20, 35, 60, 0.45);
    box-shadow: 0 8px 32px rgba(255, 215, 0, 0.1);
}

.tree-section-title {
    font-family: 'Dancing Script', cursive;
    font-size: 1.8rem;
    color: var(--gold);
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 10px;
    text-shadow: 0 0 10px rgba(255, 215, 0, 0.3);
}

.tree-section-title::before {
    content: "✦";
    font-size: 1.2rem;
    opacity: 0.8;
}

.tree-list {
    list-style: none;
    padding: 0;
}

.tree-list li {
    margin: 12px 0;
    padding: 12px 18px;
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(4px);
    border-radius: 20px;
    display: flex;
    align-items: center;
    gap: 12px;
    transition: all 0.3s ease;
    border-left: 3px solid rgba(255, 215, 0, 0.3);
    color: #e6f0ff;
}

.tree-list li:hover {
    background: rgba(255, 215, 0, 0.12);
    transform: translateX(8px);
    border-left-color: var(--gold);
}

.leaf-memory {
    background: rgba(100, 150, 80, 0.15);
    border-left: 3px solid #8baa5e;
}

.leaf-memory:hover {
    background: rgba(100, 150, 80, 0.25);
    border-left-color: #aacc77;
}

.flower-item {
    background: rgba(255, 183, 197, 0.12);
    border-left: 3px solid var(--sakura-pink);
}

.flower-item:hover {
    background: rgba(255, 183, 197, 0.22);
    border-left-color: #ffb7c5;
}

.fruit-item {
    background: rgba(255, 215, 0, 0.1);
    border-left: 3px solid var(--gold);
}

.fruit-item:hover {
    background: rgba(255, 215, 0, 0.2);
    border-left-color: #ffdd44;
}

.seed-item {
    background: rgba(139, 90, 43, 0.12);
    border-left: 3px solid #b87c4f;
}

.seed-item:hover {
    background: rgba(139, 90, 43, 0.22);
    border-left-color: #d49c6c;
}

.tree-quote {
    text-align: center;
    padding: 35px;
    background: rgba(0, 0, 0, 0.35);
    backdrop-filter: blur(10px);
    border-radius: 40px;
    margin: 40px 0;
    font-style: italic;
    line-height: 1.8;
    border: 1px solid rgba(255, 215, 0, 0.2);
    color: #f0e6d0;
    font-size: 1rem;
    transition: all 0.3s ease;
}

.tree-quote:hover {
    background: rgba(0, 0, 0, 0.45);
    border-color: rgba(255, 215, 0, 0.4);
}

/* Special styling for the letter section */
#letterSection .tree-quote {
    background: linear-gradient(135deg, rgba(255, 215, 0, 0.08), rgba(255, 183, 197, 0.08));
    border-left: 3px solid var(--gold);
    border-right: 3px solid var(--sakura-pink);
}

#letterSection .tree-quote p {
    margin: 15px 0;
}

/* Custom scrollbar for tree page */
.tree-page::-webkit-scrollbar {
    width: 8px;
}

.tree-page::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 10px;
}

.tree-page::-webkit-scrollbar-thumb {
    background: rgba(255, 215, 0, 0.3);
    border-radius: 10px;
}

.tree-page::-webkit-scrollbar-thumb:hover {
    background: rgba(255, 215, 0, 0.5);
}
        
        /* Page 19: I Love You in Many Languages */
        .languages-page {
            background: linear-gradient(135deg, #1a2a3a 0%, #0a1a2a 100%);
            min-height: 100vh;
            padding: 60px 20px;
            position: relative;
            overflow-x: hidden;
        }
        
        .world-map-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0.08;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600"><path fill="none" stroke="white" stroke-width="0.5" d="M200,150 L250,120 L300,140 L350,110 L400,130 L450,100 L500,120 L550,90 L600,110 L650,80 L700,100"/><circle cx="250" cy="140" r="15" fill="none" stroke="white"/><circle cx="450" cy="120" r="12" fill="none" stroke="white"/><circle cx="550" cy="100" r="10" fill="none" stroke="white"/><path d="M150,300 L200,280 L250,300 L300,270 L350,290 L400,260 L450,280 L500,250 L550,270 L600,240 L650,260" stroke="white" fill="none" stroke-width="0.5"/><path d="M100,450 L150,420 L200,440 L250,410 L300,430 L350,400 L400,420 L450,390 L500,410 L550,380 L600,400" stroke="white" fill="none" stroke-width="0.5"/></svg>');
            background-repeat: repeat;
            background-size: 800px;
            pointer-events: none;
        }
        
        .languages-container {
            max-width: 800px;
            margin: 0 auto;
            position: relative;
            z-index: 2;
        }
        
        .languages-title {
            font-family: 'Dancing Script', cursive;
            font-size: 2.8rem;
            text-align: center;
            color: var(--gold);
            margin-bottom: 20px;
            text-shadow: 0 0 15px rgba(255,215,0,0.5);
        }
        
        .languages-subtitle {
            text-align: center;
            font-style: italic;
            margin-bottom: 50px;
            color: #c9b37c;
        }
        
        .language-card {
            background: rgba(255,255,255,0.08);
            backdrop-filter: blur(8px);
            border-radius: 20px;
            padding: 25px;
            margin: 25px 0;
            transition: all 0.4s ease;
            opacity: 0;
            transform: translateX(-30px);
            border: 1px solid rgba(255,215,0,0.3);
            position: relative;
            overflow: hidden;
        }
        
        .language-card.animate-in {
            opacity: 1;
            transform: translateX(0);
        }
        
        .language-card:hover {
            transform: translateX(10px) scale(1.01);
            background: rgba(255,255,255,0.15);
            border-color: var(--gold);
        }
        
        .language-flag {
            font-size: 2rem;
            margin-right: 15px;
            display: inline-block;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
        }
        
        .language-phrase {
            font-family: 'Dancing Script', cursive;
            font-size: 1.8rem;
            color: var(--gold);
            margin: 10px 0;
        }
        
        .language-meaning {
            font-size: 0.9rem;
            opacity: 0.8;
            margin-top: 10px;
            line-height: 1.6;
        }
        
        .heart-center {
            text-align: center;
            margin: 50px 0;
            position: relative;
        }
        
        .glowing-heart {
            font-size: 4rem;
            animation: heartBeat 1.5s infinite;
            filter: drop-shadow(0 0 15px rgba(255,215,0,0.8));
        }
        
        .languages-closing {
            text-align: center;
            margin-top: 60px;
            padding: 40px;
            background: rgba(0,0,0,0.3);
            border-radius: 50px;
        }
        
        .floating-word {
            position: fixed;
            pointer-events: none;
            font-size: 1rem;
            opacity: 0;
            animation: wordFloat 6s ease-out forwards;
            z-index: 100;
        }
        
        @keyframes wordFloat {
            0% {
                opacity: 0;
                transform: translateY(0) scale(0.5);
            }
            20% {
                opacity: 0.8;
            }
            100% {
                opacity: 0;
                transform: translateY(-150px) scale(1.2);
            }
        }
        
/* Page 20: The Apology I Owe You - Updated with transparent theme */
.apology-page {
    background: transparent;
    min-height: 100vh;
    padding: 60px 20px;
    position: relative;
    overflow-x: hidden;
    transition: background 0.8s ease;
}

.apology-page::before {
    content: "";
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(135deg, rgba(20, 30, 45, 0.88), rgba(10, 20, 35, 0.92));
    z-index: -1;
    pointer-events: none;
}

.apology-page.transitioning::before {
    background: linear-gradient(135deg, rgba(30, 40, 55, 0.88), rgba(15, 25, 40, 0.92));
}

.apology-container {
    max-width: 800px;
    margin: 0 auto;
    position: relative;
    z-index: 2;
}

.apology-title {
    font-family: 'Dancing Script', cursive;
    font-size: 2.8rem;
    text-align: center;
    color: var(--gold);
    margin-bottom: 20px;
    text-shadow: 0 0 20px rgba(255, 215, 0, 0.4);
    background: linear-gradient(135deg, #ffd966, #ffaa66);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

.apology-date {
    text-align: center;
    font-style: italic;
    margin-bottom: 50px;
    color: #aaccff;
    text-shadow: 0 0 10px rgba(100, 150, 255, 0.3);
    letter-spacing: 1px;
}

.apology-letter {
    background: rgba(15, 25, 40, 0.45);
    backdrop-filter: blur(12px);
    border-radius: 30px;
    padding: 45px;
    margin: 30px 0;
    border: 1px solid rgba(255, 215, 0, 0.2);
    transition: all 0.5s ease;
    position: relative;
    overflow: hidden;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}

.apology-letter:hover {
    border-color: rgba(255, 215, 0, 0.4);
    background: rgba(20, 35, 55, 0.55);
    box-shadow: 0 8px 32px rgba(255, 215, 0, 0.1);
}

.apology-letter.calm {
    background: rgba(30, 45, 65, 0.6);
    backdrop-filter: blur(12px);
    border-color: var(--gold);
    box-shadow: 0 8px 32px rgba(255, 215, 0, 0.15);
}

.apology-letter p {
    margin-bottom: 20px;
    line-height: 1.8;
    font-size: 1rem;
    opacity: 0;
    transform: translateY(20px);
    animation: fadeInUp 0.5s ease forwards;
    color: #f0e8e0;
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
}

.apology-letter p:nth-child(1) { animation-delay: 0.1s; }
.apology-letter p:nth-child(2) { animation-delay: 0.2s; }
.apology-letter p:nth-child(3) { animation-delay: 0.3s; }
.apology-letter p:nth-child(4) { animation-delay: 0.4s; }
.apology-letter p:nth-child(5) { animation-delay: 0.5s; }
.apology-letter p:nth-child(6) { animation-delay: 0.6s; }
.apology-letter p:nth-child(7) { animation-delay: 0.7s; }
.apology-letter p:nth-child(8) { animation-delay: 0.8s; }
.apology-letter p:nth-child(9) { animation-delay: 0.9s; }
.apology-letter p:nth-child(10) { animation-delay: 1s; }
.apology-letter p:nth-child(11) { animation-delay: 1.1s; }
.apology-letter p:nth-child(12) { animation-delay: 1.2s; }

.apology-letter p:last-child {
    text-align: right;
    margin-top: 30px;
    font-family: 'Dancing Script', cursive;
    font-size: 1.1rem;
    color: var(--gold);
}

.rain-drop {
    position: absolute;
    width: 2px;
    height: 10px;
    background: rgba(100, 150, 200, 0.4);
    animation: rainFall 3s linear forwards;
    pointer-events: none;
}

@keyframes rainFall {
    0% { transform: translateY(-100px); opacity: 0; }
    10% { opacity: 0.5; }
    100% { transform: translateY(100vh); opacity: 0; }
}

.light-ray {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    background: radial-gradient(circle at center, rgba(255, 215, 0, 0.1) 0%, transparent 70%);
    pointer-events: none;
    opacity: 0;
    transition: opacity 1s ease;
}

.light-ray.active {
    opacity: 1;
    animation: gentlePulse 3s ease-in-out infinite;
}

@keyframes gentlePulse {
    0%, 100% {
        opacity: 0.4;
    }
    50% {
        opacity: 0.8;
    }
}

/* Decorative elements for the apology page */
.apology-container::before {
    content: "🌙";
    position: fixed;
    bottom: 20px;
    left: 20px;
    font-size: 1.5rem;
    opacity: 0.3;
    pointer-events: none;
    animation: float 6s ease-in-out infinite;
}

.apology-container::after {
    content: "⭐";
    position: fixed;
    top: 20px;
    right: 20px;
    font-size: 1.5rem;
    opacity: 0.3;
    pointer-events: none;
    animation: float 8s ease-in-out infinite reverse;
}

@keyframes float {
    0%, 100% {
        transform: translateY(0px);
    }
    50% {
        transform: translateY(-10px);
    }
}

/* Custom scrollbar for apology page */
.apology-page::-webkit-scrollbar {
    width: 8px;
}

.apology-page::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 10px;
}

.apology-page::-webkit-scrollbar-thumb {
    background: rgba(255, 215, 0, 0.3);
    border-radius: 10px;
}

.apology-page::-webkit-scrollbar-thumb:hover {
    background: rgba(255, 215, 0, 0.5);
}
        /* Page 21: The Receipt of Love */
        .receipt-page {
            background: linear-gradient(135deg, #2d2b1f 0%, #1e1c14 100%);
            min-height: 100vh;
            padding: 60px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow-x: hidden;
        }

        .receipt-wrapper {
            position: relative;
            max-width: 550px;
            width: 100%;
            margin: 0 auto;
            perspective: 1000px;
        }

        .receipt-paper {
            background: #fef7e0;
            color: #2d2b1f;
            font-family: 'Courier New', monospace;
            position: relative;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            overflow: hidden;
            transform-origin: top center;
            animation: paperUnroll 2s ease-out forwards;
        }

        @keyframes paperUnroll {
            0% {
                transform: scaleY(0);
                opacity: 0;
            }
            100% {
                transform: scaleY(1);
                opacity: 1;
            }
        }

        .receipt-content {
            padding: 40px 30px;
            font-size: 0.85rem;
            line-height: 1.8;
            letter-spacing: 0.5px;
            white-space: pre-wrap;
            font-family: 'Courier New', monospace;
            background: linear-gradient(to bottom, #fef7e0, #fff5e0);
            position: relative;
        }

        .receipt-content::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 20px;
            background: repeating-linear-gradient(
                45deg,
                transparent,
                transparent 10px,
                rgba(0,0,0,0.05) 10px,
                rgba(0,0,0,0.05) 20px
            );
            pointer-events: none;
        }

        .receipt-tear {
            position: absolute;
            bottom: -15px;
            left: 0;
            right: 0;
            height: 30px;
            background: repeating-linear-gradient(
                45deg,
                #fef7e0,
                #fef7e0 10px,
                #f0e5c0 10px,
                #f0e5c0 20px
            );
            clip-path: polygon(0% 0%, 100% 0%, 95% 100%, 5% 100%);
            animation: tearEffect 0.5s ease-out 1.8s forwards;
            opacity: 0;
        }

        @keyframes tearEffect {
            0% {
                opacity: 0;
                transform: translateY(0);
            }
            100% {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .receipt-stamp {
            position: absolute;
            bottom: 80px;
            right: 40px;
            font-size: 3rem;
            font-weight: bold;
            font-family: 'Courier New', monospace;
            color: #e34234;
            opacity: 0;
            transform: rotate(-15deg) scale(0.5);
            transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
            background: rgba(227, 66, 52, 0.1);
            padding: 5px 15px;
            border-radius: 50px;
            border: 2px solid #e34234;
            pointer-events: none;
            z-index: 10;
        }

        .receipt-stamp.show {
            opacity: 0.9;
            transform: rotate(-15deg) scale(1);
            animation: stampThump 0.3s ease-out;
        }

        @keyframes stampThump {
            0% {
                transform: rotate(-15deg) scale(0.8);
            }
            50% {
                transform: rotate(-15deg) scale(1.1);
            }
            100% {
                transform: rotate(-15deg) scale(1);
            }
        }

        .receipt-paper::after {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-image: repeating-linear-gradient(
                0deg,
                transparent,
                transparent 2px,
                rgba(0,0,0,0.02) 2px,
                rgba(0,0,0,0.02) 4px
            );
            pointer-events: none;
        }

        .receipt-page::before {
            content: "🔊";
            position: fixed;
            bottom: 20px;
            left: 20px;
            font-size: 1rem;
            opacity: 0;
            animation: registerSound 2s ease-out forwards;
            pointer-events: none;
            z-index: 1000;
        }

        @keyframes registerSound {
            0% {
                opacity: 0;
                transform: translateX(-20px);
            }
            20% {
                opacity: 0.5;
            }
            40% {
                opacity: 0;
            }
            100% {
                opacity: 0;
                transform: translateX(0);
            }
        }
        
        /* Page 22: The Ending */
        .ending-page {
            background: radial-gradient(ellipse at center, #0a0a1a 0%, #03030f 100%);
            min-height: 100vh;
            padding: 60px 20px;
            position: relative;
            overflow-x: hidden;
        }
        
        .ending-container {
            max-width: 800px;
            margin: 0 auto;
            text-align: center;
            position: relative;
            z-index: 2;
        }
        
        .ending-opening {
            font-family: 'Dancing Script', cursive;
            font-size: 2rem;
            color: var(--gold);
            margin-bottom: 60px;
            opacity: 0;
            animation: fadeInUp 1s ease forwards;
        }
        
        .numbers-grid {
            display: flex;
            justify-content: center;
            gap: 30px;
            flex-wrap: wrap;
            margin: 40px 0;
            font-family: 'Courier New', monospace;
        }
        
        .number-item {
            text-align: center;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUp 0.5s ease forwards;
        }
        
        .number-value {
            font-size: 2.5rem;
            font-weight: bold;
            color: var(--gold);
        }
        
        .handwritten-letter {
            background: rgba(255,248,225,0.95);
            padding: 50px;
            border-radius: 20px;
            margin: 40px 0;
            text-align: left;
            color: #3a2c1f;
            position: relative;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            cursor: pointer;
            transition: transform 0.3s ease;
        }
        
        .handwritten-letter:hover {
            transform: translateY(-5px);
        }
        
        .handwritten-letter p {
            margin-bottom: 15px;
            line-height: 1.8;
            font-size: 1rem;
            opacity: 0;
        }
        
        .handwritten-letter .typewriter {
            display: inline-block;
        }
        
        .tree-mini {
            margin: 40px 0;
            font-size: 3rem;
            position: relative;
        }
        
        .golden-leaf {
            position: absolute;
            top: -10px;
            right: -20px;
            font-size: 1.5rem;
            animation: leafFall 3s ease-in-out infinite;
        }
        
        @keyframes leafFall {
            0% { transform: translateY(-20px) rotate(0deg); opacity: 0; }
            20% { opacity: 1; }
            80% { opacity: 1; }
            100% { transform: translateY(100px) rotate(180deg); opacity: 0; }
        }
        
        .constellation-mini {
            margin: 40px 0;
            position: relative;
            height: 100px;
        }
        
        .star-mini {
            position: absolute;
            font-size: 1rem;
            animation: twinkle 2s infinite;
        }
        
        .star-button {
            background: transparent;
            border: 2px solid var(--gold);
            color: var(--gold);
            padding: 12px 30px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1rem;
            margin: 30px 0;
            transition: all 0.3s ease;
        }
        
        .star-button:hover {
            background: rgba(255,215,0,0.2);
            transform: scale(1.05);
        }
        
        .star-counter {
            margin: 20px 0;
            font-size: 0.9rem;
        }
        
        .final-line {
            font-family: 'Dancing Script', cursive;
            font-size: 1.5rem;
            color: var(--gold);
            margin: 40px 0;
            animation: pulse 2s infinite;
        }
        
        .hidden-message {
            display: inline-block;
            cursor: pointer;
            position: relative;
        }
        
        .secret-text {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0,0,0,0.8);
            padding: 10px 20px;
            border-radius: 30px;
            font-size: 0.8rem;
            z-index: 100;
            opacity: 0;
            transition: opacity 0.5s ease;
            white-space: nowrap;
        }
        
        .secret-text.show {
            opacity: 1;
        }
        
/* End Page - Updated with transparent theme */
#page-end {
    display: none;
    min-height: 100vh;
    align-items: center;
    justify-content: center;
    padding: 60px 20px;
    position: relative;
    background: transparent;
}

#page-end::before {
    content: "";
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: radial-gradient(ellipse at center, rgba(20, 15, 35, 0.85), rgba(8, 6, 18, 0.92));
    z-index: -1;
    pointer-events: none;
}

.end-card {
    max-width: 600px;
    background: rgba(25, 20, 45, 0.35);
    backdrop-filter: blur(15px);
    padding: 60px 45px;
    border-radius: 70px;
    text-align: center;
    border: 1px solid rgba(255, 215, 0, 0.25);
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2), 0 0 30px rgba(255, 215, 0, 0.1);
    transition: all 0.5s ease;
    animation: cardFloat 6s ease-in-out infinite;
}

.end-card:hover {
    border-color: rgba(255, 215, 0, 0.5);
    background: rgba(35, 28, 55, 0.45);
    box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3), 0 0 40px rgba(255, 215, 0, 0.2);
    transform: translateY(-5px);
}

@keyframes cardFloat {
    0%, 100% {
        transform: translateY(0px);
    }
    50% {
        transform: translateY(-8px);
    }
}

.end-card > div:first-child {
    font-size: 4rem;
    margin-bottom: 25px;
    animation: gentlePulse 2s ease-in-out infinite;
    filter: drop-shadow(0 0 15px rgba(255, 215, 0, 0.5));
}

.end-card p {
    margin: 20px 0;
    font-size: 1rem;
    line-height: 1.8;
    color: #f0e8e8;
    text-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
}

.end-card p:first-of-type {
    font-size: 1.1rem;
    font-weight: 500;
    color: #ffd966;
}

.end-card p:last-of-type {
    margin-top: 35px;
    font-family: 'Dancing Script', cursive;
    font-size: 1.2rem;
    color: #ffaa88;
}

.end-card p:first-of-type strong {
    color: var(--gold);
    font-weight: 600;
}

.end-card p:nth-child(2) {
    font-style: italic;
    border-top: 1px solid rgba(255, 215, 0, 0.2);
    border-bottom: 1px solid rgba(255, 215, 0, 0.2);
    padding: 20px 0;
    margin: 25px 0;
}

.end-card .signature-text {
    font-family: 'Dancing Script', cursive;
    font-size: 1.3rem;
    color: #ffaa88;
    margin-top: 30px;
    letter-spacing: 1px;
}

.end-card .date-text {
    font-size: 0.8rem;
    opacity: 0.6;
    margin-top: 15px;
    color: #aaccff;
}

/* Decorative sparkles around the end card */
.end-card::before {
    content: "✨";
    position: absolute;
    top: -20px;
    left: -20px;
    font-size: 1.2rem;
    opacity: 0.5;
    animation: twinkle 3s ease-in-out infinite;
}

.end-card::after {
    content: "✨";
    position: absolute;
    bottom: -20px;
    right: -20px;
    font-size: 1.2rem;
    opacity: 0.5;
    animation: twinkle 2.5s ease-in-out infinite reverse;
}

@keyframes gentlePulse {
    0%, 100% {
        transform: scale(1);
        filter: drop-shadow(0 0 15px rgba(255, 215, 0, 0.5));
    }
    50% {
        transform: scale(1.05);
        filter: drop-shadow(0 0 25px rgba(255, 215, 0, 0.8));
    }
}

/* Floating particles effect for end page */
#page-end .floating-particle {
    position: fixed;
    pointer-events: none;
    font-size: 0.8rem;
    opacity: 0;
    animation: particleFloat 8s ease-out forwards;
    z-index: 1;
}

@keyframes particleFloat {
    0% {
        opacity: 0;
        transform: translateY(0) rotate(0deg);
    }
    10% {
        opacity: 0.4;
    }
    90% {
        opacity: 0.2;
    }
    100% {
        opacity: 0;
        transform: translateY(-200px) rotate(360deg);
    }
}

/* Custom scrollbar */
#page-end::-webkit-scrollbar {
    width: 8px;
}

#page-end::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 10px;
}

#page-end::-webkit-scrollbar-thumb {
    background: rgba(255, 215, 0, 0.3);
    border-radius: 10px;
}

#page-end::-webkit-scrollbar-thumb:hover {
    background: rgba(255, 215, 0, 0.5);
}

/* Responsive adjustments */
@media (max-width: 600px) {
    .end-card {
        padding: 40px 25px;
        border-radius: 50px;
    }
    
    .end-card > div:first-child {
        font-size: 3rem;
    }
    
    .end-card p {
        font-size: 0.9rem;
    }
}
        
        .tree-section {
            transition: all 0.8s ease;
        }
        
        .visible {
            opacity: 1 !important;
            transform: translateY(0) !important;
        }
        
        #page-4, #page-5, #page-always, #page-redstring, #page-countdown, #page-tokens, #page-playlist,
        #page-recipe, #page-letters, #page-poem13, #page-poem14, #page-poem15, #page-poem16, #page-poem17,
        #page-tree, #page-languages, #page-apology, #page-receipt, #page-ending, #page-end {
            transition: opacity 1s ease;
        }
        
        @keyframes fadeInUpLine {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes twinkle {
            0%, 100% { opacity: 0.3; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.2); }
        }
    </style>
</head>
<body>

    <canvas id="canvas"></canvas>

    <section id="page-lock">
        <div style="font-size: 1rem; margin-bottom: 10px; opacity: 0.8;">Only for you, my Ko Ko</div>
        <div class="constellation">Min Khant Thwin</div>
        <div class="passcode-container">
            <p>This page was written only for you.</p>
            <input type="password" id="passcode" maxlength="8" placeholder="········">
            <div style="font-size: 0.7rem; opacity: 0.5;">Hint: Your Birthday (DDMMYYYY)</div>
        </div>
    </section>

    <section id="page-timeline">
        <div class="welcome-msg">
            <h2>Welcome Ko Ko.</h2>
            <p>You're the one who babe wants to spend my whole lifetime with. No matter how difficult things will be, babe still wants to pass through everything just to be with you, Ko Ko.</p>
            <p style="margin-top: 20px; font-size: 0.9rem;">Babe didn't realize it at first, but every small moment slowly brought us here.</p>
        </div>
        <h1 style="text-align:center; font-family:'Dancing Script'; font-size: 2rem;">From the First Message to Now</h1>
        <div class="timeline-container" id="timelineContainer"></div>
        <div class="ending-line">It didn't happen suddenly.<br>It happened slowly, honestly, and naturally.<br>And that's why it means so much to me.</div>
        <button class="continue-btn" onclick="openBday()">Open Birthday Page</button>
    </section>

    <section id="page-birthday">
        <h1 class="bday-title" id="bdayTitle">Happy 22nd Birthday, Honey 🌷✨</h1>
        <div style="margin-bottom: 30px; opacity: 0.6; letter-spacing: 2px; font-size: 0.8rem;" id="bdayDate">MARCH 25, 2026</div>
        <div class="bday-content" id="bdayContent"></div>
        <button class="continue-btn" id="to-wishes-btn" style="display:none;" onclick="goToPage4()">View My 22 Wishes</button>
    </section>
    
    <section id="page-4"><div class="wishes-header"><h2 class="wishes-script-title">22 Wishes for my 22-year-old Honey</h2><p class="wishes-intro">"Ko Ko, as you step into your 22nd year, I sent 22 whispers to the universe for you."</p></div><div class="wishes-container" id="wishesContainer"></div><div style="text-align:center;"><p style="margin-bottom: 20px;">"Close your eyes... pick one... it's already coming true."</p><button class="continue-btn" onclick="goToPage5()">Continue to Page 5</button></div></section>
    <section id="page-5"><div class="reasons-header"><h2 class="reasons-title">22 Reasons Why I Love You</h2><p class="wishes-intro">A constellation of reasons, each one a star in my sky.</p></div><div class="reasons-container" id="reasonsContainer"></div><div style="text-align:center;"><button class="continue-btn" onclick="goToAlwaysPage()">Continue to Always</button></div></section>
    <section id="page-always"><div class="always-container"><h2 class="reasons-title" style="font-size: 1.8rem;">For the days when the world feels too loud.</h2><div class="always-subtitle">"Whenever you feel tired, or whenever you doubt yourself, just tap the star."</div><div class="always-star-btn" id="alwaysStar">⭐</div><div class="always-message" id="alwaysMessage">Tap the star to receive a message from me ✨</div><div class="always-footer">This website might have many pages, but our story doesn't have an end page.<br>This is your "Always" button. You can come back here anytime.<br>I am your biggest fan, your best friend, and your Babe.<br>Always. Forever. No matter what.</div><button class="continue-btn" onclick="goToRedStringPage()">Continue to The Red String →</button></div></section>

    <!-- PAGE 7: THE THINGS DISTANCE COULDN'T TAKE (Red String Theory) -->
    <section id="page-redstring" style="display: none; padding: 60px 20px;">
        <div class="redstring-container">
            <div class="redstring-header">
                <h2 class="redstring-title">🧵 The Red String Theory 🧵</h2>
                <p class="redstring-intro">"In East Asian folklore, an invisible red thread connects those who are destined to meet. It may tangle or stretch, but never breaks."</p>
                <p class="redstring-subtitle">Distance tried to take so much from us. But here are the things it could never steal.</p>
            </div>

            <div class="shield-container">
                <svg class="shield-svg" viewBox="0 0 400 500">
                    <defs>
                        <radialGradient id="shieldGlow" cx="50%" cy="50%" r="50%">
                            <stop offset="0%" stop-color="#ffd700" stop-opacity="0.3"/>
                            <stop offset="100%" stop-color="#ff0000" stop-opacity="0"/>
                        </radialGradient>
                    </defs>
                    <path d="M200,40 L340,100 L340,260 C340,380 280,440 200,480 C120,440 60,380 60,260 L60,100 L200,40" 
                          fill="url(#shieldGlow)" stroke="#ff4444" stroke-width="2" stroke-opacity="0.3" fill-opacity="0.1"/>
                    <g id="threadWeave"></g>
                </svg>
                <div class="knots-container" id="knotsContainer"></div>
            </div>

            <div class="redstring-ending" id="redstringEnding" style="display: none;">
                <div class="shield-complete-message">
                    <p>✨ They say distance tests love. ✨</p>
                    <p>But distance didn't test us—it proved us.</p>
                    <p>Every mile was just evidence of how far we'd go.</p>
                    <p>Every wait was proof of how worth it you are.</p>
                    <p>The red string was never between our cities.</p>
                    <p>It was always here, in the things distance couldn't take.</p>
                    <p class="final-promise">And those things? They're not going anywhere.</p>
                    <div class="heart-red">❤️</div>
                </div>
            </div>

            <div class="redstring-footer">
                <p>The thread doesn't break. It only grows stronger.</p>
                <p class="signature">— Your Babe</p>
            </div>
            <button class="continue-btn" onclick="goToCountdownPage()">Continue to Countdown to Us →</button>
        </div>
    </section>

    <section id="page-countdown"><div class="countdown-header"><h2 class="countdown-title">✈️ Countdown to Us ✈️</h2><p class="countdown-intro">"Our journey hasn't ended... it's just waiting to take flight."</p></div><div class="flight-path-container"><div class="dotted-path" id="dottedPath"></div><div id="milestonesContainer"></div><div class="final-plane" id="finalPlane"><div class="plane-icon">✈️</div></div></div><div class="countdown-ending"><p>✨ This isn't just a list, Ko Ko. ✨</p><p>It's our map. One dot at a time, one flight at a time... I'll be right beside you.</p></div><button class="continue-btn" onclick="goToTokensPage()">Continue to Love Tokens</button></section>
    <section id="page-tokens"><div class="tokens-header"><h2 class="tokens-title">🎫 The International Love Token Booklet 🎫</h2><p class="tokens-subtitle">22 Promises. 22 Ways to Say "I Love You" from Across the Map.</p></div><div id="tokensContainer"></div><div class="coupon-counter"><span>🎟️ Coupons remaining: </span><span class="counter-number" id="remainingCount">22</span><span> / 22</span></div><div class="closing-note"><p>💙 Distance is just a number, but my love for you is a constant. 💙</p><p>These coupons are my promise that no matter where we are on the map —</p><p>Hong Kong or Japan — I am always yours.</p><p>Happy 22nd Birthday, my Ko Ko.</p><p class="signature">✨ The best is yet to come. ✨</p><button class="reset-tokens-btn" id="resetTokensBtn">↻ Reset All Coupons</button></div><button class="continue-btn" onclick="goToPlaylistPage()">Continue to Our Playlist</button></section>

    <section id="page-playlist">
        <div class="playlist-header">
            <h2 class="playlist-title">🎵 Soundtrack of Us 🎵</h2>
            <p class="playlist-intro">"Every love story has a soundtrack. This one is ours. Click each song to listen."</p>
        </div>
        <div class="song-list" id="playlistContainer"></div>
        <div class="playlist-ending">
            <p>🎧 Press play when you miss me. I'll be singing along too. 🎧</p>
            <p style="margin-top: 10px; font-size: 0.75rem;">Hover over each song to see why it matters. Click the link to listen.</p>
        </div>
        <button class="continue-btn" onclick="goToRecipePage()">Continue to Recipe for Us</button>
    </section>

    <!-- PAGE 11: RECIPE FOR US -->
    <section id="page-recipe" style="display: none; padding: 60px 20px;">
        <div class="recipe-container">
            <div class="mixing-bowl">🥣✨</div>
            <h1 class="recipe-title">Recipe for Us</h1>
            <p class="recipe-subtitle">A Love Story, Baked with Care</p>
            
            <h2 style="color: var(--recipe-gold); margin: 30px 0 20px;">Ingredients:</h2>
            <ul class="ingredient-list">
                <li>· 2 cups of patience — the kind that waits, listens, and never rushes</li>
                <li>· 1 cup of gentle understanding — to hold space for each other's quiet moments</li>
                <li>· 3 tablespoons of laughter — especially the kind that makes our stomachs hurt</li>
                <li>· A pinch of silly — for the late-night giggles and inside jokes no one else gets</li>
                <li>· 2 teaspoons of forgiveness — because we're human, and that's okay</li>
                <li>· 1 handful of shared adventures — from Pyin Oo Lwin to Ngwe Saung, and everywhere in between</li>
                <li>· A sprinkle of playlists — the soundtrack that plays in the background of us</li>
                <li>· A dash of spontaneity — for the unexpected trips and unplanned kisses</li>
                <li>· 1 whole heart — yours, given freely and kept safely</li>
                <li>· 1 whole heart — mine, trusting and growing with you</li>
            </ul>
            
            <h2 style="color: var(--recipe-gold); margin: 40px 0 20px;">Method:</h2>
            <ol class="method-list" style="list-style-type: none; padding-left: 0;">
                <li><strong>1.</strong> Start with a first message. Let it sit quietly. Watch it bloom into something neither of you expected.</li>
                <li><strong>2.</strong> Fold in patience slowly. Let the relationship rise at its own pace. Don't rush. Good things take time.</li>
                <li><strong>3.</strong> Add laughter generously. Stir it into everyday moments such as the accidental video calls, phone calls and snacks sent by you.</li>
                <li><strong>4.</strong> Mix in adventures. Travel together, explore together, get lost together. These are the flavors that deepen the bond.</li>
                <li><strong>5.</strong> Sprinkle with forgiveness. When things crack, don't throw it away. Patch it gently. Some things become stronger after being mended.</li>
                <li><strong>6.</strong> Let it rest. Give each other space to breathe, to grow, to miss each other. Absence makes the heart fonder, after all.</li>
                <li><strong>7.</strong> Bake at body temperature. Keep it warm, keep it close, keep it alive. Love needs consistency, not heat.</li>
                <li><strong>8.</strong> Serve fresh, every single day. Not just on birthdays or anniversaries. But on ordinary Tuesdays, in small gestures, in "I'm here" texts, in quiet mornings.</li>
            </ol>
            
            <div class="notes-section">
                <h2 style="color: var(--recipe-gold); margin-bottom: 20px;">Notes from the Chef:</h2>
                <div class="note-item" style="--rotate: -0.8deg;">· This recipe doesn't have an expiration date. It only gets better with time.</div>
                <div class="note-item" style="--rotate: 0.5deg;">· Best enjoyed with hands held, foreheads touching, and hearts wide open.</div>
                <div class="note-item" style="--rotate: -0.3deg;">· If it ever feels like it's not rising, just add more patience. It always does.</div>
            </div>
            
            <div class="secret-ingredient">
                <h2 style="color: var(--recipe-gold); margin-bottom: 20px;">A Special Ingredient Only for You, Ko Ko:</h2>
                <div class="secret-line">The secret ingredient?</div>
                <div class="secret-line">It's not in the recipe book.</div>
                <div class="secret-line">It's in the way you say my name.</div>
                <div class="secret-line">The way you remember the small things.</div>
                <div class="secret-line">The way you make me feel safe just by existing.</div>
                <div class="secret-line">That's the flavor no one else can replicate.</div>
            </div>
            
            <div class="recipe-closing">
                <p>This recipe belongs to us.</p>
                <p>Made with love, on March 25, 2026.</p>
                <p>Best shared for a lifetime. <span class="heart-pulse">💛</span></p>
            </div>
        </div>
        <button class="continue-btn" onclick="goToLettersPage()">Continue to Letters to Our Future Selves</button>
    </section>

    <!-- PAGE 12: LETTERS TO OUR FUTURE SELVES -->
    <section id="page-letters" style="display: none; padding: 60px 20px;">
        <div class="letters-container">
            <h1 class="letters-title">📮 Letters to Our Future Selves</h1>
            <p class="letters-subtitle">Sealed with love, to be opened when the time is right.</p>
            
            <div class="letter-card">
                <div class="letter-title">💌 Letter #1: To Be Opened on Our First Anniversary</div>
                <p>My Dearest Ko Ko,</p>
                <p>If you're reading this, we made it through one whole year. One year of learning each other, of figuring out what makes us laugh and what makes us cry. One year of choosing each other, even on the hard days.</p>
                <p>I hope by now, we've added more memories to our polaroid wall. I hope we've laughed until our stomachs hurt. I hope we've had at least one more trip somewhere new.</p>
                <p>But more than anything, I hope we're still the same us. Still patient. Still gentle. Still in awe of each other.</p>
                <p>I love you today, and I'll love you on the day you open this too.</p>
                <div class="letter-signature">— Your Babe</div>
            </div>
            
            <div class="letter-card">
                <div class="letter-title">🎓 Letter #2: To Be Opened When We Graduate</div>
                <p>Ko Ko,</p>
                <p>Look at us. We did it. All those late nights, all that stress, all those moments we wanted to give up, and we made it through.</p>
                <p>I remember when we first started dating, we talked about this day like it was so far away. But here we are. Graduates. Adults.</p>
                <p>Where are we going next? What's the first thing we're going to do? Are we celebrating by seeing each other, or are we already planning our next adventure?</p>
                <p>Whatever comes next, I'm glad I get to do it with you.</p>
                <div class="letter-signature">— Proud of us, Babe</div>
            </div>
            
            <div class="letter-card">
                <div class="letter-title">🏠 Letter #3: To Be Opened on the Day We Get Our First Home Together</div>
                <p>Our First Home.</p>
                <p>Ko Ko, say it out loud. Our home.</p>
                <p>Doesn't it sound beautiful?</p>
                <p>I wonder what it looks like. Is it small and cozy? Does it have big windows where morning light spills in? Is there a corner where we keep all our polaroids?</p>
                <p>This letter is coming from a version of me who can only imagine this day. But the version of me reading this? She's living it. She's standing in our home, probably barefoot, probably smiling, probably looking at you with the same heart eyes she's had since February 25, 2026.</p>
                <p>Welcome home, Ko Ko. We made it.</p>
                <div class="letter-signature">— Babe</div>
            </div>
            
            <div class="letter-card">
                <div class="letter-title">🌊 Letter #4: To Be Opened on a Day You Need to Remember</div>
                <p>If you're reading this, today might feel heavy.</p>
                <p>Stop. Breathe. Read this.</p>
                <p>Remember Ngwe Saung Night. Remember how calm the sea was. Remember sitting there, just the two of us, and feeling like the world could wait. That feeling still exists.</p>
                <p>Remember the day you gave me the promise ring. Remember how your hands were shaking a little. Remember how I didn't say anything for a moment because I was trying not to cry.</p>
                <p>Remember that I chose you. And I keep choosing you. Every single day.</p>
                <div class="letter-signature">— Your Babe</div>
            </div>
            
            <div class="letter-card">
                <div class="letter-title">💫 Letter #5: To Be Opened on Our 5th Anniversary</div>
                <p>Five years.</p>
                <p>That's 1,826 days of knowing you. 1,826 mornings of waking up grateful by seeing your messages. 1,826 nights of falling asleep by calling phone.</p>
                <p>I hope we're even stronger now than when I wrote this. I hope we've grown together, not apart. I hope you still make me laugh like you did on our first date.</p>
                <p>Here's to five more years. And then five more after that. And then infinity.</p>
                <div class="letter-signature">— Always yours, Babe</div>
            </div>
            
            <div class="letter-card">
                <div class="letter-title">💍 Letter #6: To Be Opened on Our Wedding Day (If We Choose That)</div>
                <p>Ko Ko,</p>
                <p>It's happening. Today, everyone we love is watching us. They're here to witness something I've known since February 25, 2026 — that you are the one.</p>
                <p>I've imagined this day so many times. In my head, you're always smiling. I'm always trying not to cry. And at the end of it all, we're just us — still the same people who started with a first message.</p>
                <p>I love you. I've loved you since before I even knew what that meant. And I'll love you in every version of our future.</p>
                <p>Let's go get married.</p>
                <div class="letter-signature">— Your Babe</div>
            </div>
            
            <div class="empty-envelope">
                <div class="letter-title">✉️ Letter #7: A Blank Envelope — For You to Write Back</div>
                <p>To My Ko Ko,</p>
                <p>This envelope is empty right now. But someday, I hope you'll write something back. A letter to me. To us. To whatever version of ourselves exists when you read this.</p>
                <p>Because our story isn't just mine to tell. It's ours.</p>
                <p>So here's your space. Your words. Your heart.</p>
                <p>I'll wait for it. I've always been good at waiting for you.</p>
                <div class="letter-signature">— Babe</div>
            </div>
            
            <div class="recipe-closing">
                <p>These letters are stored here, in this website, and also somewhere physical — because some things deserve to be held, touched, kept.</p>
                <p>Sealed with a promise. 💛</p>
                <p>March 25, 2026</p>
            </div>
        </div>
        <button class="continue-btn" onclick="goToPoem13()">Continue to For Your 22nd Birthday</button>
    </section>

    <!-- PAGE 13: FOR YOUR 22ND BIRTHDAY -->
    <section id="page-poem13" class="poem-page" style="display: none;">
        <div class="poem-card">
            <h1>🎂 For Your 22nd Birthday 🎂</h1>
            <div class="poem-date">March 25, 2026</div>
            <div class="poem-lines">
                <p>Twenty-two years of you.<br>Twenty-two years of the world<br>slowly becoming brighter<br>because you were in it.</p>
                <p>I'm sorry I wasn't there for all of them.<br>But I'm here now.<br>And I plan to be here<br>for every birthday after this.</p>
                <p>So here's to 22.<br>To the man you are becoming.<br>To the softness you carry,<br>the kindness you give,<br>the future you deserve.</p>
                <p>Happy birthday, my love.<br>This is only the beginning.</p>
            </div>
            <div class="heart-pulse" style="font-size: 2rem;">💛</div>
            <div class="nav-buttons">
                <button class="continue-btn" onclick="goToPoem14()">Next →</button>
            </div>
        </div>
    </section>

    <!-- PAGE 14: THE DAY YOU SPOKE FIRST -->
    <section id="page-poem14" class="poem-page" style="display: none;">
        <div class="poem-card">
            <h1>💬 The Day You Spoke First</h1>
            <div class="poem-date">August 15, 2025</div>
            <div class="poem-lines">
                <p>It started with a video I never meant to send.<br>Three days passed, and then —<br>your name lit up my screen again.</p>
                <p>Soft. Gentle. The way you've always been.</p>
                <p>I didn't know that quiet reply<br>would become the first page of our forever.<br>I didn't know that your words<br>would become the home I'd been searching for.</p>
                <p>You came back.<br>And I'm still not sure what I did to deserve that.<br>But I'm grateful.<br>Every single day, I'm grateful.</p>
            </div>
            <div style="font-size: 2rem;">💌</div>
            <div class="nav-buttons">
                <button class="continue-btn" onclick="goToPoem13()">← Previous</button>
                <button class="continue-btn" onclick="goToPoem15()">Next →</button>
            </div>
        </div>
    </section>

    <!-- PAGE 15: THE PROMISE RING -->
    <section id="page-poem15" class="poem-page" style="display: none;">
        <div class="poem-card">
            <h1>💍 The Promise Ring</h1>
            <div class="poem-date">February 25, 2026</div>
            <div class="poem-lines">
                <p>You gave me a ring.<br>Not because we needed proof.<br>But because you wanted me to know.</p>
                <p>That your hands, your heart, your tomorrows —<br>you wanted me to hold them.</p>
                <p>I wear it close,<br>like a secret only we understand.<br>A circle with no end.<br>Like us.</p>
            </div>
            <div style="font-size: 2rem;">💍</div>
            <div class="nav-buttons">
                <button class="continue-btn" onclick="goToPoem14()">← Previous</button>
                <button class="continue-btn" onclick="goToPoem16()">Next →</button>
            </div>
        </div>
    </section>

    <!-- PAGE 16: DISTANCE -->
    <section id="page-poem16" class="poem-page" style="display: none;">
        <div class="poem-card">
            <h1>✈️ Distance</h1>
            <div class="poem-date"></div>
            <div class="poem-lines">
                <p>Some days, the miles feel heavy.<br>I reach for you and find empty space.</p>
                <p>But then I remember:<br>we've crossed oceans before.<br>We found our way back once.<br>We'll do it again.</p>
                <p>Distance is just geography.<br>You are my home.<br>And home doesn't have an address.<br>Home is wherever you are.</p>
            </div>
            <div style="font-size: 2rem;">🌊</div>
            <div class="nav-buttons">
                <button class="continue-btn" onclick="goToPoem15()">← Previous</button>
                <button class="continue-btn" onclick="goToPoem17()">Next →</button>
            </div>
        </div>
    </section>

    <!-- PAGE 17: ALWAYS -->
    <section id="page-poem17" class="poem-page" style="display: none;">
        <div class="poem-card">
            <h1>♾️ Always</h1>
            <div class="poem-date"></div>
            <div class="poem-lines">
                <p>They say nothing lasts forever.<br>They haven't met us.</p>
                <p>I don't know what the future holds.<br>I don't know where we'll be in ten years,<br>or what challenges we'll face,<br>or who we'll become.</p>
                <p>But I know this:<br>I will find you.<br>In every version of every universe.<br>In every life.<br>In every moment that matters.</p>
                <p>You are my always.<br>And I am yours.</p>
            </div>
            <div class="heart-pulse" style="font-size: 2rem;">💛</div>
            <div class="nav-buttons">
                <button class="continue-btn" onclick="goToPoem16()">← Previous</button>
                <button class="continue-btn" onclick="goToTreePage()">Next →</button>
            </div>
        </div>
    </section>

    <!-- PAGE 18: A LOVE THAT GROWS - TREE OF US -->
    <section id="page-tree" class="tree-page" style="display: none;">
        <div class="tree-container">
            <h1 class="tree-title">🌳 A Love That Grows 🌳</h1>
            <p class="tree-subtitle">Rooted in Trust, Reaching for Forever</p>
            
            <div class="tree-section" id="rootsSection">
                <div class="tree-section-title">🌱 The Roots — Where We Began</div>
                <p style="margin-bottom: 20px; opacity: 0.8;">Deep in the soil, these roots hold us steady.</p>
                <ul class="tree-list">
                    <li>🌿 Patience — The kind that waited, listened, and never rushed</li>
                    <li>🌿 Understanding — To hold space for each other's quiet moments</li>
                    <li>💛 Forgiveness — Because we're human, and that's okay</li>
                    <li>🌿 Trust — The quiet knowing that we choose each other, always</li>
                    <li>🌿 Honesty — No masks, no pretense, just us</li>
                </ul>
            </div>
            
            <div class="tree-section" id="trunkSection">
                <div class="tree-section-title">🌲 The Trunk — What Holds Us Together</div>
                <div class="tree-quote">
                    "We grew not overnight, but slowly through first messages, accidental calls, and quiet mornings. Through distances crossed and tears wiped. Through choosing each other when it was easy and when it wasn't. This trunk is the story of us becoming we."
                </div>
            </div>
            
            <div class="tree-section" id="branchesSection">
                <div class="tree-section-title">🌿 The Branches — Where We're Going</div>
                <p style="margin-bottom: 20px; opacity: 0.8;">Reaching toward the light, toward tomorrow, toward everything we haven't become yet.</p>
                <ul class="tree-list">
                    <li>🌸 Branch of Adventure - Hong Kong streets, Japan's cherry blossoms, and every unknown road we'll walk together</li>
                    <li>🎓 Branch of Growth — Graduations, new careers, becoming who we're meant to be</li>
                    <li>🏠 Branch of Home — Our first place, our first kitchen, our first mornings waking up together</li>
                    <li>💛 Branch of Forever — The 5th anniversary, the 50th bouquet, the promise that never ends</li>
                </ul>
            </div>
            
            <div class="tree-section" id="leavesSection">
                <div class="tree-section-title">🍃 The Leaves — Our Memories</div>
                <p style="margin-bottom: 20px; opacity: 0.8;">Each leaf is a moment we've collected. Some are bright and golden. Some are soft and green. Some are still growing.</p>
                <ul class="tree-list">
                    <li class="leaf-memory">🍃 October 9, 2022 — Our first message</li>
                    <li class="leaf-memory">🍃 December 22, 2025 — Our first date</li>
                    <li class="leaf-memory">🍃 December 31, 2025 - January 3, 2026 — Our first trip</li>
                    <li class="leaf-memory">🍃 January 1, 2026 — Our first kiss</li>
                    <li class="leaf-memory">🍃 February 25, 2026 — The promise ring</li>
                    <li class="leaf-memory">🍃 March 12-15, 2026 — Ngwe Saung calm sea</li>
                    <li class="leaf-memory">🍃 March 25, 2026 — Your 22nd birthday</li>
                    <li class="leaf-memory">🍃 A blank leaf waiting for our next memory</li>
                </ul>
            </div>
            
            <div class="tree-section" id="flowersSection">
                <div class="tree-section-title">🌸 The Flowers — What Blooms When We're Together</div>
                <ul class="tree-list">
                    <li class="flower-item">🌸 Laughter — The kind that makes our stomachs hurt</li>
                    <li class="flower-item">🌸 Safety — The feeling of being known and still loved</li>
                    <li class="flower-item">🌸 Peace — The quiet of just existing next to you</li>
                    <li class="flower-item">🌸 Joy — Not loud, not demanding, just present</li>
                    <li class="flower-item">🌸 Wonder — Still being amazed by you, every day</li>
                </ul>
            </div>
            
            <div class="tree-section" id="fruitsSection">
                <div class="tree-section-title">🍎 The Fruits — What We'll Harvest One Day</div>
                <ul class="tree-list">
                    <li class="fruit-item">🍎 A Home — Walls that hold our story</li>
                    <li class="fruit-item">🍎 A Future — Dreams realized together</li>
                    <li class="fruit-item">🍎 A Legacy — A love that outlasts us</li>
                    <li class="fruit-item">🍎 A Lifetime — Of choosing each other</li>
                </ul>
            </div>
            
            <div class="tree-section" id="seedsSection">
                <div class="tree-section-title">🌱 The Seeds — What We Plant Today</div>
                <ul class="tree-list">
                    <li class="seed-item">🌱 A kind word when the day is heavy</li>
                    <li class="seed-item">🌱 A small surprise just because</li>
                    <li class="seed-item">🌱 A moment of presence without phones or distractions</li>
                    <li class="seed-item">🌱 A whispered "I love you" for no reason at all</li>
                    <li class="seed-item">🌱 A promise to keep growing, together</li>
                </ul>
            </div>
            
            <div class="tree-section" id="skySection">
                <div class="tree-section-title">✨ The Sky Above — What We Dream Of</div>
                <div class="tree-quote">
                    I don't know how tall this tree will grow. I don't know how many seasons we'll watch it change. But I know this: as long as we tend to it with patience, with laughter, with forgiveness, with love — it will never stop growing. And neither will we.
                </div>
            </div>
            
            <div class="tree-section" id="letterSection">
                <div class="tree-section-title">💌 A Letter to Our Tree</div>
                <div class="tree-quote">
                    <p>To the love we're building together,</p>
                    <p style="margin-top: 15px;">You started as something small. A seed planted by accident — a video I never meant to send, a reply I never expected. You grew slowly, quietly, when we weren't looking.</p>
                    <p style="margin-top: 15px;">You survived seasons of silence and distance. You stretched toward the light even when the sky felt far away. You grew roots deep enough to hold us steady through storms I never thought we'd weather.</p>
                    <p style="margin-top: 15px;">And now, look at you. Look at us.</p>
                    <p style="margin-top: 15px;">You're not just a tree anymore. You're a forest of memories, of promises, of quiet mornings and loud laughter. You're the shade we rest under when the world is too much. You're the branches we reach toward when we dream.</p>
                    <p style="margin-top: 15px;">I promise to water you. To protect your roots. To celebrate every new leaf, every blossom, every fruit. To never let you grow alone.</p>
                    <p style="margin-top: 20px;">With you, I am growing too.</p>
                    <p style="margin-top: 30px; text-align: right;">— Your Babe</p>
                </div>
            </div>
            
            <div class="tree-quote" style="margin-bottom: 40px;">
                A tree doesn't grow in a day.<br>
                It grows slowly, quietly, steadily<br>
                in soil prepared with patience,<br>
                under skies of hope,<br>
                with roots that hold and branches that reach.<br><br>
                That's us.<br>
                That's our love.<br>
                And it's only just beginning to grow.
            </div>
        </div>
        <div class="nav-buttons" style="justify-content: center; margin-top: 20px;">
            <button class="continue-btn" onclick="goToLanguagesPage()">Continue to I love you →</button>
        </div>
    </section>

    <!-- PAGE 19: I LOVE YOU IN MANY LANGUAGES -->
    <section id="page-languages" class="languages-page" style="display: none;">
        <div class="world-map-bg"></div>
        <div class="languages-container">
            <h1 class="languages-title">🌍 I Love You in Many Languages 🌍</h1>
            <p class="languages-subtitle">How My Heart Says It — Across Borders, Across Worlds, Always to You</p>
            
            <div class="heart-center">
                <div class="glowing-heart">💛</div>
                <p style="margin-top: 10px;">Words travel from everywhere in the world... all leading to you.</p>
            </div>
            
            <div class="language-card" data-lang="burmese">
                <div><span class="language-flag">🇲🇲</span> <strong>Burmese</strong></div>
                <div class="language-phrase">ချစ်တယ်</div>
                <div class="language-meaning">"It started here. In our mother tongue. In the language of our first message, our first 'I love you,' our first everything. These words feel like home because they came from you."</div>
            </div>
            
            <div class="language-card" data-lang="english">
                <div><span class="language-flag">🇬🇧</span> <strong>English</strong></div>
                <div class="language-phrase">I Love You</div>
                <div class="language-meaning">"The words I say when I'm looking at you and can't find anything more complicated to say. Sometimes the simplest words carry the most weight."</div>
            </div>
            
            <div class="language-card" data-lang="japanese">
                <div><span class="language-flag">🇯🇵</span> <strong>Japanese</strong></div>
                <div class="language-phrase">愛してる (Aishiteru)</div>
                <div class="language-meaning">"A word they don't say often, because it carries everything. That's how I feel about you. Every time I say it, I mean all of it."</div>
            </div>
            
            <div class="language-card" data-lang="french">
                <div><span class="language-flag">🇫🇷</span> <strong>French</strong></div>
                <div class="language-phrase">Je t'aime</div>
                <div class="language-meaning">"The language of romance. But to me, it sounds like late-night phone calls and the way you say my name when you're tired. Soft. Warm. Real."</div>
            </div>
            
            <div class="language-card" data-lang="italian">
                <div><span class="language-flag">🇮🇹</span> <strong>Italian</strong></div>
                <div class="language-phrase">Ti amo</div>
                <div class="language-meaning">"A love that feels like sunshine and pasta and laughing until we can't breathe. I imagine us saying this in a tiny Italian café someday. Just a dream for now. But dreams come true, right?"</div>
            </div>
            
            <div class="language-card" data-lang="spanish">
                <div><span class="language-flag">🇪🇸</span> <strong>Spanish</strong></div>
                <div class="language-phrase">Te amo / Te quiero</div>
                <div class="language-meaning">"One for the deep, forever kind. One for the everyday, I-choose-you kind. Both belong to you."</div>
            </div>
            
            <div class="language-card" data-lang="portuguese">
                <div><span class="language-flag">🇵🇹</span> <strong>Portuguese</strong></div>
                <div class="language-phrase">Eu te amo</div>
                <div class="language-meaning">"The words sound like waves: soft, endless, reaching for the shore. Like our love. Like you."</div>
            </div>
            
            <div class="language-card" data-lang="german">
                <div><span class="language-flag">🇩🇪</span> <strong>German</strong></div>
                <div class="language-phrase">Ich liebe dich</div>
                <div class="language-meaning">"Like the way you hold my hand when I'm overthinking. Like the way you stay."</div>
            </div>
            
            <div class="language-card" data-lang="dutch">
                <div><span class="language-flag">🇳🇱</span> <strong>Dutch</strong></div>
                <div class="language-phrase">Ik hou van jou</div>
                <div class="language-meaning">"Warm like knitted sweaters and hot chocolate. Comforting. Familiar. Home."</div>
            </div>

            <div class="language-card" data-lang="swedish">
                <div><span class="language-flag">🇸🇪</span> <strong>Swedish</strong></div>
                <div class="language-phrase">Jag älskar dig</div>
                <div class="language-meaning">"Quiet. Gentle. Like snow falling. Like the peace I feel when I'm with you."</div>
            </div>

            <div class="language-card" data-lang="finnish">
                <div><span class="language-flag">🇫🇮</span> <strong>Finnish</strong></div>
                <div class="language-phrase">Minä rakastan sinua</div>
                <div class="language-meaning">"Long. Patient. Worth saying fully. Like our story."</div>
            </div>

            <div class="language-card" data-lang="russian">
                <div><span class="language-flag">🇷🇺</span> <strong>Russian</strong></div>
                <div class="language-phrase">Я тебя люблю (Ya tebya lyublyu)</div>
                <div class="language-meaning">"Dramatic, passionate, like something from a novel. But when I say it, I mean it simply: I love you. That's all."</div>
            </div>

            <div class="language-card" data-lang="korean">
                <div><span class="language-flag">🇰🇷</span> <strong>Korean</strong></div>
                <div class="language-phrase">사랑해 (Saranghae)</div>
                <div class="language-meaning">"The way K-dramas taught me this word. But you taught me what it means. There's a difference."</div>
            </div>
            
            <div class="language-card" data-lang="chinese">
                <div><span class="language-flag">🇨🇳</span> <strong>Chinese (Mandarin)</strong></div>
                <div class="language-phrase">我爱你 (Wǒ ài nǐ)</div>
                <div class="language-meaning">"Pronounced with intention. Written with care. Felt in the heart."</div>
            </div>
            
            <div class="language-card" data-lang="thai">
                <div><span class="language-flag">🇹🇭</span> <strong>Thai</strong></div>
                <div class="language-phrase">ฉันรักคุณ (Chan rak khun)</div>
                <div class="language-meaning">"Sweet like mango sticky rice. Soft like the way you look at me when you think I'm not watching."</div>
            </div>
            
            <div class="language-card" data-lang="vietnamese">
                <div><span class="language-flag">🇻🇳</span> <strong>Vietnamese</strong></div>
                <div class="language-phrase">Anh yêu em / Em yêu anh</div>
                <div class="language-meaning">"Careful, gentle, chosen. Like every word we've ever exchanged."</div>
            </div>
            
            <div class="language-card" data-lang="greek">
                <div><span class="language-flag">🇬🇷</span> <strong>Greek</strong></div>
                <div class="language-phrase">Σ' αγαπώ (S'agapó)</div>
                <div class="language-meaning">"Ancient and eternal. Like the love I want to have with you — timeless."</div>
            </div>

            <div class="language-card" data-lang="hebrew">
                <div><span class="language-flag">🇮🇱</span> <strong>Hebrew</strong></div>
                <div class="language-phrase">אני אוהב אותך (Ani ohev otach)</div>
                <div class="language-meaning">"A language of prayer and poetry. When I say this, I'm praying you'll always know how much you mean to me."</div>
            </div>

            <div class="language-card" data-lang="turkish">
                <div><span class="language-flag">🇹🇷</span> <strong>Turkish</strong></div>
                <div class="language-phrase">Seni seviyorum</div>
                <div class="language-meaning">"Musical, warm, like tea shared on a quiet afternoon. That's us."</div>
            </div>

            <div class="language-card" data-lang="polish">
                <div><span class="language-flag">🇵🇱</span> <strong>Polish</strong></div>
                <div class="language-phrase">Kocham cię</div>
                <div class="language-meaning">"Short. Simple. To the point. Like the way I know, without a doubt, that you are the one."</div>
            </div>

            <div class="language-card" data-lang="icelandic">
                <div><span class="language-flag">🇮🇸</span> <strong>Icelandic</strong></div>
                <div class="language-phrase">Ég elska þig</div>
                <div class="language-meaning">"Like something out of a fairytale. And loving you does feel like magic sometimes."</div>
            </div>

            <div class="language-card" data-lang="arabic">
                <div><span class="language-flag">🇦🇪</span> <strong>Arabic</strong></div>
                <div class="language-phrase">أحبك (Uhibbuka)</div>
                <div class="language-meaning">"Written from right to left, like reading a heart backward — but the meaning is always forward. Always toward you."</div>
            </div>
            
            <div class="languages-closing">
                <p style="font-size: 1.2rem;">Sealed with a thousand hearts,</p>
                <p style="font-family: 'Dancing Script', cursive; font-size: 1.5rem; margin-top: 15px;">Your Babe</p>
                <p style="margin-top: 10px;">March 25, 2026 — ∞</p>
                <div class="heart-center" style="margin-top: 20px;">
                    <span class="glowing-heart">💛</span>
                </div>
            </div>
        </div>
        <div class="nav-buttons" style="justify-content: center; margin-top: 20px;">
            <button class="continue-btn" onclick="goToTreePage()">← Previous</button>
            <button class="continue-btn" onclick="goToApologyPage()">Continue to Page 20 →</button>
        </div>
    </section>

    <!-- PAGE 20: THE APOLOGY I OWE YOU -->
    <section id="page-apology" class="apology-page" style="display: none;">
        <div class="apology-container">
            <h1 class="apology-title">The Apology I Owe You</h1>
            <p class="apology-date">March 2026</p>
            <div class="apology-letter" id="apologyLetter">
                <p>I'm sorry for the days I was distant.</p>
                <p>Not because of anything you did. Never because of you. But because I was fighting battles inside myself that I didn't know how to explain. There were storms in my head that I couldn't name, and instead of letting you in, I built walls. Not to keep you out but because I didn't want you to see me breaking.</p>
                <p>I'm sorry for the times I made you guess what I was feeling.</p>
                <p>I know you tried. I know you waited. I know you gave me space when I asked for it and reached for me when I couldn't reach for myself. But you shouldn't have to decode me. You shouldn't have to piece together my silence to understand what I couldn't say.</p>
                <p>I'm sorry for not always letting you in.</p>
                <p>There were nights I wanted to tell you everything… the weight, the doubt, the exhaustion that had nothing to do with us. But the words felt too heavy. Too messy. Too much. So I stayed quiet, and you stayed patient, and I hate that I made you wait.</p>
                <p>You've never made me feel like I had to earn your love.</p>
                <p>That's what breaks my heart the most. You loved me in my quiet. You loved me in my distance. You loved me when I couldn't even love myself. And instead of letting that heal me, I let it confuse me, wondering how someone could be so gentle with a heart that felt so hard to hold.</p>
                <p>But sometimes I still try. I try to be perfect. I try to be enough. I try to shrink the parts of me that feel too complicated, too emotional, too much. I try to earn the love you already give freely. And I'm sorry for that too. Because I know it must hurt to watch someone you love search for something you've already handed them.</p>
                <p>I'm working on being softer with myself. Not for you though you deserve the softest version of me. But for me. Because I want to stop being a stranger to my own heart. I want to stop treating myself like I need to prove something to be worthy of you.</p>
                <p>Because you deserve the version of me that isn't afraid to be seen. The version that doesn't hide behind silence. The version that lets you hold me when I'm breaking. The version that says "I need you" instead of "I'm fine."</p>
                <p>You've always seen me clearly. Even when I tried to blur the edges. You've always stayed. Even when I gave you reasons to leave. You've always loved me. Even when I forgot how to love myself.</p>
                <p>And I'm sorry it took me this long to say it. I'm sorry for the days I made you wonder. I'm sorry for the nights I made you wait. I'm sorry for the times I made your love feel like something I had to earn instead of something I could just receive.</p>
                <p>I'm sorry for the distance. But I'm not sorry for us. Not a single moment. Not a single storm. Because every time I pulled away, you taught me that love doesn't leave. It waits. It breathes. It stays.</p>
                <p>I'm learning to stay too. Not just with you but with myself. I'm learning that being seen isn't something to fear. That my complicated parts don't scare you. That you want all of me not just the easy parts.</p>
                <p>So this is my apology. Not just for what I did. But for what I kept from you. The weight. The doubt. The storms I thought I had to carry alone. I'm putting them down now. Right here. Between us.</p>
                <p>Because you've always been the one I could fall into. I just forgot how to let myself. Thank you for catching me anyway. Every single time.</p>
                <p>I'm sorry isn't enough. But it's where I start. And I'm starting here with this letter, with this honesty, with this version of me that isn't afraid to say: I need you. I want you. I choose you. And I'm sorry for the days I made you doubt that.</p>
                <p style="text-align: right; margin-top: 30px;">— Your Babe<br>March 2026</p>
            </div>
        </div>
        <div class="nav-buttons" style="justify-content: center; margin-top: 20px;">
            <button class="continue-btn" onclick="goToLanguagesPage()">← Previous</button>
            <button class="continue-btn" onclick="goToReceiptPage()">Continue to Page 21 →</button>
        </div>
    </section>

    <!-- PAGE 21: THE RECEIPT OF LOVE -->
    <section id="page-receipt" class="receipt-page" style="display: none;">
        <div class="receipt-wrapper">
            <div class="receipt-paper" id="receiptPaper">
                <div class="receipt-content">
                    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━<br>
   THE OFFICIAL LOVE RECEIPT

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━<br>
  ITEMS PURCHASED:

─────────────────────────────<br>
 1 x Heart (original owner: you)
                          [FREE]
<br>
 1 x Promise Ring         [PRICELESS]
<br>
 ∞ x Kisses          [UNLIMITED]
<br>
 365 x Good morning texts  
                 [COMPLIMENTARY]
<br>
 22 x Birthday wishes [INCLUDED]
<br>
 1000 x "I miss you" voice notes 
                  [ON BACKORDER]
<br>
 1 x Babe who is always right 
                      [DISPUTED]
<br>
─────────────────────────────<br>                                      
 SUBTOTAL: All of my love
 TAX: Your cuteness
 SHIPPING: Distance(temporary)
 TOTAL: FOREVER
<br>
─────────────────────────────<br>                               
 PAYMENT METHOD: Your smile
 RECEIPT NUMBER: #02252026    
 CASHIER: Your Babe

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━<br>
  NO RETURNS. NO EXCHANGES.
    VALID FOR A LIFETIME.
                    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                </div>
                <div class="receipt-tear"></div>
            </div>
            <div class="receipt-stamp" id="receiptStamp">PAID ✓</div>
        </div>
        <div class="nav-buttons" style="justify-content: center; margin-top: 20px;">
            <button class="continue-btn" onclick="goToApologyPage()">← Previous</button>
            <button class="continue-btn" onclick="goToEndingPage()">Continue to Page 22 →</button>
        </div>
    </section>

    <!-- PAGE 22: THE ENDING -->
    <section id="page-ending" class="ending-page" style="display: none;">
        <div class="ending-container">
            <div class="ending-opening">
                "Every story has an ending. But ours? Ours is just learning how to begin again."
            </div>
            
            <div class="numbers-grid" id="numbersGrid">
                <div class="number-item"><div class="number-value" id="num1">0</div><div>first message that started everything</div></div>
                <div class="number-item"><div class="number-value" id="num3">0</div><div>days you waited to reply</div></div>
                <div class="number-item"><div class="number-value" id="num22a">0</div><div>wishes I whispered for you</div></div>
                <div class="number-item"><div class="number-value" id="num22b">0</div><div>reasons I love you</div></div>
                <div class="number-item"><div class="number-value" id="num19">0</div><div>pages of words I never knew I had</div></div>
                <div class="number-item"><div class="number-value" id="numInf">∞</div><div>the number of times I'll choose you</div></div>
            </div>
            
            <div class="handwritten-letter" id="handwrittenLetter">
                <p><strong>My Ko Ko,</strong></p>
                <p>You made it to the end.</p>
                <p>If you're reading this, you've scrolled through 22 pages of my heart. You've read my unspoken apologies, my silly jokes, my deepest fears, my brightest hopes. You've seen the parts of me I don't show anyone else.</p>
                <p>And you're still here.</p>
                <p>That means more than I can put into words.</p>
                <p>This website started as a birthday gift. But somewhere along the way, it became something else. A time capsule. A love letter. A place where I could leave all the words I never knew how to say.</p>
                <p>I don't know where we'll be when you read this again. Maybe we're together. Maybe we're apart. Maybe we're somewhere in between.</p>
                <p>But this page will always exist. These words will always be true.</p>
                <p>I love you. Not because it's easy. Not because it's convenient. Not because we're perfect.</p>
                <p>I love you because you make me want to be brave. You make me want to be softer. You make me want to write 22 pages just to tell you what I feel.</p>
                <p>You make me want to try.</p>
                <p>And that's more than I ever thought I deserved.</p>
                <p style="text-align: right; margin-top: 20px;">— Your Babe</p>
            </div>
            
            <div class="tree-mini">
                🌳
                <div class="golden-leaf">🍂</div>
            </div>
            <p>Our tree doesn't end here.<br>It keeps growing.<br>In the spaces between our calls.<br>In the quiet mornings.<br>In the laughter we haven't laughed yet.</p>
            
            <div class="constellation-mini" id="constellationMini">
                <span class="star-mini" style="top: 0; left: 20%;">✨</span>
                <span class="star-mini" style="top: 30px; left: 50%;">✨</span>
                <span class="star-mini" style="top: 60px; left: 80%;">✨</span>
                <span class="star-mini" style="top: 100px; left: 10%;">✨</span>
                <span class="star-mini" style="top: 120px; left: 40%;">✨</span>
            </div>
            <p>They say stars take years to reach us.<br>Maybe that's why loving you feels so ancient.<br>Like something that's always been true.<br>Like something that was waiting to arrive.</p>
            
            <div class="final-line">"You were never just a page in my story. You're the reason I kept writing."</div>
            
            <div class="final-words">
                ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━<br>
                This website has 22 pages.<br>
                But our story has no end.<br><br>
                I don't know what comes next.<br>
                I don't know where we'll be in a year.<br>
                I don't know if we'll laugh at these pages<br>
                or cry at them or both.<br><br>
                But I know this:<br><br>
                Every word here is true.<br>
                Every feeling is real.<br>
                Every page was written for you.<br><br>
                Thank you for reading.<br>
                Thank you for staying.<br>
                Thank you for being you.<br>
                ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━<br><br>
                With all the love I have,<br>
                Your Babe<br><br>
                March 25, 2026 — ∞<br>
                ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
            </div>
            
            <button class="star-button" id="starButton">✨ LEAVE A STAR FOR US ✨</button>
            <div class="star-counter" id="starCounter">⭐ 0 stars left for our story</div>
            
            <div class="hidden-message" id="hiddenMessageTrigger">.</div>
        </div>
        <div class="secret-text" id="secretText">Psst. If you're reading this, you really did read everything. Thank you for caring enough to notice the small things. That's why I love you.</div>
        <div class="nav-buttons" style="justify-content: center; margin-top: 20px;">
            <button class="continue-btn" onclick="goToReceiptPage()">← Previous</button>
            <button class="continue-btn" onclick="goToEndPage()">Finish →</button>
        </div>
    </section>

    <!-- END PAGE -->
    <section id="page-end" style="display: none;">
        <div class="end-card">
            <div style="font-size: 4rem; margin-bottom: 20px; animation: heartBeat 1.5s infinite;">❤️</div>
            <p style="margin: 20px 0; font-size: 1rem; line-height: 1.8;">This isn't an end.<br>It's just another beginning.</p>
            <p style="margin: 30px 0; font-style: italic;">Thank you for every page we've written together.<br>Here's to all the pages we haven't written yet.</p>
            <p style="margin-top: 40px;">Always,<br>Your Babe</p>
            <p style="margin-top: 20px; font-size: 0.8rem; opacity: 0.6;">March 25, 2026 — ∞</p>
        </div>
    </section>

    <script>
        // Starfield Canvas
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        
        function initCanvas() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
        
        class Particle { 
            constructor() { this.reset(); } 
            reset() { 
                this.x = Math.random() * canvas.width; 
                this.y = Math.random() * canvas.height; 
                this.size = Math.random() * 2 + 0.5; 
                this.speed = Math.random() * 1.5 + 0.3; 
            } 
            update() { 
                this.y += this.speed; 
                if (this.y > canvas.height) { 
                    this.y = -5; 
                    this.x = Math.random() * canvas.width; 
                } 
            } 
            draw() { 
                ctx.fillStyle = 'rgba(255, 255, 255, 0.8)'; 
                ctx.beginPath(); 
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2); 
                ctx.fill(); 
            } 
        }
        
        function animate() { 
            ctx.clearRect(0, 0, canvas.width, canvas.height); 
            particles.forEach(p => { p.update(); p.draw(); }); 
            requestAnimationFrame(animate); 
        }
        
        initCanvas();
        for (let i = 0; i < 120; i++) particles.push(new Particle());
        animate();
        window.addEventListener('resize', () => { initCanvas(); });

        // Locker
        document.getElementById('passcode').addEventListener('input', (e) => {
            if(e.target.value === '25032004') {
                document.getElementById('page-lock').style.opacity = '0';
                setTimeout(() => {
                    document.getElementById('page-lock').style.display='none';
                    document.getElementById('page-timeline').style.display='block'; 
                }, 1500);
            }
        });

        // Timeline Data
        const timelineMemories = [
            { date: "2022 — The Beginning", text: "Babe met Ko Ko as the class admin of our C3. That was the starting point… the place where our connection quietly began.", side: "left", top: 80, glow: false },
            { date: "October 9, 2022", text: "Our first message. Of course, Babe was the one who texted first just to send Ko Ko an appreciation message for being our admin. It was such a small message, but now it feels like the first page of us.", side: "right", top: 330, glow: false },
            { date: "2023-2025", text: "Babe is really sorry for everything that happened during that time. Babe never meant to hurt Ko Ko's feelings. I also don't know how to show my guilt or how to explain what I'm feeling inside. Babe is really sorry, Ko Ko. Please forgive me ☹️", side: "left", top: 600, glow: false },
            { date: "August 15, 2025", text: "Babe accidentally sent a piano practice video on Snapchat. Three days later, Ko Ko replied to me. Babe still remembers how surprised babe felt when babe saw Ko Ko's replies. It was soft and gentle, just like Ko Ko always is. Babe also felt guilty for everything babe had done before.", side: "right", top: 880, glow: false },
            { date: "December 9, 2025", text: "My first rose bouquet and birthday cake from you, Ko Ko. That day felt very special to babe more than babe expected.", side: "left", top: 1130, glow: false },
            { date: "December 22, 2025", text: "Our first date. The first time Ko Ko and Babe met in person. It didn't feel awkward at all. It just felt funny and warmth.", side: "right", top: 1380, glow: false },
            { date: "December 31, 2025 – January 3, 2026", text: "Our first trip together Pyin Oo Lwin. That was the moment babe felt that this relationship was becoming something deeper.", side: "left", top: 1630, glow: false },
            { date: "January 1, 2026", text: "Our first kiss. A quiet memory babe will always keep in my heart.", side: "right", top: 1880, glow: false },
            { date: "January 16, 2026", text: "A memory that only Ko Ko and Babe understand 👀", side: "left", top: 2080, glow: false },
            { date: "January 20, 2026", text: "My first appreciation post for Ko Ko on my spam account. A small way of saying thank you for everything Ko Ko does for me.", side: "right", top: 2280, glow: false },
            { date: "February 11 – 15, 2026", text: "Meeting again in Yangon. Every moment with you felt peaceful and comfortable.", side: "left", top: 2480, glow: false },
            { date: "February 14, 2026", text: "Our first Valentine's Day together. We even did a street interview, and we laughed so much that day. It's one of the memories babe will never forget.", side: "right", top: 2730, glow: false },
            { date: "February 18, 2026", text: "The second bouquet from Ko Ko. Ko Ko always shows love in such a quiet and sincere way. Hee hee", side: "left", top: 2980, glow: false },
            { date: "February 24 – 28, 2026", text: "Our Mandalay trip. More memories, more understanding, and more moments that made me feel closer to you, Ko Ko.", side: "right", top: 3230, glow: false },
            { date: "💗 February 25, 2026 💗", text: "✨ The day Ko Ko confessed to babe. The day babe received her promise ring. And the day babe got the third bouquet from Ko Ko. ✨", side: "center", top: 3520, glow: true },
            { date: "March 12 – 15, 2026", text: "Our second trip Ngwe Saung. Calm sea, calm days, and calm love.", side: "left", top: 3730, glow: false },
            { date: "🎂 March 25, 2026 🎂", text: "🎈 Ko Ko's 22nd birthday. Ko Ko and Babe's first mensiversary. And this website belongs to Ko Ko. 🎈", side: "center", top: 4030, glow: true }
        ];

        function buildTimeline() {
            const container = document.getElementById('timelineContainer');
            container.innerHTML = '<div class="timeline-line"></div>';
            timelineMemories.forEach((m, i) => {
                const star = document.createElement('div');
                star.className = 'star-point' + (m.glow ? ' glow-bright' : '');
                star.style.top = m.top + 'px';
                star.setAttribute('onclick', `reveal(${i})`);
                container.appendChild(star);
                const box = document.createElement('div');
                box.id = `m${i}`;
                let sideClass = m.side;
                if (m.side === 'center') sideClass = 'center-spec';
                box.className = `memory-box ${sideClass}`;
                box.style.top = m.top + 'px';
                box.innerHTML = `<div class="date-label">${m.date}</div><p>${m.text}</p>`;
                container.appendChild(box);
            });
            container.style.minHeight = (timelineMemories[timelineMemories.length-1].top + 200) + 'px';
        }
        function reveal(id) { const box = document.getElementById(`m${id}`); if (box) box.classList.toggle('active'); }
        buildTimeline();

        // Birthday Content with line-by-line transition
        const birthdayParagraphs = [
            "Today isn't just your birthday. It's the day the person I love was born.",
            "Happy 22nd birthday, honey. My babe boo finally turns 22 and becomes an adult, but for me, you're still the same cutie pie I fell for. And I love you so, so much.",
            "NGL, Ko Ko, you're super duper cute, and I keep falling for your cuteness again and again, every single day without even realizing it.",
            "Thank you for entering my life for the second time. I'm really grateful for our connection because it feels like something that was meant to come back, not something temporary. I really don't want to lose you, Ko Ko. We already missed each other once, right? I don't want that to happen again. That's why I decided to fix and build this relationship as much as I can, no matter what happens.",
            "One of my biggest blessings in 2026 is having you. Thank you for being my boyfriend and for always understanding me, even when I'm silly, emotional, or overthinking. You still stay, you still comfort me, and you still try to make me smile. I truly can't thank you enough for everything you've done for me.",
            "I hope this year gives you everything you deserve: happiness, success, confidence, peace of mind, and all the things you've been quietly praying for. I hope your dreams come closer to you step by step, and I hope life becomes softer and kinder to you this year.",
            "I also hope you always stay the same kind, gentle, and patient person that made me fall for you in the first place. Please don't let the world change your heart too much, because your kindness is something really rare 👀",
            "Babes loves Ko Ko so so much more than I can explain properly and more than words can ever show.",
            "And once again, happy 22nd birthday, my cutie honey 🥳💕"
        ];
        
        function buildBirthday() { 
            const container = document.getElementById('bdayContent'); 
            container.innerHTML = '';
            birthdayParagraphs.forEach((p, i) => { 
                const para = document.createElement('p'); 
                para.textContent = p; 
                container.appendChild(para); 
                setTimeout(() => {
                    para.style.opacity = '1';
                    para.style.transform = 'translateY(0)';
                }, i * 500 + 500);
            }); 
        }
        buildBirthday();
        
        function openBday() { 
            document.getElementById('page-timeline').style.opacity = '0'; 
            setTimeout(() => { 
                document.getElementById('page-timeline').style.display = 'none'; 
                document.getElementById('page-birthday').style.display = 'block'; 
                setTimeout(() => document.getElementById('to-wishes-btn').style.display = 'block', birthdayParagraphs.length * 500 + 1000);
            }, 1000); 
        }

        // Wishes
        const wishes = ["Health: I wish for you to stay strong and healthy so we can go on many more trips together.","Success: I wish for all your hard work to turn into the success you've been dreaming of.","Peace of Mind: I wish for your mind to be calm and free from any worries or stress.","Happiness: I wish for your smile to stay as bright and genuine as it is every day.","Confidence: I wish for you to always see yourself through my eyes—perfect and capable.","Wealth: I wish for abundance and financial ease in everything you do.","Safe Travels: I wish for every journey we take—like Ngwe Saung and Mandalay—to be safe and beautiful.","Good Sleep: I wish for you to always have peaceful nights and sweet dreams.","Kind People: I wish for the world to be as kind to you as you are to everyone else.","Strength: I wish for you to have the courage to overcome any challenge life throws at you.","Our Love: I wish for 'us' to grow stronger and deeper with every passing month.","Understanding: I wish for us to always find our way back to each other through any misunderstanding.","Patience: I wish for the same gentle patience you show me to always stay in your heart.","Joyful Mornings: I wish for you to wake up every morning feeling excited about the day.","Protection: I wish for you to be protected from any harm or negative energy.","Achieving Dreams: I wish for every small goal you've set for age 22 to come true.","Laughter: I wish for more days like our Valentine's street interview—full of pure laughter.","No Overthinking: I wish for your heart to be so full of love that there's no room for doubt.","Comfort: I wish for my arms to always be the place where you feel most at home.","Bright Future: I wish for your path to be clear and lit with many opportunities.","Growth: I wish for us to grow up together, side by side, as better versions of ourselves.","Forever: My final wish is that I get to celebrate every single birthday with you for the rest of our lives."];
        const wishIcons = ['💪','🚀','🌊','☀️','💎','💰','✈️','🌙','🤝','🏔️','🫂','🧩','⏳','🌄','🛡️','🎯','😂','🧠','🏠','✨','🌿','♾️'];
        
        function buildWishes() { 
            const container = document.getElementById('wishesContainer'); 
            wishes.forEach((w, i) => { 
                const div = document.createElement('div'); 
                div.className = 'wish-item'; 
                div.innerHTML = `<span class="wish-num">${String(i+1).padStart(2,'0')}</span> <span class="wish-icon">${wishIcons[i]}</span> ${w}`; 
                container.appendChild(div); 
            }); 
            const observer = new IntersectionObserver((entries) => { entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('reveal'); }); }, { threshold: 0.2 });
            document.querySelectorAll('.wish-item').forEach(it => observer.observe(it));
        }
        buildWishes();
        
        function goToPage4() { 
            document.getElementById('page-birthday').style.opacity = '0'; 
            setTimeout(() => { 
                document.getElementById('page-birthday').style.display = 'none'; 
                document.getElementById('page-4').style.display = 'block'; 
                window.scrollTo(0, 0); 
                setTimeout(() => document.getElementById('page-4').style.opacity = '1', 100); 
            }, 1000); 
        }
        
        // Reasons
        const reasons = ["The way you look at me.", "The way you hold my hand.", "The way you make me feel safe.","The way you respect my feelings.", "The way you say my name.", "Your warm hugs.","The way you try to make me laugh.", "Your kindness to everyone.", "The way you remember small details.","The way you are my 'home.'", "the way you smiles", "the way you speak","the way you treat", "the way you text back", "the way you give me attention","the way you sleep in my chest", "the way you care", "your patience","your childish version", "your mature version", "your voice","Simply because you are You."];
        
        function buildReasons() { 
            const container = document.getElementById('reasonsContainer'); 
            const line = document.createElement('div'); 
            line.className = 'constellation-line'; 
            line.id = 'constellationLine'; 
            container.appendChild(line); 
            reasons.forEach((r, i) => { 
                const div = document.createElement('div'); 
                div.className = 'reason-item'; 
                div.setAttribute('data-index', i); 
                div.innerHTML = `<div class="reason-star"></div><div class="reason-text"><span class="reason-icon">✨</span> ${r}</div>`; 
                container.appendChild(div); 
            }); 
        }
        buildReasons();
        
        function goToPage5() { 
            document.getElementById('page-4').style.opacity = '0'; 
            setTimeout(() => { 
                document.getElementById('page-4').style.display = 'none'; 
                const pg5 = document.getElementById('page-5'); 
                pg5.style.display = 'block'; 
                window.scrollTo(0, 0); 
                setTimeout(() => { 
                    pg5.style.opacity = '1'; 
                    const reasonsItems = document.querySelectorAll('.reason-item'); 
                    const line = document.getElementById('constellationLine'); 
                    let revealedCount = 0; 
                    const observer = new IntersectionObserver((entries) => { 
                        entries.forEach(entry => { 
                            if (entry.isIntersecting && !entry.target.classList.contains('revealed')) { 
                                entry.target.classList.add('revealed'); 
                                revealedCount++; 
                                line.style.height = (revealedCount / reasonsItems.length) * 100 + '%'; 
                            } 
                        }); 
                    }, { threshold: 0.3 }); 
                    reasonsItems.forEach(reason => observer.observe(reason)); 
                }, 100); 
            }, 1000); 
        }
        
        // Always Messages
        const alwaysMessages = ["Ko Ko, if the world feels too loud or work feels too heavy today, just stay here for a moment. Close your eyes and imagine we are back at Ngwe Saung, watching the calm sea together. I am right here, holding your hand. You don't have to be strong every second with me, you can just be you. I've got you, always.","In case no one told you today: I am so incredibly proud of the man you are becoming. I see how hard you work and how much you care about the things you do. You are capable of everything you've been praying for. Don't let a single bad day make you forget how much potential you have. You're doing great, honey.","Are you missing me? Because I'm definitely missing you. Whenever we are apart, remember our first date on December 22nd. Remember how it is happy and excited day. That's because our hearts already knew each other. We've found our way back to each other once we will always find our way back, no matter the distance.","If you ever look in the mirror and doubt yourself, please try to see yourself through my eyes for just one minute. You would see someone kind, patient, and so handsome. You would see the person who made me fall in love all over again just by being himself. You are more than enough, exactly as you are.","Hey, cutie pie! This is your reminder that you have the most infectious smile and the cutest laugh (especially that day of our street interview!). Don't let the 'adult' world take away your 'childish' side that I love so much. Take a break, eat something yummy, and remember that Babe loves you super duper much!","It didn't happen suddenly; it happened honestly. And my promise to you is just as honest. No matter what challenges come our way in 2026 and beyond, I choose you. I choose to fix things, to build with you, and to grow with you. My heart has found its home in you, and I'm not going anywhere.","I know that brain of yours likes to overthink sometimes. But here is the only truth you need today: You are loved. You are wanted. You are precious to me. Let go of the 'what ifs' and just feel the 'what is' and what is true is that you are my favorite person in the whole world.","Thank you for being the patient, gentle soul you are. Even when I'm silly or emotional, you stay. That rare kindness of yours is why I fell for you. If you're feeling unappreciated today, please know that I see every little thing you do for me, and I cherish it all. You are a rare gem, Ko Ko.","Think about all the trips we haven't taken yet. Think about the third, fourth, and fiftieth bouquet I'm going to get from you. We have so many more 'firsts' waiting for us. Hang in there today, because the future we are building together is going to be so beautiful. I can't wait to experience it all with you.","This website might have many pages, but our story doesn't have an end page. This is your 'Always' button. You can come back here at 2 AM or 2 PM, on your best day or your worst. I am your biggest fan, your best friend, and your Babe. Always. Forever. No matter what."];
        let messageIndex = 0;
        
        function goToAlwaysPage() { 
            document.getElementById('page-5').style.opacity = '0'; 
            setTimeout(() => { 
                document.getElementById('page-5').style.display = 'none'; 
                document.getElementById('page-always').style.display = 'block'; 
                window.scrollTo(0, 0); 
                setTimeout(() => document.getElementById('page-always').style.opacity = '1', 100); 
            }, 1000); 
        }
        
        document.getElementById('alwaysStar').addEventListener('click', function() { 
            const messageBox = document.getElementById('alwaysMessage'); 
            messageBox.classList.remove('show'); 
            setTimeout(() => { 
                messageBox.innerHTML = alwaysMessages[messageIndex % alwaysMessages.length]; 
                messageBox.classList.add('show'); 
                messageIndex++; 
            }, 200); 
        });
        
        // The Things Distance Couldn't Take - Data
        const distanceProofs = [
            { id: 1, title: "The Sound of Your Voice in My Head", line: "No border can silence how you say my name.", truth: "Even when calls drop, when signal fades, when time zones separate—I still hear you. Your laugh echoes in quiet moments. Your voice is the background music of my thoughts. Distance can't take the sound of you.", icon: "🎵", position: { top: "18%", left: "50%" } },
            { id: 2, title: "The Way You Know Me Without Asking", line: "You finish my sentences from across an ocean.", truth: "How you know when I'm overthinking before I say a word. How you send the right song at the right moment. How you remember the tiny detail I mentioned weeks ago. Distance can't take your understanding of my soul.", icon: "🔍", position: { top: "28%", left: "68%" } },
            { id: 3, title: "Our Inside Language", line: "Words only we understand.", truth: "The nicknames. The shorthand. The references no one else gets. The silly phrases that started as jokes and became love letters. Distance can't take the language we built together, brick by brick.", icon: "🤫", position: { top: "42%", left: "75%" } },
            { id: 4, title: "The Feeling of Safe", line: "You are my home, and home doesn't have an address.", truth: "The exhale when I see your name light up my phone. The peace in knowing someone is on my side. The warmth that exists even when you're not beside me. Distance can't take the feeling of being held by you.", icon: "🛡️", position: { top: "58%", left: "72%" } },
            { id: 5, title: "Our Dreams of Tomorrow", line: "Everything we're building still stands.", truth: "The future we sketch in late-night calls. The plans for Hong Kong streets. The Japan cherry blossoms we'll walk under. The home we haven't built yet. Distance can't take what we're creating together.", icon: "🌅", position: { top: "72%", left: "60%" } },
            { id: 6, title: "The Proof That We Survived", line: "We already made it through once.", truth: "We lost each other and found our way back. We weathered silence and chose to speak. We stretched thin and held. Distance can't take the evidence that we are resilient.", icon: "💪", position: { top: "78%", left: "42%" } },
            { id: 7, title: "Your Hand in Mine (In Memory)", line: "I can still feel where you fit.", truth: "The way your fingers lace with mine. The warmth of your palm. The way your hand knows mine like a familiar language. Distance can't take the muscle memory of us.", icon: "🤝", position: { top: "72%", left: "25%" } },
            { id: 8, title: "The Certainty of Us", line: "I don't question. I just know.", truth: "Not 'if.' Not 'maybe.' Just 'when.' Just 'still.' Just 'always.' Distance can't take the knowing that settled in my bones the day I chose you.", icon: "✨", position: { top: "58%", left: "18%" } },
            { id: 9, title: "The Small Things You Remember", line: "You collect the details I forget I shared.", truth: "My favorite snack. The song I hummed once. The dream I mentioned at 2 AM. The thing I loved as a child. Distance can't take how you store pieces of me in your heart.", icon: "🌸", position: { top: "42%", left: "15%" } },
            { id: 10, title: "The Silence That Isn't Empty", line: "We don't need words to feel close.", truth: "The calls where no one speaks but no one hangs up. The peace of just existing in the same moment. The comfort that needs no filling. Distance can't take the stillness we share.", icon: "🤍", position: { top: "28%", left: "22%" } },
            { id: 11, title: "The Promise to Keep Coming Back", line: "I will always find my way to you.", truth: "No matter how far. No matter how long. No matter what tries to pull us apart. Distance can't take the vow that lives in my chest: I will return. Always.", icon: "💍", position: { top: "18%", left: "32%" }, isSpecial: true }
        ];

        let revealedKnots = [];
        let currentCard = null;

        function buildRedStringPage() {
            const container = document.getElementById('knotsContainer');
            if (!container) return;
            container.innerHTML = '';
            
            const svgWeave = document.getElementById('threadWeave');
            if (svgWeave) svgWeave.innerHTML = '';
            
            const points = distanceProofs.map(p => p.position);
            for (let i = 0; i < points.length; i++) {
                const next = (i + 1) % points.length;
                const start = points[i];
                const end = points[next];
                
                const startX = parseFloat(start.left) / 100 * 400;
                const startY = parseFloat(start.top) / 100 * 500;
                const endX = parseFloat(end.left) / 100 * 400;
                const endY = parseFloat(end.top) / 100 * 500;
                
                const path = document.createElementNS("http://www.w3.org/2000/svg", "path");
                const d = `M ${startX} ${startY} C ${(startX + endX) / 2} ${startY}, ${(startX + endX) / 2} ${endY}, ${endX} ${endY}`;
                path.setAttribute("d", d);
                path.setAttribute("stroke", "#ff6666");
                path.setAttribute("stroke-width", "2");
                path.setAttribute("fill", "none");
                path.setAttribute("stroke-dasharray", "5,5");
                path.classList.add("thread-segment");
                if (svgWeave) svgWeave.appendChild(path);
            }
            
            distanceProofs.forEach((proof, index) => {
                const knot = document.createElement('div');
                knot.className = 'knot-point';
                if (proof.isSpecial) knot.classList.add('special');
                knot.style.top = proof.position.top;
                knot.style.left = proof.position.left;
                knot.setAttribute('data-id', proof.id);
                knot.setAttribute('data-index', index);
                knot.addEventListener('click', () => showMemoryCard(proof, knot));
                container.appendChild(knot);
            });
        }

        function showMemoryCard(proof, knotElement) {
            if (currentCard) currentCard.remove();
            
            const card = document.createElement('div');
            card.className = 'memory-card';
            card.innerHTML = `
                <div class="memory-card-close">✕</div>
                <div class="memory-card-icon">${proof.icon}</div>
                <div class="memory-card-title">${proof.title}</div>
                <div class="memory-card-line">"${proof.line}"</div>
                <div class="memory-card-truth">${proof.truth}</div>
            `;
            
            document.body.appendChild(card);
            currentCard = card;
            setTimeout(() => card.classList.add('show'), 10);
            
            if (!revealedKnots.includes(proof.id)) {
                revealedKnots.push(proof.id);
                knotElement.classList.add('revealed');
                
                const threadSegments = document.querySelectorAll('.thread-segment');
                const index = distanceProofs.findIndex(p => p.id === proof.id);
                if (threadSegments[index]) {
                    threadSegments[index].classList.add('active');
                    setTimeout(() => threadSegments[index].classList.remove('active'), 500);
                }
            }
            
            if (revealedKnots.length === distanceProofs.length) {
                setTimeout(() => {
                    const ending = document.getElementById('redstringEnding');
                    if (ending) ending.style.display = 'block';
                }, 500);
            }
            
            card.addEventListener('click', (e) => {
                if (e.target.classList.contains('memory-card-close') || e.target === card) {
                    card.classList.remove('show');
                    setTimeout(() => card.remove(), 300);
                    currentCard = null;
                }
            });
        }

        function goToRedStringPage() {
            const alwaysPage = document.getElementById('page-always');
            if (alwaysPage) alwaysPage.style.opacity = '0';
            setTimeout(() => {
                if (alwaysPage) alwaysPage.style.display = 'none';
                const redStringPage = document.getElementById('page-redstring');
                if (redStringPage) {
                    redStringPage.style.display = 'block';
                    window.scrollTo(0, 0);
                    buildRedStringPage();
                    setTimeout(() => redStringPage.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        // Milestones
        const milestones = [ 
            { phase: "🌱 PHASE 1: THE FOUNDATION", num: "01", icon: "📞", title: "The \"I'm Here\" Button", detail: "A promise that even if it's 3 AM, we pick up the phone." },
            { phase: "🌱 PHASE 1: THE FOUNDATION", num: "02", icon: "📚", title: "The \"Study With Me\" Stream", detail: "Keeping a video call open while we both grind for exams." },
            { phase: "🌱 PHASE 1: THE FOUNDATION", num: "03", icon: "📍", title: "The \"Halfway\" Meetup", detail: "Finding a new city in between to explore for a weekend." },
            { phase: "🌱 PHASE 1: THE FOUNDATION", num: "04", icon: "📵", title: "The \"No Camera\" Day", detail: "Finally seeing each other and putting the phones away for 24 hours." },
            { phase: "🌱 PHASE 1: THE FOUNDATION", num: "05", icon: "🎂", title: "The 1st Anniversary", detail: "A celebration of the full 365 days since we made it official." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "06", icon: "🏃‍♀️", title: "The First Airport Sprint", detail: "Seeing who runs faster for the first hug at Haneda or HKG." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "07", icon: "🌸", title: "Cherry Blossom Date", detail: "Walking under the Sakura in Tokyo together." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "08", icon: "🚠", title: "The Peak Tram at Night", detail: "Looking at the Hong Kong skyline and finally not being alone." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "09", icon: "🏰", title: "Disney Double-Header", detail: "Visiting Tokyo Disneyland and then HK Disneyland to compare." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "10", icon: "🎮", title: "Universal Studios Japan", detail: "Wearing matching Mario hats in Osaka." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "11", icon: "🍜", title: "Dim Sum vs. Ramen", detail: "A food tour where we decide whose \"student city\" has better food." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "12", icon: "⛰️", title: "A Mountain Hike", detail: "Reaching the top together and taking a selfie with the view." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "13", icon: "🌍", title: "The \"Dream\" Destination", detail: "Saving up for that one big trip we always talk about." },
            { phase: "✈️ PHASE 2: THE ADVENTURES", num: "14", icon: "🗺️", title: "Our 3rd \"First\" Trip", detail: "Somewhere neither of us has ever been before." },
            { phase: "🏡 PHASE 3: THE FUTURE", num: "15", icon: "🏡", title: "First Home Visit", detail: "Meeting the families or hosting a small dinner together." },
            { phase: "🏡 PHASE 3: THE FUTURE", num: "16", icon: "🎓", title: "Graduation Day", detail: "Being the loudest person cheering for you when you walk across the stage." },
            { phase: "🏡 PHASE 3: THE FUTURE", num: "17", icon: "👩‍🎓", title: "Graduate Together", detail: "Being at each other's ceremonies in a few years." },
            { phase: "🏡 PHASE 3: THE FUTURE", num: "18", icon: "🛋️", title: "Our First \"Big\" Purchase", detail: "Buying something together for our future \"home.\"" },
            { phase: "🏡 PHASE 3: THE FUTURE", num: "19", icon: "🏠", title: "The First \"Home\" Together", detail: "Planning where we will live after university ends." },
            { phase: "♾️ PHASE 4: THE FOREVER", num: "20", icon: "🌹", title: "The 4th, 5th, and 50th Bouquet", detail: "I'm keeping count, Ko Ko!" },
            { phase: "♾️ PHASE 4: THE FOREVER", num: "21", icon: "♾️", title: "The \"Forever\" Plan", detail: "Simply being there to celebrate every single March 25th for the rest of our lives." },
            { phase: "♾️ PHASE 4: THE FOREVER", num: "22", icon: "✈️❤️", title: "The End of Distance", detail: "The day we never have to say \"Goodbye at the airport\" ever again." }
        ];
        
        function buildCountdown() { 
            const container = document.getElementById('milestonesContainer'); 
            if (!container) return;
            let lastPhase = ""; 
            milestones.forEach((m) => { 
                if (m.phase !== lastPhase) { 
                    const phaseDiv = document.createElement('div'); 
                    phaseDiv.className = 'phase-header'; 
                    phaseDiv.textContent = m.phase; 
                    container.appendChild(phaseDiv); 
                    lastPhase = m.phase; 
                } 
                const div = document.createElement('div'); 
                div.className = 'milestone-item'; 
                div.innerHTML = `<div class="flight-dot"></div><div><span class="milestone-number">${m.num}</span> <span class="milestone-icon">${m.icon}</span> <strong class="milestone-text">${m.title}</strong><div class="milestone-detail">${m.detail}</div></div>`; 
                container.appendChild(div); 
            }); 
        }
        
        function setupFlightPathObserver() { 
            const milestonesEl = document.querySelectorAll('.milestone-item'); 
            const path = document.getElementById('dottedPath'); 
            const finalPlane = document.getElementById('finalPlane'); 
            let revealedCount = 0; 
            const observer = new IntersectionObserver((entries) => { 
                entries.forEach(entry => { 
                    if (entry.isIntersecting && !entry.target.classList.contains('revealed')) { 
                        entry.target.classList.add('revealed'); 
                        revealedCount++; 
                        if (path) path.style.height = (revealedCount / milestonesEl.length) * 100 + '%'; 
                        if (revealedCount === milestonesEl.length && finalPlane) { 
                            setTimeout(() => finalPlane.classList.add('show'), 500); 
                        } 
                    } 
                }); 
            }, { threshold: 0.4 }); 
            milestonesEl.forEach(m => observer.observe(m)); 
        }
        
        function goToCountdownPage() { 
            const redStringPage = document.getElementById('page-redstring');
            if (redStringPage) redStringPage.style.opacity = '0'; 
            setTimeout(() => { 
                if (redStringPage) redStringPage.style.display = 'none'; 
                const countdownPage = document.getElementById('page-countdown'); 
                if (countdownPage) {
                    countdownPage.style.display = 'block'; 
                    window.scrollTo(0, 0); 
                    buildCountdown(); 
                    setTimeout(() => { 
                        countdownPage.style.opacity = '1'; 
                        setupFlightPathObserver(); 
                    }, 100);
                }
            }, 1000); 
        }
        
        // Love Tokens
        const coupons = [ 
            { category: "distance", style: "hk", icon: "📞", title: "The 'Pick Up' Pass", desc: "I'll stop whatever I'm doing to answer your call, even if it's 3 AM." },
            { category: "distance", style: "hk", icon: "🎵", title: "Midnight Lullaby", desc: "A voice note or live song of me singing you to sleep." },
            { category: "distance", style: "hk", icon: "💬", title: "The 'Vent' Session", desc: "60 minutes of me just listening to you complain. No advice, just support." },
            { category: "distance", style: "hk", icon: "🎬", title: "Virtual Movie Choice", desc: "You get to pick the movie for our Netflix date (and I won't fall asleep!)." },
            { category: "distance", style: "hk", icon: "📸", title: "The 'Check-In' Special", desc: "I'll send you cute selfies and updates all day to make you feel like I'm right there." },
            { category: "distance", style: "hk", icon: "🍜", title: "Digital Dinner Date", desc: "I'll order your favorite food to your dorm while we eat together on FaceTime." },
            { category: "distance", style: "hk", icon: "📚", title: "Study Buddy Token", desc: "We stay on mute on a video call for 3 hours so we don't feel lonely while studying." },
            { category: "reunion", style: "japan", icon: "🏃‍♀️", title: "The Airport Sprint", desc: "Valid for one extra-long, bone-crushing hug the moment we see each other." },
            { category: "reunion", style: "japan", icon: "🗺️", title: "Tour Guide Babe", desc: "I'll plan the entire day's itinerary in Hong Kong so you just have to show up." },
            { category: "reunion", style: "japan", icon: "🌸", title: "Sakura Stroll", desc: "A slow walk under the cherry blossoms in Japan, holding hands the whole time." },
            { category: "reunion", style: "japan", icon: "📵", title: "The 'No Phone' Day", desc: "24 hours of total focus on you. No social media, no distractions." },
            { category: "reunion", style: "japan", icon: "✅", title: "The 'Yes' Day", desc: "For one whole day, I have to say 'Yes' to whatever food or activity you pick." },
            { category: "reunion", style: "japan", icon: "🎁", title: "Souvenir Fund", desc: "I'll buy you that one silly thing you want but think is too expensive." },
            { category: "reunion", style: "japan", icon: "💋", title: "Public Display of Affection", desc: "One extra-long kiss in a beautiful spot (The Peak or Shibuya Crossing!)." },
            { category: "silly", style: "gold", icon: "🤝", title: "The 'Forgive Me' Pass", desc: "Instant truce. Use this to end any small argument immediately." },
            { category: "silly", style: "gold", icon: "👑", title: "The 'You Are Right' Token", desc: "I will verbally admit you are right and I am wrong. (Use wisely!)" },
            { category: "silly", style: "gold", icon: "🍫", title: "Snack Box Subscription", desc: "I'll curate a box of HK's best snacks and mail them to your Japan address." },
            { category: "silly", style: "gold", icon: "🤗", title: "The 'Stop Overthinking' Hug", desc: "A reminder of exactly why I love you and why we are okay." },
            { category: "silly", style: "gold", icon: "💆", title: "Massage Appointment", desc: "30 minutes of me being your personal masseuse after a long day of classes." },
            { category: "silly", style: "gold", icon: "✉️", title: "Handwritten Letter", desc: "I'll mail you a physical, scented letter that you can keep under your pillow." },
            { category: "silly", style: "gold", icon: "🧸", title: "The 'Childish' Pass", desc: "You can be as silly, clingy, or 'baby' as you want today without judgment." },
            { category: "silly", style: "gold", icon: "♾️", title: "The Lifetime Renewal", desc: "A promise that no matter how many coupons you use, I'll always be your Babe." }
        ];
        
        let redeemedCoupons = JSON.parse(localStorage.getItem('redeemedCoupons')) || [];
        
        function saveRedeemed() { localStorage.setItem('redeemedCoupons', JSON.stringify(redeemedCoupons)); }
        
        function redeemCoupon(index) { 
            if (!redeemedCoupons.includes(index)) { 
                redeemedCoupons.push(index); 
                localStorage.setItem(`stamp_${index}`, new Date().toISOString()); 
                saveRedeemed(); 
                updateTokensDisplay(); 
            } 
        }
        
        function resetAllCoupons() { 
            redeemedCoupons = []; 
            for (let i = 0; i < coupons.length; i++) localStorage.removeItem(`stamp_${i}`); 
            saveRedeemed(); 
            updateTokensDisplay(); 
        }
        
        function updateTokensDisplay() { 
            const container = document.getElementById('tokensContainer'); 
            if (!container) return;
            const remaining = coupons.length - redeemedCoupons.length; 
            const remainingSpan = document.getElementById('remainingCount');
            if (remainingSpan) remainingSpan.innerText = remaining; 
            const categories = { distance: { title: "📞 Distance Coupons", tickets: [] }, reunion: { title: "✈️ Reunion Coupons", tickets: [] }, silly: { title: "🧸 Sweet & Silly Coupons", tickets: [] } }; 
            coupons.forEach((coupon, idx) => { categories[coupon.category].tickets.push({ ...coupon, idx }); }); 
            container.innerHTML = ''; 
            for (let [cat, data] of Object.entries(categories)) { 
                const section = document.createElement('div'); 
                section.className = 'category-section'; 
                section.innerHTML = `<h3 class="category-title ${cat}">${data.title}</h3><div class="tickets-grid"></div>`; 
                container.appendChild(section); 
                const grid = section.querySelector('.tickets-grid'); 
                data.tickets.forEach(ticket => { 
                    const isRedeemed = redeemedCoupons.includes(ticket.idx); 
                    const styleClass = ticket.style === 'hk' ? 'hk-style' : ticket.style === 'japan' ? 'japan-style' : 'gold-style'; 
                    const card = document.createElement('div'); 
                    card.className = `ticket-card ${styleClass} ${isRedeemed ? 'redeemed' : ''}`; 
                    const stampDate = localStorage.getItem(`stamp_${ticket.idx}`); 
                    const formattedDate = stampDate ? new Date(stampDate).toLocaleDateString('en-GB') : ''; 
                    card.innerHTML = `<div class="stamp-overlay">REDEEMED<br><span class="ticket-stamp-date">${formattedDate}</span></div><div class="ticket-icon">${ticket.icon}</div><div class="ticket-title">${ticket.title}</div><div class="ticket-description">${ticket.desc}</div><button class="redeem-btn" data-idx="${ticket.idx}" ${isRedeemed ? 'disabled' : ''}>${isRedeemed ? '✓ Redeemed' : '🎫 Redeem'}</button>`; 
                    grid.appendChild(card); 
                    setTimeout(() => card.classList.add('animate-in'), 50); 
                    const btn = card.querySelector('.redeem-btn'); 
                    if (btn) btn.addEventListener('click', (e) => { e.stopPropagation(); const idx = parseInt(btn.dataset.idx); if (!redeemedCoupons.includes(idx)) redeemCoupon(idx); }); 
                }); 
            } 
        }
        
        function goToTokensPage() { 
            const countdownPage = document.getElementById('page-countdown');
            if (countdownPage) countdownPage.style.opacity = '0'; 
            setTimeout(() => { 
                if (countdownPage) countdownPage.style.display = 'none'; 
                const tokensPage = document.getElementById('page-tokens'); 
                if (tokensPage) {
                    tokensPage.style.display = 'block'; 
                    window.scrollTo(0, 0); 
                    updateTokensDisplay(); 
                    setTimeout(() => tokensPage.style.opacity = '1', 100);
                }
            }, 1000); 
        }
        
        document.getElementById('resetTokensBtn')?.addEventListener('click', resetAllCoupons);
        
        // Playlist
        const songs = [ 
            { title: "ပြုံးနေဖို့ (Nae Pho)", artist: "D Vision & Grace", url: "https://www.youtube.com/watch?v=P45kPRaKWVM", meaning: "For the days you need a gentle reminder to find your smile. This song is a whisper in your ear, a nudge to let a little light in." },
            { title: "ဝမ်းနည်းတတ်တဲ့ ချစ်သူ", artist: "Idiots", url: "https://www.youtube.com/watch?v=y56qMbkVZ4c", meaning: "This song makes me think of how you understand my feelings, even when I don't say them out loud." },
            { title: "တန်ဖိုးထားပါ့မယ်", artist: "Kaung Kaung", url: "https://www.youtube.com/watch?v=ItMN2XI5PTY", meaning: "I promise to always value you, Ko Ko. You're so precious to me." },
            { title: "ကမ္ဘာအဆက်ဆက်", artist: "Bunny Phyoe & Amara Hpone", url: "https://www.youtube.com/watch?v=sQpzLLVOHcI", meaning: "Forever with you is all I want. This song is our promise." },
            { title: "အမှတ်တမဲ့", artist: "D-Vision & Lu Hpring", url: "https://www.youtube.com/watch?v=Y54HE19JPNs", meaning: "Sometimes love happens unexpectedly. That's how it was with us." },
            { title: "I Like Me Better", artist: "Lauv", url: "https://www.youtube.com/watch?v=a7fzkqLozwA", meaning: "When I'm with you, I become a better version of myself." },
            { title: "Enchanted", artist: "Taylor Swift", url: "https://www.youtube.com/watch?v=igIfiqqVHtA", meaning: "The first time we met again, I was completely enchanted by you." },
            { title: "10,000 Hours", artist: "Justin Bieber", url: "https://www.youtube.com/watch?v=Y2E71oe0aSM", meaning: "I'd spend 10,000 hours and 10,000 more just to know you better." },
            { title: "Say You Won't Let Go", artist: "James Arthur", url: "https://www.youtube.com/watch?v=0yW7w8F2TVA", meaning: "This is our song. I'll never let go of you." },
            { title: "Until I Found You", artist: "Stephen Sanchez", url: "https://www.youtube.com/watch?v=GxldQ9eX2wo", meaning: "I didn't know what love was until I found you, Ko Ko." }
        ];
        
        function buildPlaylist() { 
            const container = document.getElementById('playlistContainer'); 
            if (!container) return;
            container.innerHTML = '';
            songs.forEach((song) => { 
                const div = document.createElement('div'); 
                div.className = 'song-item'; 
                div.innerHTML = `
                    <div class="song-icon">🎵</div>
                    <div class="song-content">
                        <div class="song-title">"${song.title}" by ${song.artist}</div>
                        <div class="song-desc">${song.meaning}</div>
                        <a href="${song.url}" target="_blank" class="song-link" rel="noopener noreferrer">🎵 Listen on YouTube →</a>
                    </div>
                `; 
                container.appendChild(div); 
            }); 
        }
        
        function goToPlaylistPage() { 
            const tokensPage = document.getElementById('page-tokens');
            if (tokensPage) tokensPage.style.opacity = '0'; 
            setTimeout(() => { 
                if (tokensPage) tokensPage.style.display = 'none'; 
                const playlistPage = document.getElementById('page-playlist'); 
                if (playlistPage) {
                    playlistPage.style.display = 'block'; 
                    window.scrollTo(0, 0); 
                    buildPlaylist(); 
                    setTimeout(() => playlistPage.style.opacity = '1', 100);
                }
            }, 1000); 
        }
        
        // Navigation functions for new pages
        function goToRecipePage() {
            const playlistPage = document.getElementById('page-playlist');
            if (playlistPage) playlistPage.style.opacity = '0';
            setTimeout(() => {
                if (playlistPage) playlistPage.style.display = 'none';
                const recipePage = document.getElementById('page-recipe');
                if (recipePage) {
                    recipePage.style.display = 'block';
                    window.scrollTo(0, 0);
                    setTimeout(() => recipePage.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        function goToLettersPage() {
            const recipePage = document.getElementById('page-recipe');
            if (recipePage) recipePage.style.opacity = '0';
            setTimeout(() => {
                if (recipePage) recipePage.style.display = 'none';
                const lettersPage = document.getElementById('page-letters');
                if (lettersPage) {
                    lettersPage.style.display = 'block';
                    window.scrollTo(0, 0);
                    setTimeout(() => lettersPage.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        function goToPoem13() {
            const lettersPage = document.getElementById('page-letters');
            if (lettersPage) lettersPage.style.opacity = '0';
            setTimeout(() => {
                if (lettersPage) lettersPage.style.display = 'none';
                const poemPage = document.getElementById('page-poem13');
                if (poemPage) {
                    poemPage.style.display = 'flex';
                    window.scrollTo(0, 0);
                    setTimeout(() => poemPage.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        function goToPoem14() {
            const poem13 = document.getElementById('page-poem13');
            if (poem13) poem13.style.opacity = '0';
            setTimeout(() => {
                if (poem13) poem13.style.display = 'none';
                const poem14 = document.getElementById('page-poem14');
                if (poem14) {
                    poem14.style.display = 'flex';
                    window.scrollTo(0, 0);
                    setTimeout(() => poem14.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        function goToPoem15() {
            const poem14 = document.getElementById('page-poem14');
            if (poem14) poem14.style.opacity = '0';
            setTimeout(() => {
                if (poem14) poem14.style.display = 'none';
                const poem15 = document.getElementById('page-poem15');
                if (poem15) {
                    poem15.style.display = 'flex';
                    window.scrollTo(0, 0);
                    setTimeout(() => poem15.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        function goToPoem16() {
            const poem15 = document.getElementById('page-poem15');
            if (poem15) poem15.style.opacity = '0';
            setTimeout(() => {
                if (poem15) poem15.style.display = 'none';
                const poem16 = document.getElementById('page-poem16');
                if (poem16) {
                    poem16.style.display = 'flex';
                    window.scrollTo(0, 0);
                    setTimeout(() => poem16.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        function goToPoem17() {
            const poem16 = document.getElementById('page-poem16');
            if (poem16) poem16.style.opacity = '0';
            setTimeout(() => {
                if (poem16) poem16.style.display = 'none';
                const poem17 = document.getElementById('page-poem17');
                if (poem17) {
                    poem17.style.display = 'flex';
                    window.scrollTo(0, 0);
                    setTimeout(() => poem17.style.opacity = '1', 100);
                }
            }, 1000);
        }
        
        function initTreePage() {
            const treeSections = document.querySelectorAll('#page-tree .tree-section');
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        entry.target.classList.add('visible');
                    }
                });
            }, { threshold: 0.2 });
            treeSections.forEach(section => observer.observe(section));
        }
        
        function initLanguagesPage() {
            const languageCards = document.querySelectorAll('#page-languages .language-card');
            languageCards.forEach((card, index) => {
                setTimeout(() => {
                    card.classList.add('animate-in');
                }, index * 100);
            });
        }
        
        function goToTreePage() {
            const poem17 = document.getElementById('page-poem17');
            if (poem17) poem17.style.opacity = '0';
            setTimeout(() => {
                if (poem17) poem17.style.display = 'none';
                const treePage = document.getElementById('page-tree');
                if (treePage) {
                    treePage.style.display = 'block';
                    window.scrollTo(0, 0);
                    setTimeout(() => {
                        treePage.style.opacity = '1';
                        initTreePage();
                    }, 100);
                }
            }, 1000);
        }
        
        function goToLanguagesPage() {
            const treePage = document.getElementById('page-tree');
            if (treePage) treePage.style.opacity = '0';
            setTimeout(() => {
                if (treePage) treePage.style.display = 'none';
                const languagesPage = document.getElementById('page-languages');
                if (languagesPage) {
                    languagesPage.style.display = 'block';
                    window.scrollTo(0, 0);
                    setTimeout(() => {
                        languagesPage.style.opacity = '1';
                        initLanguagesPage();
                    }, 100);
                }
            }, 1000);
        }
        
        function goToApologyPage() {
            const languagesPage = document.getElementById('page-languages');
            if (languagesPage) languagesPage.style.opacity = '0';
            setTimeout(() => {
                if (languagesPage) languagesPage.style.display = 'none';
                const apologyPage = document.getElementById('page-apology');
                if (apologyPage) {
                    apologyPage.style.display = 'block';
                    window.scrollTo(0, 0);
                    setTimeout(() => {
                        apologyPage.style.opacity = '1';
                        const apologyLetter = document.getElementById('apologyLetter');
                        if (apologyLetter) {
                            setTimeout(() => {
                                apologyLetter.classList.add('calm');
                                const lightRay = document.createElement('div');
                                lightRay.className = 'light-ray active';
                                apologyLetter.appendChild(lightRay);
                            }, 3000);
                        }
                    }, 100);
                }
            }, 1000);
        }
        
        function goToReceiptPage() {
            const apologyPage = document.getElementById('page-apology');
            if (apologyPage) apologyPage.style.opacity = '0';
            setTimeout(() => {
                if (apologyPage) apologyPage.style.display = 'none';
                const receiptPage = document.getElementById('page-receipt');
                if (receiptPage) {
                    receiptPage.style.display = 'flex';
                    window.scrollTo(0, 0);
                    setTimeout(() => {
                        receiptPage.style.opacity = '1';
                        
                        const receiptPaper = document.getElementById('receiptPaper');
                        const receiptStamp = document.getElementById('receiptStamp');
                        
                        if (receiptPaper) {
                            const newPaper = receiptPaper.cloneNode(true);
                            receiptPaper.parentNode.replaceChild(newPaper, receiptPaper);
                            newPaper.id = 'receiptPaper';
                            newPaper.style.animation = 'none';
                            newPaper.offsetHeight;
                            newPaper.style.animation = 'paperUnroll 2s ease-out forwards';
                        }
                        
                        if (receiptStamp) {
                            receiptStamp.classList.remove('show');
                            setTimeout(() => {
                                receiptStamp.classList.add('show');
                                
                                const wrapper = document.querySelector('.receipt-wrapper');
                                if (wrapper) {
                                    const stampEffect = document.createElement('div');
                                    stampEffect.style.position = 'absolute';
                                    stampEffect.style.bottom = '80px';
                                    stampEffect.style.right = '40px';
                                    stampEffect.style.width = '100px';
                                    stampEffect.style.height = '100px';
                                    stampEffect.style.borderRadius = '50%';
                                    stampEffect.style.background = 'radial-gradient(circle, rgba(227,66,52,0.3) 0%, rgba(227,66,52,0) 70%)';
                                    stampEffect.style.pointerEvents = 'none';
                                    stampEffect.style.animation = 'stampThump 0.3s ease-out';
                                    wrapper.appendChild(stampEffect);
                                    setTimeout(() => stampEffect.remove(), 300);
                                }
                            }, 2100);
                        }
                    }, 100);
                }
            }, 1000);
        }
        
        function goToEndingPage() {
            const receiptPage = document.getElementById('page-receipt');
            if (receiptPage) receiptPage.style.opacity = '0';
            setTimeout(() => {
                if (receiptPage) receiptPage.style.display = 'none';
                const endingPage = document.getElementById('page-ending');
                if (endingPage) {
                    endingPage.style.display = 'block';
                    window.scrollTo(0, 0);
                    setTimeout(() => {
                        endingPage.style.opacity = '1';
                        
                        const num1 = document.getElementById('num1');
                        const num3 = document.getElementById('num3');
                        const num22a = document.getElementById('num22a');
                        const num22b = document.getElementById('num22b');
                        const num19 = document.getElementById('num19');
                        
                        let count = 0;
                        const interval = setInterval(() => {
                            count++;
                            if (num1 && count <= 1) num1.innerText = count;
                            if (num3 && count <= 3) num3.innerText = count;
                            if (num22a && count <= 22) num22a.innerText = count;
                            if (num22b && count <= 22) num22b.innerText = count;
                            if (num19 && count <= 19) num19.innerText = count;
                            if (count >= 22) clearInterval(interval);
                        }, 50);
                        
                        const letter = document.getElementById('handwrittenLetter');
                        if (letter) {
                            const paragraphs = letter.querySelectorAll('p');
                            paragraphs.forEach((p, idx) => {
                                p.style.opacity = '0';
                                setTimeout(() => {
                                    p.style.opacity = '1';
                                    p.style.transition = 'opacity 0.3s ease';
                                }, idx * 150);
                            });
                        }
                        
                        let starCount = 0;
                        const starButton = document.getElementById('starButton');
                        const starCounter = document.getElementById('starCounter');
                        const constellation = document.getElementById('constellationMini');
                        
                        if (starButton) {
                            starButton.addEventListener('click', () => {
                                starCount++;
                                if (starCounter) starCounter.innerHTML = `⭐ ${starCount} stars left for our story`;
                                const newStar = document.createElement('span');
                                newStar.className = 'star-mini';
                                newStar.innerHTML = '⭐';
                                newStar.style.position = 'absolute';
                                newStar.style.top = Math.random() * 100 + 'px';
                                newStar.style.left = Math.random() * 80 + 10 + '%';
                                newStar.style.animation = 'twinkle 2s infinite';
                                if (constellation) constellation.appendChild(newStar);
                                const starMsg = document.createElement('div');
                                starMsg.style.position = 'fixed';
                                starMsg.style.bottom = '100px';
                                starMsg.style.left = '50%';
                                starMsg.style.transform = 'translateX(-50%)';
                                starMsg.style.background = 'rgba(0,0,0,0.8)';
                                starMsg.style.padding = '10px 20px';
                                starMsg.style.borderRadius = '30px';
                                starMsg.style.color = '#ffd966';
                                starMsg.style.zIndex = '1000';
                                starMsg.innerHTML = '✨ Thank you. This star will stay here, always. ✨';
                                document.body.appendChild(starMsg);
                                setTimeout(() => starMsg.remove(), 3000);
                            });
                        }
                        
                        const hiddenTrigger = document.getElementById('hiddenMessageTrigger');
                        const secretText = document.getElementById('secretText');
                        if (hiddenTrigger) {
                            hiddenTrigger.addEventListener('click', () => {
                                if (secretText) secretText.classList.add('show');
                                setTimeout(() => secretText?.classList.remove('show'), 5000);
                            });
                        }
                    }, 100);
                }
            }, 1000);
        }
        
function initEndPage() {
    const endPage = document.getElementById('page-end');
    if (!endPage) return;
    
    // Create floating particles
    function createFloatingParticle() {
        const particle = document.createElement('div');
        particle.className = 'floating-particle';
        const icons = ['✨', '⭐', '💫', '🌟', '💛', '🌙', '🕊️', '💌'];
        particle.textContent = icons[Math.floor(Math.random() * icons.length)];
        particle.style.left = Math.random() * 100 + '%';
        particle.style.fontSize = (Math.random() * 1.5 + 0.5) + 'rem';
        particle.style.animationDuration = (Math.random() * 5 + 5) + 's';
        particle.style.animationDelay = Math.random() * 3 + 's';
        endPage.appendChild(particle);
        
        setTimeout(() => {
            particle.remove();
        }, 10000);
    }
    
    // Create particles periodically
    let particleInterval;
    
    const observer = new MutationObserver((mutations) => {
        mutations.forEach((mutation) => {
            if (mutation.target.style.display === 'flex' || mutation.target.style.display === 'block') {
                particleInterval = setInterval(() => {
                    if (endPage.style.display === 'flex') {
                        createFloatingParticle();
                    }
                }, 2000);
            } else if (mutation.target.style.display === 'none') {
                if (particleInterval) clearInterval(particleInterval);
            }
        });
    });
    
    observer.observe(endPage, { attributes: true, attributeFilter: ['style'] });
}

// Call this when the end page is first loaded
// Add to your existing goToEndPage function
function goToEndPage() {
    const endingPage = document.getElementById('page-ending');
    if (endingPage) endingPage.style.opacity = '0';
    setTimeout(() => {
        if (endingPage) endingPage.style.display = 'none';
        const endPage = document.getElementById('page-end');
        if (endPage) {
            endPage.style.display = 'flex';
            window.scrollTo(0, 0);
            setTimeout(() => {
                endPage.style.opacity = '1';
                initEndPage();
            }, 100);
        }
    }, 1000);
}
    </script>
</body>
</html>
