const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const box = 20; // kích thước mỗi ô
let snake = [{x: 8 * box, y: 8 * box}]; // bắt đầu ở giữa
let direction = "RIGHT";
let food = {
    x: Math.floor(Math.random() * 20) * box,
    y: Math.floor(Math.random() * 20) * box
};
let score = 0;

// Lắng nghe phím
document.addEventListener("keydown", changeDirection);

function changeDirection(event) {
    if(event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
    else if(event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
    else if(event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
    else if(event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
}

// Vẽ rắn
function drawSnake() {
    ctx.fillStyle = "lime";
    snake.forEach(part => ctx.fillRect(part.x, part.y, box, box));
}

// Vẽ thức ăn
function drawFood() {
    ctx.fillStyle = "red";
    ctx.fillRect(food.x, food.y, box, box);
}

// Kiểm tra va chạm
function collision(head, array) {
    for(let i = 0; i < array.length; i++){
        if(head.x === array[i].x && head.y === array[i].y) return true;
    }
    return false;
}

// Cập nhật game
function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawSnake();
    drawFood();

    let snakeX = snake[0].x;
    let snakeY = snake[0].y;

    if(direction === "LEFT") snakeX -= box;
    if(direction === "RIGHT") snakeX += box;
    if(direction === "UP") snakeY -= box;
    if(direction === "DOWN") snakeY += box;

    // Khi ăn thức ăn
    if(snakeX === food.x && snakeY === food.y){
        score++;
        document.getElementById('score').innerText = score;
        food = {
            x: Math.floor(Math.random() * 20) * box,
            y: Math.floor(Math.random() * 20) * box
        };
    } else {
        snake.pop();
    }

    let newHead = {x: snakeX, y: snakeY};

    // Kiểm tra va chạm
    if(snakeX < 0 || snakeX >= 400 || snakeY < 0 || snakeY >= 400 || collision(newHead, snake)){
        clearInterval(game);
        alert("Game Over! Điểm của bạn: " + score);
        return;
    }

    snake.unshift(newHead);
}

// Tốc độ game
let game = setInterval(draw, 150);
