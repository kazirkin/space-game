🚀 **Создаём космическую игру шаг за шагом!** 🌟

Привет, будущий разработчик! Сегодня мы напишем **крутую браузерную игру**, где ты будешь управлять **космическим кораблём**, уклоняться от метеоритов и зарабатывать очки! Ты сможешь **копировать код** и сразу запускать игру. Если захочешь понять, как всё работает — читай комментарии! 😉

---

## **🔹 1. Подготовка HTML**

Создай файл `index.html` и вставь в него код:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Космическая игра</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Космическая игра 🚀</h1>
    <div id="game">
        <div class="player" id="player"></div>
    </div>
    <script src="game.js"></script>
</body>
</html>
```

Этот файл создаёт **игровое поле** и **космический корабль**. ✨

---

## **🔹 2. Подключаем стили CSS**

Создай файл `styles.css` и вставь в него этот код:

```css
body {
    background-color: black;
    color: white;
    text-align: center;
}

#game {
    width: 400px;
    height: 500px;
    border: 2px solid white;
    position: relative;
    margin: auto;
    overflow: hidden;
    background-color: navy;
}

.player {
    width: 40px;
    height: 40px;
    background-color: yellow;
    position: absolute;
    bottom: 10px;
    left: 180px;
}

.meteor {
    width: 30px;
    height: 30px;
    background-color: red;
    position: absolute;
    top: 0;
}
```

Теперь у нас есть **визуальное оформление** игры. 🔥

---

## **🔹 3. Управление кораблём**

Создай файл `game.js` и добавь в него этот код:

```js
const player = document.getElementById("player");
let playerX = 180;
const speed = 10;

document.addEventListener("keydown", (event) => {
    if (event.key === "ArrowLeft" && playerX > 0) {
        playerX -= speed;
    }
    if (event.key === "ArrowRight" && playerX < 360) {
        playerX += speed;
    }
    player.style.left = playerX + "px";
});
```

📌 **Что здесь происходит?**
- `keydown` — отслеживает нажатия клавиш.
- `ArrowLeft` и `ArrowRight` — перемещают корабль влево и вправо.
- Мы меняем `left` у корабля, чтобы он двигался.

Попробуй **запустить игру**, двигай корабль стрелками! 🚀

---

## **🔹 4. Добавляем метеориты**

Теперь добавим **падающие метеориты**:

```js
function createMeteor() {
    let meteor = document.createElement("div");
    meteor.classList.add("meteor");
    meteor.style.left = Math.random() * 370 + "px";
    meteor.style.top = "0px";
    document.getElementById("game").appendChild(meteor);

    let fallInterval = setInterval(() => {
        let meteorTop = parseInt(meteor.style.top);
        if (meteorTop > 500) {
            clearInterval(fallInterval);
            meteor.remove();
        } else {
            meteor.style.top = (meteorTop + 5) + "px";
        }
    }, 50);
}

setInterval(createMeteor, 2000);
```

📌 **Что делает этот код?**
- `createMeteor()` создаёт метеориты в случайных местах.
- `setInterval()` двигает их вниз каждую секунду.
- Если метеорит выходит за экран, он удаляется.

Запусти код и увидишь, как **метеориты падают!** 🌠

---

## **🔹 5. Добавляем стрельбу**

Теперь корабль сможет стрелять **лазером**:

```js
function shootLaser() {
    let laser = document.createElement("div");
    laser.classList.add("laser");
    laser.style.left = (playerX + 18) + "px";
    laser.style.bottom = "50px";
    document.getElementById("game").appendChild(laser);

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

document.addEventListener("keydown", (event) => {
    if (event.key === " ") {
        shootLaser();
    }
});
```

📌 **Как это работает?**
- Создаём `div` для лазера.
- Каждые 50 мс двигаем его вверх.
- Если лазер улетает за экран — удаляем его.
- Стреляем **по пробелу**!

Попробуй **подстрелить метеориты!** 🔥

---

## **🎉 Поздравляю! Ты сделал свою игру!** 🎮

Попробуй **доработать её**: добавь **жизни, бонусы, музыку**! Если есть вопросы — спрашивай! 😃

