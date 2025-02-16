# Создаём игру "Космическая битва"

Добро пожаловать в руководство по созданию браузерной игры "Космическая битва"! В этой игре игрок управляет космическим кораблём, уклоняется от метеоритов и уничтожает их с помощью лазеров. Давайте разберём код поэтапно и создадим свою игру!



## 1. Создаём HTML-структуру
Начнём с создания базовой разметки страницы. Этот файл будет основой для нашей игры.

### Код `index.html`:
```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Космическая битва</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>🚀 Космическая битва 🚀</h1>
    <p>Управление: ← → для движения, ПРОБЕЛ — выстрел</p>
    <div id="game">
        <div id="player"></div>
    </div>
    <p>Очки: <span id="score">0</span></p>
    <script src="script.js"></script>
</body>
</html>
```

### Разбор кода:
1. Используем стандартную структуру HTML-документа.
2. Добавляем заголовок и описание управления.
3. Создаём контейнер `#game`, в котором будут происходить игровые события.
4. Размещаем `#player` — наш корабль.
5. Добавляем отображение очков.
6. Подключаем CSS для стилизации и `script.js` для логики игры.

---

## 2. Стилизуем игру с помощью CSS

### Код `style.css`:
```css
body {
    text-align: center;
    font-family: Arial, sans-serif;
    background-color: black;
    color: white;
}

#game {
    position: relative;
    width: 400px;
    height: 500px;
    border: 2px solid white;
    margin: auto;
    background-color: darkblue;
    overflow: hidden;
}

#player {
    position: absolute;
    bottom: 10px;
    left: 180px;
    width: 40px;
    height: 40px;
    background-color: cyan;
}

.laser {
    position: absolute;
    width: 5px;
    height: 20px;
    background-color: red;
}

.meteor {
    position: absolute;
    width: 30px;
    height: 30px;
    background-color: orange;
}
```

### Разбор кода:
1. Делаем фон страницы черным, текст — белым.
2. `#game` — это игровое поле с тёмно-синим фоном и белой рамкой.
3. `#player` — это корабль игрока, цвет `cyan`.
4. `.laser` — лазеры, вылетающие из корабля.
5. `.meteor` — метеориты, которые нужно сбивать.

---

## 3. Добавляем игровую логику на JavaScript

### Код `script.js`:
```js
const game = document.getElementById("game");
const player = document.getElementById("player");
const scoreDisplay = document.getElementById("score");
const livesDisplay = document.createElement("p");
document.body.insertBefore(livesDisplay, game);

let playerX = 180; // Позиция игрока
const speed = 10; // Скорость перемещения
let score = 0; // Очки
let lives = 3; // Количество жизней
let meteorSpeed = 5; // Начальная скорость метеоритов
let meteorInterval = 2000; // Интервал появления метеоритов

livesDisplay.textContent = `❤️ Жизни: ${lives}`;
```

### Разбор кода:
1. Получаем ссылки на элементы HTML.
2. Создаём переменные для хранения состояния игры: позиция игрока, скорость, очки, жизни, скорость метеоров.
3. Отображаем количество жизней.

---

## 4. Реализуем управление игроком
```js
// Управление кораблём
document.addEventListener("keydown", (event) => {
    if (event.key === "ArrowLeft" && playerX > 0) {
        playerX -= speed;
    }
    if (event.key === "ArrowRight" && playerX < 360) {
        playerX += speed;
    }
    if (event.key === " ") {
        shootLaser(); // Выстрел
    }
    player.style.left = playerX + "px";
});
```

### Разбор кода:
1. Отслеживаем нажатия клавиш.
2. Двигаем корабль влево и вправо.
3. При нажатии пробела вызываем `shootLaser()` для стрельбы.

---

## 5. Создаём лазеры и метеориты
```js

// Создание выстрела (лазера)
function shootLaser() {
    let laser = document.createElement("div");
    laser.classList.add("laser");
    laser.style.left = (playerX + 18) + "px";
    laser.style.bottom = "50px";
    game.appendChild(laser);

    let laserInterval = setInterval(() => {
        let laserBottom = parseInt(laser.style.bottom);
        if (laserBottom > 500) {
            clearInterval(laserInterval);
            laser.remove();
        } else {
            laser.style.bottom = (laserBottom + 10) + "px";
        }
    }, 50);
}
```

### Разбор кода:
1. Создаём лазер.
2. Устанавливаем позицию выстрела.
3. Добавляем его в игру.

---

