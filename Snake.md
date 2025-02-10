---
layout: base
title: Snake Game 
description: Snake Game
hide: true
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #222;
        }
        canvas {
            border: 2px solid white;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        
        const box = 20;
        let snake = [{x: 10 * box, y: 10 * box}];
        let food = { x: Math.floor(Math.random() * 20) * box, y: Math.floor(Math.random() * 20) * box };
        let direction = "RIGHT";
        let score = 0;

        document.addEventListener("keydown", changeDirection);
        function changeDirection(event) {
            const key = event.keyCode;
            if (key === 37 && direction !== "RIGHT") direction = "LEFT";
            else if (key === 38 && direction !== "DOWN") direction = "UP";
            else if (key === 39 && direction !== "LEFT") direction = "RIGHT";
            else if (key === 40 && direction !== "UP") direction = "DOWN";
        }

        function draw() {
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, box, box);
            
            ctx.fillStyle = "lime";
            snake.forEach((part, index) => {
                ctx.fillRect(part.x, part.y, box, box);
                ctx.strokeStyle = "black";
                ctx.strokeRect(part.x, part.y, box, box);
            });

            let snakeX = snake[0].x;
            let snakeY = snake[0].y;
            
            if (direction === "LEFT") snakeX -= box;
            if (direction === "UP") snakeY -= box;
            if (direction === "RIGHT") snakeX += box;
            if (direction === "DOWN") snakeY += box;
            
            if (snakeX === food.x && snakeY === food.y) {
                score++;
                food = { x: Math.floor(Math.random() * 20) * box, y: Math.floor(Math.random() * 20) * box };
            } else {
                snake.pop();
            }
            
            let newHead = { x: snakeX, y: snakeY };
            
            if (snakeX < 0 || snakeY < 0 || snakeX >= canvas.width || snakeY >= canvas.height || collision(newHead, snake)) {
                clearInterval(game);
                alert("Game Over! Score: " + score);
                return;
            }
            
            snake.unshift(newHead);
        }

        function collision(head, body) {
            return body.some(part => part.x === head.x && part.y === head.y);
        }
        
        let game = setInterval(draw, 100);
    </script>
</body>
</html>