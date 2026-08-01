<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>200+ Flags Battle Simulation</title>
    <style>
        body {
            background-color: #0d1b2a;
            color: #fff;
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 10px;
        }
        h1 {
            color: #00f5d4;
            margin-bottom: 5px;
            font-size: 24px;
        }
        #status {
            font-size: 16px;
            color: #ff9f1c;
            margin-bottom: 10px;
        }
        .container {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
            align-items: flex-start;
        }
        .arena {
            width: 500px;
            height: 500px;
            border: 5px solid #48cae4;
            border-radius: 50%;
            position: relative;
            background-color: #1b263b;
            overflow: hidden;
            box-shadow: 0 0 25px rgba(72, 202, 228, 0.5);
        }
        .flag-item {
            position: absolute;
            width: 30px;
            height: 20px;
            font-size: 18px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
        }
        .scoreboard {
            width: 280px;
            background: #1b263b;
            border: 2px solid #48cae4;
            border-radius: 10px;
            padding: 15px;
            text-align: left;
            height: 500px;
            overflow-y: auto;
        }
        .scoreboard h3 {
            margin-top: 0;
            color: #ffb703;
            border-bottom: 1px solid #48cae4;
            padding-bottom: 5px;
            font-size: 16px;
        }
        .score-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 6px;
            font-size: 13px;
        }
    </style>
