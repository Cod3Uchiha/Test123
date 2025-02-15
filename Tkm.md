```html
<!DOCTYPE html>
<html>
  <head>
    <title>Snake Game with Buttons</title>
    <style>
      body {
        background-color: #000;
        color: #fff;
        font-family: Arial, Helvetica, sans-serif;
      }

      .snake-board {
        width: 400px;
        height: 400px;
        border: 1px solid #fff;
        margin: 0 auto;
      }

      .snake-square {
        width: 10px;
        height: 10px;
        background-color: #00ff00;
        border: 1px solid #fff;
        float: left;
      }

      .snake-head {
        background-color: #ff0000;
      }

      .snake-tail {
        background-color: #000;
      }

      .snake-food {
        width: 10px;
        height: 10px;
        background-color: #ff0000;
        border: 1px solid #fff;
        position: absolute;
      }

      .button-container {
        text-align: center;
      }

      button {
        padding: 5px 10px;
        margin: 5px;
        background-color: #fff;
        color: #000;
        border: 1px solid #000;
      }
    </style>
  </head>

  <body>
    <div class="snake-board"></div>

    <div class="button-container">
      <button id="start-button">Start</button>
      <button id="stop-button">Stop</button>
      <button id="turn-left-button">Turn Left</button>
      <button id="turn-right-button">Turn Right</button>
    </div>

    <script>
      const snakeBoard = document.querySelector('.snake-board');
      const snakeHead = document.createElement('div');
      snakeHead.classList.add('snake-square', 'snake-head');
      snakeHead.style.left = '0px';
      snakeHead.style.top = '0px';
      snakeBoard.appendChild(snakeHead);

      const snakeBody = [];

      let snakeLength = 1;
      let snakeDirection = 'right';
      let gameInterval;
      let foodPosition;

      const createFood = () => {
        const food = document.createElement('div');
        food.classList.add('snake-food');
        food.style.left = `${Math.floor(Math.random() * 39) * 10}px`;
        food.style.top = `${Math.floor(Math.random() * 39) * 10}px`;
        snakeBoard.appendChild(food);
        foodPosition = {
          x: parseInt(food.style.left),
          y: parseInt(food.style.top),
        };
      };

      const moveSnake = () => {
        let newSnakeHeadPosition;
        switch (snakeDirection) {
          case 'right':
            newSnakeHeadPosition = {
              x: snakeHead.offsetLeft + 10,
              y: snakeHead.offsetTop,
            };
            break;
          case 'left':
            newSnakeHeadPosition = {
              x: snakeHead.offsetLeft - 10,
              y: snakeHead.offsetTop,
            };
            break;
          case 'up':
            newSnakeHeadPosition = {
              x: snakeHead.offsetLeft,
              y: snakeHead.offsetTop - 10,
            };
            break;
          case 'down':
            newSnakeHeadPosition = {
              x: snakeHead.offsetLeft,
              y: snakeHead.offsetTop + 10,
            };
            break;
        }

        if (
          newSnakeHeadPosition.x < 0 ||
          newSnakeHeadPosition.x > snakeBoard.offsetWidth - 10 ||
          newSnakeHeadPosition.y < 0 ||
          newSnakeHeadPosition.y > snakeBoard.offsetHeight - 10
        ) {
          clearInterval(gameInterval);
          alert('Game Over!');
        }

        if (
          newSnakeHeadPosition.x === foodPosition.x &&
          newSnakeHeadPosition.y === foodPosition.y
        ) {
          snakeLength++;
          createFood();
        } else {
          snakeBody.pop().remove();
        }

        const newSnakeHead = document.createElement('div');
        newSnakeHead.classList.add('snake-square', 'snake-head');
        newSnakeHead.style.left = `${newSnakeHeadPosition.x}px`;
        newSnakeHead.style.top = `${newSnakeHeadPosition.y}px`;
        snakeBoard.appendChild(newSnakeHead);
        snakeBody.unshift(snakeHead);
        snakeHead = newSnakeHead;
      };

      const startGame = () => {
        gameInterval = setInterval(moveSnake, 100);
        createFood();
      };

      const stopGame = () => {
        clearInterval(gameInterval);
      };

      const turnLeft = () => {
        switch (snakeDirection) {
          case 'right':
            snakeDirection = 'up';
            break;
          case 'left':
            snakeDirection = 'down';
            break;
          case 'up':
            snakeDirection = 'left';
            break;
          case 'down':
            snakeDirection = 'right';
            break;
        }
      };

      const turnRight = () => {
        switch (snakeDirection) {
          case 'right':
            snakeDirection = 'down';
            break;
          case 'left':
            snakeDirection = 'up';
            break;
          case 'up':
            snakeDirection = 'right';
            break;
          case 'down':
            snakeDirection = 'left';
            break;
        }
      };

      document.getElementById('start-button').addEventListener('click', startGame);
      document.getElementById('stop-button').addEventListener('click', stopGame);
      document.getElementById('turn-left-button').addEventListener('click', turnLeft);
      document.getElementById('turn-right-button').addEventListener('click', turnRight);
    </script>
  </body>
</html>

```