---
toc: true
comments: true
layout: post
title: Physics simulation
description: A simple bouncing ball simulation with inputs to change variables
courses: { calendar: {week: 2} }
type: hacks
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Physics Simulator</title>
    <style>
        body {
            margin: 20px;
            text-align: center;
        }
        canvas {
            border: 1px solid #000;
            display: block;
            margin: 0 auto;
        }
        button {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Physics simulator</h1>

    <canvas id="simulator" width="400" height="400"></canvas>
    <div>
        <label for="velocityX">Initial Velocity X:</label>
        <input type="number" id="velocityX" value="2">
    </div>
    <div>
        <label for="velocityY">Initial Velocity Y:</label>
        <input type="number" id="velocityY" value="-3">
    </div>
    <button id="reset-button" style="color: white">Reset</button>

    <script>
        const canvas = document.getElementById('simulator');
        const ctx = canvas.getContext('2d');

        const ball = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 20,
            color: '#3498db',
            velocityX: 0, // Initial value will be set by user input
            velocityY: 0, // Initial value will be set by user input
            gravity: 0.1,
            friction: 0.98,
        };

        function drawBall() {
            ctx.beginPath();
            ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
            ctx.fillStyle = ball.color;
            ctx.fill();
            ctx.closePath();
        }

        function resetBall() {
            ball.x = canvas.width / 2;
            ball.y = canvas.height / 2;
            ball.velocityX = parseFloat(document.getElementById('velocityX').value);
            ball.velocityY = parseFloat(document.getElementById('velocityY').value);
        }

        document.getElementById('reset-button').addEventListener('click', resetBall);

        function update() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Apply gravity
            ball.velocityY += ball.gravity;

            // Apply friction to slow down the ball
            ball.velocityX *= ball.friction;
            ball.velocityY *= ball.friction;

            // Update ball position
            ball.x += ball.velocityX;
            ball.y += ball.velocityY;

            // Bounce off the walls
            if (ball.x + ball.radius > canvas.width || ball.x - ball.radius < 0) {
                ball.velocityX = -ball.velocityX;
            }

            if (ball.y + ball.radius > canvas.height || ball.y - ball.radius < 0) {
                ball.velocityY = -ball.velocityY;
            }

            drawBall();

            requestAnimationFrame(update);
        }

        resetBall();
        update();
    </script>
</body>
</html>
