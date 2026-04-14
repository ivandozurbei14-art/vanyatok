<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>VanyaTok</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            font-family: 'Inter', sans-serif;
        }
        
        :root {
            --primary: #fe2c55;
            --secondary: #25f4ee;
            --dark: #000000;
            --light: #ffffff;
            --gray: #161823;
        }
        
        body {
            background: var(--dark);
            color: var(--light);
            overflow: hidden;
            touch-action: none;
        }
        
        /* Splash Screen */
        .splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(135deg, var(--primary), #ff6b6b);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 10000;
            transition: opacity 0.5s, visibility 0.5s;
        }
        
        .splash-screen.hidden {
            opacity: 0;
            visibility: hidden;
        }
        
        .splash-logo {
            font-size: 48px;
            font-weight: 800;
            margin-bottom: 20px;
            animation: pulse 2s infinite;
            text-shadow: 0 4px 20px rgba(0,0,0,0.3);
        }
        
        .splash-text {
            font-size: 18px;
            opacity: 0.9;
            margin-bottom: 40px;
        }
        
        .splash-loader {
            width: 50px;
            height: 50px;
            border: 4px solid rgba(255,255,255,0.3);
            border-top-color: white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        /* Auth Screens */
        .auth-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: var(--dark);
            z-index: 9999;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
        }
        
        .auth-container.hidden {
            display: none;
        }
        
        .auth-header {
            padding: 40px 30px;
            text-align: center;
        }
        
        .auth-logo {
            font-size: 36px;
            font-weight: 800;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 10px;
        }
        
        .auth-subtitle {
            color: rgba(255,255,255,0.6);
            font-size: 14px;
        }
        
        .auth-form {
            padding: 0 30px;
            flex: 1;
        }
        
        .input-group {
            margin-bottom: 20px;
        }
        
        .input-group label {
            display: block;
            margin-bottom: 8px;
            font-size: 14px;
            color: rgba(255,255,255,0.8);
        }
        
        .input-group input {
            width: 100%;
            padding: 15px;
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 12px;
            background: rgba(255,255,255,0.05);
            color: white;
            font-size: 16px;
            outline: none;
            transition: border-color 0.3s;
        }
        
        .input-group input:focus {
            border-color: var(--primary);
        }
        
        .btn-primary {
            width: 100%;
            padding: 16px;
            background: linear-gradient(135deg, var(--primary), #ff6b6b);
            border: none;
            border-radius: 12px;
            color: white;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            margin-top: 10px;
        }
        
        .btn-primary:active {
            transform: scale(0.98);
        }
        
        .btn-secondary {
            width: 100%;
            padding: 16px;
            background: transparent;
            border: 1px solid rgba(255,255,255,0.3);
            border-radius: 12px;
            color: white;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            margin-top: 15px;
            transition: background 0.3s;
        }
        
        .btn-secondary:hover {
            background: rgba(255,255,255,0.1);
        }
        
        .auth-footer {
            padding: 30px;
            text-align: center;
            color: rgba(255,255,255,0.5);
            font-size: 14px;
        }
        
        .auth-footer a {
            color: var(--secondary);
            text-decoration: none;
            cursor: pointer;
        }
        
        /* Main App */
        .app-container {
            width: 100vw;
            height: 100vh;
            position: relative;
            overflow: hidden;
            display: none;
        }
        
        .app-container.active {
            display: block;
        }
        
        /* Navigation */
        .top-nav {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 15px 20px;
            z-index: 100;
            background: linear-gradient(to bottom, rgba(0,0,0,0.6), transparent);
        }
        
        .nav-tabs {
            display: flex;
            gap: 25px;
            font-size: 16px;
            font-weight: 600;
        }
        
        .nav-tab {
            opacity: 0.6;
            cursor: pointer;
            transition: opacity 0.3s;
            position: relative;
        }
        
        .nav-tab.active {
            opacity: 1;
        }
        
        .nav-tab.active::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 50%;
            transform: translateX(-50%);
            width: 30px;
            height: 3px;
            background: var(--light);
            border-radius: 2px;
        }
        
        .search-icon {
            position: absolute;
            right: 20px;
            font-size: 20px;
            cursor: pointer;
        }
        
        /* Video Feed */
        .video-container {
            width: 100%;
            height: 100%;
            position: relative;
            overflow: hidden;
        }
        
        .video-wrapper {
            width: 100%;
            height: 100%;
            position: absolute;
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .video-item {
            width: 100%;
            height: 100%;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
            background: var(--gray);
            overflow: hidden;
        }
        
        .animated-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }
        
        .gradient-1 {
            background: linear-gradient(45deg, #ff006e, #8338ec, #3a86ff);
            background-size: 400% 400%;
            animation: gradientShift 8s ease infinite;
        }
        
        .gradient-2 {
            background: linear-gradient(-45deg, #f72585, #7209b7, #3a0ca3, #4361ee);
            background-size: 400% 400%;
            animation: gradientShift 10s ease infinite;
        }
        
        .gradient-3 {
            background: linear-gradient(135deg, #ff9f1c, #ffbf69, #cbf3f0, #2ec4b6);
            background-size: 400% 400%;
            animation: gradientShift 12s ease infinite;
        }
        
        .gradient-4 {
            background: linear-gradient(225deg, #ff006e, #ffbe0b, #fb5607);
            background-size: 400% 400%;
            animation: gradientShift 9s ease infinite;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .particles {
            position: absolute;
            width: 100%;
            height: 100%;
            overflow: hidden;
            z-index: 2;
        }
        
        .particle {
            position: absolute;
            width: 10px;
            height: 10px;
            background: rgba(255,255,255,0.3);
            border-radius: 50%;
            animation: float 15s infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(100vh) rotate(0deg); opacity: 0; }
            10% { opacity: 1; }
            90% { opacity: 1; }
            100% { transform: translateY(-100vh) rotate(720deg); opacity: 0; }
        }
        
        .video-bg {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            z-index: 3;
        }
        
        .video-overlay {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            padding: 20px 80px 100px 20px;
            background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
            z-index: 10;
        }
        
        .user-info {
            margin-bottom: 10px;
        }
        
        .username {
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 5px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .verified {
            color: var(--secondary);
            font-size: 14px;
        }
        
        .description {
            font-size: 14px;
            line-height: 1.4;
            opacity: 0.95;
            margin-bottom: 8px;
        }
        
        .music-info {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 13px;
            opacity: 0.9;
        }
        
        .music-icon {
            animation: rotate 3s linear infinite;
        }
        
        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
        
        /* Sidebar Actions */
        .actions-sidebar {
            position: absolute;
            right: 8px;
            bottom: 100px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
            z-index: 50;
        }
        
        .action-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .action-btn:active {
            transform: scale(0.9);
        }
        
        .action-icon {
            width: 48px;
            height: 48px;
            background: rgba(255,255,255,0.15);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            backdrop-filter: blur(10px);
            transition: all 0.3s;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .action-btn.liked .action-icon {
            background: var(--primary);
            border-color: var(--primary);
            animation: likeBounce 0.4s;
        }
        
        @keyframes likeBounce {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }
        
        .action-count {
            font-size: 12px;
            font-weight: 600;
            text-shadow: 0 1px 3px rgba(0,0,0,0.5);
        }
        
        .avatar-btn {
            width: 52px;
            height: 52px;
            border-radius: 50%;
            border: 2px solid white;
            overflow: hidden;
            margin-bottom: 5px;
            position: relative;
            box-shadow: 0 2px 10px rgba(0,0,0,0.3);
        }
        
        .avatar-btn img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .follow-btn {
            position: absolute;
            bottom: -8px;
            left: 50%;
            transform: translateX(-50%);
            width: 22px;
            height: 22px;
            background: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            font-weight: bold;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
        }
        
        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 10px 0 25px;
            background: var(--dark);
            border-top: 1px solid rgba(255,255,255,0.1);
            z-index: 100;
        }
        
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 3px;
            cursor: pointer;
            opacity: 0.6;
            transition: opacity 0.3s;
            position: relative;
        }
        
        .nav-item.active {
            opacity: 1;
        }
        
        .nav-item span:first-child {
            font-size: 24px;
        }
        
        .nav-item span:last-child {
            font-size: 10px;
        }
        
        .create-btn {
            width: 45px;
            height: 35px;
            background: linear-gradient(135deg, var(--secondary), var(--primary));
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .create-btn:active {
            transform: scale(0.95);
        }
        
        .create-btn::before,
        .create-btn::after {
            content: '';
            position: absolute;
            background: white;
            border-radius: 1px;
        }
        
        .create-btn::before {
            width: 20px;
            height: 2px;
        }
        
        .create-btn::after {
            width: 2px;
            height: 20px;
        }
        
        /* Chat with Bot */
        .chat-container {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: var(--dark);
            z-index: 200;
            display: none;
            flex-direction: column;
        }
        
        .chat-container.active {
            display: flex;
        }
        
        .chat-header {
            display: flex;
            align-items: center;
            padding: 15px 20px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            background: rgba(0,0,0,0.5);
        }
        
        .back-btn {
            font-size: 24px;
            margin-right: 15px;
            cursor: pointer;
        }
        
        .chat-user-info {
            display: flex;
            align-items: center;
            gap: 10px;
            flex: 1;
        }
        
        .chat-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
        }
        
        .chat-name {
            font-weight: 600;
        }
        
        .chat-status {
            font-size: 12px;
            color: #4ade80;
        }
        
        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .message {
            max-width: 75%;
            padding: 12px 16px;
            border-radius: 20px;
            font-size: 14px;
            line-height: 1.4;
            animation: messageSlide 0.3s ease;
        }
        
        @keyframes messageSlide {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .message.user {
            align-self: flex-end;
            background: var(--primary);
            border-bottom-right-radius: 4px;
        }
        
        .message.bot {
            align-self: flex-start;
            background: rgba(255,255,255,0.1);
            border-bottom-left-radius: 4px;
        }
        
        .chat-input-area {
            padding: 15px 20px;
            border-top: 1px solid rgba(255,255,255,0.1);
            display: flex;
            gap: 10px;
            align-items: center;
        }
        
        .chat-input {
            flex: 1;
            padding: 12px 20px;
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 25px;
            background: rgba(255,255,255,0.05);
            color: white;
            font-size: 14px;
            outline: none;
        }
        
        .chat-input::placeholder {
            color: rgba(255,255,255,0.4);
        }
        
        .send-btn {
            width: 45px;
            height: 45px;
            background: var(--primary);
            border: none;
            border-radius: 50%;
            color: white;
            font-size: 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: transform 0.2s;
        }
        
        .send-btn:active {
            transform: scale(0.95);
        }
        
        /* Upload Modal */
        .upload-modal {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0,0,0,0.9);
            z-index: 300;
            display: none;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        
        .upload-modal.active {
            display: flex;
        }
        
        .upload-content {
            width: 100%;
            max-width: 400px;
            background: var(--gray);
            border-radius: 20px;
            padding: 30px;
            text-align: center;
        }
        
        .upload-area {
            border: 2px dashed rgba(255,255,255,0.3);
            border-radius: 15px;
            padding: 40px 20px;
            margin: 20px 0;
            cursor: pointer;
            transition: border-color 0.3s;
        }
        
        .upload-area:hover {
            border-color: var(--primary);
        }
        
        .upload-icon {
            font-size: 48px;
            margin-bottom: 10px;
        }
        
        .upload-text {
            color: rgba(255,255,255,0.6);
            font-size: 14px;
        }
        
        .upload-preview {
            width: 100%;
            max-height: 300px;
            border-radius: 10px;
            margin-top: 20px;
            display: none;
        }
        
        /* Profile Page */
        .profile-container {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: var(--dark);
            z-index: 150;
            display: none;
            overflow-y: auto;
        }
        
        .profile-container.active {
            display: block;
        }
        
        .profile-header {
            padding: 60px 20px 30px;
            text-align: center;
            background: linear-gradient(to bottom, rgba(254,44,85,0.3), transparent);
        }
        
        .profile-avatar {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            border: 3px solid var(--primary);
            margin: 0 auto 15px;
            overflow: hidden;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
        }
        
        .profile-name {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .profile-handle {
            color: rgba(255,255,255,0.6);
            font-size: 14px;
            margin-bottom: 20px;
        }
        
        .profile-stats {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-bottom: 20px;
        }
        
        .stat {
            text-align: center;
        }
        
        .stat-value {
            font-size: 20px;
            font-weight: 700;
        }
        
        .stat-label {
            font-size: 12px;
            color: rgba(255,255,255,0.6);
        }
        
        .profile-actions {
            display: flex;
            gap: 10px;
            justify-content: center;
        }
        
        .profile-btn {
            padding: 10px 30px;
            border-radius: 20px;
            border: none;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .profile-btn:active {
            transform: scale(0.95);
        }
        
        .profile-btn.primary {
            background: var(--primary);
            color: white;
        }
        
        .profile-btn.secondary {
            background: rgba(255,255,255,0.1);
            color: white;
        }
        
        .profile-videos {
            padding: 20px;
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 2px;
        }
        
        .profile-video {
            aspect-ratio: 3/4;
            background: rgba(255,255,255,0.1);
            position: relative;
            overflow: hidden;
        }
        
        .profile-video::after {
            content: '▶';
            position: absolute;
            bottom: 5px;
            left: 5px;
            font-size: 12px;
        }
        
        /* Animations */
        .like-animation {
            position: absolute;
            pointer-events: none;
            animation: float-up 1s ease-out forwards;
            font-size: 40px;
            color: var(--primary);
            z-index: 100;
        }
        
        @keyframes float-up {
            0% {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
            100% {
                opacity: 0;
                transform: translateY(-100px) scale(1.5);
            }
        }
        
        .double-tap-heart {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0);
            font-size: 100px;
            color: var(--primary);
            opacity: 0;
            pointer-events: none;
            z-index: 60;
        }
        
        .double-tap-heart.show {
            animation: heart-burst 0.8s ease-out forwards;
        }
        
        @keyframes heart-burst {
            0% {
                transform: translate(-50%, -50%) scale(0);
                opacity: 1;
            }
            50% {
                transform: translate(-50%, -50%) scale(1.2);
                opacity: 1;
            }
            100% {
                transform: translate(-50%, -50%) scale(1);
                opacity: 0;
            }
        }
        
        .loading {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 50px;
            height: 50px;
            border: 4px solid rgba(255,255,255,0.2);
            border-top-color: var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            z-index: 20;
        }
        
        @keyframes spin {
            to { transform: translate(-50%, -50%) rotate(360deg); }
        }
        
        .progress-bar {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 3px;
            background: rgba(255,255,255,0.3);
            z-index: 20;
        }
        
        .progress-fill {
            height: 100%;
            background: var(--primary);
            width: 0%;
            transition: width 0.1s linear;
        }
        
        .play-btn {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80px;
            height: 80px;
            background: rgba(0,0,0,0.5);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            cursor: pointer;
            z-index: 30;
            opacity: 0;
            transition: opacity 0.3s;
            backdrop-filter: blur(5px);
        }
        
        .play-btn.show {
            opacity: 1;
        }
        
        .toast {
            position: fixed;
            bottom: 100px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0,0,0,0.9);
            color: white;
            padding: 12px 24px;
            border-radius: 25px;
            font-size: 14px;
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s;
            pointer-events: none;
        }
        
        .toast.show {
            opacity: 1;
        }
        
        /* Typing indicator */
        .typing-indicator {
            display: flex;
            gap: 5px;
            padding: 15px;
            align-self: flex-start;
        }
        
        .typing-dot {
            width: 8px;
            height: 8px;
            background: rgba(255,255,255,0.5);
            border-radius: 50%;
            animation: typing 1.4s infinite;
        }
        
        .typing-dot:nth-child(2) { animation-delay: 0.2s; }
        .typing-dot:nth-child(3) { animation-delay: 0.4s; }
        
        @keyframes typing {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }
    </style>
</head>
<body>
    <!-- Splash Screen -->
    <div class="splash-screen" id="splashScreen">
        <div class="splash-logo">VanyaTok</div>
        <div class="splash-text">Short videos, endless fun</div>
        <div class="splash-loader"></div>
    </div>
    
    <!-- Auth Container -->
    <div class="auth-container" id="authContainer">
        <div class="auth-header">
            <div class="auth-logo">VanyaTok</div>
            <div class="auth-subtitle">Join the community</div>
        </div>
        
        <div class="auth-form" id="loginForm">
            <div class="input-group">
                <label>Email or Username</label>
                <input type="text" id="loginUsername" placeholder="Enter your username">
            </div>
            <div class="input-group">
                <label>Password</label>
                <input type="password" id="loginPassword" placeholder="Enter your password">
            </div>
            <button class="btn-primary" onclick="login()">Log In</button>
            <button class="btn-secondary" onclick="showRegister()">Create Account</button>
        </div>
        
        <div class="auth-form" id="registerForm" style="display: none;">
            <div class="input-group">
                <label>Username</label>
                <input type="text" id="regUsername" placeholder="Choose a username">
            </div>
            <div class="input-group">
                <label>Email</label>
                <input type="email" id="regEmail" placeholder="Enter your email">
            </div>
            <div class="input-group">
                <label>Password</label>
                <input type="password" id="regPassword" placeholder="Create a password">
            </div>
            <button class="btn-primary" onclick="register()">Sign Up</button>
            <button class="btn-secondary" onclick="showLogin()">Already have an account?</button>
        </div>
        
        <div class="auth-footer">
            By continuing, you agree to our <a>Terms of Service</a> and <a>Privacy Policy</a>
        </div>
    </div>
    
    <!-- Main App -->
    <div class="app-container" id="appContainer">
        <!-- Top Navigation -->
        <div class="top-nav">
            <div class="nav-tabs">
                <span class="nav-tab" onclick="switchTab('following')">Following</span>
                <span class="nav-tab active" onclick="switchTab('foryou')">For You</span>
            </div>
            <div class="search-icon">🔍</div>
        </div>
        
        <!-- Video Feed -->
        <div class="video-container" id="videoContainer">
            <div class="video-wrapper" id="videoWrapper"></div>
        </div>
        
        <!-- Bottom Navigation -->
        <div class="bottom-nav">
            <div class="nav-item active" onclick="navigateTo('home')">
                <span>🏠</span>
                <span>Home</span>
            </div>
            <div class="nav-item" onclick="navigateTo('friends')">
                <span>👥</span>
                <span>Friends</span>
            </div>
            <div class="create-btn" onclick="openUpload()"></div>
            <div class="nav-item" onclick="openChat()">
                <span>💬</span>
                <span>Inbox</span>
            </div>
            <div class="nav-item" onclick="openProfile()">
                <span>👤</span>
                <span>Profile</span>
            </div>
        </div>
    </div>
    
    <!-- Chat with Bot -->
    <div class="chat-container" id="chatContainer">
        <div class="chat-header">
            <span class="back-btn" onclick="closeChat()">←</span>
            <div class="chat-user-info">
                <div class="chat-avatar">🤖</div>
                <div>
                    <div class="chat-name">VanyaBot</div>
                    <div class="chat-status">Online</div>
                </div>
            </div>
        </div>
        <div class="chat-messages" id="chatMessages">
            <div class="message bot">👋 Hi! I'm VanyaBot. How can I help you today?</div>
        </div>
        <div class="chat-input-area">
            <input type="text" class="chat-input" id="chatInput" placeholder="Message..." onkeypress="if(event.key==='Enter')sendMessage()">
            <button class="send-btn" onclick="sendMessage()">➤</button>
        </div>
    </div>
    
    <!-- Upload Modal -->
    <div class="upload-modal" id="uploadModal">
        <div class="upload-content">
            <h2>Upload Video</h2>
            <div class="upload-area" onclick="document.getElementById('fileInput').click()">
                <div class="upload-icon">📹</div>
                <div class="upload-text">Tap to select video</div>
            </div>
            <input type="file" id="fileInput" accept="video/*" style="display: none;" onchange="handleFileSelect(event)">
            <video class="upload-preview" id="uploadPreview" controls></video>
            <input type="text" class="chat-input" id="videoCaption" placeholder="Add a caption..." style="margin-top: 15px;">
            <button class="btn-primary" onclick="uploadVideo()" style="margin-top: 15px;">Post</button>
            <button class="btn-secondary" onclick="closeUpload()">Cancel</button>
        </div>
    </div>
    
    <!-- Profile Page -->
    <div class="profile-container" id="profileContainer">
        <div style="position: sticky; top: 0; background: var(--dark); padding: 15px; display: flex; align-items: center; z-index: 10;">
            <span style="font-size: 24px; margin-right: 15px; cursor: pointer;" onclick="closeProfile()">←</span>
            <span style="font-weight: 600;">Profile</span>
        </div>
        <div class="profile-header">
            <div class="profile-avatar" id="profileAvatar">👤</div>
            <div class="profile-name" id="profileName">User</div>
            <div class="profile-handle" id="profileHandle">@username</div>
            <div class="profile-stats">
                <div class="stat">
                    <div class="stat-value">0</div>
                    <div class="stat-label">Following</div>
                </div>
                <div class="stat">
                    <div class="stat-value">0</div>
                    <div class="stat-label">Followers</div>
                </div>
                <div class="stat">
                    <div class="stat-value">0</div>
                    <div class="stat-label">Likes</div>
                </div>
            </div>
            <div class="profile-actions">
                <button class="profile-btn primary" onclick="editProfile()">Edit Profile</button>
                <button class="profile-btn secondary" onclick="logout()">Logout</button>
            </div>
        </div>
        <div class="profile-videos" id="profileVideos">
            <div class="profile-video"></div>
            <div class="profile-video"></div>
            <div class="profile-video"></div>
            <div class="profile-video"></div>
            <div class="profile-video"></div>
            <div class="profile-video"></div>
        </div>
    </div>
    
    <!-- Toast -->
    <div class="toast" id="toast"></div>

    <script>
        // App State
        let currentUser = null;
        let currentIndex = 0;
        let isDragging = false;
        let startY = 0;
        let currentTranslate = 0;
        let prevTranslate = 0;
        
        // Sample Videos
        const videos = [
            {
                id: 1,
                username: "dancemaster",
                verified: true,
                description: "New trend 2024 🔥 Try this challenge! #dance #viral",
                music: "Original Sound - dancemaster",
                likes: "1.2M",
                comments: "45.2K",
                shares: "12.5K",
                avatar: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100",
                videoUrl: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerBlazes.mp4",
                gradient: "gradient-1"
            },
            {
                id: 2,
                username: "foodie_life",
                verified: false,
                description: "Pasta recipe in 15 minutes 🍝 #food #recipe #cooking",
                music: "Yummy - Justin Bieber",
                likes: "856K",
                comments: "23.1K",
                shares: "45.3K",
                avatar: "https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=100",
                videoUrl: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4",
                gradient: "gradient-2"
            },
            {
                id: 3,
                username: "travel_daily",
                verified: true,
                description: "This place changed my life 🌊 #travel #nature #wanderlust",
                music: "Sunset Lover - Petit Biscuit",
                likes: "2.4M",
                comments: "89.5K",
                shares: "156K",
                avatar: "https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=100",
                videoUrl: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4",
                gradient: "gradient-3"
            },
            {
                id: 4,
                username: "tech_reviews",
                verified: false,
                description: "New gadget review 📱 Worth buying? #tech #review",
                music: "Cyberpunk - Synthwave",
                likes: "234K",
                comments: "12.8K",
                shares: "3.4K",
                avatar: "https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=100",
                videoUrl: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/TearsOfSteel.mp4",
                gradient: "gradient-4"
            },
            {
                id: 5,
                username: "fitness_guru",
                verified: true,
                description: "5 min abs workout 💪 #fitness #workout #health",
                music: "Power - Kanye West",
                likes: "567K",
                comments: "34.2K",
                shares: "28.1K",
                avatar: "https://images.unsplash.com/photo-1438761681033-6461ffad8d80?w=100",
                videoUrl: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4",
                gradient: "gradient-1"
            },
            {
                id: 6,
                username: "art_studio",
                verified: false,
                description: "Portrait in 60 seconds 🎨 #art #drawing #sketch",
                music: "Lo-Fi Study Beats",
                likes: "892K",
                comments: "56.7K",
                shares: "67.3K",
                avatar: "https://images.unsplash.com/photo-1544005313-94ddf0286df2?w=100",
                videoUrl: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/SubaruOutbackOnStreetAndDirt.mp4",
                gradient: "gradient-2"
            }
        ];
        
        // Bot Responses
        const botResponses = [
            "That's interesting! Tell me more.",
            "I love watching videos on VanyaTok!",
            "Have you tried the new dance challenge?",
            "You can upload your own videos by clicking the + button!",
            "Don't forget to follow your favorite creators!",
            "What's your favorite type of content?",
            "I'm here to help you 24/7!",
            "Check out the trending page for viral videos!",
            "You can edit your profile anytime!",
            "Thanks for chatting with me! 😊"
        ];
        
        // Initialize
        window.onload = function() {
            setTimeout(() => {
                document.getElementById('splashScreen').classList.add('hidden');
                checkAuth();
            }, 2000);
        };
        
        function checkAuth() {
            const savedUser = localStorage.getItem('vanyatok_user');
            if (savedUser) {
                currentUser = JSON.parse(savedUser);
                showApp();
            } else {
                document.getElementById('authContainer').classList.remove('hidden');
            }
        }
        
        // Auth Functions
        function showRegister() {
            document.getElementById('loginForm').style.display = 'none';
            document.getElementById('registerForm').style.display = 'block';
        }
        
        function showLogin() {
            document.getElementById('registerForm').style.display = 'none';
            document.getElementById('loginForm').style.display = 'block';
        }
        
        function register() {
            const username = document.getElementById('regUsername').value;
            const email = document.getElementById('regEmail').value;
            const password = document.getElementById('regPassword').value;
            
            if (!username || !email || !password) {
                showToast('Please fill in all fields');
                return;
            }
            
            currentUser = {
                username: username,
                email: email,
                avatar: '👤'
            };
            
            localStorage.setItem('vanyatok_user', JSON.stringify(currentUser));
            showToast('Welcome to VanyaTok!');
            showApp();
        }
        
        function login() {
            const username = document.getElementById('loginUsername').value;
            const password = document.getElementById('loginPassword').value;
            
            if (!username || !password) {
                showToast('Please enter username and password');
                return;
            }
            
            currentUser = {
                username: username,
                email: username + '@vanyatok.com',
                avatar: '👤'
            };
            
            localStorage.setItem('vanyatok_user', JSON.stringify(currentUser));
            showToast('Welcome back!');
            showApp();
        }
        
        function logout() {
            localStorage.removeItem('vanyatok_user');
            currentUser = null;
            location.reload();
        }
        
        function showApp() {
            document.getElementById('authContainer').classList.add('hidden');
            document.getElementById('appContainer').classList.add('active');
            initVideos();
            updateProfile();
        }
        
        // Video Functions
        function initVideos() {
            renderVideos();
            setupVideoEvents();
            playCurrentVideo();
        }
        
        function renderVideos() {
            const wrapper = document.getElementById('videoWrapper');
            wrapper.innerHTML = videos.map((video, index) => `
                <div class="video-item" data-index="${index}">
                    <div class="animated-bg ${video.gradient}"></div>
                    <div class="particles">
                        ${Array(10).fill(0).map((_, i) => `
                            <div class="particle" style="
                                left: ${Math.random() * 100}%;
                                animation-delay: ${Math.random() * 15}s;
                                animation-duration: ${15 + Math.random() * 10}s;
                            "></div>
                        `).join('')}
                    </div>
                    <video class="video-bg" loop playsinline data-index="${index}">
                        <source src="${video.videoUrl}" type="video/mp4">
                    </video>
                    <div class="loading" id="loading-${index}"></div>
                    <div class="play-btn" id="playBtn-${index}">▶️</div>
                    <div class="double-tap-heart" id="heart-${index}">❤️</div>
                    
                    <div class="video-overlay">
                        <div class="user-info">
                            <div class="username">
                                @${video.username}
                                ${video.verified ? '<span class="verified">✓</span>' : ''}
                            </div>
                            <div class="description">${video.description}</div>
                            <div class="music-info">
                                <span class="music-icon">🎵</span>
                                <span>${video.music}</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="actions-sidebar">
                        <div class="avatar-btn">
                            <img src="${video.avatar}" onerror="this.style.display='none'">
                            <div class="follow-btn">+</div>
                        </div>
                        <div class="action-btn like-btn" onclick="toggleLike(this, ${index})">
                            <div class="action-icon">❤️</div>
                            <span class="action-count" id="likes-${index}">${video.likes}</span>
                        </div>
                        <div class="action-btn" onclick="showComments(${index})">
                            <div class="action-icon">💬</div>
                            <span class="action-count">${video.comments}</span>
                        </div>
                        <div class="action-btn" onclick="shareVideo(${index})">
                            <div class="action-icon">↗️</div>
                            <span class="action-count">${video.shares}</span>
                        </div>
                        <div class="action-btn">
                            <div class="action-icon" style="animation: rotate 3s linear infinite;">💿</div>
                        </div>
                    </div>
                    
                    <div class="progress-bar">
                        <div class="progress-fill" id="progress-${index}"></div>
                    </div>
                </div>
            `).join('');
        }
        
        function setupVideoEvents() {
            const container = document.getElementById('videoContainer');
            
            container.addEventListener('touchstart', touchStart, {passive: false});
            container.addEventListener('touchmove', touchMove, {passive: false});
            container.addEventListener('touchend', touchEnd);
            
            container.addEventListener('mousedown', touchStart);
            container.addEventListener('mousemove', touchMove);
            container.addEventListener('mouseup', touchEnd);
            container.addEventListener('mouseleave', touchEnd);
            
            // Double tap to like
            let lastTap = 0;
            container.addEventListener('click', (e) => {
                if (e.target.closest('.action-btn') || e.target.closest('.nav-item')) return;
                
                const currentTime = new Date().getTime();
                const tapLength = currentTime - lastTap;
                
                if (tapLength < 300 && tapLength > 0) {
                    const videoItem = e.target.closest('.video-item');
                    if (videoItem) {
                        const index = parseInt(videoItem.dataset.index);
                        const heart = document.getElementById(`heart-${index}`);
                        heart.classList.remove('show');
                        void heart.offsetWidth;
                        heart.classList.add('show');
                        
                        const likeBtn = videoItem.querySelector('.like-btn');
                        if (!likeBtn.classList.contains('liked')) {
                            toggleLike(likeBtn, index);
                        }
                    }
                }
                lastTap = currentTime;
            });
            
            // Video events
            document.querySelectorAll('.video-bg').forEach((video, index) => {
                video.addEventListener('timeupdate', () => {
                    if (video.duration) {
                        const progress = (video.currentTime / video.duration) * 100;
                        document.getElementById(`progress-${index}`).style.width = `${progress}%`;
                    }
                });
                
                video.addEventListener('loadeddata', () => {
                    document.getElementById(`loading-${index}`).style.display = 'none';
                });
                
                video.addEventListener('error', () => {
                    document.getElementById(`loading-${index}`).style.display = 'none';
                });
            });
        }
        
        function touchStart(e) {
            isDragging = true;
            startY = e.type.includes('mouse') ? e.pageY : e.touches[0].clientY;
            prevTranslate = currentIndex * -window.innerHeight;
            document.getElementById('videoWrapper').style.transition = 'none';
        }
        
        function touchMove(e) {
            if (!isDragging) return;
            e.preventDefault();
            const currentY = e.type.includes('mouse') ? e.pageY : e.touches[0].clientY;
            const diff = currentY - startY;
            currentTranslate = prevTranslate + diff;
            document.getElementById('videoWrapper').style.transform = `translateY(${currentTranslate}px)`;
        }
        
        function touchEnd(e) {
            if (!isDragging) return;
            isDragging = false;
            const movedBy = currentTranslate - prevTranslate;
            
            document.getElementById('videoWrapper').style.transition = 'transform 0.4s cubic-bezier(0.4, 0, 0.2, 1)';
            
            if (movedBy < -50 && currentIndex < videos.length - 1) {
                currentIndex++;
            } else if (movedBy > 50 && currentIndex > 0) {
                currentIndex--;
            }
            
            updatePosition();
            setTimeout(playCurrentVideo, 400);
        }
        
        function updatePosition() {
            document.getElementById('videoWrapper').style.transform = `translateY(${-currentIndex * 100}vh)`;
        }
        
        function playCurrentVideo() {
            document.querySelectorAll('.video-bg').forEach((video, index) => {
                if (index === currentIndex) {
                    video.play().catch(() => {
                        document.getElementById(`playBtn-${index}`).classList.add('show');
                    });
                } else {
                    video.pause();
                    video.currentTime = 0;
                }
            });
        }
        
        // Interactions
        function toggleLike(btn, index) {
            btn.classList.toggle('liked');
            const countEl = document.getElementById(`likes-${index}`);
            
            if (btn.classList.contains('liked')) {
                createParticles(btn);
                showToast('Added to liked');
            }
        }
        
        function createParticles(element) {
            const rect = element.getBoundingClientRect();
            for (let i = 0; i < 5; i++) {
                const p = document.createElement('div');
                p.className = 'like-animation';
                p.innerHTML = '❤️';
                p.style.left = (rect.left + rect.width/2 + (Math.random()-0.5)*30) + 'px';
                p.style.top = rect.top + 'px';
                document.body.appendChild(p);
                setTimeout(() => p.remove(), 1000);
            }
        }
        
        function showComments(index) {
            showToast(`Comments: ${videos[index].comments}`);
        }
        
        function shareVideo(index) {
            if (navigator.share) {
                navigator.share({
                    title: 'VanyaTok Video',
                    text: videos[index].description,
                    url: window.location.href
                });
            } else {
                navigator.clipboard.writeText(window.location.href);
                showToast('Link copied!');
            }
        }
        
        // Chat Functions
        function openChat() {
            document.getElementById('chatContainer').classList.add('active');
        }
        
        function closeChat() {
            document.getElementById('chatContainer').classList.remove('active');
        }
        
        function sendMessage() {
            const input = document.getElementById('chatInput');
            const text = input.value.trim();
            if (!text) return;
            
            const messages = document.getElementById('chatMessages');
            
            // User message
            const userMsg = document.createElement('div');
            userMsg.className = 'message user';
            userMsg.textContent = text;
            messages.appendChild(userMsg);
            
            input.value = '';
            messages.scrollTop = messages.scrollHeight;
            
            // Typing indicator
            const typing = document.createElement('div');
            typing.className = 'typing-indicator';
            typing.id = 'typing';
            typing.innerHTML = '<div class="typing-dot"></div><div class="typing-dot"></div><div class="typing-dot"></div>';
            messages.appendChild(typing);
            messages.scrollTop = messages.scrollHeight;
            
            // Bot response
            setTimeout(() => {
                document.getElementById('typing').remove();
                
                const botMsg = document.createElement('div');
                botMsg.className = 'message bot';
                
                // Smart responses
                if (text.toLowerCase().includes('hello') || text.toLowerCase().includes('hi')) {
                    botMsg.textContent = 'Hello there! 👋 Welcome to VanyaTok!';
                } else if (text.toLowerCase().includes('video') || text.toLowerCase().includes('upload')) {
                    botMsg.textContent = 'You can upload videos by clicking the + button at the bottom! 📹';
                } else if (text.toLowerCase().includes('like') || text.toLowerCase().includes('heart')) {
                    botMsg.textContent = 'Double tap any video to like it! ❤️';
                } else if (text.toLowerCase().includes('follow')) {
                    botMsg.textContent = 'Tap the + button on any profile to follow them!';
                } else {
                    botMsg.textContent = botResponses[Math.floor(Math.random() * botResponses.length)];
                }
                
                messages.appendChild(botMsg);
                messages.scrollTop = messages.scrollHeight;
            }, 1500);
        }
        
        // Upload Functions
        function openUpload() {
            document.getElementById('uploadModal').classList.add('active');
        }
        
        function closeUpload() {
            document.getElementById('uploadModal').classList.remove('active');
            document.getElementById('uploadPreview').style.display = 'none';
            document.getElementById('videoCaption').value = '';
        }
        
        function handleFileSelect(e) {
            const file = e.target.files[0];
            if (file) {
                const url = URL.createObjectURL(file);
                const preview = document.getElementById('uploadPreview');
                preview.src = url;
                preview.style.display = 'block';
            }
        }
        
        function uploadVideo() {
            const preview = document.getElementById('uploadPreview');
            if (preview.style.display === 'none') {
                showToast('Please select a video first');
                return;
            }
            
            showToast('Uploading...');
            setTimeout(() => {
                showToast('Video posted successfully!');
                closeUpload();
                
                // Add to profile
                const grid = document.getElementById('profileVideos');
                const newVideo = document.createElement('div');
                newVideo.className = 'profile-video';
                newVideo.style.background = 'linear-gradient(135deg, #ff006e, #8338ec)';
                grid.insertBefore(newVideo, grid.firstChild);
            }, 2000);
        }
        
        // Profile Functions
        function openProfile() {
            document.getElementById('profileContainer').classList.add('active');
        }
        
        function closeProfile() {
            document.getElementById('profileContainer').classList.remove('active');
        }
        
        function updateProfile() {
            if (currentUser) {
                document.getElementById('profileName').textContent = currentUser.username;
                document.getElementById('profileHandle').textContent = '@' + currentUser.username.toLowerCase();
            }
        }
        
        function editProfile() {
            showToast('Edit profile coming soon!');
        }
        
        // Navigation
        function navigateTo(page) {
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            event.currentTarget.classList.add('active');
            
            if (page === 'home') {
                showToast('Home');
            } else if (page === 'friends') {
                showToast('Friends - Coming soon!');
            }
        }
        
        function switchTab(tab) {
            document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
            event.target.classList.add('active');
            showToast(tab === 'foryou' ? 'For You' : 'Following');
        }
        
        // Utility
        function showToast(message) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 2500);
        }
        
        // Keyboard shortcuts
        document.addEventListener('keydown', (e) => {
            if (!document.getElementById('appContainer').classList.contains('active')) return;
            
            if (e.key === 'ArrowDown' && currentIndex < videos.length - 1) {
                currentIndex++;
                updatePosition();
                playCurrentVideo();
            } else if (e.key === 'ArrowUp' && currentIndex > 0) {
                currentIndex--;
                updatePosition();
                playCurrentVideo();
            }
        });
    </script>
</body>
</html>
