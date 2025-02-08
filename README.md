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
        #diamondMessage, #errorMessage {
            display: none;
            font-size: 18px;
            color: red;
            margin-top: 20px;
        }
        #errorMessage {
            font-size: 24px;
            font-weight: bold;
        }
        .freeFireText {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 100px;
            font-weight: bold;
            color: red;
            text-shadow: 3px 3px 5px rgba(0, 0, 0, 0.7);
            z-index: -1;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Алмаз Генератор</h1>
    <p>ID енгізіңіз:</p>
    <input type="text" id="idInput" maxlength="12" placeholder="ID">
    <p>Алмаздың саны:</p>
    <input type="text" id="extraInput" maxlength="5" placeholder="5 сан">
    
    <p><img src="https://yt3.googleusercontent.com/rt855NZ6C3MYfNsekzonK483XJyZBS_3LIFzbCXpwKx0AD8HUJqmCcjN8nN3eMa6-UE6QNPJZg=s900-c-k-c0x00ffffff-no-rj" class="hacker"></p>
    
    <p id="randomNumbers">Генерация күтілуде...</p>

    <p class="link">
        <a href="https://www.tiktok.com/@ff_sns22?_t=ZS-8tiOHTkfb5G&_r=1" target="_blank" id="registerLink" onclick="setRegistered()">Осы сілтемеге тіркеліңіз</a>
    </p>
    
    <button onclick="confirmRegistration()">Тіркелдім</button>
    <button id="completeButton" style="display: none; margin-top: 10px;" onclick="completeOperation()">Операцияны аяқтау</button>

    <p id="diamondMessage">⚠️ Алмаз 12 сағат ішінде түседі!</p>
    <p id="errorMessage">❌ Вы ещё не подписывались!</p>

    <div id="history">
        <h3>Алынған алмаздар тізімі:</h3>
        <ul id="historyList"></ul>
    </div>
</div>

<div class="freeFireText">FREE FIRE</div>

<script>
    let isRegistered = false;
    let registerClicked = false;
    let isGenerationComplete = false;
    let historyData = {};
    let randomInterval;

    function setRegistered() {
        isRegistered = true;
    }

    function confirmRegistration() {
        if (registerClicked) {
            document.getElementById("errorMessage").innerText = "❌ Произошла ошибка, повторите позже!";
            document.getElementById("errorMessage").style.display = "block";
            
            setTimeout(() => {
                document.getElementById("errorMessage").style.display = "none";
                location.reload();
            }, 4000);
            
            return;
        }

        if (!isRegistered) {
            document.getElementById("errorMessage").style.display = "block";
            setTimeout(() => {
                document.getElementById("errorMessage").style.display = "none";
            }, 4000);
            return;
        }

        registerClicked = true;

        let id = document.getElementById("idInput").value.trim();
        let diamonds = document.getElementById("extraInput").value.trim();

        if (id === "" || diamonds === "") {
            alert("ID және алмаз санын енгізіңіз!");
            return;
        }

        let historyList = document.getElementById("historyList");
        let newEntry = document.createElement("li");
        newEntry.id = "entry-" + id;
        newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: ⏳ В обработке...";
        historyList.appendChild(newEntry);
        historyData[id] = newEntry;
        document.getElementById("history").style.display = "block";
        
        startRandomGeneration();
    }

    function startRandomGeneration() {
        let elapsed = 0;
        let speed = 100;

        randomInterval = setInterval(() => {
            document.getElementById("randomNumbers").innerText = "Генерация: " + generateRandomString();
            elapsed += speed;

            if (elapsed >= 60000) {
                clearInterval(randomInterval);
                document.getElementById("randomNumbers").innerText = "✅ Генерация аяқталды!";
                isGenerationComplete = true;
                document.getElementById("completeButton").style.display = "block";
            }
        }, speed);
    }

    function generateRandomString() {
        let chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        let randomStr = "";
        let length = Math.floor(Math.random() * 4) + 4;
        for (let i = 0; i < length; i++) {
            randomStr += chars[Math.floor(Math.random() * chars.length)] + " ";
        }
        return randomStr.trim();
    }

    function completeOperation() {
        if (!isGenerationComplete) {
            alert("Генерация жүріп жатыр...");
            return;
        }

        let id = document.getElementById("idInput").value.trim();
        let diamonds = document.getElementById("extraInput").value.trim();
        let newEntry = document.getElementById("entry-" + id);

        if (!id.endsWith(" ")) {
            newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: ❌ Неверный данные!";
        } else {
            newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: ✅ Алмаз отправлено!";
        }

        document.getElementById("completeButton").style.display = "none";
    }
</script>

</body>
</html>
