<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SportAI - Shaxsiy Sport Maslahatchi</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Outfit:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #FF6B35;
            --secondary: #004E89;
            --accent: #F7B801;
            --dark: #1A1A2E;
            --light: #F4F4F9;
            --success: #00D9A3;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Outfit', sans-serif;
            background: var(--tg-theme-bg-color, linear-gradient(135deg, #1A1A2E 0%, #16213E 100%));
            color: var(--tg-theme-text-color, var(--light));
            min-height: 100vh;
            height: 100%;
            overflow-x: hidden;
            overflow-y: auto;
            margin: 0;
            padding: 0;
            -webkit-overflow-scrolling: touch;
        }
        
        html {
            height: 100%;
            overflow-x: hidden;
        }

        .background-pattern {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0.03;
            background-image: 
                repeating-linear-gradient(45deg, transparent, transparent 35px, rgba(255,255,255,.1) 35px, rgba(255,255,255,.1) 70px);
            pointer-events: none;
            z-index: 0;
        }

        .container {
            max-width: 100%;
            margin: 0;
            padding: 0.5rem;
            position: relative;
            z-index: 1;
        }

        header {
            text-align: center;
            margin-bottom: 1rem;
            padding-top: 0.25rem;
            animation: fadeInDown 0.8s ease-out;
        }

        h1 {
            font-family: 'Bebas Neue', cursive;
            font-size: 2rem;
            letter-spacing: 0.08em;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 0.2rem;
            text-shadow: 0 0 60px rgba(255, 107, 53, 0.3);
        }

        .subtitle {
            font-size: 0.75rem;
            color: #B8C1EC;
            font-weight: 300;
            letter-spacing: 0.02em;
        }

        .main-content {
            display: flex;
            flex-direction: column;
            gap: 0.75rem;
            animation: fadeInUp 1s ease-out 0.3s both;
        }

        .input-section {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 1rem;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
        }

        .form-title {
            font-family: 'Bebas Neue', cursive;
            font-size: 1.3rem;
            color: var(--accent);
            margin-bottom: 0.75rem;
            letter-spacing: 0.08em;
        }

        .form-group {
            margin-bottom: 0.75rem;
        }

        label {
            display: block;
            margin-bottom: 0.3rem;
            font-weight: 600;
            color: var(--light);
            font-size: 0.8rem;
            letter-spacing: 0.02em;
        }

        input, select {
            width: 100%;
            padding: 0.65rem;
            border: 2px solid rgba(255, 255, 255, 0.1);
            background: rgba(0, 0, 0, 0.3);
            border-radius: 8px;
            color: var(--light);
            font-size: 0.9rem;
            font-family: 'Outfit', sans-serif;
            transition: all 0.3s ease;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--primary);
            background: rgba(0, 0, 0, 0.4);
            box-shadow: 0 0 0 4px rgba(255, 107, 53, 0.1);
        }

        select option {
            background: var(--dark);
            color: var(--light);
        }

        .btn-generate {
            width: 100%;
            padding: 0.85rem;
            background: linear-gradient(135deg, var(--primary), #FF8C61);
            border: none;
            border-radius: 8px;
            color: white;
            font-size: 0.95rem;
            font-weight: 700;
            font-family: 'Bebas Neue', cursive;
            letter-spacing: 0.1em;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 20px rgba(255, 107, 53, 0.4);
            margin-top: 0.25rem;
        }

        .btn-generate:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 30px rgba(255, 107, 53, 0.6);
        }

        .btn-generate:active {
            transform: translateY(0);
        }

        .btn-generate:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .results-section {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 1rem;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
            min-height: 150px;
            display: flex;
            flex-direction: column;
        }

        .results-title {
            font-family: 'Bebas Neue', cursive;
            font-size: 1.3rem;
            color: var(--success);
            margin-bottom: 0.75rem;
            letter-spacing: 0.08em;
        }

        .loading {
            text-align: center;
            padding: 2rem;
        }

        .spinner {
            border: 3px solid rgba(255, 255, 255, 0.1);
            border-top: 3px solid var(--primary);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 0.75rem;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .recommendation-card {
            background: rgba(0, 0, 0, 0.3);
            border-left: 3px solid var(--accent);
            border-radius: 8px;
            padding: 0.75rem;
            margin-bottom: 0.75rem;
            animation: slideInRight 0.5s ease-out;
        }

        .recommendation-card h3 {
            color: var(--accent);
            font-family: 'Bebas Neue', cursive;
            font-size: 1.1rem;
            margin-bottom: 0.5rem;
            letter-spacing: 0.05em;
        }

        .recommendation-card p {
            line-height: 1.4;
            color: #E0E0E0;
            margin-bottom: 0.5rem;
            font-size: 0.85rem;
        }

        .recommendation-card ul {
            list-style: none;
            padding-left: 0;
        }

        .recommendation-card li {
            padding: 0.3rem 0;
            padding-left: 1rem;
            position: relative;
            color: #C0C0C0;
            line-height: 1.3;
            font-size: 0.8rem;
        }

        .recommendation-card li:before {
            content: "▶";
            position: absolute;
            left: 0;
            color: var(--primary);
            font-size: 0.65rem;
        }

        .stat-box {
            display: inline-block;
            background: linear-gradient(135deg, var(--secondary), #006BA6);
            padding: 0.35rem 0.7rem;
            border-radius: 5px;
            margin: 0.15rem 0.15rem 0.15rem 0;
            font-weight: 600;
            font-size: 0.7rem;
        }

        .empty-state {
            text-align: center;
            padding: 2rem;
            color: #666;
        }

        .empty-state svg {
            width: 60px;
            height: 60px;
            opacity: 0.3;
            margin-bottom: 0.75rem;
        }

        .empty-state p {
            font-size: 0.85rem;
        }

        @keyframes fadeInDown {
            from {
                opacity: 0;
                transform: translateY(-30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes slideInRight {
            from {
                opacity: 0;
                transform: translateX(30px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        @media (max-width: 400px) {
            h1 {
                font-size: 1.8rem;
            }
            
            .subtitle {
                font-size: 0.7rem;
            }
            
            .form-title, .results-title {
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>
    <div class="background-pattern"></div>
    
    <div class="container">
        <header>
            <h1>SPORTAI</h1>
            <p class="subtitle">Shaxsiy Sun'iy Intellekt Sport Maslahatchi</p>
        </header>

        <div class="main-content">
            <div class="input-section">
                <h2 class="form-title">Ma'lumotlaringizni kiriting</h2>
                
                <form id="userForm">
                    <div class="form-group">
                        <label for="age">Yoshingiz (yil)</label>
                        <input type="number" id="age" min="10" max="100" required>
                    </div>

                    <div class="form-group">
                        <label for="weight">Vazningiz (kg)</label>
                        <input type="number" id="weight" min="30" max="200" step="0.1" required>
                    </div>

                    <div class="form-group">
                        <label for="height">Bo'yingiz (sm)</label>
                        <input type="number" id="height" min="100" max="250" required>
                    </div>

                    <div class="form-group">
                        <label for="gender">Jinsingiz</label>
                        <select id="gender" required>
                            <option value="">Tanlang</option>
                            <option value="male">Erkak</option>
                            <option value="female">Ayol</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label for="activity">Faollik darajasi</label>
                        <select id="activity" required>
                            <option value="">Tanlang</option>
                            <option value="sedentary">Kam harakatli (o'tirib ishlaydigan)</option>
                            <option value="light">Engil faol (haftada 1-3 kun)</option>
                            <option value="moderate">O'rtacha faol (haftada 3-5 kun)</option>
                            <option value="active">Faol (haftada 6-7 kun)</option>
                            <option value="very_active">Juda faol (kuniga 2 marta)</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label for="goal">Maqsadingiz</label>
                        <select id="goal" required>
                            <option value="">Tanlang</option>
                            <option value="lose_weight">Vazn yo'qotish</option>
                            <option value="maintain">Vazn va sog'liqni saqlash</option>
                            <option value="gain_muscle">Mushak massasi o'stirish</option>
                            <option value="fitness">Umumiy fitnes va sog'liq</option>
                        </select>
                    </div>

                    <button type="submit" class="btn-generate" id="generateBtn">
                        Maslahat olish
                    </button>
                </form>
            </div>

            <div class="results-section">
                <h2 class="results-title">Shaxsiy tavsiyalar</h2>
                <div id="results">
                    <div class="empty-state">
                        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/>
                        </svg>
                        <p>Ma'lumotlarni kiriting va maslahat oling</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Initialize Telegram Web App
        let tg = window.Telegram.WebApp;
        tg.expand();
        tg.enableClosingConfirmation();
        
        // Set theme colors
        document.body.style.background = tg.themeParams.bg_color || 'linear-gradient(135deg, #1A1A2E 0%, #16213E 100%)';
        
        document.getElementById('userForm').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const age = parseInt(document.getElementById('age').value);
            const weight = parseFloat(document.getElementById('weight').value);
            const height = parseInt(document.getElementById('height').value);
            const gender = document.getElementById('gender').value;
            const activity = document.getElementById('activity').value;
            const goal = document.getElementById('goal').value;
            
            // Telegram haptic feedback
            if (tg.HapticFeedback) {
                tg.HapticFeedback.impactOccurred('medium');
            }
            
            const resultsDiv = document.getElementById('results');
            const generateBtn = document.getElementById('generateBtn');
            
            // Show loading
            generateBtn.disabled = true;
            generateBtn.textContent = 'Tayyorlanmoqda...';
            resultsDiv.innerHTML = `
                <div class="loading">
                    <div class="spinner"></div>
                    <p>AI shaxsiy rejangizni tayyorlayapti...</p>
                </div>
            `;
            
            // Simulate processing
            await new Promise(resolve => setTimeout(resolve, 2000));
            
            // Generate recommendations
            const recommendations = generateRecommendations(age, weight, height, gender, activity, goal);
            
            // Display results
            displayResults(recommendations, age, weight, height);
            
            // Success haptic feedback
            if (tg.HapticFeedback) {
                tg.HapticFeedback.notificationOccurred('success');
            }
            
            // Scroll to results
            setTimeout(() => {
                document.getElementById('results').scrollIntoView({ behavior: 'smooth', block: 'start' });
            }, 100);
            
            generateBtn.disabled = false;
            generateBtn.textContent = 'Maslahat olish';
        });

        function generateRecommendations(age, weight, height, gender, activity, goal) {
            const bmi = (weight / ((height/100) ** 2)).toFixed(1);
            
            // Calculate BMR (Basal Metabolic Rate)
            let bmr;
            if (gender === 'male') {
                bmr = 10 * weight + 6.25 * height - 5 * age + 5;
            } else {
                bmr = 10 * weight + 6.25 * height - 5 * age - 161;
            }
            
            // Activity multipliers
            const activityMultipliers = {
                'sedentary': 1.2,
                'light': 1.375,
                'moderate': 1.55,
                'active': 1.725,
                'very_active': 1.9
            };
            
            const tdee = Math.round(bmr * activityMultipliers[activity]);
            
            // Adjust calories based on goal
            let targetCalories = tdee;
            if (goal === 'lose_weight') {
                targetCalories = Math.round(tdee - 500);
            } else if (goal === 'gain_muscle') {
                targetCalories = Math.round(tdee + 300);
            }
            
            // BMI Analysis
            let bmiAnalysis = '';
            if (bmi < 18.5) {
                bmiAnalysis = 'Sizning tana massasi indeksingiz normaldan past. Sog\'lom ovqatlanish va kuch mashqlari orqali mushak massasini oshirish tavsiya etiladi.';
            } else if (bmi < 25) {
                bmiAnalysis = 'Sizning tana massasi indeksingiz normal holatda. Joriy holatni saqlash uchun muvozanatli ovqatlanish va muntazam sport bilan shug\'ullanishda davom eting.';
            } else if (bmi < 30) {
                bmiAnalysis = 'Sizning tana massasi indeksingiz biroz yuqori. Sog\'lom ovqatlanish va muntazam aerob mashqlar orqali vazningizni kamaytirish tavsiya etiladi.';
            } else {
                bmiAnalysis = 'Sizning tana massasi indeksingiz normaldan yuqori. Shifokor maslahati bilan dietani tuzish va bosqichma-bosqich jismoniy faollikni oshirish muhim.';
            }
            
            // Exercise recommendations based on goal
            let exercises = [];
            if (goal === 'lose_weight') {
                exercises = [
                    { name: 'Tez yurish yoki yugurish', duration: '30-40 daqiqa', muscles: 'Oyoqlar, yurak-qon tomir tizimi' },
                    { name: 'Sakrash arqoni', duration: '10-15 daqiqa', muscles: 'Butun tana, koordinatsiya' },
                    { name: 'Burpees (sakrash-tushish)', duration: '3 set x 10-15 marta', muscles: 'Butun tana, kuch va chidamlilik' },
                    { name: 'Velosiped haydash', duration: '30-45 daqiqa', muscles: 'Oyoqlar, yurak salomatligi' },
                    { name: 'Suzish', duration: '30 daqiqa', muscles: 'Butun tana mushaklari' },
                    { name: 'Planka (taxta)', duration: '3 set x 30-60 soniya', muscles: 'Qorin, bel, yelka' }
                ];
            } else if (goal === 'gain_muscle') {
                exercises = [
                    { name: 'Gantelli bilan bench press', duration: '4 set x 8-12 marta', muscles: 'Ko\'krak, uch boshli mushak' },
                    { name: 'Turgan holatda cho\'kish (squat)', duration: '4 set x 10-15 marta', muscles: 'Oyoqlar, dumba, bel' },
                    { name: 'Deadlift (og\'irlik ko\'tarish)', duration: '3 set x 8-10 marta', muscles: 'Bel, oyoqlar, tutqich' },
                    { name: 'Pull-up (tortilish)', duration: '3 set x maksimal', muscles: 'Orqa, biceps, yelka' },
                    { name: 'Dumbbell shoulder press', duration: '3 set x 10-12 marta', muscles: 'Yelka mushaklari' },
                    { name: 'Planka dinamik', duration: '3 set x 45 soniya', muscles: 'Qorin, stabilizator mushaklar' }
                ];
            } else if (goal === 'fitness') {
                exercises = [
                    { name: 'Yengil yugurish', duration: '20-30 daqiqa', muscles: 'Yurak-qon tomir, oyoqlar' },
                    { name: 'Yoga yoki cho\'zish', duration: '15-20 daqiqa', muscles: 'Egiluvchanlik, muvozanat' },
                    { name: 'Push-up (pol-yugurish)', duration: '3 set x 10-20 marta', muscles: 'Ko\'krak, qo\'llar, yelka' },
                    { name: 'Cho\'kish (squat)', duration: '3 set x 15-20 marta', muscles: 'Oyoqlar, dumba' },
                    { name: 'Velosiped', duration: '25-30 daqiqa', muscles: 'Oyoqlar, yurak' },
                    { name: 'Plank', duration: '3 set x 30-45 soniya', muscles: 'Qorin, bel' }
                ];
            } else {
                exercises = [
                    { name: 'Sekin yurish', duration: '30-40 daqiqa', muscles: 'Yurak salomatligi, oyoqlar' },
                    { name: 'Yengil cho\'zish mashqlari', duration: '10-15 daqiqa', muscles: 'Egiluvchanlik' },
                    { name: 'Qo\'l va oyoq mashqlari', duration: '3 set x 12-15 marta', muscles: 'Butun tana' },
                    { name: 'Nafas mashqlari', duration: '10 daqiqa', muscles: 'O\'pka salomatligi' },
                    { name: 'Muvozanat mashqlari', duration: '10 daqiqa', muscles: 'Koordinatsiya' }
                ];
            }
            
            // Daily schedule
            const schedule = {
                morning: '6:00-7:00 - Uyg\'onish, 1 stakan iliq suv ichish, 10-15 daqiqa yengil cho\'zish',
                breakfast: '7:30-8:30 - To\'yimli nonushta (oqsil + uglevod)',
                workout: goal === 'lose_weight' ? '9:00-10:00 - Asosiy kardio mashg\'ulot' : '17:00-18:30 - Asosiy sport mashg\'ulot',
                lunch: '13:00-14:00 - Tushlik (muvozanatli ovqat)',
                snack: '16:00 - Yengil gazak (meva, yong\'oq)',
                dinner: '19:00-20:00 - Kechki ovqat (yengil, oqsilga boy)',
                evening: '21:00-21:30 - Yengil cho\'zish, meditatsiya',
                sleep: '22:00-22:30 - Uyqu (7-8 soat)'
            };
            
            // Nutrition advice
            let nutrition = {
                eat: [],
                avoid: [],
                water: ''
            };
            
            if (goal === 'lose_weight') {
                nutrition.eat = [
                    'Oqsil: tovuq go\'shti, baliq, tuxum oqsili',
                    'Sabzavotlar: barcha turdagi sabzavotlar (ko\'p miqdorda)',
                    'Murakkab uglevod: yulaf, jigarrang guruch, bulgur',
                    'Sog\'lom yog\': avokado, yong\'oq, zaytun moyi'
                ];
                nutrition.avoid = [
                    'Shakar va shirinliklar',
                    'Gazlangan ichimliklar',
                    'Fast food va qovurilgan taomlar',
                    'Oq un mahsulotlari'
                ];
                nutrition.water = `Kuniga ${Math.round(weight * 0.033)} litr toza suv iching (har soatda 1 stakan)`;
            } else if (goal === 'gain_muscle') {
                nutrition.eat = [
                    'Yuqori oqsilli taomlar: tovuq, mol go\'shti, baliq, tuxum',
                    'Murakkab uglevod: kartoshka, guruch, makaron, yulaf',
                    'Sog\'lom yog\': yong\'oq, avokado, zaytun moyi',
                    'Sut mahsulotlari: tvorog, qatiq, sut'
                ];
                nutrition.avoid = [
                    'Bo\'sh kaloriyali ichimliklar',
                    'Haddan tashqari shakar',
                    'Alkogol'
                ];
                nutrition.water = `Kuniga ${Math.round(weight * 0.04)} litr suv iching, ayniqsa mashqdan keyin`;
            } else {
                nutrition.eat = [
                    'Muvozanatli ovqatlanish: oqsil, uglevod, yog\'',
                    'Ko\'p sabzavot va mevalar',
                    'Butun donli mahsulotlar',
                    'Oq go\'sht va baliq'
                ];
                nutrition.avoid = [
                    'Haddan tashqari tuz',
                    'Qovurilgan taomlar',
                    'Shirinliklar'
                ];
                nutrition.water = `Kuniga ${Math.round(weight * 0.03)} litr toza suv iching`;
            }
            
            // Additional tips
            let tips = [];
            if (goal === 'lose_weight') {
                tips = [
                    'Sekin ovqatlaning va yaxshilab chaynang',
                    'Kichik likopchalarda ovqatlaning',
                    'Mashqdan oldin va keyin cho\'ziling',
                    'Haftada kamida 5 kun 30 daqiqa kardio qiling',
                    'Kechqurun ovqatni 3 soat oldin tugatung',
                    'Stress darajasini kamaytiring - stress vazn ortishiga olib keladi'
                ];
            } else if (goal === 'gain_muscle') {
                tips = [
                    'Mashqdan keyin 30 daqiqa ichida oqsil iste\'mol qiling',
                    'Har bir mushak guruhiga dam bering (48 soat)',
                    'Progressiv yuklama - har hafta ozgina og\'irlikni oshiring',
                    '7-8 soat sifatli uyqu juda muhim',
                    'Suv rejimini saqlang',
                    'Mashqlarni to\'g\'ri texnikada bajaring'
                ];
            } else {
                tips = [
                    'Muntazamlik - muvaffaqiyat kaliti',
                    'O\'z tanangizga quloq soling, og\'riq bo\'lsa to\'xtang',
                    'Asta-sekin boshlang, sekin-asta yuklamani oshiring',
                    'Sifatli uyqu va dam olish juda muhim',
                    'Ijobiy fikrlang va motivatsiyangizni saqlang'
                ];
            }
            
            return {
                bmi: bmi,
                bmiAnalysis: bmiAnalysis,
                calories: targetCalories,
                tdee: tdee,
                exercises: exercises,
                schedule: schedule,
                nutrition: nutrition,
                tips: tips
            };
        }

        function displayResults(recommendations, age, weight, height) {
            const bmi = recommendations.bmi;
            let bmiCategory = '';
            if (bmi < 18.5) bmiCategory = 'Kam';
            else if (bmi < 25) bmiCategory = 'Normal';
            else if (bmi < 30) bmiCategory = 'Ortiq';
            else bmiCategory = 'Semizlik';

            const resultsDiv = document.getElementById('results');
            
            let exercisesHTML = recommendations.exercises.map(ex => `
                <li><strong>${ex.name}</strong> - ${ex.duration}<br>
                    <span style="color: #999; font-size: 0.9em;">Ishlayotgan mushaklar: ${ex.muscles}</span>
                </li>
            `).join('');
            
            let nutritionEatHTML = recommendations.nutrition.eat.map(item => `<li>${item}</li>`).join('');
            let nutritionAvoidHTML = recommendations.nutrition.avoid.map(item => `<li>${item}</li>`).join('');
            let tipsHTML = recommendations.tips.map(tip => `<li>${tip}</li>`).join('');
            
            resultsDiv.innerHTML = `
                <div style="margin-bottom: 1rem;">
                    <span class="stat-box">BMI: ${bmi} (${bmiCategory})</span>
                    <span class="stat-box">${age} yosh</span>
                    <span class="stat-box">${weight} kg / ${height} sm</span>
                </div>

                <div class="recommendation-card">
                    <h3>📊 Tana Massasi Indeksi (BMI)</h3>
                    <p>Sizning BMI: <strong>${bmi}</strong> - ${bmiCategory}</p>
                    <p>${recommendations.bmiAnalysis}</p>
                </div>

                <div class="recommendation-card">
                    <h3>🔥 Kunlik Kalori Ehtiyoji</h3>
                    <p>Asosiy metabolizm: <strong>${recommendations.tdee}</strong> kaloriya/kun</p>
                    <p>Maqsadingiz uchun tavsiya: <strong>${recommendations.calories}</strong> kaloriya/kun</p>
                </div>

                <div class="recommendation-card">
                    <h3>💪 Bugungi Sport Mashqlari</h3>
                    <p>Quyidagi mashqlarni bajaring:</p>
                    <ul>
                        ${exercisesHTML}
                    </ul>
                </div>

                <div class="recommendation-card">
                    <h3>⏰ Kunlik Faoliyat Rejasi</h3>
                    <ul>
                        <li><strong>Ertalab:</strong> ${recommendations.schedule.morning}</li>
                        <li><strong>Nonushta:</strong> ${recommendations.schedule.breakfast}</li>
                        <li><strong>Sport:</strong> ${recommendations.schedule.workout}</li>
                        <li><strong>Tushlik:</strong> ${recommendations.schedule.lunch}</li>
                        <li><strong>Gazak:</strong> ${recommendations.schedule.snack}</li>
                        <li><strong>Kechki ovqat:</strong> ${recommendations.schedule.dinner}</li>
                        <li><strong>Kechqurun:</strong> ${recommendations.schedule.evening}</li>
                        <li><strong>Uyqu:</strong> ${recommendations.schedule.sleep}</li>
                    </ul>
                </div>

                <div class="recommendation-card">
                    <h3>🥗 Ovqatlanish Maslahatlari</h3>
                    <p><strong>Iste'mol qiling:</strong></p>
                    <ul>
                        ${nutritionEatHTML}
                    </ul>
                    <p style="margin-top: 1rem;"><strong>Qochish kerak:</strong></p>
                    <ul>
                        ${nutritionAvoidHTML}
                    </ul>
                    <p style="margin-top: 1rem;"><strong>💧 Suv rejimi:</strong> ${recommendations.nutrition.water}</p>
                </div>

                <div class="recommendation-card">
                    <h3>✨ Qo'shimcha Maslahatlar</h3>
                    <ul>
                        ${tipsHTML}
                    </ul>
                </div>

                <div style="text-align: center; margin-top: 1rem; padding: 1rem; background: rgba(0,217,163,0.1); border-radius: 8px;">
                    <p style="color: var(--success); font-weight: 600; font-size: 0.9rem;">
                        🎯 Muvaffaqiyatlar! Muntazam mashg'ulotlar va to'g'ri ovqatlanish orqali maqsadingizga erishishingiz mumkin!
                    </p>
                </div>
            `;
        }
    </script>
</body>
</html>