</head>
<body>

    <h1>200+ FLAGS BATTLE SIMULATOR</h1>
    <div id="status">Status: Battle in Progress (Auto-Looping)</div>

    <div class="container">
        <!-- যুদ্ধের বৃত্ত বা এরিনা -->
        <div class="arena" id="arena"></div>

        <!-- লিডারবোর্ড বা বিজয়ী তালিকা -->
        <div class="scoreboard">
            <h3>🏆 Leaderboard (Wins)</h3>
            <div id="scoreList"></div>
        </div>
    </div>

    <script>
        // ২০০+ দেশের পতাকা এবং নামের তালিকা
        const flagsData = [
            { name: "Afghanistan", emoji: "🇦🇫" }, { name: "Albania", emoji: "🇦🇱" }, { name: "Algeria", emoji: "🇩🇿" },
            { name: "Andorra", emoji: "🇦🇩" }, { name: "Angola", emoji: "🇦🇴" }, { name: "Antigua and Barbuda", emoji: "🇦🇬" },
            { name: "Argentina", emoji: "🇦🇷" }, { name: "Armenia", emoji: "🇦🇲" }, { name: "Australia", emoji: "🇦🇺" },
            { name: "Austria", emoji: "🇦🇹" }, { name: "Azerbaijan", emoji: "🇦🇿" }, { name: "Bahamas", emoji: "🇧🇸" },
            { name: "Bahrain", emoji: "🇧🇭" }, { name: "Bangladesh", emoji: "🇧🇩" }, { name: "Barbados", emoji: "🇧🇧" },
            { name: "Belarus", emoji: "🇧🇾" }, { name: "Belgium", emoji: "🇧🇪" }, { name: "Belize", emoji: "🇧🇿" },
            { name: "Benin", emoji: "🇧🇯" }, { name: "Bhutan", emoji: "🇧🇹" }, { name: "Bolivia", emoji: "🇧🇴" },
            { name: "Bosnia and Herzegovina", emoji: "🇧🇦" }, { name: "Botswana", emoji: "🇧🇼" }, { name: "Brazil", emoji: "🇧🇷" },
            { name: "Brunei", emoji: "🇧🇳" }, { name: "Bulgaria", emoji: "🇧🇬" }, { name: "Burkina Faso", emoji: "🇧🇫" },
            { name: "Burundi", emoji: "🇧🇮" }, { name: "Cabo Verde", emoji: "🇨🇻" }, { name: "Cambodia", emoji: "🇰🇭" },
            { name: "Cameroon", emoji: "🇨🇲" }, { name: "Canada", emoji: "🇨🇦" }, { name: "Central African Republic", emoji: "🇨🇫" },
            { name: "Chad", emoji: "🇹🇩" }, { name: "Chile", emoji: "🇨🇱" }, { name: "China", emoji: "🇨🇳" },
            { name: "Colombia", emoji: "🇨🇴" }, { name: "Comoros", emoji: "🇰🇲" }, { name: "Congo", emoji: "🇨🇬" },
            { name: "Costa Rica", emoji: "🇨🇷" }, { name: "Croatia", emoji: "🇭🇷" }, { name: "Cuba", emoji: "🇨🇺" },
            { name: "Cyprus", emoji: "🇨🇾" }, { name: "Czech Republic", emoji: "🇨🇿" }, { name: "Denmark", emoji: "🇩🇰" },
            { name: "Djibouti", emoji: "🇩🇯" }, { name: "Dominica", emoji: "🇩🇲" }, { name: "Dominican Republic", emoji: "🇩🇴" },
            { name: "Ecuador", emoji: "🇪🇨" }, { name: "Egypt", emoji: "🇪🇬" }, { name: "El Salvador", emoji: "🇸🇻" },
            { name: "Equatorial Guinea", emoji: "🇬🇶" }, { name: "Eritrea", emoji: "🇪🇷" }, { name: "Estonia", emoji: "🇪🇪" },
            { name: "Eswatini", emoji: "🇸🇿" }, { name: "Ethiopia", emoji: "🇪🇹" }, { name: "Fiji", emoji: "🇫🇯" },
            { name: "Finland", emoji: "🇫🇮" }, { name: "France", emoji: "🇫🇷" }, { name: "Gabon", emoji: "🇬🇦" },
            { name: "Gambia", emoji: "🇬🇲" }, { name: "Georgia", emoji: "🇬🇪" }, { name: "Germany", emoji: "🇩🇪" },
            { name: "Ghana", emoji: "🇬🇭" }, { name: "Greece", emoji: "🇬🇷" }, { name: "Grenada", emoji: "🇬🇩" },
            { name: "Guatemala", emoji: "🇬🇹" }, { name: "Guinea", emoji: "🇬🇳" }, { name: "Guinea-Bissau", emoji: "🇬🇼" },
            { name: "Guyana", emoji: "🇬🇾" }, { name: "Haiti", emoji: "🇭🇹" }, { name: "Honduras", emoji: "🇭🇳" },
            { name: "Hungary", emoji: "🇭🇺" }, { name: "Iceland", emoji: "🇮🇸" }, { name: "India", emoji: "🇮🇳" },
            { name: "Indonesia", emoji: "🇮🇩" }, { name: "Iran", emoji: "🇮🇷" }, { name: "Iraq", emoji: "🇮🇶" },
            { name: "Ireland", emoji: "🇮🇪" }, { name: "Israel", emoji: "🇮🇱" }, { name: "Italy", emoji: "🇮🇹" },
            { name: "Jamaica", emoji: "🇯🇲" }, { name: "Japan", emoji: "🇯🇵" }, { name: "Jordan", emoji: "🇯🇴" },
            { name: "Kazakhstan", emoji: "🇰🇿" }, { name: "Kenya", emoji: "🇰🇪" }, { name: "Kiribati", emoji: "🇰🇮" },
            { name: "North Korea", emoji: "🇰🇵" }, { name: "South Korea", emoji: "🇰🇷" }, { name: "Kuwait", emoji: "🇰🇼" },
            { name: "Kyrgyzstan", emoji: "🇰🇬" }, { name: "Laos", emoji: "🇱🇦" }, { name: "Latvia", emoji: "🇱🇻" },
            { name: "Lebanon", emoji: "🇱🇧" }, { name: "Lesotho", emoji: "🇱🇸" }, { name: "Liberia", emoji: "🇱🇷" },
            { name: "Libya", emoji: "🇱🇾" }, { name: "Liechtenstein", emoji: "🇱🇮" }, { name: "Lithuania", emoji: "🇱🇹" },
            { name: "Luxembourg", emoji: "🇱🇺" }, { name: "Madagascar", emoji: "🇲🇬" }, { name: "Malawi", emoji: "🇲🇼" },
            { name: "Malaysia", emoji: "🇲🇾" }, { name: "Maldives", emoji: "🇲🇻" }, { name: "Mali", emoji: "🇲🇱" },
            { name: "Malta", emoji: "🇲🇹" }, { name: "Marshall Islands", emoji: "🇲🇭" }, { name: "Mauritania", emoji: "🇲🇷" },
            { name: "Mauritius", emoji: "🇲🇺" }, { name: "Mexico", emoji: "🇲🇽" }, { name: "Micronesia", emoji: "🇫🇲" },
            { name: "Moldova", emoji: "🇲🇩" }, { name: "Monaco", emoji: "🇲🇨" }, { name: "Mongolia", emoji: "🇲🇳" },
            { name: "Montenegro", emoji: "🇲🇪" }, { name: "Morocco", emoji: "🇲🇦" }, { name: "Mozambique", emoji: "🇲🇿" },
            { name: "Myanmar", emoji: "🇲🇲" }, { name: "Namibia", emoji: "🇳🇦" }, { name: "Nauru", emoji: "🇳🇷" },
            { name: "Nepal", emoji: "🇳🇵" }, { name: "Netherlands", emoji: "🇳🇱" }, { name: "New Zealand", emoji: "🇳🇿" },
            { name: "Nicaragua", emoji: "🇳🇮" }, { name: "Niger", emoji: "🇳🇪" }, { name: "Nigeria", emoji: "🇳🇬" },
            { name: "North Macedonia", emoji: "🇲🇰" }, { name: "Norway", emoji: "🇳🇴" }, { name: "Oman", emoji: "🇴🇲" },
            { name: "Pakistan", emoji: "🇵🇰" }, { name: "Palau", emoji: "🇵🇼" }, { name: "Palestine", emoji: "🇵🇸" },
            { name: "Panama", emoji: "🇵🇦" }, { name: "Papua New Guinea", emoji: "🇵🇬" }, { name: "Paraguay", emoji: "🇵🇾" },
            { name: "Peru", emoji: "🇵🇪" }, { name: "Philippines", emoji: "🇵🇭" }, { name: "Poland", emoji: "🇵🇱" },
            { name: "Portugal", emoji: "🇵🇹" }, { name: "Qatar", emoji: "🇶🇦" }, { name: "Romania", emoji: "🇷🇴" },
            { name: "Russia", emoji: "🇷🇺" }, { name: "Rwanda", emoji: "🇷🇼" }, { name: "Saint Kitts and Nevis", emoji: "🇰🇳" },
            { name: "Saint Lucia", emoji: "🇱🇨" }, { name: "Samoa", emoji: "🇼🇸" }, { name: "San Marino", emoji: "🇸🇲" },
            { name: "Saudi Arabia", emoji: "🇸🇦" }, { name: "Senegal", emoji: "🇸🇳" }, { name: "Serbia", emoji: "🇷🇸" },
            { name: "Seychelles", emoji: "🇸🇨" }, { name: "Sierra Leone", emoji: "🇸🇱" }, { name: "Singapore", emoji: "🇸🇬" },
            { name: "Slovakia", emoji: "🇸🇰" }, { name: "Slovenia", emoji: "🇸🇮" }, { name: "Solomon Islands", emoji: "🇸🇧" },
            { name: "Somalia", emoji: "🇸🇴" }, { name: "South Africa", emoji: "🇿🇦" }, { name: "Spain", emoji: "🇪🇸" },
            { name: "Sri Lanka", emoji: "🇱🇰" }, { name: "Sudan", emoji: "🇸🇩" }, { name: "Suriname", emoji: "🇸🇷" },
            { name: "Sweden", emoji: "🇸🇪" }, { name: "Switzerland", emoji: "🇨🇭" }, { name: "Syria", emoji: "🇸🇾" },
            { name: "Taiwan", emoji: "🇹🇼" }, { name: "Tajikistan", emoji: "🇹🇯" }, { name: "Tanzania", emoji: "🇹🇿" },
            { name: "Thailand", emoji: "🇹🇭" }, { name: "Timor-Leste", emoji: "🇹🇱" }, { name: "Togo", emoji: "🇹🇬" },
            { name: "Tonga", emoji: "🇹🇴" }, { name: "Trinidad and Tobago", emoji: "🇹🇹" }, { name: "Tunisia", emoji: "🇹🇳" },
            { name: "Turkey", emoji: "🇹🇷" }, { name: "Turkmenistan", emoji: "🇹🇲" }, { name: "Tuvalu", emoji: "🇹🇻" },
            { name: "Uganda", emoji: "🇺🇬" }, { name: "Ukraine", emoji: "🇺🇦" }, { name: "UAE", emoji: "🇦🇪" },
            { name: "UK", emoji: "🇬🇧" }, { name: "USA", emoji: "🇺🇸" }, { name: "Uruguay", emoji: "🇺🇾" },
            { name: "Uzbekistan", emoji: "🇺🇿" }, { name: "Vanuatu", emoji: "🇻🇺" }, { name: "Vatican City", emoji: "🇻🇦" },
            { name: "Venezuela", emoji: "🇻🇪" }, { name: "Vietnam", emoji: "🇻🇳" }, { name: "Yemen", emoji: "🇾🇪" },
            { name: "Zambia", emoji: "🇿🇲" }, { name: "Zimbabwe", emoji: "🇿🇼" }
        ];

        let scores = {};
        flagsData.forEach(f => scores[f.name] = 0);

        const arena = document.getElementById('arena');
        const scoreList = document.getElementById('scoreList');
        const statusText = document.getElementById('status');

        let activeFlags = [];

        function initBattle() {
            arena.innerHTML = '';
            activeFlags = [];

            // সব দেশের পতাকা বৃত্তের ভেতর রেন্ডম পজিশনে ছড়ানো
            flagsData.forEach((data) => {
                const el = document.createElement('div');
                el.className = 'flag-item';
                el.innerText = data.emoji;
                
                // বৃত্তের ভেতরে র‍্যান্ডম কোঅর্ডিনেট (Radius ~ 210px)
                const angle = Math.random() * Math.PI * 2;
                const r = Math.random() * 200;
                const x = 235 + r * Math.cos(angle);
                const y = 235 + r * Math.sin(angle);

                el.style.left = `${x}px`;
                el.style.top = `${y}px`;

                arena.appendChild(el);

                activeFlags.push({
                    name: data.name,
                    element: el,
                    x: x,
                    y: y,
                    dx: (Math.random() - 0.5) * 5, 
                    dy: (Math.random() - 0.5) * 5
                });
            });
            updateScoreboard();
        }

        function updateBattle() {
            activeFlags.forEach(flag => {
                flag.x += flag.dx;
                flag.y += flag.dy;

                // বৃত্তের সীমানা চেক (Radius 215px)
                const dxFromCenter = flag.x - 235;
                const dyFromCenter = flag.y - 235;
                const distFromCenter = Math.sqrt(dxFromCenter * dxFromCenter + dyFromCenter * dyFromCenter);

                if (distFromCenter > 215) {
                    flag.dx *= -1;
                    flag.dy *= -1;
                }

                flag.element.style.left = `${flag.x}px`;
                flag.element.style.top = `${flag.y}px`;
            });

            // দ্রুত পতাকা কমিয়ে আনার জন্য এলিমিনেশন রেট বাড়ানো হয়েছে
            if (activeFlags.length > 1) {
                // প্রতি লুপে দ্রুত পতাকা বাদ পড়ার লজিক
                for(let i = 0; i < 2; i++) {
                    if(activeFlags.length > 1 && Math.random() < 0.3) {
                        const indexToRemove = Math.floor(Math.random() * activeFlags.length);
                        const removed = activeFlags.splice(indexToRemove, 1)[0];
                        removed.element.remove();
                    }
                }
            } else if (activeFlags.length === 1) {
                const winner = activeFlags[0];
                scores[winner.name]++;
                statusText.innerText = `🎉 Winner: ${winner.name} ${winner.element.innerText} | Restarting...`;
                updateScoreboard();
                
                setTimeout(initBattle, 3000);
                return;
            }

            requestAnimationFrame(updateBattle);
        }

        function updateScoreboard() {
            scoreList.innerHTML = '';
            const sortedScores = Object.entries(scores).sort((a, b) => b[1] - a[1]);
            
            sortedScores.forEach(([name, score]) => {
                if(score > 0) { // কেবল জয়ী দেশগুলোকে লিডারবোর্ডে দেখাবে
                    const div = document.createElement('div');
                    div.className = 'score-item';
                    div.innerHTML = `<span>${name}</span> <strong>${score} wins</strong>`;
                    scoreList.appendChild(div);
                }
            });
        }

        // খেলা শুরু
        initBattle();
        requestAnimationFrame(updateBattle);
    </script>
</body>
</html>
