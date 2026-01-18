
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Голосование</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
        }
        .container {
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .options {
            margin: 20px 0;
        }
        .option {
            margin: 10px 0;
            padding: 10px;
            border: 1px solid #eee;
            border-radius: 4px;
            display: flex;
            align-items: center;
        }
        .option input {
            margin-right: 10px;
        }
        button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #0056b3;
        }
        .results {
            margin-top: 20px;
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 4px;
        }
        .result-item {
            margin: 10px 0;
            padding: 8px;
            background-color: #e9ecef;
            border-radius: 4px;
            opacity: 1;
            transition: opacity 0.3s ease;
        }
        /* Плавное появление при обновлении */
        .fade-in {
            opacity: 0;
            animation: fadeIn 0.5s forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Опрос: Какой ваш любимый цвет?</h1>
        
        <div class="options">
            <div class="option">
                <input type="radio" id="red" name="color" value="red">
                <label for="red">Красный</label>
            </div>
            <div class="option">
                <input type="radio" id="blue" name="color" value="blue">
                <label for="blue">Синий</label>
            </div>
            <div class="option">
                <input type="radio" id="green" name="color" value="green">
                <label for="green">Зелёный</label>
            </div>
            <div class="option">
                <input type="radio" id="yellow" name="color" value="yellow">
                <label for="yellow">Жёлтый</label>
            </div>
        </div>
        
        <button onclick="submitVote()">Проголосовать</button>
        
        <div class="results" id="results">
            <h3>Результаты голосования:</h3>
            <div id="result-list"></div>
        </div>
    </div>

    <script>
        // Инициализация данных голосования
        const votes = JSON.parse(localStorage.getItem('votes')) || {
            red: 0,
            blue: 0,
            green: 0,
            yellow: 0
        };

        // Текущее состояние результатов (для сравнения при обновлении)
        let currentResults = { ...votes };

        // Функция для отображения результатов
        function displayResults() {
            const resultList = document.getElementById('result-list');
            
            // Проверяем, изменились ли данные
            const hasChanges = Object.keys(votes).some(key => votes[key] !== currentResults[key]);
            
            if (!hasChanges) return; // Если нет изменений — не обновляем
            
            // Обновляем текущее состояние
            currentResults = { ...votes };
            
            // Очищаем контейнер
            resultList.innerHTML = '';
            
            for (const [color, count] of Object.entries(votes)) {
                const item = document.createElement('div');
                item.className = 'result-item fade-in';
                
                let colorName = '';
                switch(color) {
                    case 'red': colorName = 'Красный'; break;
                    case 'blue': colorName = 'Синий'; break;
                    case 'green': colorName = 'Зелёный'; break;
                    case 'yellow': colorName = 'Жёлтый'; break;
                }
                
                item.textContent = `${colorName}: ${count} голосов`;
                resultList.appendChild(item);
            }
        }

        // Функция для отправки голоса
        function submitVote() {
            const selectedOption = document.querySelector('input[name="color"]:checked');
            
            if (!selectedOption) {
                alert('Пожалуйста, выберите вариант!');
                return;
            }
            
            const choice = selectedOption.value;
            votes[choice]++;
            
            // Сохраняем в локальное хранилище
            localStorage.setItem('votes', JSON.stringify(votes));
            
            // Сразу обновляем интерфейс
            displayResults();
            
            alert('Ваш голос учтен!');
        }

        // Автоматическое обновление каждую секунду
        setInterval(() => {
            // Перечитываем данные из localStorage (на случай, если они изменились в другой вкладке/окне)
            const storedVotes = JSON.parse(localStorage.getItem('votes'));
            if (storedVotes) {
                Object.assign(votes, storedVotes);
            }
            displayResults();
        }, 1000);

        // При загрузке страницы показываем текущие результаты
        window.onload = displayResults;
    </script>
</body>
</html>
