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
        button {
            background: lime;
            color: black;
            border: none;
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
            margin: 5px;
        }
        #randomNumbers {
            font-size: 20px;
            margin-top: 20px;
        }
        .hacker {
            width: 150px;
            height: auto;
        }
        #diamondMessage, #errorMessage {
            display: none;
            font-size: 18px;
            margin-top: 20px;
        }
        #errorMessage {
            color: red;
            font-size: 24px;
            font-weight: bold;
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

    <div class="button-container">
        <button onclick="confirmRegistration()">Тіркелдім</button>
        <button id="completeButton" style="display: none;" onclick="completeOperation()">Операцияны аяқтау</button>
    </div>

    <p id="diamondMessage">⚠️ Алмаз 12 сағат ішінде түседі!</p>
    <p id="errorMessage">❌ Вы ещё не подписывались!</p>

    <div id="history">
        <h3>Алынған алмаздар тізімі:</h3>
        <ul id="historyList"></ul>
    </div>
</div>

<script>
    let isRegistered = false;
    let registerClicked = false;
    let isGenerationComplete = false;
    let historyData = {};
    let completeButtonClickCount = 0;
    let randomInterval;

    function confirmRegistration() {
        if (!isRegistered) {
            document.getElementById("errorMessage").innerText = "❌ Вы ещё не подписывались!";
            document.getElementById("errorMessage").style.display = "block";
            setTimeout(() => {
                document.getElementById("errorMessage").style.display = "none";
            }, 4000);
            return;
        }

        let id = document.getElementById("idInput").value.trim();
        let diamonds = document.getElementById("extraInput").value.trim();

        if (id === "" || diamonds === "") {
            document.getElementById("errorMessage").innerText = "❌ Неверные данные!";
            document.getElementById("errorMessage").style.display = "block";
            setTimeout(() => {
                document.getElementById("errorMessage").style.display = "none";
            }, 4000);
            return;
        }

        registerClicked = true;

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

            if (elapsed >= 5000) { // 5 секунд
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

        let id = document.getElementById("idInput").value;
        let diamonds = document.getElementById("extraInput").value.trim();
        let newEntry = document.getElementById("entry-" + id.trim());

        completeButtonClickCount++;

        if (completeButtonClickCount === 1) {
            newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: ❌ Неверные данные!";
        } else if (completeButtonClickCount === 2) {
            newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: Ваши алмазы отправлены будет через 10 часов";
        }

        document.getElementById("completeButton").style.display = "block";
    }

    function setRegistered() {
        isRegistered = true;
    }
</script>

</body>
</html>
