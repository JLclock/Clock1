<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Relógio Mundial</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(to right, #000428, #004e92);
            color: white;
            font-family: 'Arial', sans-serif;
            margin: 0;
            text-align: center;
        }
        .clock-container {
            text-align: center;
            width: 90vw;
            max-width: 600px;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
        }
        .time {
            font-size: 100px;
            font-weight: bold;
            text-shadow: 4px 4px 10px rgba(0, 0, 0, 0.5);
        }
        .select-container {
            margin-top: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        select {
            font-size: 18px;
            padding: 10px;
            border-radius: 5px;
            background: #333;
            color: white;
            border: none;
            margin: 5px;
        }
        h2 {
            font-size: 30px;
            text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.5);
        }
    </style>
</head>
<body>
    <div class="clock-container">
        <h2 id="title">Relógio Mundial</h2>
        <div class="time" id="clock">00:00:00</div>
        <div class="select-container">
            <select id="language"></select>
            <select id="timezone"></select>
            <select id="hourFormat">
                <option value="24">24h</option>
                <option value="12">12h</option>
            </select>
        </div>
    </div>
    <script>
        const translations = {
            pt: { title: "Relógio Mundial", zones: { "UTC": "UTC", "America/New_York": "Nova York (UTC-5)", "Europe/London": "Londres (UTC+0)", "Asia/Tokyo": "Tóquio (UTC+9)", "America/Sao_Paulo": "São Paulo (UTC-3)" } },
            en: { title: "World Clock", zones: { "UTC": "UTC", "America/New_York": "New York (UTC-5)", "Europe/London": "London (UTC+0)", "Asia/Tokyo": "Tokyo (UTC+9)", "America/Sao_Paulo": "São Paulo (UTC-3)" } }
        };

        function populateSelects() {
            const langSelect = document.getElementById("language");
            const tzSelect = document.getElementById("timezone");
            langSelect.innerHTML = '';
            tzSelect.innerHTML = '';
            Object.keys(translations).forEach(lang => {
                let option = new Option(lang.toUpperCase(), lang);
                langSelect.add(option);
            });
            updateLanguage();
        }
        
        function updateClock() {
            const timezone = document.getElementById("timezone").value;
            const format = document.getElementById("hourFormat").value;
            const now = new Date();
            const options = { timeZone: timezone, hour: "2-digit", minute: "2-digit", second: "2-digit", hour12: format === "12" };
            document.getElementById("clock").textContent = new Intl.DateTimeFormat("pt-BR", options).format(now);
            localStorage.setItem("timezone", timezone);
            localStorage.setItem("hourFormat", format);
        }

        function updateLanguage() {
            const selectedLang = document.getElementById("language").value;
            document.getElementById("title").textContent = translations[selectedLang].title;
            const tzSelect = document.getElementById("timezone");
            tzSelect.innerHTML = '';
            Object.keys(translations[selectedLang].zones).forEach(zone => {
                let option = new Option(translations[selectedLang].zones[zone], zone);
                tzSelect.add(option);
            });
            localStorage.setItem("language", selectedLang);
            updateClock();
        }

        document.getElementById("timezone").addEventListener("change", updateClock);
        document.getElementById("language").addEventListener("change", updateLanguage);
        document.getElementById("hourFormat").addEventListener("change", updateClock);
        
        window.onload = function() {
            populateSelects();
            document.getElementById("language").value = localStorage.getItem("language") || "pt";
            document.getElementById("hourFormat").value = localStorage.getItem("hourFormat") || "24";
            updateLanguage();
            document.getElementById("timezone").value = localStorage.getItem("timezone") || "UTC";
            updateClock();
            setInterval(updateClock, 1000);
        };
    </script>
</body>
</html>
