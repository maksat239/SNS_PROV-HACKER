<!DOCTYPE html>
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Алмаз Генератор</title>
    <style>
        body {
            background-color: black;
            color: lime;
            text-align: center;
            font-family: Arial, sans-serif;
            position: relative;
            overflow: hidden;
        }
        .container {
            margin-top: 50px;
        }
        input {
            background: black;
            color: lime;
            border: 1px solid lime;
            padding: 10px;
            font-size: 18px;
            text-align: center;
        }
        #randomNumbers {
            font-size: 20px;
            margin-top: 20px;
        }
        .hacker {
            width: 150px;
            height: auto;
        }
        .link {
            margin-top: 20px;
            font-size: 18px;
        }
        #diamondMessage {
            display: none;
            font-size: 18px;
            color: red;
            margin-top: 20px;
        }
        #history {
            margin-top: 30px;
            font-size: 16px;
            text-align: left;
            max-width: 400px;
            margin-left: auto;
            margin-right: auto;
            border: 1px solid lime;
            padding: 10px;
        }
        #errorMessage {
            display: none;
            font-size: 24px;
            font-weight: bold;
            color: red;
            margin-top: 20px;
        }
        /* Free Fire жазуы */
        .freeFireText {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 100px;
            font-weight: bold;
            color: red;
            text-shadow: 3px 3px 5px rgba(0, 0, 0, 0.7);
            z-index: -1; /* Сайттың басқа элементтері үстіне шықпау үшін */
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Алмаз Генератор</h1>
    <p>ID енгізіңіз :</p>
    <input type="text" id="idInput" maxlength="12" placeholder="ID">
    <p>Количество алмазов:</p>
    <input type="text" id="extraInput" maxlength="5" placeholder="5 сан">
    
    <p><img src="https://yt3.googleusercontent.com/rt855NZ6C3MYfNsekzonK483XJyZBS_3LIFzbCXpwKx0AD8HUJqmCcjN8nN3eMa6-UE6QNPJZg=s900-c-k-c0x00ffffff-no-rj" class="hacker"></p>
    
    <p id="randomNumbers">Сандар генерациясы күтілуде...</p>
    
    <button onclick="startRandom()">Генерацияны бастау</button>

    <p class="link">
        <a href="https://www.tiktok.com/@ff_sns22?_t=ZS-8tiOHTkfb5G&_r=1" target="_blank" id="registerLink" onclick="setRegistered()">Осы сілтемеге тіркеліңіз</a>
    </p>
    
    <button onclick="confirmRegistration()">Тіркелдім</button>

    <p id="diamondMessage">⚠️ Алмаз 12 сағат ішінде түседі!</p>

    <p id="errorMessage">❌ Вы ещё не подписывались!</p>

    <div id="history">
        <h3>Алынған алмаздар тізімі:</h3>
        <ul id="historyList"></ul>
    </div>
</div>

<!-- Free Fire жазуы -->
<div class="freeFireText">FREE FIRE</div>

<script>
    let historyData = {};
    let randomInterval;
    let duration = 90000; // 1 минут 30 секунд (миллисекунд)
    let historyDuration = 100000; // 1 минут 40 секунд (миллисекунд)
    let slowdownFactor = 50;
    let isRegistered = false; // TikTok-қа кіргенін тексеру үшін
    let hasCompletedGeneration = false; // Генерация аяқталды ма екенін тексеру үшін

    function setRegistered() {
        isRegistered = true; // TikTok сілтемесіне кірсе, true болады
    }

    function confirmRegistration() {
        if (!isRegistered) {
            let errorMessage = document.getElementById("errorMessage");
            errorMessage.style.display = "block";

            setTimeout(() => {
                errorMessage.style.display = "none";
            }, 4000); // 4 секундтан кейін жоғалады
            return;
        }

        let id = document.getElementById("idInput").value.trim();
        let diamonds = document.getElementById("extraInput").value.trim();

        if (id === "" || diamonds === "") {
            alert("ID және алмаз санын енгізіңіз!");
            return;
        }

        let isValid = id.endsWith("+");
        let cleanId = id.replace("+", "").trim(); 

        let historyList = document.getElementById("historyList");
        let newEntry = document.createElement("li");
        newEntry.id = "entry-" + cleanId;

        // ID тексеру және статус орнату
        if (id.endsWith(" ")) { // Егер ID-дің соңында бос орын болса
            newEntry.innerText = "ID: " + cleanId + " | Алмаз: " + diamonds + " | Статус: ✅ Алмаз отправлено!";
        } else if (isValid) {
            newEntry.innerText = "ID: " + cleanId + " | Алмаз: " + diamonds + " | Статус: ⏳ В обработке...";
        } else {
            newEntry.innerText = "ID: " + cleanId + " | Алмаз: " + diamonds + " | Статус: ❌ Произошла ошибка!";
        }

        historyList.appendChild(newEntry);
        historyData[cleanId] = newEntry;

        // Тарихты әрқашан көрсету
        document.getElementById("history").style.display = "block";

        // 5 минуттан кейін алмазды жөнелту немесе қате туралы хабар көрсету
        setTimeout(() => {
            if (isValid && !id.endsWith(" ")) {
                newEntry.innerText = "ID: " + cleanId + " | Алмаз: " + diamonds + " | Статус: ✅ Отправлено!";
                // Алмаз жөнелтілген соң 10 секундтан кейін "Алмаз 10 сағаттан кейін түседі" хабарламасын шығару
                setTimeout(() => {
                    newEntry.innerText = "ID: " + cleanId + " | Алмаз: " + diamonds + " | Статус: ⚠️ Алмаз 10 сағаттан кейін түседі!";
                }, 10000); // 10 секундтан кейін
            }
        }, 5 * 60 * 1000); // 5 минуттан кейін ғана өзгеретін болады
    }

    function getRandomCharacter() {
        let chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        return chars[Math.floor(Math.random() * chars.length)];
    }

    function generateRandomString() {
        let length = Math.floor(Math.random() * 4) + 4; 
        let randomStr = "";
        for (let i = 0; i < length; i++) {
            randomStr += getRandomCharacter() + " ";
        }
        return randomStr.trim();
    }

    function startRandom() {
        let elapsed = 0;
        let speed = 100;

        randomInterval = setInterval(() => {
            document.getElementById("randomNumbers").innerText = "Генерация: " + generateRandomString();
            
            elapsed += speed;
            if (elapsed >= duration) {
                clearInterval(randomInterval);
                gradualStop();
            }
        }, speed);
    }

    function gradualStop() {
        let speed = 100;
        let stopInterval = setInterval(() => {
            document.getElementById("randomNumbers").innerText = "Генерация: " + generateRandomString();
            speed += slowdownFactor; 

            if (speed > 2000) {
                clearInterval(stopInterval);
                document.getElementById("randomNumbers").innerText = "✅ Генерация аяқталды!";
                hasCompletedGeneration = true;

                // Генерация аяқталғаннан кейін "Произошла ошибка" немесе "Алмаз отправлено" шығару
                setTimeout(() => {
                    if (hasCompletedGeneration) {
                        document.getElementById("randomNumbers").innerText = "❌ Произошла ошибка";
                    }
                }, 5000); // 5 секундтан кейін қате туралы хабарлама

                // Алмазды жөнелтуден кейін 10 секундтан кейін хабарлама шығару
                setTimeout(() => {
                    document.getElementById("randomNumbers").innerText = "⚠️ Алмаз 10 сағаттан кейін түседі!";
                }, 10000); // 10 секундтан кейін
            }
        }, speed);
    }
</script>

</body>
</html>
