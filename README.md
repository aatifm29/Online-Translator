<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Universal Translator - Translate Between All Languages</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #1a2a6c);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            color: #333;
        }

        .translator-container {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
            width: 100%;
            max-width: 1000px;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(to right, #4e54c8, #8f94fb);
            color: white;
            padding: 25px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }

        .translator-body {
            padding: 30px;
        }

        .language-selector {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            flex-wrap: wrap;
            gap: 15px;
        }

        .language-box {
            display: flex;
            flex-direction: column;
            width: 45%;
            min-width: 200px;
        }

        .language-box label {
            font-weight: 600;
            margin-bottom: 8px;
            color: #333;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        select {
            padding: 12px 15px;
            border-radius: 8px;
            border: 1px solid #ddd;
            background: white;
            font-size: 1rem;
            color: #333;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }

        select:focus {
            outline: none;
            border-color: #4e54c8;
            box-shadow: 0 0 0 3px rgba(78, 84, 200, 0.2);
        }

        .swap-btn {
            background: #4e54c8;
            color: white;
            border: none;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            font-size: 1.5rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .swap-btn:hover {
            background: #3a3fb8;
            transform: rotate(180deg);
        }

        .text-areas {
            display: flex;
            gap: 20px;
            margin-bottom: 25px;
            flex-wrap: wrap;
        }

        .text-box {
            flex: 1;
            min-width: 300px;
        }

        .text-box h3 {
            margin-bottom: 10px;
            color: #333;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        textarea {
            width: 100%;
            height: 200px;
            padding: 15px;
            border-radius: 10px;
            border: 1px solid #ddd;
            font-size: 1rem;
            resize: vertical;
            transition: all 0.3s ease;
            background: #f9f9f9;
        }

        textarea:focus {
            outline: none;
            border-color: #4e54c8;
            background: white;
            box-shadow: 0 0 0 3px rgba(78, 84, 200, 0.1);
        }

        .action-buttons {
            display: flex;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary {
            background: #4e54c8;
            color: white;
        }

        .btn-primary:hover {
            background: #3a3fb8;
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }

        .btn-secondary {
            background: #f1f1f1;
            color: #333;
        }

        .btn-secondary:hover {
            background: #e1e1e1;
        }

        .notification {
            position: fixed;
            bottom: 20px;
            right: 20px;
            padding: 15px 25px;
            background: #4CAF50;
            color: white;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            transform: translateY(100px);
            opacity: 0;
            transition: all 0.4s ease;
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .notification.show {
            transform: translateY(0);
            opacity: 1;
        }

        .notification.error {
            background: #f44336;
        }

        .loading {
            display: none;
            text-align: center;
            margin: 15px 0;
        }

        .loading.show {
            display: block;
        }

        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: #4e54c8;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .history {
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid #eee;
        }

        .history h3 {
            margin-bottom: 15px;
            color: #333;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .history-item {
            background: #f9f9f9;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .history-item:hover {
            background: #f1f1f1;
            transform: translateY(-2px);
        }

        .history-item .source {
            font-weight: 600;
            margin-bottom: 5px;
        }

        .history-item .translation {
            color: #666;
            font-size: 0.9rem;
        }

        .history-item .languages {
            font-size: 0.8rem;
            color: #888;
            margin-top: 5px;
        }

        .clear-history {
            background: none;
            border: none;
            color: #888;
            font-size: 0.9rem;
            cursor: pointer;
            margin-top: 10px;
            text-decoration: underline;
        }

        .clear-history:hover {
            color: #333;
        }

        .api-selector {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            gap: 10px;
        }

        .api-btn {
            padding: 8px 15px;
            border: 1px solid #ddd;
            background: white;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .api-btn.active {
            background: #4e54c8;
            color: white;
            border-color: #4e54c8;
        }

        .footer {
            text-align: center;
            padding: 20px;
            color: #666;
            font-size: 0.9rem;
            background: #f9f9f9;
            border-top: 1px solid #eee;
        }

        @media (max-width: 768px) {
            .language-selector {
                flex-direction: column;
                align-items: stretch;
            }
            
            .language-box {
                width: 100%;
            }
            
            .swap-btn {
                align-self: center;
                margin: 10px 0;
            }
            
            .text-areas {
                flex-direction: column;
            }
            
            .header h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="translator-container">
        <div class="header">
            <h1><i class="fas fa-language"></i> Universal Translator</h1>
            <p>Translate text between 100+ languages including all Indian languages</p>
        </div>
        
        <div class="translator-body">
            <div class="api-selector">
                <button class="api-btn active" data-api="mock">Demo Mode</button>
                <button class="api-btn" data-api="libre">LibreTranslate</button>
                <button class="api-btn" data-api="google">Google Translate</button>
            </div>
            
            <div class="language-selector">
                <div class="language-box">
                    <label for="source-lang">
                        <i class="fas fa-globe"></i> From:
                    </label>
                    <select id="source-lang">
                        <option value="auto">Auto Detect</option>
                        <option value="en">English</option>
                        <option value="es">Spanish</option>
                        <option value="fr">French</option>
                        <option value="de">German</option>
                        <option value="it">Italian</option>
                        <option value="pt">Portuguese</option>
                        <option value="ru">Russian</option>
                        <option value="ja">Japanese</option>
                        <option value="ko">Korean</option>
                        <option value="zh">Chinese</option>
                        <option value="hi">Hindi</option>
                        <option value="bn">Bengali</option>
                        <option value="te">Telugu</option>
                        <option value="mr">Marathi</option>
                        <option value="ta">Tamil</option>
                        <option value="ur">Urdu</option>
                        <option value="gu">Gujarati</option>
                        <option value="kn">Kannada</option>
                        <option value="or">Odia</option>
                        <option value="ml">Malayalam</option>
                        <option value="pa">Punjabi</option>
                        <option value="as">Assamese</option>
                        <option value="sa">Sanskrit</option>
                        <option value="ar">Arabic</option>
                        <option value="tr">Turkish</option>
                        <option value="pl">Polish</option>
                        <option value="nl">Dutch</option>
                        <option value="sv">Swedish</option>
                        <option value="da">Danish</option>
                        <option value="no">Norwegian</option>
                        <option value="fi">Finnish</option>
                        <option value="cs">Czech</option>
                        <option value="sk">Slovak</option>
                        <option value="hu">Hungarian</option>
                        <option value="ro">Romanian</option>
                        <option value="bg">Bulgarian</option>
                        <option value="hr">Croatian</option>
                        <option value="sr">Serbian</option>
                        <option value="sl">Slovenian</option>
                        <option value="et">Estonian</option>
                        <option value="lv">Latvian</option>
                        <option value="lt">Lithuanian</option>
                        <option value="mt">Maltese</option>
                        <option value="ga">Irish</option>
                        <option value="cy">Welsh</option>
                        <option value="eu">Basque</option>
                        <option value="ca">Catalan</option>
                        <option value="gl">Galician</option>
                        <option value="is">Icelandic</option>
                        <option value="mk">Macedonian</option>
                        <option value="sq">Albanian</option>
                        <option value="hy">Armenian</option>
                        <option value="az">Azerbaijani</option>
                        <option value="be">Belarusian</option>
                        <option value="ka">Georgian</option>
                        <option value="kk">Kazakh</option>
                        <option value="ky">Kyrgyz</option>
                        <option value="mn">Mongolian</option>
                        <option value="tg">Tajik</option>
                        <option value="tk">Turkmen</option>
                        <option value="uz">Uzbek</option>
                        <option value="am">Amharic</option>
                        <option value="ha">Hausa</option>
                        <option value="ig">Igbo</option>
                        <option value="sw">Swahili</option>
                        <option value="yo">Yoruba</option>
                        <option value="zu">Zulu</option>
                        <option value="af">Afrikaans</option>
                        <option value="bs">Bosnian</option>
                        <option value="ceb">Cebuano</option>
                        <option value="co">Corsican</option>
                        <option value="fy">Frisian</option>
                        <option value="haw">Hawaiian</option>
                        <option value="hmn">Hmong</option>
                        <option value="ilo">Ilocano</option>
                        <option value="jw">Javanese</option>
                        <option value="kg">Kongo</option>
                        <option value="ku">Kurdish</option>
                        <option value="la">Latin</option>
                        <option value="lb">Luxembourgish</option>
                        <option value="mg">Malagasy</option>
                        <option value="mi">Maori</option>
                        <option value="my">Myanmar (Burmese)</option>
                        <option value="ne">Nepali</option>
                        <option value="ny">Nyanja (Chichewa)</option>
                        <option value="ps">Pashto</option>
                        <option value="rn">Kirundi</option>
                        <option value="sm">Samoan</option>
                        <option value="sd">Sindhi</option>
                        <option value="si">Sinhala (Sinhalese)</option>
                        <option value="so">Somali</option>
                        <option value="st">Sesotho</option>
                        <option value="su">Sundanese</option>
                        <option value="ti">Tigrinya</option>
                        <option value="ts">Tsonga</option>
                        <option value="tt">Tatar</option>
                        <option value="ug">Uyghur</option>
                        <option value="vi">Vietnamese</option>
                    </select>
                </div>
                
                <button class="swap-btn" id="swap-btn" title="Swap Languages">
                    <i class="fas fa-exchange-alt"></i>
                </button>
                
                <div class="language-box">
                    <label for="target-lang">
                        <i class="fas fa-globe"></i> To:
                    </label>
                    <select id="target-lang">
                        <option value="hi">Hindi</option>
                        <option value="en">English</option>
                        <option value="es">Spanish</option>
                        <option value="fr">French</option>
                        <option value="de">German</option>
                        <option value="it">Italian</option>
                        <option value="pt">Portuguese</option>
                        <option value="ru">Russian</option>
                        <option value="ja">Japanese</option>
                        <option value="ko">Korean</option>
                        <option value="zh">Chinese</option>
                        <option value="bn">Bengali</option>
                        <option value="te">Telugu</option>
                        <option value="mr">Marathi</option>
                        <option value="ta">Tamil</option>
                        <option value="ur">Urdu</option>
                        <option value="gu">Gujarati</option>
                        <option value="kn">Kannada</option>
                        <option value="or">Odia</option>
                        <option value="ml">Malayalam</option>
                        <option value="pa">Punjabi</option>
                        <option value="as">Assamese</option>
                        <option value="sa">Sanskrit</option>
                        <option value="ar">Arabic</option>
                        <option value="tr">Turkish</option>
                        <option value="pl">Polish</option>
                        <option value="nl">Dutch</option>
                        <option value="sv">Swedish</option>
                        <option value="da">Danish</option>
                        <option value="no">Norwegian</option>
                        <option value="fi">Finnish</option>
                        <option value="cs">Czech</option>
                        <option value="sk">Slovak</option>
                        <option value="hu">Hungarian</option>
                        <option value="ro">Romanian</option>
                        <option value="bg">Bulgarian</option>
                        <option value="hr">Croatian</option>
                        <option value="sr">Serbian</option>
                        <option value="sl">Slovenian</option>
                        <option value="et">Estonian</option>
                        <option value="lv">Latvian</option>
                        <option value="lt">Lithuanian</option>
                        <option value="mt">Maltese</option>
                        <option value="ga">Irish</option>
                        <option value="cy">Welsh</option>
                        <option value="eu">Basque</option>
                        <option value="ca">Catalan</option>
                        <option value="gl">Galician</option>
                        <option value="is">Icelandic</option>
                        <option value="mk">Macedonian</option>
                        <option value="sq">Albanian</option>
                        <option value="hy">Armenian</option>
                        <option value="az">Azerbaijani</option>
                        <option value="be">Belarusian</option>
                        <option value="ka">Georgian</option>
                        <option value="kk">Kazakh</option>
                        <option value="ky">Kyrgyz</option>
                        <option value="mn">Mongolian</option>
                        <option value="tg">Tajik</option>
                        <option value="tk">Turkmen</option>
                        <option value="uz">Uzbek</option>
                        <option value="am">Amharic</option>
                        <option value="ha">Hausa</option>
                        <option value="ig">Igbo</option>
                        <option value="sw">Swahili</option>
                        <option value="yo">Yoruba</option>
                        <option value="zu">Zulu</option>
                        <option value="af">Afrikaans</option>
                        <option value="bs">Bosnian</option>
                        <option value="ceb">Cebuano</option>
                        <option value="co">Corsican</option>
                        <option value="fy">Frisian</option>
                        <option value="haw">Hawaiian</option>
                        <option value="hmn">Hmong</option>
                        <option value="ilo">Ilocano</option>
                        <option value="jw">Javanese</option>
                        <option value="kg">Kongo</option>
                        <option value="ku">Kurdish</option>
                        <option value="la">Latin</option>
                        <option value="lb">Luxembourgish</option>
                        <option value="mg">Malagasy</option>
                        <option value="mi">Maori</option>
                        <option value="my">Myanmar (Burmese)</option>
                        <option value="ne">Nepali</option>
                        <option value="ny">Nyanja (Chichewa)</option>
                        <option value="ps">Pashto</option>
                        <option value="rn">Kirundi</option>
                        <option value="sm">Samoan</option>
                        <option value="sd">Sindhi</option>
                        <option value="si">Sinhala (Sinhalese)</option>
                        <option value="so">Somali</option>
                        <option value="st">Sesotho</option>
                        <option value="su">Sundanese</option>
                        <option value="ti">Tigrinya</option>
                        <option value="ts">Tsonga</option>
                        <option value="tt">Tatar</option>
                        <option value="ug">Uyghur</option>
                        <option value="vi">Vietnamese</option>
                    </select>
                </div>
            </div>
            
            <div class="text-areas">
                <div class="text-box">
                    <h3>
                        <i class="fas fa-edit"></i> Text to Translate
                    </h3>
                    <textarea id="source-text" placeholder="Enter text to translate..."></textarea>
                </div>
                
                <div class="text-box">
                    <h3>
                        <i class="fas fa-language"></i> Translated Text
                    </h3>
                    <textarea id="translated-text" placeholder="Translation will appear here..." readonly></textarea>
                </div>
            </div>
            
            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>Translating...</p>
            </div>
            
            <div class="action-buttons">
                <button class="btn btn-primary" id="translate-btn">
                    <i class="fas fa-language"></i> Translate
                </button>
                <button class="btn btn-secondary" id="clear-btn">
                    <i class="fas fa-eraser"></i> Clear
                </button>
                <button class="btn btn-secondary" id="copy-btn">
                    <i class="fas fa-copy"></i> Copy Translation
                </button>
                <button class="btn btn-secondary" id="speak-btn">
                    <i class="fas fa-volume-up"></i> Speak
                </button>
            </div>
            
            <div class="history" id="history">
                <h3>
                    <i class="fas fa-history"></i> Translation History
                </h3>
                <div id="history-list"></div>
                <button class="clear-history" id="clear-history">Clear History</button>
            </div>
        </div>
        
        <div class="footer">
            <p>Universal Translator | Supports 100+ languages including all Indian languages</p>
        </div>
    </div>
    
    <div class="notification" id="notification">
        <i class="fas fa-check-circle"></i>
        <span id="notification-text"></span>
    </div>

    <script>
        // DOM Elements
        const sourceLang = document.getElementById('source-lang');
        const targetLang = document.getElementById('target-lang');
        const sourceText = document.getElementById('source-text');
        const translatedText = document.getElementById('translated-text');
        const translateBtn = document.getElementById('translate-btn');
        const clearBtn = document.getElementById('clear-btn');
        const copyBtn = document.getElementById('copy-btn');
        const speakBtn = document.getElementById('speak-btn');
        const swapBtn = document.getElementById('swap-btn');
        const notification = document.getElementById('notification');
        const notificationText = document.getElementById('notification-text');
        const loading = document.getElementById('loading');
        const historyList = document.getElementById('history-list');
        const clearHistoryBtn = document.getElementById('clear-history');
        const apiButtons = document.querySelectorAll('.api-btn');
        
        // Translation history
        let translationHistory = JSON.parse(localStorage.getItem('translationHistory')) || [];
        let currentApi = 'mock';
        
        // Mock translation dictionary (for demonstration purposes)
        const mockTranslations = {
            "en-hi": {
                "hello": "नमस्ते",
                "goodbye": "अलविदा",
                "thank you": "धन्यवाद",
                "please": "कृपया",
                "yes": "हाँ",
                "no": "नहीं",
                "water": "पानी",
                "food": "खाना",
                "friend": "दोस्त",
                "love": "प्यार",
                "india": "भारत",
                "language": "भाषा",
                "how are you": "आप कैसे हैं",
                "what is your name": "आपका नाम क्या है",
                "where are you from": "आप कहाँ से हैं",
                "i love you": "मैं तुमसे प्यार करता हूँ",
                "good morning": "सुप्रभात",
                "good night": "शुभ रात्रि",
                "welcome": "स्वागत है",
                "excuse me": "माफ़ कीजिए",
                "i am sorry": "मुझे माफ़ करें",
                "help": "मदद",
                "price": "कीमत",
                "time": "समय",
                "day": "दिन",
                "night": "रात",
                "today": "आज",
                "tomorrow": "कल",
                "yesterday": "कल",
                "week": "सप्ताह",
                "month": "महीना",
                "year": "साल",
                "mother": "माँ",
                "father": "पिता",
                "sister": "बहन",
                "brother": "भाई",
                "son": "बेटा",
                "daughter": "बेटी",
                "house": "घर",
                "car": "कार",
                "book": "किताब",
                "phone": "फोन",
                "computer": "कंप्यूटर",
                "work": "काम",
                "school": "स्कूल",
                "hospital": "अस्पताल",
                "restaurant": "रेस्टोरेंट",
                "hotel": "होटल",
                "market": "बाजार",
                "store": "दुकान",
                "money": "पैसा",
                "bank": "बैंक",
                "doctor": "डॉक्टर",
                "teacher": "शिक्षक",
                "student": "छात्र",
                "child": "बच्चा",
                "man": "आदमी",
                "woman": "औरत",
                "boy": "लड़का",
                "girl": "लड़की",
                "people": "लोग",
                "family": "परिवार",
                "country": "देश",
                "city": "शहर",
                "village": "गाँव",
                "road": "सड़क",
                "river": "नदी",
                "mountain": "पहाड़",
                "tree": "पेड़",
                "flower": "फूल",
                "fruit": "फल",
                "vegetable": "सब्जी",
                "meat": "मांस",
                "fish": "मछली",
                "bread": "रोटी",
                "rice": "चावल",
                "milk": "दूध",
                "tea": "चाय",
                "coffee": "कॉफी",
                "water": "पानी",
                "juice": "रस",
                "beer": "बीयर",
                "wine": "शराब",
                "music": "संगीत",
                "dance": "नृत्य",
                "song": "गीत",
                "movie": "फिल्म",
                "game": "खेल",
                "sport": "खेल",
                "football": "फुटबॉल",
                "cricket": "क्रिकेट",
                "tennis": "टेनिस",
                "travel": "यात्रा",
                "holiday": "छुट्टी",
                "vacation": "छुट्टी",
                "trip": "यात्रा",
                "flight": "उड़ान",
                "train": "ट्रेन",
                "bus": "बस",
                "car": "कार",
                "bicycle": "साइकिल",
                "walk": "चलना",
                "run": "दौड़ना",
                "swim": "तैरना",
                "fly": "उड़ना",
                "drive": "ड्राइव करना",
                "eat": "खाना",
                "drink": "पीना",
                "sleep": "सोना",
                "wake up": "जागना",
                "read": "पढ़ना",
                "write": "लिखना",
                "listen": "सुनना",
                "speak": "बोलना",
                "see": "देखना",
                "watch": "देखना",
                "look": "देखना",
                "think": "सोचना",
                "know": "जानना",
                "understand": "समझना",
                "learn": "सीखना",
                "teach": "सिखाना",
                "study": "पढ़ाई करना",
                "work": "काम करना",
                "play": "खेलना",
                "sing": "गाना",
                "dance": "नाचना",
                "cook": "खाना बनाना",
                "clean": "साफ करना",
                "wash": "धोना",
                "buy": "खरीदना",
                "sell": "बेचना",
                "give": "देना",
                "take": "लेना",
                "bring": "लाना",
                "send": "भेजना",
                "receive": "प्राप्त करना",
                "call": "कॉल करना",
                "talk": "बात करना",
                "meet": "मिलना",
                "visit": "मिलने जाना",
                "help": "मदद करना",
                "support": "समर्थन करना",
                "love": "प्यार करना",
                "like": "पसंद करना",
                "hate": "नफरत करना",
                "want": "चाहना",
                "need": "ज़रूरत होना",
                "have": "होना",
                "has": "है",
                "had": "था",
                "do": "करना",
                "does": "करता है",
                "did": "किया",
                "will": "करेगा",
                "would": "करेगा",
                "can": "कर सकता है",
                "could": "कर सकता था",
                "should": "चाहिए",
                "must": "ज़रूरी है",
                "may": "हो सकता है",
                "might": "हो सकता है",
                "here": "यहाँ",
                "there": "वहाँ",
                "where": "कहाँ",
                "when": "कब",
                "why": "क्यों",
                "how": "कैसे",
                "what": "क्या",
                "which": "कौन सा",
                "who": "कौन",
                "whom": "किसको",
                "whose": "किसका",
                "this": "यह",
                "that": "वह",
                "these": "ये",
                "those": "वे",
                "i": "मैं",
                "you": "तुम",
                "he": "वह",
                "she": "वह",
                "it": "यह",
                "we": "हम",
                "they": "वे",
                "me": "मुझे",
                "him": "उसे",
                "her": "उसे",
                "us": "हमें",
                "them": "उन्हें",
                "my": "मेरा",
                "your": "तुम्हारा",
                "his": "उसका",
                "her": "उसकी",
                "its": "इसका",
                "our": "हमारा",
                "their": "उनका",
                "mine": "मेरा",
                "yours": "तुम्हारा",
                "hers": "उसकी",
                "ours": "हमारा",
                "theirs": "उनका"
            },
            "hi-en": {
                "नमस्ते": "hello",
                "अलविदा": "goodbye",
                "धन्यवाद": "thank you",
                "कृपया": "please",
                "हाँ": "yes",
                "नहीं": "no",
                "पानी": "water",
                "खाना": "food",
                "दोस्त": "friend",
                "प्यार": "love",
                "भारत": "india",
                "भाषा": "language",
                "आप कैसे हैं": "how are you",
                "आपका नाम क्या है": "what is your name",
                "आप कहाँ से हैं": "where are you from",
                "मैं तुमसे प्यार करता हूँ": "i love you",
                "सुप्रभात": "good morning",
                "शुभ रात्रि": "good night",
                "स्वागत है": "welcome",
                "माफ़ कीजिए": "excuse me",
                "मुझे माफ़ करें": "i am sorry",
                "मदद": "help",
                "कीमत": "price",
                "समय": "time",
                "दिन": "day",
                "रात": "night",
                "आज": "today",
                "कल": "tomorrow",
                "सप्ताह": "week",
                "महीना": "month",
                "साल": "year",
                "माँ": "mother",
                "पिता": "father",
                "बहन": "sister",
                "भाई": "brother",
                "बेटा": "son",
                "बेटी": "daughter",
                "घर": "house",
                "कार": "car",
                "किताब": "book",
                "फोन": "phone",
                "कंप्यूटर": "computer",
                "काम": "work",
                "स्कूल": "school",
                "अस्पताल": "hospital",
                "रेस्टोरेंट": "restaurant",
                "होटल": "hotel",
                "बाजार": "market",
                "दुकान": "store",
                "पैसा": "money",
                "बैंक": "bank",
                "डॉक्टर": "doctor",
                "शिक्षक": "teacher",
                "छात्र": "student",
                "बच्चा": "child",
                "आदमी": "man",
                "औरत": "woman",
                "लड़का": "boy",
                "लड़की": "girl",
                "लोग": "people",
                "परिवार": "family",
                "देश": "country",
                "शहर": "city",
                "गाँव": "village",
                "सड़क": "road",
                "नदी": "river",
                "पहाड़": "mountain",
                "पेड़": "tree",
                "फूल": "flower",
                "फल": "fruit",
                "सब्जी": "vegetable",
                "मांस": "meat",
                "मछली": "fish",
                "रोटी": "bread",
                "चावल": "rice",
                "दूध": "milk",
                "चाय": "tea",
                "कॉफी": "coffee",
                "रस": "juice",
                "बीयर": "beer",
                "शराब": "wine",
                "संगीत": "music",
                "नृत्य": "dance",
                "गीत": "song",
                "फिल्म": "movie",
                "खेल": "game",
                "खेल": "sport",
                "फुटबॉल": "football",
                "क्रिकेट": "cricket",
                "टेनिस": "tennis",
                "यात्रा": "travel",
                "छुट्टी": "holiday",
                "छुट्टी": "vacation",
                "यात्रा": "trip",
                "उड़ान": "flight",
                "ट्रेन": "train",
                "बस": "bus",
                "कार": "car",
                "साइकिल": "bicycle",
                "चलना": "walk",
                "दौड़ना": "run",
                "तैरना": "swim",
                "उड़ना": "fly",
                "ड्राइव करना": "drive",
                "खाना": "eat",
                "पीना": "drink",
                "सोना": "sleep",
                "जागना": "wake up",
                "पढ़ना": "read",
                "लिखना": "write",
                "सुनना": "listen",
                "बोलना": "speak",
                "देखना": "see",
                "देखना": "watch",
                "देखना": "look",
                "सोचना": "think",
                "जानना": "know",
                "समझना": "understand",
                "सीखना": "learn",
                "सिखाना": "teach",
                "पढ़ाई करना": "study",
                "काम करना": "work",
                "खेलना": "play",
                "गाना": "sing",
                "नाचना": "dance",
                "खाना बनाना": "cook",
                "साफ करना": "clean",
                "धोना": "wash",
                "खरीदना": "buy",
                "बेचना": "sell",
                "देना": "give",
                "लेना": "take",
                "लाना": "bring",
                "भेजना": "send",
                "प्राप्त करना": "receive",
                "कॉल करना": "call",
                "बात करना": "talk",
                "मिलना": "meet",
                "मिलने जाना": "visit",
                "मदद करना": "help",
                "समर्थन करना": "support",
                "प्यार करना": "love",
                "पसंद करना": "like",
                "नफरत करना": "hate",
                "चाहना": "want",
                "ज़रूरत होना": "need",
                "होना": "have",
                "है": "has",
                "था": "had",
                "करना": "do",
                "करता है": "does",
                "किया": "did",
                "करेगा": "will",
                "करेगा": "would",
                "कर सकता है": "can",
                "कर सकता था": "could",
                "चाहिए": "should",
                "ज़रूरी है": "must",
                "हो सकता है": "may",
                "हो सकता है": "might",
                "यहाँ": "here",
                "वहाँ": "there",
                "कहाँ": "where",
                "कब": "when",
                "क्यों": "why",
                "कैसे": "how",
                "क्या": "what",
                "कौन सा": "which",
                "कौन": "who",
                "किसको": "whom",
                "किसका": "whose",
                "यह": "this",
                "वह": "that",
                "ये": "these",
                "वे": "those",
                "मैं": "i",
                "तुम": "you",
                "वह": "he",
                "वह": "she",
                "यह": "it",
                "हम": "we",
                "वे": "they",
                "मुझे": "me",
                "उसे": "him",
                "उसे": "her",
                "हमें": "us",
                "उन्हें": "them",
                "मेरा": "my",
                "तुम्हारा": "your",
                "उसका": "his",
                "उसकी": "her",
                "इसका": "its",
                "हमारा": "our",
                "उनका": "their",
                "मेरा": "mine",
                "तुम्हारा": "yours",
                "उसकी": "hers",
                "हमारा": "ours",
                "उनका": "theirs"
            },
            "en-mr": {
                "hello": "नमस्कार",
                "goodbye": "निरोप",
                "thank you": "धन्यवाद",
                "please": "कृपया",
                "yes": "होय",
                "no": "नाही",
                "water": "पाणी",
                "food": "अन्न",
                "friend": "मित्र",
                "love": "प्रेम",
                "india": "भारत",
                "language": "भाषा",
                "how are you": "तुम्ही कसे आहात",
                "what is your name": "तुमचे नाव काय आहे",
                "where are you from": "तुम्ही कुठून आहात",
                "i love you": "मी तुम्हाला प्रेम करतो",
                "good morning": "शुभ सकाळ",
                "good night": "शुभ रात्री",
                "welcome": "स्वागत",
                "excuse me": "माफ करा",
                "i am sorry": "मी माफी मागतो",
                "help": "मदत",
                "price": "किंमत",
                "time": "वेळ",
                "day": "दिवस",
                "night": "रात्र",
                "today": "आज",
                "tomorrow": "उद्या",
                "yesterday": "काल",
                "week": "आठवडा",
                "month": "महिना",
                "year": "वर्ष",
                "mother": "आई",
                "father": "वडील",
                "sister": "बहीण",
                "brother": "भाऊ",
                "son": "मुलगा",
                "daughter": "मुलगी",
                "house": "घर",
                "car": "कार",
                "book": "पुस्तक",
                "phone": "फोन",
                "computer": "संगणक",
                "work": "काम",
                "school": "शाळा",
                "hospital": "रुग्णालय",
                "restaurant": "हॉटेल",
                "hotel": "हॉटेल",
                "market": "बाजार",
                "store": "दुकान",
                "money": "पैसे",
                "bank": "बँक",
                "doctor": "डॉक्टर",
                "teacher": "शिक्षक",
                "student": "विद्यार्थी",
                "child": "बाल",
                "man": "पुरुष",
                "woman": "स्त्री",
                "boy": "मुलगा",
                "girl": "मुलगी",
                "people": "लोक",
                "family": "कुटुंब",
                "country": "देश",
                "city": "शहर",
                "village": "गाव",
                "road": "रस्ता",
                "river": "नदी",
                "mountain": "पर्वत",
                "tree": "झाड",
                "flower": "फूल",
                "fruit": "फळ",
                "vegetable": "भाजी",
                "meat": "मांस",
                "fish": "मासे",
                "bread": "भाकरी",
                "rice": "तांदूळ",
                "milk": "दूध",
                "tea": "चहा",
                "coffee": "कॉफी",
                "water": "पाणी",
                "juice": "रस",
                "beer": "बीअर",
                "wine": "वाइन",
                "music": "संगीत",
                "dance": "नृत्य",
                "song": "गीत",
                "movie": "चित्रपट",
                "game": "खेळ",
                "sport": "खेळ",
                "football": "फुटबॉल",
                "cricket": "क्रिकेट",
                "tennis": "टेनिस",
                "travel": "प्रवास",
                "holiday": "सुट्टी",
                "vacation": "सुट्टी",
                "trip": "प्रवास",
                "flight": "उड्डाण",
                "train": "ट्रेन",
                "bus": "बस",
                "car": "कार",
                "bicycle": "सायकल",
                "walk": "चालणे",
                "run": "धावणे",
                "swim": "पोहरणे",
                "fly": "उड्डाण करणे",
                "drive": "चालवणे",
                "eat": "खाणे",
                "drink": "प्याहार करणे",
                "sleep": "झोपणे",
                "wake up": "जागणे",
                "read": "वाचणे",
                "write": "लिहिणे",
                "listen": "ऐकणे",
                "speak": "बोलणे",
                "see": "पाहणे",
                "watch": "पाहणे",
                "look": "पाहणे",
                "think": "विचार करणे",
                "know": "माहित असणे",
                "understand": "समजणे",
                "learn": "शिकणे",
                "teach": "शिकवणे",
                "study": "अभ्यास करणे",
                "work": "काम करणे",
                "play": "खेळणे",
                "sing": "गाणे",
                "dance": "नाचणे",
                "cook": "बनवणे",
                "clean": "स्वच्छ करणे",
                "wash": "धुणे",
                "buy": "खरेदी करणे",
                "sell": "विकणे",
                "give": "देणे",
                "take": "घेणे",
                "bring": "आणणे",
                "send": "पाठवणे",
                "receive": "मिळवणे",
                "call": "कॉल करणे",
                "talk": "बोलणे",
                "meet": "भेटणे",
                "visit": "भेट देणे",
                "help": "मदत करणे",
                "support": "पाठिंबा देणे",
                "love": "प्रेम करणे",
                "like": "आवडणे",
                "hate": "तिरस्कार करणे",
                "want": "हवे असणे",
                "need": "गरज असणे",
                "have": "असणे",
                "has": "आहे",
                "had": "होते",
                "do": "करणे",
                "does": "करतो",
                "did": "केले",
                "will": "करेल",
                "would": "करेल",
                "can": "शकतो",
                "could": "शकतो",
                "should": "केले पाहिजे",
                "must": "केलेच पाहिजे",
                "may": "शकतो",
                "might": "शकतो",
                "here": "इथे",
                "there": "तिथे",
                "where": "कुठे",
                "when": "केव्हा",
                "why": "का",
                "how": "कसे",
                "what": "काय",
                "which": "कोणते",
                "who": "कोण",
                "whom": "कोणाला",
                "whose": "कोणाचे",
                "this": "हे",
                "that": "ते",
                "these": "हे",
                "those": "ते",
                "i": "मी",
                "you": "तुम्ही",
                "he": "तो",
                "she": "ती",
                "it": "ते",
                "we": "आम्ही",
                "they": "ते",
                "me": "मला",
                "him": "त्याला",
                "her": "तिला",
                "us": "आम्हाला",
                "them": "त्यांना",
                "my": "माझे",
                "your": "तुमचे",
                "his": "त्याचे",
                "her": "तिचे",
                "its": "त्याचे",
                "our": "आमचे",
                "their": "त्यांचे",
                "mine": "माझे",
                "yours": "तुमचे",
                "hers": "तिचे",
                "ours": "आमचे",
                "theirs": "त्यांचे"
            },
            "mr-en": {
                "नमस्कार": "hello",
                "निरोप": "goodbye",
                "धन्यवाद": "thank you",
                "कृपया": "please",
                "होय": "yes",
                "नाही": "no",
                "पाणी": "water",
                "अन्न": "food",
                "मित्र": "friend",
                "प्रेम": "love",
                "भारत": "india",
                "भाषा": "language",
                "तुम्ही कसे आहात": "how are you",
                "तुमचे नाव काय आहे": "what is your name",
                "तुम्ही कुठून आहात": "where are you from",
                "मी तुम्हाला प्रेम करतो": "i love you",
                "शुभ सकाळ": "good morning",
                "शुभ रात्री": "good night",
                "स्वागत": "welcome",
                "माफ करा": "excuse me",
                "मी माफी मागतो": "i am sorry",
                "मदत": "help",
                "किंमत": "price",
                "वेळ": "time",
                "दिवस": "day",
                "रात्र": "night",
                "आज": "today",
                "उद्या": "tomorrow",
                "काल": "yesterday",
                "आठवडा": "week",
                "महिना": "month",
                "वर्ष": "year",
                "आई": "mother",
                "वडील": "father",
                "बहीण": "sister",
                "भाऊ": "brother",
                "मुलगा": "son",
                "मुलगी": "daughter",
                "घर": "house",
                "कार": "car",
                "पुस्तक": "book",
                "फोन": "phone",
                "संगणक": "computer",
                "काम": "work",
                "शाळा": "school",
                "रुग्णालय": "hospital",
                "हॉटेल": "restaurant",
                "हॉटेल": "hotel",
                "बाजार": "market",
                "दुकान": "store",
                "पैसे": "money",
                "बँक": "bank",
                "डॉक्टर": "doctor",
                "शिक्षक": "teacher",
                "विद्यार्थी": "student",
                "बाल": "child",
                "पुरुष": "man",
                "स्त्री": "woman",
                "मुलगा": "boy",
                "मुलगी": "girl",
                "लोक": "people",
                "कुटुंब": "family",
                "देश": "country",
                "शहर": "city",
                "गाव": "village",
                "रस्ता": "road",
                "नदी": "river",
                "पर्वत": "mountain",
                "झाड": "tree",
                "फूल": "flower",
                "फळ": "fruit",
                "भाजी": "vegetable",
                "मांस": "meat",
                "मासे": "fish",
                "भाकरी": "bread",
                "तांदूळ": "rice",
                "दूध": "milk",
                "चहा": "tea",
                "कॉफी": "coffee",
                "पाणी": "water",
                "रस": "juice",
                "बीअर": "beer",
                "वाइन": "wine",
                "संगीत": "music",
                "नृत्य": "dance",
                "गीत": "song",
                "चित्रपट": "movie",
                "खेळ": "game",
                "खेळ": "sport",
                "फुटबॉल": "football",
                "क्रिकेट": "cricket",
                "टेनिस": "tennis",
                "प्रवास": "travel",
                "सुट्टी": "holiday",
                "सुट्टी": "vacation",
                "प्रवास": "trip",
                "उड्डाण": "flight",
                "ट्रेन": "train",
                "बस": "bus",
                "कार": "car",
                "सायकल": "bicycle",
                "चालणे": "walk",
                "धावणे": "run",
                "पोहरणे": "swim",
                "उड्डाण करणे": "fly",
                "चालवणे": "drive",
                "खाणे": "eat",
                "प्याहार करणे": "drink",
                "झोपणे": "sleep",
                "जागणे": "wake up",
                "वाचणे": "read",
                "लिहिणे": "write",
                "ऐकणे": "listen",
                "बोलणे": "speak",
                "पाहणे": "see",
                "पाहणे": "watch",
                "पाहणे": "look",
                "विचार करणे": "think",
                "माहित असणे": "know",
                "समजणे": "understand",
                "शिकणे": "learn",
                "शिकवणे": "teach",
                "अभ्यास करणे": "study",
                "काम करणे": "work",
                "खेळणे": "play",
                "गाणे": "sing",
                "नाचणे": "dance",
                "बनवणे": "cook",
                "स्वच्छ करणे": "clean",
                "धुणे": "wash",
                "खरेदी करणे": "buy",
                "विकणे": "sell",
                "देणे": "give",
                "घेणे": "take",
                "आणणे": "bring",
                "पाठवणे": "send",
                "मिळवणे": "receive",
                "कॉल करणे": "call",
                "बोलणे": "talk",
                "भेटणे": "meet",
                "भेट देणे": "visit",
                "मदत करणे": "help",
                "पाठिंबा देणे": "support",
                "प्रेम करणे": "love",
                "आवडणे": "like",
                "तिरस्कार करणे": "hate",
                "हवे असणे": "want",
                "गरज असणे": "need",
                "असणे": "have",
                "आहे": "has",
                "होते": "had",
                "करणे": "do",
                "करतो": "does",
                "केले": "did",
                "करेल": "will",
                "करेल": "would",
                "शकतो": "can",
                "शकतो": "could",
                "केले पाहिजे": "should",
                "केलेच पाहिजे": "must",
                "शकतो": "may",
                "शकतो": "might",
                "इथे": "here",
                "तिथे": "there",
                "कुठे": "where",
                "केव्हा": "when",
                "का": "why",
                "कसे": "how",
                "काय": "what",
                "कोणते": "which",
                "कोण": "who",
                "कोणाला": "whom",
                "कोणाचे": "whose",
                "हे": "this",
                "ते": "that",
                "हे": "these",
                "ते": "those",
                "मी": "i",
                "तुम्ही": "you",
                "तो": "he",
                "ती": "she",
                "ते": "it",
                "आम्ही": "we",
                "ते": "they",
                "मला": "me",
                "त्याला": "him",
                "तिला": "her",
                "आम्हाला": "us",
                "त्यांना": "them",
                "माझे": "my",
                "तुमचे": "your",
                "त्याचे": "his",
                "तिचे": "her",
                "त्याचे": "its",
                "आमचे": "our",
                "त्यांचे": "their",
                "माझे": "mine",
                "तुमचे": "yours",
                "तिचे": "hers",
                "आमचे": "ours",
                "त्यांचे": "theirs"
            }
        };

        // Show notification
        function showNotification(message, isError = false) {
            notificationText.textContent = message;
            notification.classList.add('show');
            
            if (isError) {
                notification.classList.add('error');
                notification.innerHTML = '<i class="fas fa-exclamation-circle"></i> ' + message;
            } else {
                notification.classList.remove('error');
                notification.innerHTML = '<i class="fas fa-check-circle"></i> ' + message;
            }
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }
        
        // Mock translation function
        function mockTranslate(text, from, to) {
            const key = `${from}-${to}`;
            const dictionary = mockTranslations[key];
            
            if (!dictionary) {
                // Try to find a translation through English as an intermediate language
                if (from !== 'en' && mockTranslations[`en-${to}`]) {
                    const toEnKey = `${from}-en`;
                    const toEnDict = mockTranslations[toEnKey];
                    
                    if (toEnDict) {
                        // First translate to English
                        const enText = text
                            .toLowerCase()
                            .split(' ')
                            .map(word => {
                                const cleanWord = word.replace(/[.,!?;:]/g, '');
                                const punctuation = word.replace(cleanWord, '');
                                return toEnDict[cleanWord] 
                                    ? toEnDict[cleanWord] + punctuation 
                                    : `[${word}]`;
                            })
                            .join(' ');
                        
                        // Then translate from English to target language
                        return mockTranslate(enText, 'en', to);
                    }
                }
                
                return `[Demo: Translation from ${from} to ${to} not available]`;
            }
            
            // Simple word-by-word translation (for demo only)
            return text
                .toLowerCase()
                .split(' ')
                .map(word => {
                    // Remove punctuation for lookup
                    const cleanWord = word.replace(/[.,!?;:]/g, '');
                    const punctuation = word.replace(cleanWord, '');
                    
                    return dictionary[cleanWord] 
                        ? dictionary[cleanWord] + punctuation 
                        : `[${word}]`;
                })
                .join(' ');
        }
        
        // Translate text using selected API
        async function translateText() {
            const text = sourceText.value.trim();
            
            if (!text) {
                showNotification("Please enter text to translate", true);
                return;
            }
            
            const from = sourceLang.value;
            const to = targetLang.value;
            
            // Show loading
            loading.classList.add('show');
            translateBtn.disabled = true;
            
            try {
                let translated;
                
                if (currentApi === 'mock') {
                    // Use mock translation
                    translated = mockTranslate(text, from, to);
                } else if (currentApi === 'libre') {
                    // Use LibreTranslate API
                    const response = await fetch('https://libretranslate.de/translate', {
                        method: 'POST',
                        body: JSON.stringify({
                            q: text,
                            source: from,
                            target: to,
                            format: 'text'
                        }),
                        headers: {
                            'Content-Type': 'application/json'
                        }
                    });
                    
                    if (!response.ok) {
                        throw new Error('LibreTranslate failed');
                    }
                    
                    const data = await response.json();
                    translated = data.translatedText;
                } else if (currentApi === 'google') {
                    // Use Google Translate API via a proxy
                    const url = `https://translate.googleapis.com/translate_a/single?client=gtx&sl=${from}&tl=${to}&dt=t&q=${encodeURIComponent(text)}`;
                    const response = await fetch(url);
                    
                    if (!response.ok) {
                        throw new Error('Google Translate failed');
                    }
                    
                    const data = await response.json();
                    translated = data[0].map(item => item[0]).join('');
                }
                
                translatedText.value = translated;
                
                // Add to history
                addToHistory(text, translated, from, to);
                
                showNotification("Translation completed!");
            } catch (error) {
                console.error('Translation error:', error);
                
                // Fallback to mock translation
                const mockResult = mockTranslate(text, from, to);
                translatedText.value = mockResult;
                
                showNotification("Using demo translation. Real translation services may be unavailable.", true);
            } finally {
                loading.classList.remove('show');
                translateBtn.disabled = false;
            }
        }
        
        // Swap languages
        function swapLanguages() {
            const temp = sourceLang.value;
            sourceLang.value = targetLang.value;
            targetLang.value = temp;
            
            // Swap text if both text areas have content
            if (sourceText.value.trim() && translatedText.value.trim()) {
                const tempText = sourceText.value;
                sourceText.value = translatedText.value;
                translatedText.value = tempText;
            }
        }
        
        // Clear all text
        function clearAll() {
            sourceText.value = '';
            translatedText.value = '';
            showNotification("Text cleared");
        }
        
        // Copy translation to clipboard
        function copyTranslation() {
            if (!translatedText.value.trim()) {
                showNotification("No translation to copy", true);
                return;
            }
            
            translatedText.select();
            document.execCommand('copy');
            showNotification("Copied to clipboard!");
        }
        
        // Speak translated text
        function speakText() {
            if (!translatedText.value.trim()) {
                showNotification("No text to speak", true);
                return;
            }
            
            // Check if browser supports speech synthesis
            if ('speechSynthesis' in window) {
                const utterance = new SpeechSynthesisUtterance(translatedText.value);
                
                // Set language based on target language
                utterance.lang = targetLang.value;
                
                speechSynthesis.speak(utterance);
                showNotification("Speaking translation...");
            } else {
                showNotification("Speech synthesis not supported in your browser", true);
            }
        }
        
        // Add to translation history
        function addToHistory(source, translation, from, to) {
            const historyItem = {
                source,
                translation,
                from,
                to,
                timestamp: new Date().toISOString()
            };
            
            // Add to beginning of array
            translationHistory.unshift(historyItem);
            
            // Keep only last 10 items
            if (translationHistory.length > 10) {
                translationHistory = translationHistory.slice(0, 10);
            }
            
            // Save to localStorage
            localStorage.setItem('translationHistory', JSON.stringify(translationHistory));
            
            // Update history display
            updateHistoryDisplay();
        }
        
        // Update history display
        function updateHistoryDisplay() {
            historyList.innerHTML = '';
            
            if (translationHistory.length === 0) {
                historyList.innerHTML = '<p style="color: #888; text-align: center;">No translation history yet</p>';
                return;
            }
            
            translationHistory.forEach((item, index) => {
                const historyItem = document.createElement('div');
                historyItem.className = 'history-item';
                
                const fromLang = sourceLang.querySelector(`option[value="${item.from}"]`).textContent;
                const toLang = targetLang.querySelector(`option[value="${item.to}"]`).textContent;
                
                historyItem.innerHTML = `
                    <div class="source">${item.source}</div>
                    <div class="translation">${item.translation}</div>
                    <div class="languages">${fromLang} → ${toLang}</div>
                `;
                
                historyItem.addEventListener('click', () => {
                    sourceText.value = item.source;
                    translatedText.value = item.translation;
                    sourceLang.value = item.from;
                    targetLang.value = item.to;
                });
                
                historyList.appendChild(historyItem);
            });
        }
        
        // Clear history
        function clearHistory() {
            translationHistory = [];
            localStorage.removeItem('translationHistory');
            updateHistoryDisplay();
            showNotification("History cleared");
        }
        
        // Event Listeners
        translateBtn.addEventListener('click', translateText);
        clearBtn.addEventListener('click', clearAll);
        copyBtn.addEventListener('click', copyTranslation);
        speakBtn.addEventListener('click', speakText);
        swapBtn.addEventListener('click', swapLanguages);
        clearHistoryBtn.addEventListener('click', clearHistory);
        
        // API selector
        apiButtons.forEach(btn => {
            btn.addEventListener('click', () => {
                apiButtons.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                currentApi = btn.dataset.api;
                
                if (currentApi === 'mock') {
                    showNotification("Using demo translation mode");
                } else if (currentApi === 'libre') {
                    showNotification("Using LibreTranslate API");
                } else if (currentApi === 'google') {
                    showNotification("Using Google Translate API");
                }
            });
        });
        
        // Auto-translate on Enter key (optional)
        sourceText.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && e.ctrlKey) {
                translateText();
            }
        });
        
        // Initialize
        window.addEventListener('DOMContentLoaded', () => {
            updateHistoryDisplay();
            
            // Set default sample text
            sourceText.value = "Hello friend! How are you today? I hope you're having a wonderful day.";
        });
    </script>
</body>
</html>
