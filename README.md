<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Выбор даты и времени</title>
    <!-- Подключаем официальный скрипт Telegram -->
    <script src="https://telegram.org"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--tg-theme-bg-color, #ffffff);
            color: var(--tg-theme-text-color, #000000);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        label { font-weight: bold; font-size: 14px; }
        input, select {
            width: 100%;
            padding: 12px;
            border: 1px solid var(--tg-theme-hint-color, #ccc);
            border-radius: 8px;
            background-color: var(--tg-theme-secondary-bg-color, #f0f0f0);
            color: var(--tg-theme-text-color, #000000);
            box-sizing: border-box;
            font-size: 16px;
        }
        button {
            margin-top: 10px;
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 8px;
            background-color: var(--tg-theme-button-color, #2481cc);
            color: var(--tg-theme-button-text-color, #ffffff);
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <label for="date">Выберите дату:</label>
    <input type="date" id="date" required>

    <label for="time">Выберите время:</label>
    <select id="time">
        <option value="10:00">10:00</option>
        <option value="12:00">12:00</option>
        <option value="14:00">14:00</option>
        <option value="16:00">16:00</option>
        <option value="18:00">18:00</option>
        <option value="20:00">20:00</option>
    </select>

    <button id="sendData">Подтвердить слот</button>

    <script>
        // Инициализируем Telegram Web App
        const tg = window.Telegram.WebApp;
        tg.ready();
        tg.expand(); // Расширяем на весь экран

        // Устанавливаем минимальную дату — сегодня
        const today = new Date().toISOString().split('T')[0];
        document.getElementById('date').min = today;
        document.getElementById('date').value = today;

        document.getElementById('sendData').addEventListener('click', () => {
            const rawDate = document.getElementById('date').value; // ГГГГ-ММ-ДД
            const time = document.getElementById('time').value;

            if (!rawDate) {
                alert("Пожалуйста, выберите дату");
                return;
            }

            // Форматируем дату в привычный вашему боту формат ДД.ММ.ГГГГ
            const [year, month, day] = rawDate.split('-');
            const formattedDate = `${day}.${month}.${year}`;

            // Формируем JSON-строку для передачи в бот
            const data = JSON.stringify({
                date: formattedDate,
                time: time
            });

            // Отправляем данные боту и закрываем приложение
            tg.sendData(data);
            tg.close();
        });
    </script>
</body>
</html>
