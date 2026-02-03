<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Practice: -ing vs To-Infinitive</title>
    <style>
        :root {
            --primary: #3498db;
            --correct: #2ecc71;
            --typo: #f1c40f;
            --wrong: #e74c3c;
            --bg: #f4f7f6;
            --text: #2c3e50;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            width: 100%;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
        }

        .slide { display: none; }
        .slide.active { display: block; animation: fadeIn 0.5s; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        h1 { color: var(--primary); }
        
        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            margin: 10px;
            transition: 0.3s;
        }

        button:hover { opacity: 0.8; }

        .word-list {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 10px;
            margin: 20px 0;
        }

        .word-item {
            padding: 10px;
            background: #ecf0f1;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .exercise-row {
            text-align: left;
            margin-bottom: 20px;
            padding: 15px;
            border-bottom: 1px solid #eee;
        }

        input {
            padding: 8px;
            border: 2px solid #bdc3c7;
            border-radius: 5px;
            width: 150px;
            font-size: 16px;
        }

        .feedback-pop {
            font-size: 0.9em;
            margin-top: 5px;
            font-style: italic;
            display: none;
        }

        .correct { border-color: var(--correct) !important; background-color: #eafaf1; }
        .typo { border-color: var(--typo) !important; background-color: #fef9e7; }
        .wrong { border-color: var(--wrong) !important; background-color: #fdedec; }

        .word-bank {
            background: #fdf2e9;
            padding: 15px;
            border-radius: 10px;
            border: 1px dashed #e67e22;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .mic-btn { font-size: 24px; background: #9b59b6; }
    </style>
</head>
<body>

<div class="container">
    <div class="slide active" id="slide-1">
        <h1>Hi Giovanni! 👋</h1>
        <p>Pronto a dominare la sfida tra <strong>-ing</strong> e <strong>Infinito</strong>?<br>In questa lezione interattiva esploreremo le regole e faremo pratica.</p>
        <button onclick="nextSlide(2)">Inizia lo Studio</button>
    </div>

    <div class="slide" id="slide-2">
        <h2>Vocabulary & Audio 🔊</h2>
        <p>Clicca sulle parole per ascoltare la pronuncia corretta di un native speaker.</p>
        <div class="word-list" id="study-list">
            </div>
        <button onclick="nextSlide(3)">Vai agli Esercizi</button>
    </div>

    <div class="slide" id="slide-3">
        <h2>Fill in the Gaps Game ✍️</h2>
        <div class="word-bank" id="gap-bank"></div>
        <div id="gap-exercises"></div>
        <button onclick="checkGaps()">Verifica Risposte</button>
        <button onclick="nextSlide(4)">Pronunciation Practice</button>
    </div>

    <div class="slide" id="slide-4">
        <h2>Pronunciation Practice 🗣️</h2>
        <p>Guarda il contesto e prova a dire la frase corretta. <strong>Niente aiuti scritti!</strong></p>
        <div class="word-bank" id="pron-bank"></div>
        <div id="pron-exercises"></div>
        <button onclick="location.reload()">Ricomincia</button>
    </div>
</div>

<script>
    const vocabulary = [
        { word: 'Enjoy', type: 'ing', note: 'Si usa dopo verbi di gradimento.' },
        { word: 'Decide', type: 'to', note: 'Si usa per decisioni e piani futuri.' },
        { word: 'Avoid', type: 'ing', note: 'Si usa sempre con la forma -ing.' },
        { word: 'Hope', type: 'to', note: 'Esprime un desiderio per il futuro.' },
        { word: 'Suggest', type: 'ing', note: 'Si usa per proporre un\'idea.' },
        { word: 'Manage', type: 'to', note: 'Riuscire a fare qualcosa di difficile.' },
        { word: 'Stop', type: 'both', note: 'Cambia significato se seguito da -ing o to.' },
        { word: 'Remember', type: 'both', note: 'Cambia significato tra passato e futuro.' }
    ];

    const gapSentences = [
        { text: "I really enjoy _________ (play) tennis.", answer: "playing", why: "Dopo 'Enjoy' si usa sempre la forma -ing." },
        { text: "We decided _________ (buy) a new car.", answer: "to buy", why: "'Decide' richiede l'infinito con 'to' perché è una scelta futura." },
        { text: "_________ (smoke) is bad for your health.", answer: "smoking", why: "Quando l'azione è il SOGGETTO della frase, usiamo -ing." },
        { text: "I'm tired of _________ (wait).", answer: "waiting", why: "Dopo le PREPOSIZIONI (come 'of') si usa sempre -ing." },
        { text: "I went to the bar _________ (drink) a coffee.", answer: "to drink", why: "Usiamo 'to + infinito' per spiegare lo SCOPO di un'azione (Perché?)." },
        { text: "He suggested _________ (order) pizza.", answer: "ordering", why: "Dopo 'Suggest' usiamo la forma -ing." },
        { text: "Avoid _________ (touch) the wet paint.", answer: "touching", why: "'Avoid' è un verbo che vuole sempre la forma -ing." },
        { text: "I hope _________ (see) you soon.", answer: "to see", why: "'Hope' guarda al futuro, quindi richiede l'infinito con 'to'." }
    ];

    const pronSentences = [
        { context: "Scenario: Stai pianificando il tuo prossimo viaggio.", target: "I hope to travel soon", bankWord: "to travel" },
        { context: "Scenario: Ti piace molto nuotare come hobby.", target: "Swimming is my passion", bankWord: "swimming" },
        { context: "Scenario: Hai deciso di studiare di più.", target: "I decided to study more", bankWord: "to study" }
    ];

    function speak(text) {
        const msg = new SpeechSynthesisUtterance();
        msg.text = text;
        msg.lang = 'en-US';
        msg.rate = 0.9;
        window.speechSynthesis.speak(msg);
    }

    function init() {
        // Init Study List
        const studyDiv = document.getElementById('study-list');
        vocabulary.forEach(v => {
            const item = document.createElement('div');
            item.className = 'word-item';
            item.innerHTML = `<strong>${v.word}</strong> 🔊`;
            item.onclick = () => speak(v.word);
            studyDiv.appendChild(item);
        });

        // Init Gap Exercises
        const bank = gapSentences.map(s => s.answer);
        shuffle(bank);
        document.getElementById('gap-bank').innerText = "Word Bank: " + bank.join(" | ");

        const gapDiv = document.getElementById('gap-exercises');
        gapSentences.forEach((s, i) => {
            const row = document.createElement('div');
            row.className = 'exercise-row';
            row.innerHTML = `
                <p>${i+1}. ${s.text}</p>
                <input type="text" id="gap-${i}" placeholder="Type here...">
                <div id="feedback-${i}" class="feedback-pop"></div>
            `;
            gapDiv.appendChild(row);
        });

        // Init Pronunciation
        const pBank = pronSentences.map(p => p.bankWord);
        shuffle(pBank);
        document.getElementById('pron-bank').innerText = "Word Bank: " + pBank.join(" | ");
        const pronDiv = document.getElementById('pron-exercises');
        pronSentences.forEach((p, i) => {
            const row = document.createElement('div');
            row.className = 'exercise-row';
            row.innerHTML = `
                <p><strong>Context:</strong> ${p.context}</p>
                <button class="mic-btn" onclick="startSpeech(${i})">🎤 Tap & Speak</button>
                <div id="pron-feedback-${i}" class="feedback-pop"></div>
            `;
            pronDiv.appendChild(row);
        });
    }

    function shuffle(array) {
        array.sort(() => Math.random() - 0.5);
    }

    function nextSlide(n) {
        document.querySelectorAll('.slide').forEach(s => s.classList.remove('active'));
        document.getElementById('slide-' + n).classList.add('active');
    }

    function levenshtein(a, b) {
        if(a.length == 0) return b.length; 
        if(b.length == 0) return a.length; 
        var matrix = [];
        for(var i = 0; i <= b.length; i++){ matrix[i] = [i]; }
        for(var j = 0; j <= a.length; j++){ matrix[0][j] = j; }
        for(i = 1; i <= b.length; i++){
            for(j = 1; j <= a.length; j++){
                if(b.charAt(i-1) == a.charAt(j-1)){ matrix[i][j] = matrix[i-1][j-1]; } 
                else { matrix[i][j] = Math.min(matrix[i-1][j-1] + 1, Math.min(matrix[i][j-1] + 1, matrix[i-1][j] + 1)); }
            }
        }
        return matrix[b.length][a.length];
    }

    function checkGaps() {
        gapSentences.forEach((s, i) => {
            const input = document.getElementById('gap-' + i);
            const feedback = document.getElementById('feedback-' + i);
            const val = input.value.trim().toLowerCase();
            const target = s.answer.toLowerCase();
            
            feedback.style.display = 'block';
            feedback.innerHTML = `<strong>Why?</strong> ${s.why}`;

            if (val === target) {
                input.className = 'correct';
            } else if (levenshtein(val, target) <= 2 && val.length > 3) {
                input.className = 'typo';
                feedback.innerHTML = `<em>Typo! Correct spelling: <strong>${s.answer}</strong></em><br>` + feedback.innerHTML;
            } else {
                input.className = 'wrong';
            }
        });
    }

    function startSpeech(i) {
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        recognition.lang = 'en-US';
        recognition.start();

        recognition.onresult = (event) => {
            const speechResult = event.results[0][0].transcript.toLowerCase();
            const feedback = document.getElementById('pron-feedback-' + i);
            feedback.style.display = 'block';
            
            const target = pronSentences[i].target.toLowerCase();
            if (speechResult.includes(pronSentences[i].bankWord.toLowerCase())) {
                feedback.innerHTML = `<span style="color:green">Well done! You said: "${speechResult}"</span>`;
            } else {
                feedback.innerHTML = `<span style="color:red">Almost! Try focusing on the structure "${pronSentences[i].bankWord}". You said: "${speechResult}"</span>`;
            }
        };
    }

    window.onload = init;
</script>

</body>
</html>