## 6. Добавляем метеориты и их падение
```js
// Создание метеоритов
function createMeteor() {
    let meteor = document.createElement("div");
    meteor.classList.add("meteor");

    // Рандомный шанс сделать супер-метеорит
    let isSuper = Math.random() < 0.2;
    if (isSuper) {
        meteor.classList.add("super-meteor");
        meteor.style.backgroundColor = "red"; // Супер-метеорит красный
    }

    meteor.style.left = Math.random() * 370 + "px";
    meteor.style.top = "0px";
    game.appendChild(meteor);

    let meteorInterval = setInterval(() => {
        let meteorTop = parseInt(meteor.style.top);
        if (meteorTop > 500) {
            clearInterval(meteorInterval);
            meteor.remove();
        } else {
            meteor.style.top = (meteorTop + meteorSpeed) + "px"; // Скорость падения
        }

        document.querySelectorAll(".laser").forEach((laser) => {
            if (isColliding(laser, meteor)) {
                score++;
                scoreDisplay.textContent = score;

                // Уничтожаем метеорит с эффектом взрыва
                explode(meteor);

                laser.remove();
                meteor.remove();
                increaseDifficulty();
            }
        });

// Функция взрыва метеорита
function explode(meteor) {
    meteor.style.backgroundColor = "yellow";
    setTimeout(() => meteor.remove(), 300);
}

```

### Разбор кода:
1. Создаём метеорит.
2. Делаем его падение случайным.
3. Добавляем в игру.

---

## 7. Добавляем столкновения и очки
```js
       // Столкновение с игроком
        if (isColliding(meteor, player)) {
            lives--; // Отнимаем жизнь
            livesDisplay.textContent = `❤️ Жизни: ${lives}`;
            meteor.remove();

            if (lives <= 0) {
                alert("Ты проиграл! Очки: " + score);
                location.reload();
            }
        }
    }, 50);
}

// Проверка столкновений
function isColliding(a, b) {
    let aRect = a.getBoundingClientRect();
    let bRect = b.getBoundingClientRect();
    return !(
        aRect.top > bRect.bottom ||
        aRect.bottom < bRect.top ||
        aRect.left > bRect.right ||
        aRect.right < bRect.left
    );
}

```

### Разбор кода:
1. Проверяем столкновения лазеров с метеоритами.
2. Удаляем метеорит, если он был сбит.
3. Начисляем очки.

---


## 8. Добавляем усложнение по мере игры
```js
     function increaseDifficulty() {
    if (score % 5 === 0) { // Каждые 5 очков игра усложняется
        meteorSpeed += 1;
        if (meteorInterval > 500) {
            meteorInterval -= 200;
        }
        clearInterval(meteorCreation);
        meteorCreation = setInterval(createMeteor, meteorInterval);
    }
}

```

---

## 8. Добавляем бонусы
```js

// Создание бонусов
function createBonus() {
    let bonus = document.createElement("div");
    bonus.classList.add("bonus");
    bonus.style.left = Math.random() * 370 + "px";
    bonus.style.top = "0px";
    game.appendChild(bonus);

    let bonusInterval = setInterval(() => {
        let bonusTop = parseInt(bonus.style.top);
        if (bonusTop > 500) {
            clearInterval(bonusInterval);
            bonus.remove();
        } else {
            bonus.style.top = (bonusTop + 3) + "px";
        }

        if (isColliding(bonus, player)) {
            let bonusType = Math.random() < 0.5 ? "shield" : "slow";
            applyBonus(bonusType);
            bonus.remove();
        }
    }, 50);
}

// Применение бонуса
function applyBonus(type) {
    if (type === "shield") {
        lives++; // Добавляем жизнь
        livesDisplay.textContent = `❤️ Жизни: ${lives}`;
    } else if (type === "slow") {
        meteorSpeed = Math.max(3, meteorSpeed - 2); // Замедляем метеориты
        setTimeout(() => meteorSpeed += 2, 5000); // Через 5 сек возвращаем скорость
    }
}

// Запуск метеоритов и бонусов
let meteorCreation = setInterval(createMeteor, meteorInterval);
setInterval(createBonus, 10000); // Бонус появляется раз в 10 сек
```

### Разбор кода:
1. Иногда создаётся бонусный объект.
2. Бонусы могут добавлять жизни или замедлять метеориты.

---

Поздравляю! 🎉 Вы создали свою первую браузерную игру! Попробуйте изменить код, добавить новые механики и улучшить игру!





