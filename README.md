<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>注音符號魔法之旅 教學計畫</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
    <!-- 引入 Tone.js 函式庫，用於生成聲音 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.min.js"></script>
    <!-- Chosen Palette: Playful Pastels -->
    <!-- Application Structure Plan: The application uses a vertical stepper/timeline navigation on the left to represent the 11-week curriculum. This linear structure is the most logical way to present a teaching plan. Clicking a week updates the main content area on the right, showing that week's detailed plan including theme, objectives, key symbols, and activities. This design allows teachers and parents to easily track progress and access specific weekly content in an intuitive, non-cluttered manner. -->
    <!-- Visualization & Content Choices: Report Info: 11-week Bopomofo curriculum. Goal: Provide an easy-to-navigate interactive plan. Viz/Presentation Method: The core structure is a vertical timeline navigator built with HTML/CSS. Each week's content is displayed in structured cards. The Bopomofo symbols are presented as interactive 'chips'. Interaction: Users click a week on the timeline to display its content. Hovering over a symbol chip provides a subtle visual feedback. Justification: This structure mirrors the pedagogical flow of the source report, which is essential for a teaching plan. It organizes the information logically for its target users (teachers/parents). Library/Method: Vanilla JavaScript for dynamic content rendering and state management. Tailwind CSS for styling. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: #FDF8F0; /* 柔和的米白色背景 */
        }
        /* 注音符號晶片樣式 */
        .bopomofo-chip {
            transition: all 0.2s ease-in-out; /* 平滑過渡效果 */
            cursor: pointer; /* 顯示為可點擊 */
        }
        /* 注音符號晶片懸停效果 */
        .bopomofo-chip:hover { 
            transform: scale(1.1); /* 放大效果 */
            background-color: #FEEBC8; /* 淺橘色背景 */
        }
        /* 步驟導航器活躍項目的圓圈樣式 */
        .stepper-item.active .stepper-circle {
            background-color: #F59E0B; /* 橘色背景 */
            color: white; /* 白色文字 */
            border-color: #F59E0B; /* 橘色邊框 */
        }
        /* 步驟導航器活躍項目的標題樣式 */
        .stepper-item.active .stepper-title {
            color: #D97706; /* 深橘色文字 */
            font-weight: 700; /* 加粗字體 */
        }
        /* 步驟導航器線條樣式 */
        .stepper-line {
            border-color: #E5E7EB; /* 淺灰色邊框 */
        }
        /* 步驟導航器活躍項目後的線條樣式 */
        .stepper-item.active ~ .stepper-item .stepper-line {
            border-color: #E5E7EB; /* 淺灰色邊框 */
        }
        /* 步驟導航器已完成項目的圓圈樣式 */
        .stepper-item.completed .stepper-circle {
            background-color: #FCD34D; /* 淺黃色背景 */
            border-color: #FCD34D; /* 淺黃色邊框 */
        }
        /* 步驟導航器已完成項目的線條樣式 */
        .stepper-item.completed .stepper-line {
            border-color: #FCD34D; /* 淺黃色邊框 */
        }
        /* 淡入動畫 */
        @keyframes fade-in {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
            animation: fade-in 0.5s ease-out forwards;
        }
    </style>
</head>
<body class="antialiased text-stone-800">

    <div class="container mx-auto p-4 md:p-8">
        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-amber-600">注音符號魔法之旅 🌈</h1>
            <p class="text-lg text-stone-500 mt-2">小一新生十一週學習計畫互動指南</p>
        </header>

        <main class="flex flex-col md:flex-row gap-8">
            <!-- 步驟導航區塊 -->
            <aside id="stepper-nav" class="w-full md:w-1/4 lg:w-1/5 flex md:flex-col overflow-x-auto md:overflow-x-visible pb-4 md:pb-0">
                <!-- 步驟項目將由 JavaScript 注入 -->
            </aside>

            <!-- 主要內容顯示區塊 (用於每週計畫) -->
            <section id="weekly-plan-content-area" class="w-full md:w-3/4 lg:w-4/5 bg-white rounded-2xl shadow-lg p-6 md:p-8 min-h-[60vh]">
                <!-- 每週內容將由 JavaScript 注入 -->
            </section>

            <!-- 注音符號總覽內容區塊 (預設隱藏) -->
            <section id="bopomofo-overview-content-area" class="w-full md:w-3/4 lg:w-4/5 bg-white rounded-2xl shadow-lg p-6 md:p-8 min-h-[60vh] hidden">
                <!-- 總覽內容將由 JavaScript 注入 -->
            </section>

            <!-- 注音符號遊戲內容區塊 (預設隱藏) -->
            <section id="bopomofo-game-content-area" class="w-full md:w-3/4 lg:w-4/5 bg-white rounded-2xl shadow-lg p-6 md:p-8 min-h-[60vh] hidden">
                <!-- 遊戲內容將由 JavaScript 注入 -->
            </section>

            <!-- 注音符號結合韻遊戲內容區塊 (預設隱藏) -->
            <section id="bopomofo-compound-game-content-area" class="w-full md:w-3/4 lg:w-4/5 bg-white rounded-2xl shadow-lg p-6 md:p-8 min-h-[60vh] hidden">
                <!-- 結合韻遊戲內容將由 JavaScript 注入 -->
            </section>

            <!-- 注音符號 ㄇ、ㄈ、ㄩ 辨識特訓班遊戲內容區塊 (預設隱藏) -->
            <section id="bopomofo-m-f-yu-game-content-area" class="w-full md:w-3/4 lg:w-4/5 bg-white rounded-2xl shadow-lg p-6 md:p-8 min-h-[60vh] hidden">
                <!-- ㄇ、ㄈ、ㄩ 遊戲內容將由 JavaScript 注入 -->
            </section>

            <!-- 注音冒險王遊戲介紹內容區塊 (預設隱藏) -->
            <section id="game-intro-content-area" class="w-full md:w-3/4 lg:w-4/5 bg-white rounded-2xl shadow-lg p-6 md:p-8 min-h-[60vh] hidden">
                <!-- 遊戲介紹內容將由 JavaScript 注入 -->
            </section>
            
        </main>
        
        <footer class="text-center mt-12 text-stone-400 text-sm">
            <p>每週內容視實際上課進度與學校活動安排日程調整，為小一新生的學習旅程加油！ &copy; 2025</p>
        </footer>
    </div>

    <script>
        // 初始化 Tone.js 的合成器，用於播放聲音
        const synth = new Tone.Synth().toDestination();

        /**
         * 播放注音符號的聲音
         * @param {string} symbol - 要播放聲音的注音符號
         */
        function playSound(symbol) {
            console.log(`播放 ${symbol} 的聲音...`);
            // 播放一個中音 C 的音調，持續八分音符的時長
            // 這裡使用單一音調作為聲音的模擬，實際應用中可根據符號設計更複雜的音效
            synth.triggerAttackRelease("C4", "8n"); 
        }

        // 全域注音符號數據，用於總覽頁面
        const allBopomofoData = {
            initials: ['ㄅ', 'ㄆ', 'ㄇ', 'ㄈ', 'ㄉ', 'ㄊ', 'ㄋ', 'ㄌ', 'ㄍ', 'ㄎ', 'ㄏ', 'ㄐ', 'ㄑ', 'ㄒ', 'ㄓ', 'ㄔ', 'ㄕ', 'ㄖ', 'ㄗ', 'ㄘ', 'ㄙ'],
            finals: ['ㄚ', 'ㄛ', 'ㄜ', 'ㄝ', 'ㄞ', 'ㄟ', 'ㄠ', 'ㄡ', 'ㄢ', 'ㄣ', 'ㄤ', 'ㄥ', 'ㄦ', 'ㄧ', 'ㄨ', 'ㄩ'],
            compoundFinals: [
                '(ㄧㄚ)', '(ㄧㄛ)', '(ㄧㄝ)', '(ㄧㄞ)', '(ㄧㄠ)', '(ㄧㄡ)', '(ㄧㄢ)', '(ㄧㄣ)', '(ㄧㄤ)', '(ㄧㄥ)',
                '(ㄨㄚ)', '(ㄨㄛ)', '(ㄨㄞ)', '(ㄨㄟ)', '(ㄨㄢ)', '(ㄨㄣ)', '(ㄨㄤ)', '(ㄨㄥ)',
                '(ㄩㄝ)', '(ㄩㄢ)', '(ㄩㄣ)', '(ㄩㄥ)'
            ]
        };

        // 注音符號教學計畫數據
        const teachingPlan = [
            // 第 1 週
            {
                week: 1,
                title: "注音總動員 - 認識注音王國", // Updated title
                objective: "認識注音符號的聲符、韻符、結合韻和聲調概念。",
                focus: [
                    { type: '聲符', content: "音節開頭，發音像「ㄅ、ㄆ、ㄇ」。" },
                    { type: '韻符', content: "音節主體，發音像「ㄚ、ㄛ、ㄜ」。" },
                    { type: '結合韻', content: "介符變魔術，兩個音變一個音！" },
                    { type: '聲調', content: "高低變化，讓聲音更有趣！" },
                ],
                symbols: [], // 本週無特定符號，主要為概念介紹
                game: { name: "聲調變變變", description: "用動作或手勢表達聽到的聲調。" }
            },
            // 第 2 週
            {
                week: 2,
                title: "初探注音-注音符號初體驗", // Updated title
                objective: "熟練 ㄅ、ㄆ、ㄇ、ㄉ、ㄧ、ㄠ 的讀、寫、拼音。",
                focus: [
                    { type: '發音與書寫', content: "正確練習本週基本符號。" },
                    { type: '拼音練習', content: "開始雙拼練習，例如：ㄅㄚ、ㄉㄧ。" },
                ],
                symbols: ['ㄅ', 'ㄆ', 'ㄇ', 'ㄉ', 'ㄧ', 'ㄠ'],
                game: { name: "注音賓果", description: "老師念拼音，學生圈出對應的符號。" }
            },
            // 第 3 週
            {
                week: 3,
                title: "進階學習-挑戰翹舌音", // Updated title
                objective: "熟練 ㄈ、ㄜ、ㄓ、ㄔ、ㄨ、ㄚ 的讀、寫、拼音。",
                focus: [
                    { type: '翹舌音特訓', content: "特別練習 ㄓ、ㄔ 的發音，可搭配舌頭位置示意圖。" },
                    { type: '三拼初體驗', content: "初步接觸三拼，如「ㄓㄨㄚ」（聲符 + 介符 + 韻符）。" },
                ],
                symbols: ['ㄈ', 'ㄜ', 'ㄓ', 'ㄔ', 'ㄨ', 'ㄚ'],
                game: { name: "找音標圖", description: "老師念注音，學生從圖卡中找出對應的圖案。" }
            },
            // 第 4 週
            {
                week: 4,
                title: "擴展音系-鼻音、介音大集合！", // Updated title
                objective: "熟練 ㄌ、ㄑ、ㄗ、ㄩ、ㄛ、ㄢ、ㄤ 的讀、寫、拼音。",
                focus: [
                    { type: '相似音區分', content: "仔細分辨 ㄗ 與 ㄑ 的發音差異。" },
                    { type: '鼻音韻符', content: "掌握 ㄢ、ㄤ 的發音技巧。" },
                    { type: '介符應用', content: "學習介符 ㄩ 的拼音方式。" },
                ],
                symbols: ['ㄌ', 'ㄑ', 'ㄗ', 'ㄩ', 'ㄛ', 'ㄢ', 'ㄤ'],
                game: { name: "注音接龍", description: "老師說一個詞，學生接下一個同音或同韻的詞。" }
            },
            // 第 5 週
            {
                week: 5,
                title: "深化理解-拼音越來越厲害", // Updated title
                objective: "熟練 ㄒ、ㄕ、ㄟ、ㄡ、ㄦ 及結合韻 (ㄧㄠ)、(ㄨㄢ) 的讀、寫、拼音。",
                focus: [
                    { type: '發音區分', content: "練習分辨 ㄒ 與 ㄕ 的細微差別。" },
                    { type: '結合韻練習', content: "將 (ㄧㄠ) 和 (ㄨㄢ) 當作一個整體來認讀和拼音。" },
                ],
                symbols: ['ㄒ', 'ㄕ', 'ㄟ', 'ㄡ', 'ㄦ', '(ㄧㄠ)', '(ㄨㄢ)'],
                game: { name: "詞語拼圖", description: "將詞語的注音和對應圖片分開，讓學生進行配對。" }
            },
            // 第 6 週
            {
                week: 6,
                title: "挑戰自我-加速拼讀任務", // Updated title
                objective: "熟練 ㄋ、ㄍ、ㄞ、ㄥ 及結合韻 (ㄧㄚ)、(ㄧㄡ)、(ㄧㄤ) 的讀、寫、拼音。",
                focus: [
                    { type: '速度與準確度', content: "提升整體拼音速度，同時保持發音的準確性。" },
                    { type: '結合韻練習', content: "重點練習 (ㄧㄚ)、(ㄧㄡ)、(ㄧㄤ) 這三個結合韻。" },
                ],
                symbols: ['ㄋ', 'ㄍ', 'ㄞ', 'ㄥ', '(ㄧㄚ)', '(ㄧㄡ)', '(ㄧㄤ)'],
                game: { name: "注音符號鬼牌", description: "類似心臟病的反應遊戲，抽到特定符號時需快速念出並拍打。" }
            },
            // 第 7 週
            {
                week: 7,
                title: "鞏固所學-注音小達人持續進步", // Updated title
                objective: "熟練 ㄐ、ㄙ、ㄝ 及結合韻 (ㄧㄝ)、(ㄨㄚ)、(ㄨㄛ)、(ㄨㄥ) 的讀、寫、拼音。",
                focus: [
                    { type: '常用結合韻', content: "掌握這些日常生活中經常出現的結合韻拼讀。" },
                    { type: '發音複習', content: "再次複習 ㄐ 與 ㄙ 的發音方式。" },
                ],
                symbols: ['ㄐ', 'ㄙ', 'ㄝ', '(ㄧㄝ)', '(ㄨㄚ)', '(ㄨㄛ)', '(ㄨㄥ)'],
                game: { name: "注音故事接龍", description: "學生輪流說出一個包含指定注音的詞語，串成一個小故事。" }
            },
            // 第 8 週
            {
                week: 8,
                title: "提升流暢度-閱讀越來越厲害", // Updated title
                objective: "熟練 ㄊ、ㄎ、ㄣ 及結合韻 (ㄧㄢ)、(ㄧㄥ)、(ㄨㄞ)、(ㄨㄟ) 的讀、寫、拼音。",
                focus: [
                    { type: '提升流暢度', content: "目標是能不假思索地快速拼讀出單詞。" },
                    { type: '鼻音區分', content: "加強分辨 ㄣ 和 ㄥ 鼻音的細微差異。" },
                ],
                symbols: ['ㄊ', 'ㄎ', 'ㄣ', '(ㄧㄢ)', '(ㄧㄥ)', '(ㄨㄞ)', '(ㄨㄟ)'],
                game: { name: "我是小小播報員", description: "讓學生輪流上台，嘗試流暢地朗讀一小段注音短文。" }
            },
            // 第 9 週
            {
                week: 9,
                title: "挑戰複雜度-攻克難關，注音無敵", // Updated title
                objective: "熟練 ㄖ、ㄘ 及結合韻 (ㄧㄛ)、(ㄨㄤ)、(ㄩㄢ)、(ㄩㄥ) 的讀、寫、拼音。",
                focus: [
                    { type: '發音區分', content: "練習分辨 ㄖ 和 ㄘ 的發音。" },
                    { type: '複雜結合韻', content: "挑戰較少見但重要的介音 ㄩ 相關結合韻。" },
                ],
                symbols: ['ㄖ', 'ㄘ', '(ㄧㄛ)', '(ㄨㄤ)', '(ㄩㄢ)', '(ㄩㄥ)'],
                game: { name: "注音尋寶", description: "將寫有注音的紙條藏在教室各處，讓學生找到後正確拼讀。" }
            },
             // 第 10 週
            {
                week: 10,
                title: "最後一哩路-注音符號大滿貫", // Updated title
                objective: "熟練 (ㄧㄞ)、(ㄧㄣ)、(ㄨㄣ)、(ㄩㄝ)、(ㄩㄣ) 的讀、寫、拼音，並初步應用。",
                focus: [
                    { type: '最後的結合韻', content: "學習所有剩餘的結合韻，完成注音符號拼圖。" },
                    { type: '初步應用', content: "綜合應用所有注音，進行簡單的造詞與短句練習。" },
                ],
                symbols: ['(ㄧㄞ)', '(ㄧㄣ)', '(ㄨㄣ)', '(ㄩㄝ)', '(ㄩㄣ)'],
                game: { name: "結合韻配對遊戲", description: "製作結合韻卡片，讓學生找出相同或相關的結合韻。" }
            },
            // 第 11 週
            {
                week: 11,
                title: "畢業慶典-我是注音小博士", // Updated title
                objective: "全面複習所有注音符號，提升拼讀流暢度與注音應用能力。",
                focus: [
                    { type: '總體辨識', content: "快速認讀所有符號。" },
                    { type: '流暢拼讀', content: "提升各音節的拼讀速度與準確性。" },
                    { type: '聲調掌握', content: "熟練聲調運用。" },
                    { type: '注音應用', content: "閱讀注音讀物，嘗試注音寫作。" },
                ],
                symbols: [], // 本週為總複習，無特定新增符號
                game: { name: "注音大富翁", description: "結合所有學習內容的總結遊戲，邊玩邊複習。" }
            },
        ];

        // 步驟導航的配置，定義了每個按鈕的類型和對應的數據索引
        const stepperConfig = [
            { type: 'week', weekNum: 1, teachingPlanIndex: 0, title: "注音總動員 - 認識注音王國" },
            { type: 'overview', title: "注音符號總覽" },
            { type: 'game', title: "形音大考驗" },
            { type: 'compound-game', title: "結合韻　　　魔法練習課" },
            { type: 'week', weekNum: 2, teachingPlanIndex: 1, title: "初探注音-注音符號初體驗" },
            { type: 'game-intro', title: "注音冒險王　　　遊戲介紹" }, // 新增的遊戲介紹頁面
            { type: 'week', weekNum: 3, teachingPlanIndex: 2, title: "進階學習-挑戰翹舌音" },
            { type: 'week', weekNum: 4, teachingPlanIndex: 3, title: "擴展音系-鼻音、介音大集合！" },
            { type: 'm-f-yu-game', title: "ㄇㄈㄩ　　　辨識特訓班" }, 
            { type: 'week', weekNum: 5, teachingPlanIndex: 4, title: "深化理解-拼音越來越厲害" },
            { type: 'week', weekNum: 6, teachingPlanIndex: 5, title: "挑戰自我-加速拼讀任務" },
            { type: 'week', weekNum: 7, teachingPlanIndex: 6, title: "鞏固所學-注音小達人持續進步" },
            { type: 'week', weekNum: 8, teachingPlanIndex: 7, title: "提升流暢度-閱讀越來越厲害" },
            { type: 'week', weekNum: 9, teachingPlanIndex: 8, title: "挑戰複雜度-攻克難關，注音無敵" },
            { type: 'week', weekNum: 10, teachingPlanIndex: 9, title: "最後一哩路-注音符號大滿貫" },
            { type: 'week', weekNum: 11, teachingPlanIndex: 10, title: "畢業慶典-我是注音小博士" },
        ];

        // 全域變數，用於儲存 DOM 元素引用
        let stepperNav;
        let weeklyPlanContentArea;
        let bopomofoOverviewContentArea;
        let bopomofoGameContentArea; 
        let bopomofoCompoundGameContentArea; 
        let bopomofoMFYGameContentArea; 
        let gameIntroContentArea; // 新增的遊戲介紹內容區塊引用

        // 全域狀態，追蹤當前顯示的步驟索引
        let currentActiveStepperIndex = 0; 

        /**
         * 渲染步驟導航器
         * 根據 stepperConfig 數據生成側邊導航欄
         */
        function renderStepper() {
            stepperNav.innerHTML = stepperConfig.map((item, index) => {
                let circleText = '';
                if (item.type === 'week') {
                    circleText = item.weekNum;
                } else if (item.type === 'overview') {
                    circleText = '總';
                } else if (item.type === 'game') {
                    circleText = '形';
                } else if (item.type === 'compound-game') {
                    circleText = '結';
                } else if (item.type === 'm-f-yu-game') {
                    circleText = '辨'; // '辨' for ㄇㄈㄩ辨識特訓班
                } else if (item.type === 'game-intro') { // 新增的遊戲介紹頁面圓圈文字
                    circleText = '遊'; 
                }

                return `
                <div class="stepper-item flex items-center md:items-start md:flex-col md:flex-1 cursor-pointer" onclick="navigateToPage(${index})">
                    <div class="flex flex-col md:flex-row items-center">
                        <div class="stepper-circle w-10 h-10 flex-shrink-0 bg-white border-2 border-gray-300 rounded-full flex items-center justify-center font-bold text-lg text-gray-500 transition-all duration-300">
                            ${circleText}
                        </div>
                        <div class="md:ml-4 text-center md:text-left">
                            <h3 class="stepper-title text-md font-semibold text-stone-600 transition-all duration-300 mt-2 md:mt-0">
                                ${item.type === 'week' ? `第 ${item.weekNum} 週` : item.title}
                            </h3>
                            <p class="text-xs text-stone-400 hidden lg:block">${item.type === 'week' ? item.title : ''}</p>
                        </div>
                    </div>
                    <!-- 步驟線條，最後一個項目不顯示 -->
                    ${index < stepperConfig.length - 1 ? '<div class="stepper-line flex-1 md:flex-auto md:w-px h-full md:h-px bg-gray-300 mx-auto md:ml-5 md:mt-2 transition-all duration-300 -order-1 md:order-none"></div>' : ''}
                </div>
                `;
            }).join('');
        }

        /**
         * 渲染指定週的教學計畫內容
         * @param {number} teachingPlanIndex - teachingPlan 陣列中的索引
         */
        function renderTeachingPlanWeek(teachingPlanIndex) {
            const weekData = teachingPlan[teachingPlanIndex]; // 獲取該週的數據

            let symbolsHTML = '';
            // 如果有本週學習符號，則生成對應的 HTML
            if (weekData.symbols && weekData.symbols.length > 0) {
                symbolsHTML = `
                    <h3 class="text-xl font-bold text-amber-700 mb-4 mt-6">🎯 本週學習符號</h3>
                    <div class="flex flex-wrap gap-3">
                        ${weekData.symbols.map(symbol => 
                            `<div class="bopomofo-chip bg-amber-100 text-amber-800 font-bold text-2xl px-4 py-2 rounded-lg shadow-sm" onclick="playSound('${symbol}')">${symbol}</div>`
                        ).join('')}
                    </div>
                `;
            }
            
            // 更新教學計畫內容區塊的 HTML
            weeklyPlanContentArea.innerHTML = `
                <div class="animate-fade-in">
                    <h2 class="text-3xl font-bold text-amber-800 mb-1">第 ${weekData.week} 週</h2>
                    <p class="text-2xl font-semibold text-stone-600 mb-6">${weekData.title}</p>
                    
                    <!-- 學習目標卡片 -->
                    <div class="bg-amber-50 border-l-4 border-amber-400 p-4 rounded-r-lg mb-6">
                        <p class="font-semibold text-amber-900">學習目標：<span class="font-normal">${weekData.objective}</span></p>
                    </div>

                    <!-- 教學重點區塊 -->
                    <h3 class="text-xl font-bold text-amber-700 mb-4">💡 教學重點</h3>
                    <ul class="list-none space-y-2 mb-6">
                        ${weekData.focus.map(f => `
                            <li class="flex items-start">
                                ${f.type ? `<span class="bg-amber-200 text-amber-800 text-xs font-bold mr-3 px-2.5 py-1 rounded-full">${f.type}</span>` : ''}
                                <span class="text-stone-700">${f.content}</span>
                            </li>
                        `).join('')}
                    </ul>

                    <!-- 本週學習符號區塊（如果存在） -->
                    ${symbolsHTML}

                    <!-- 每週遊戲卡片 -->
                    <div class="mt-8 bg-green-50 border-l-4 border-green-400 p-4 rounded-r-lg">
                        <h3 class="text-xl font-bold text-green-800 mb-2">🎲 每週遊戲</h3>
                        <p class="font-semibold text-green-900">${weekData.game.name}</p>
                        <p class="text-stone-600 text-sm mt-1">${weekData.game.description}</p>
                    </div>
                    
                    <!-- 建議作業與評量卡片 -->
                    <div class="mt-4 bg-sky-50 border-l-4 border-sky-400 p-4 rounded-r-lg">
                        <h3 class="text-xl font-bold text-sky-800 mb-2">✏️ 建議作業與評量</h3>
                        <p class="text-stone-600 text-sm" id="assessment-text-${weekData.week}"></p>
                    </div>
                </div>
            `;
            // 根據週數動態設定建議作業與評量內容
            const assessmentTextElement = document.getElementById(`assessment-text-${weekData.week}`);
            if (assessmentTextElement) {
                if (weekData.week === 1) { // 針對第一週進行內容修改
                    assessmentTextElement.textContent = "邊看邊念注音符號表，發音拼讀預備ＧＯ！";
                } else if (weekData.week === 11) {
                    assessmentTextElement.textContent = "迎接小一注音符號檢測，多聽多寫多練習！";
                } else {
                    assessmentTextElement.textContent = "每日安排鞏固作業（描寫和拼讀），週四進行小測驗，檢視本週學習成效。";
                }
            }
        }
        
        /**
         * 渲染注音符號總覽頁面
         */
        function renderBopomofoOverview() {
            const createBopomofoChips = (symbols, bgColor, textColor) => symbols.map(symbol => 
                `<div class="bopomofo-chip ${bgColor} ${textColor} font-bold text-2xl px-4 py-2 rounded-lg shadow-sm" onclick="playSound('${symbol}')">${symbol}</div>`
            ).join('');

            bopomofoOverviewContentArea.innerHTML = `
                <div class="animate-fade-in">
                    <h2 class="text-3xl font-bold text-blue-800 mb-6">📚 注音符號總覽</h2>
                    <div class="mb-6">
                        <h3 class="text-xl font-bold text-blue-700 mb-3">聲符 (Initials)</h3>
                        <div class="flex flex-wrap gap-3">
                            ${createBopomofoChips(allBopomofoData.initials, 'bg-blue-100', 'text-blue-800')}
                        </div>
                    </div>
                    <div class="mb-6">
                        <h3 class="text-xl font-bold text-blue-700 mb-3">韻符 (Finals)</h3>
                        <div class="flex flex-wrap gap-3">
                            ${createBopomofoChips(allBopomofoData.finals, 'bg-blue-100', 'text-blue-800')}
                        </div>
                    </div>
                    <div class="mb-6">
                        <h3 class="text-xl font-bold text-blue-700 mb-3">結合韻 (Compound Finals)</h3>
                        <div class="flex flex-wrap gap-3">
                            ${createBopomofoChips(allBopomofoData.compoundFinals, 'bg-blue-100', 'text-blue-800')}
                        </div>
                    </div>
                    <button onclick="navigateToPage(0)" class="mt-8 bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded-lg shadow-md transition-all duration-300">
                        返回教學計畫 (第 1 週)
                    </button>
                </div>
            `;
        }

        /**
         * 渲染注音符號遊戲頁面 (形音大考驗！)
         */
        function renderBopomofoGame() {
            bopomofoGameContentArea.innerHTML = `
                <div class="animate-fade-in">
                    <h2 class="text-3xl font-bold text-purple-800 mb-6">注音符號：形音大考驗！</h2>
                    <p class="text-lg text-stone-700 mb-4">小一新生們，我們已經學了很多注音符號了，你們真的很棒！今天，我們要來玩一個特別的挑戰，幫助大家把長得很像或是聲音很像的注音符號分清楚，變成注音小偵探！</p>

                    <h3 class="text-2xl font-bold text-purple-700 mt-8 mb-4">形狀像，聲音差很多：小心陷阱！</h3>
                    <p class="text-stone-700 mb-4">有些注音符號長得很像，就像雙胞胎，但它們的聲音卻完全不同！我們一起來看看，怎麼把這些「假雙胞胎」分辨出來。</p>

                    <!-- ㄉ 和 ㄌ -->
                    <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-yellow-800 mb-2">
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄉ')">ㄉ</span> 和 
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄌ')">ㄌ</span>：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀：</span> 
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄉ</span> 的尾巴是「向上勾」的，像一支小小的釣竿；
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄌ</span> 的尾巴是「向下勾」的，像一個倒著的鉤子。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音：</span>
                            <br>
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄉ</span> (d)： 舌尖輕輕頂住上排牙齒後面，氣流突然衝出，像「叮咚」的「叮」。
                            <br>
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄌ</span> (l)： 舌尖輕輕頂住上排牙齒後面，但氣流從舌頭兩邊流出，像「拉」東西的「拉」。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            記住小秘訣： 「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄉ</span>」像釣竿，往上釣；「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄌ</span>」像溜滑梯，往下溜。
                        </p>
                    </div>

                    <!-- ㄣ 和 ㄥ -->
                    <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-yellow-800 mb-2">
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄣ')">ㄣ</span> 和 
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄥ')">ㄥ</span>： (這個是鼻音韻符，要注意是嘴巴有沒有張開喔！)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀：</span> 
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄣ</span> 像一個小小的「門」或「人」字；
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄥ</span> 像一個大大的「弓」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音：</span>
                            <br>
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄣ</span> (en)： 嘴巴微微張開，舌尖抵住下排牙齒，氣流從鼻子出來，像「嗯？」的「嗯」。
                            <br>
                            <span class="bopomofo-chip bg-yellow-200 text-yellow-900 text-xl px-2 py-0.5 rounded-lg">ㄥ</span> (eng)： 嘴巴張得比較開，舌根抬高，氣流從鼻子出來，像「嗡嗡」的「嗡」。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            記住小秘訣： 「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄣ</span>」嘴巴eng 起來；「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄥ</span>」嘴巴eng 開開。
                        </p>
                    </div>

                    <h3 class="text-2xl font-bold text-purple-700 mt-8 mb-4">形狀差，聲音卻很像：仔細聽！</h3>
                    <p class="text-stone-700 mb-4">還有些注音符號，雖然長得不一樣，但它們的聲音卻有點相似，不仔細聽很容易搞混！</p>

                    <!-- ㄗ 和 ㄐ -->
                    <div class="bg-green-50 border-l-4 border-green-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-green-800 mb-2">
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄗ')">ㄗ</span> 和 
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄐ')">ㄐ</span>：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀：</span> 
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄗ</span> 像數字「7」；
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄐ</span> 像一個「鉤子」加上一個「小彎」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音：</span>
                            <br>
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄗ</span> (z)： 舌尖抵住下排牙齒後方，氣流從中間細縫衝出，像「滋滋」的聲音。
                            <br>
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄐ</span> (j)： 舌面中部貼近上顎，發音更靠前，有點像「雞」的聲音。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            記住小秘訣： 「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄗ</span>」在資料上，舌尖平；「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄐ</span>」在雞肉裡，舌面捲。
                        </p>
                    </div>

                    <!-- ㄘ 和 ㄑ -->
                    <div class="bg-green-50 border-l-4 border-green-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-green-800 mb-2">
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄘ')">ㄘ</span> 和 
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄑ')">ㄑ</span>：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀：</span> 
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄘ</span> 像英文字母「C」；
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄑ</span> 像一個「勾勾」加上一個「口」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音：</span>
                            <br>
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄘ</span> (c)： 舌尖抵住下排牙齒後方，氣流衝出，比 ㄗ 更有力，像「刺」的聲音。
                            <br>
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄑ</span> (q)： 舌面中部貼近上顎，發音比 ㄘ 更靠前，有點像「七」的聲音。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            記住小秘訣： 「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄘ</span>」像刺的尖，舌尖平；「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄑ</span>」像氣球聲，舌面彎。
                        </p>
                    </div>

                    <!-- ㄙ 和 ㄒ -->
                    <div class="bg-green-50 border-l-4 border-green-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-green-800 mb-2">
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄙ')">ㄙ</span> 和 
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-2xl px-3 py-1.5 rounded-lg" onclick="playSound('ㄒ')">ㄒ</span>：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀：</span> 
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄙ</span> 像數字「4」；
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄒ</span> 像一個「叉叉」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音：</span>
                            <br>
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄙ</span> (s)： 舌尖抵住下排牙齒後方，氣流從中間細縫流出，像「嘶嘶」的聲音。
                            <br>
                            <span class="bopomofo-chip bg-green-200 text-green-900 text-xl px-2 py-0.5 rounded-lg">ㄒ</span> (x)： 舌面中部抬高，氣流從舌面和硬顎間的細縫流出，像「西瓜」的「西」。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            記住小秘訣： 「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄙ</span>」在絲巾上，舌尖平；「<span class="bopomofo-chip bg-purple-200 text-purple-900 text-xl px-2 py-0.5 rounded-lg">ㄒ</span>」在鞋子裡，舌面高。
                        </p>
                    </div>

                    <h3 class="text-2xl font-bold text-blue-700 mt-8 mb-4">小撇步：發音矯正小遊戲</h3>
                    <ul class="list-disc list-inside text-stone-700 space-y-2">
                        <li><span class="font-bold">鏡子練習：</span> 請小朋友拿著小鏡子，觀察自己的嘴型和舌頭位置，和老師的示範做比較。</li>
                        <li><span class="font-bold">「找出不同音」遊戲：</span> 老師念一組音，例如：「ㄉㄚ、ㄌㄚ、ㄌㄚ」，讓小朋友找出哪個音不一樣。</li>
                        <li><span class="font-bold">「動作口訣」：</span> 為每個容易混淆的音編一個小動作或口訣，幫助記憶發音時舌頭或嘴巴的位置。例如：翹舌音（ㄓㄔㄕㄖ）就讓舌頭往上捲。</li>
                    </ul>

                    <button onclick="navigateToPage(6)" class="mt-8 bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded-lg shadow-md transition-all duration-300">
                        返回教學計畫 (第 4 週)
                    </button>
                </div>
            `;
        }

        /**
         * 渲染注音符號結合韻遊戲頁面
         */
        function renderBopomofoCompoundGame() {
            const createBopomofoChip = (symbol, colorClass = 'bg-blue-200 text-blue-900') => 
                `<span class="bopomofo-chip ${colorClass} text-xl px-2 py-0.5 rounded-lg" onclick="playSound('${symbol}')">${symbol}</span>`;

            bopomofoCompoundGameContentArea.innerHTML = `
                <div class="animate-fade-in">
                    <h2 class="text-3xl font-bold text-teal-800 mb-6">注音符號：結合韻魔法練習課！</h2>
                    <p class="text-lg text-stone-700 mb-4">嗨，小朋友們！我們之前認識了聲符和韻符，也知道中間有三個小幫手「ㄧ、ㄨ、ㄩ」叫做介符。今天，我們要來玩一個超有趣的魔法遊戲，把這些小幫手和後面的韻符合起來，變成一個全新的「結合韻」！</p>
                    <p class="text-lg text-stone-700 mb-4">結合韻就像變魔術一樣，兩個或三個符號合在一起，就會發出一個新的聲音。學會了它，你們就能拼出更多好聽的字和詞喔！</p>

                    <h3 class="text-2xl font-bold text-teal-700 mt-8 mb-4">第一關：介符「ㄧ」的魔法</h3>
                    <p class="text-stone-700 mb-4">「ㄧ」很喜歡跟其他韻符手牽手，變出好多新聲音！</p>

                    <!-- ㄧ + ㄚ = ㄧㄚ -->
                    <div class="bg-orange-50 border-l-4 border-orange-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-orange-800 mb-2">
                            ${createBopomofoChip('ㄧ')} + ${createBopomofoChip('ㄚ')} = ${createBopomofoChip('(ㄧㄚ)', 'bg-orange-200 text-orange-900 text-2xl px-3 py-1.5')} (ya)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄧ」，然後嘴巴慢慢張大，變成「ㄚ」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄧㄚ)')}、${createBopomofoChip('(ㄧㄚ)')}、${createBopomofoChip('(ㄧㄚ)')}
                            (鴨子、呀！)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：小${createBopomofoChip('ㄧ')}愛${createBopomofoChip('ㄚ')}，變${createBopomofoChip('(ㄧㄚ)')}。
                        </p>
                    </div>

                    <!-- ㄧ + ㄠ = ㄧㄠ -->
                    <div class="bg-orange-50 border-l-4 border-orange-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-orange-800 mb-2">
                            ${createBopomofoChip('ㄧ')} + ${createBopomofoChip('ㄠ')} = ${createBopomofoChip('(ㄧㄠ)', 'bg-orange-200 text-orange-900 text-2xl px-3 py-1.5')} (yao)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄧ」，然後嘴巴慢慢變成「ㄠ」的樣子。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄧㄠ)')}、${createBopomofoChip('(ㄧㄠ)')}、${createBopomofoChip('(ㄧㄠ)')}
                            (腰、搖)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：小${createBopomofoChip('ㄧ')}愛${createBopomofoChip('ㄠ')}，變${createBopomofoChip('(ㄧㄠ)')}。
                        </p>
                    </div>

                    <!-- ㄧ + ㄡ = ㄧㄡ -->
                    <div class="bg-orange-50 border-l-4 border-orange-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-orange-800 mb-2">
                            ${createBopomofoChip('ㄧ')} + ${createBopomofoChip('ㄡ')} = ${createBopomofoChip('(ㄧㄡ)', 'bg-orange-200 text-orange-900 text-2xl px-3 py-1.5')} (you)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄧ」，然後嘴巴慢慢變成「ㄡ」的圓形。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄧㄡ)')}、${createBopomofoChip('(ㄧㄡ)')}、${createBopomofoChip('(ㄧㄡ)')}
                            (游泳、有)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：小${createBopomofoChip('ㄧ')}愛${createBopomofoChip('ㄡ')}，變${createBopomofoChip('(ㄧㄡ)')}。
                        </p>
                    </div>

                    <h3 class="text-2xl font-bold text-teal-700 mt-8 mb-4">第二關：介符「ㄨ」的魔法</h3>
                    <p class="text-stone-700 mb-4">「ㄨ」也很愛玩變身遊戲，它會把聲音變得很圓潤喔！</p>

                    <!-- ㄨ + ㄚ = ㄨㄚ -->
                    <div class="bg-purple-50 border-l-4 border-purple-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-purple-800 mb-2">
                            ${createBopomofoChip('ㄨ')} + ${createBopomofoChip('ㄚ')} = ${createBopomofoChip('(ㄨㄚ)', 'bg-purple-200 text-purple-900 text-2xl px-3 py-1.5')} (wa)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄨ」，嘴巴保持圓圓的，然後變成「ㄚ」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄨㄚ)')}、${createBopomofoChip('(ㄨㄚ)')}、${createBopomofoChip('(ㄨㄚ)')}
                            (挖土、哇！)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：大嘴${createBopomofoChip('ㄨ')}愛${createBopomofoChip('ㄚ')}，變${createBopomofoChip('(ㄨㄚ)')}。
                        </p>
                    </div>

                    <!-- ㄨ + ㄛ = ㄨㄛ -->
                    <div class="bg-purple-50 border-l-4 border-purple-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-purple-800 mb-2">
                            ${createBopomofoChip('ㄨ')} + ${createBopomofoChip('ㄛ')} = ${createBopomofoChip('(ㄨㄛ)', 'bg-purple-200 text-purple-900 text-2xl px-3 py-1.5')} (wo)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄨ」，嘴巴保持圓圓的，然後變成「ㄛ」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄨㄛ)')}、${createBopomofoChip('(ㄨㄛ)')}、${createBopomofoChip('(ㄨㄛ)')}
                            (我、握手)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：大嘴${createBopomofoChip('ㄨ')}愛${createBopomofoChip('ㄛ')}，變${createBopomofoChip('(ㄨㄛ)')}。
                        </p>
                    </div>

                    <!-- ㄨ + ㄞ = ㄨㄞ -->
                    <div class="bg-purple-50 border-l-4 border-purple-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-purple-800 mb-2">
                            ${createBopomofoChip('ㄨ')} + ${createBopomofoChip('ㄞ')} = ${createBopomofoChip('(ㄨㄞ)', 'bg-purple-200 text-purple-900 text-2xl px-3 py-1.5')} (wai)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄨ」，然後嘴巴張開變成「ㄞ」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄨㄞ)')}、${createBopomofoChip('(ㄨㄞ)')}、${createBopomofoChip('(ㄨㄞ)')}
                            (歪掉、外)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：大嘴${createBopomofoChip('ㄨ')}愛${createBopomofoChip('ㄞ')}，變${createBopomofoChip('(ㄨㄞ)')}。
                        </p>
                    </div>

                    <h3 class="text-2xl font-bold text-teal-700 mt-8 mb-4">第三關：介符「ㄩ」的魔法</h3>
                    <p class="text-stone-700 mb-4">「ㄩ」的聲音比較特別，它變出來的結合韻也很可愛喔！</p>

                    <!-- ㄩ + ㄝ = ㄩㄝ -->
                    <div class="bg-blue-50 border-l-4 border-blue-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-blue-800 mb-2">
                            ${createBopomofoChip('ㄩ')} + ${createBopomofoChip('ㄝ')} = ${createBopomofoChip('(ㄩㄝ)', 'bg-blue-200 text-blue-900 text-2xl px-3 py-1.5')} (yue)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄩ」，嘴巴保持圓圓的，然後變成「ㄝ」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄩㄝ)')}、${createBopomofoChip('(ㄩㄝ)')}、${createBopomofoChip('(ㄩㄝ)')}
                            (月亮、約定)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：小魚${createBopomofoChip('ㄩ')}愛${createBopomofoChip('ㄝ')}，變${createBopomofoChip('(ㄩㄝ)')}。
                        </p>
                    </div>

                    <!-- ㄩ + ㄢ = ㄩㄢ -->
                    <div class="bg-blue-50 border-l-4 border-blue-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-blue-800 mb-2">
                            ${createBopomofoChip('ㄩ')} + ${createBopomofoChip('ㄢ')} = ${createBopomofoChip('(ㄩㄢ)', 'bg-blue-200 text-blue-900 text-2xl px-3 py-1.5')} (yuan)
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">怎麼變：</span>先念「ㄩ」，然後嘴巴慢慢變成「ㄢ」的樣子。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">練習：</span>${createBopomofoChip('(ㄩㄢ)')}、${createBopomofoChip('(ㄩㄢ)')}、${createBopomofoChip('(ㄩㄢ)')}
                            (圓圈、元)
                        </p>
                        <p class="text-pink-700 font-semibold mt-2">
                            小口訣：小魚${createBopomofoChip('ㄩ')}愛${createBopomofoChip('ㄢ')}，變${createBopomofoChip('(ㄩㄢ)')}。
                        </p>
                    </div>

                    <button onclick="navigateToPage(0)" class="mt-8 bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded-lg shadow-md transition-all duration-300">
                        返回教學計畫 (第 1 週)
                    </button>
                </div>
            `;
        }

        /**
         * 渲染注音符號 ㄇ、ㄈ、ㄩ 辨識特訓班遊戲頁面
         */
        function renderBopomofoMFYGame() {
            const createBopomofoChip = (symbol, colorClass = 'bg-blue-200 text-blue-900') => 
                `<span class="bopomofo-chip ${colorClass} text-xl px-2 py-0.5 rounded-lg" onclick="playSound('${symbol}')">${symbol}</span>`;

            bopomofoMFYGameContentArea.innerHTML = `
                <div class="animate-fade-in">
                    <h2 class="text-3xl font-bold text-green-800 mb-6">注音符號：ㄇ、ㄈ、ㄩ 辨識特訓班！</h2>
                    <p class="text-lg text-stone-700 mb-4">小朋友們，今天我們要來當小偵探，仔細觀察三個注音符號：ㄇ、ㄈ、ㄩ！它們雖然長得不太一樣，但有時候我們會不小心看錯或念錯。別擔心，老師會教大家小撇步，讓你們一眼就能認出它們，還能正確發音喔！</p>

                    <h3 class="text-2xl font-bold text-green-700 mt-8 mb-4">第一關：形狀辨識</h3>
                    <p class="text-stone-700 mb-4">我們來看看它們長什麼樣子，每個符號都有自己的特色！</p>

                    <!-- ㄇ -->
                    <div class="bg-lime-50 border-l-4 border-lime-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-lime-800 mb-2">
                            ${createBopomofoChip('ㄇ', 'bg-lime-200 text-lime-900 text-2xl px-3 py-1.5')} (m)：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀特點：</span> 彎彎的，像一個倒過來的「W」，又像一扇打開的「門」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">圖像聯想：</span> 像「ㄇ」字型的帽子、或「ㄇ」字型的門。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            小口訣： 兩個彎彎像帽子，這是 ${createBopomofoChip('ㄇ')}。
                        </p>
                    </div>

                    <!-- ㄈ -->
                    <div class="bg-lime-50 border-l-4 border-lime-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-lime-800 mb-2">
                            ${createBopomofoChip('ㄈ', 'bg-lime-200 text-lime-900 text-2xl px-3 py-1.5')} (f)：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀特點：</span> 像一個字母「F」，或是像一隻小鳥在天空「飛」。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">圖像聯想：</span> 像「ㄈ」字型的飛機、或「ㄈ」字型的風車。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            小口訣： 一個彎彎像飛機，這是 ${createBopomofoChip('ㄈ')}。
                        </p>
                    </div>

                    <!-- ㄩ -->
                    <div class="bg-lime-50 border-l-4 border-lime-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-lime-800 mb-2">
                            ${createBopomofoChip('ㄩ', 'bg-lime-200 text-lime-900 text-2xl px-3 py-1.5')} (ü)：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">形狀特點：</span> 像一個水桶，上面有兩個小小的點點，像是水桶的提把，也像一條魚在游動。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">圖像聯想：</span> 像「ㄩ」字型的魚缸，或像「ㄩ」字型的雨滴。
                        </p>
                        <p class="text-purple-700 font-semibold mt-2">
                            小口訣： 水桶加點兩把手，這是 ${createBopomofoChip('ㄩ')}。
                        </p>
                    </div>
                    
                    <h3 class="text-2xl font-bold text-green-700 mt-8 mb-4">第二關：聲音辨識與發音練習</h3>
                    <p class="text-stone-700 mb-4">現在，我們來聽聽它們的聲音，並練習正確的發音，觀察自己的嘴巴有沒有像老師一樣喔！</p>

                    <!-- ㄇ 發音 -->
                    <div class="bg-emerald-50 border-l-4 border-emerald-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-emerald-800 mb-2">
                            ${createBopomofoChip('ㄇ', 'bg-emerald-200 text-emerald-900 text-2xl px-3 py-1.5')} (m)：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音方式：</span> 雙唇緊閉，氣流從鼻腔出來，發出「m」的聲音。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">嘴型：</span> 雙唇閉合。
                        </p>
                        <p class="text-stone-700 mt-2">
                            <span class="font-bold">小練習：</span> 唸唸看：${createBopomofoChip('ㄇㄚ')}、${createBopomofoChip('ㄇㄚ')}、${createBopomofoChip('ㄇㄚ')}（媽媽），${createBopomofoChip('ㄇㄛ')}、${createBopomofoChip('ㄇㄛ')}、${createBopomofoChip('ㄇㄛ')}（摸摸）。
                        </p>
                    </div>

                    <!-- ㄈ 發音 -->
                    <div class="bg-emerald-50 border-l-4 border-emerald-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-emerald-800 mb-2">
                            ${createBopomofoChip('ㄈ', 'bg-emerald-200 text-emerald-900 text-2xl px-3 py-1.5')} (f)：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音方式：</span> 上排牙齒輕輕碰到下唇，氣流從牙齒和嘴唇的縫隙中出來，發出「f」的聲音。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">嘴型：</span> 上牙輕觸下唇。
                        </p>
                        <p class="text-stone-700 mt-2">
                            <span class="font-bold">小練習：</span> 唸唸看：${createBopomofoChip('ㄈㄚ')}、${createBopomofoChip('ㄈㄚ')}、${createBopomofoChip('ㄈㄚ')}（發財），${createBopomofoChip('ㄈㄟ')}、${createBopomofoChip('ㄈㄟ')}、${createBopomofoChip('ㄈㄟ')}（飛）。
                        </p>
                    </div>

                    <!-- ㄩ 發音 -->
                    <div class="bg-emerald-50 border-l-4 border-emerald-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-emerald-800 mb-2">
                            ${createBopomofoChip('ㄩ', 'bg-emerald-200 text-emerald-900 text-2xl px-3 py-1.5')} (ü)：
                        </h4>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">發音方式：</span> 雙唇撮圓，舌尖輕觸下排牙齒，發出像「yu」的聲音，但嘴巴要一直保持圓圓的。
                        </p>
                        <p class="text-stone-700 mb-2">
                            <span class="font-bold">嘴型：</span> 雙唇撮圓。
                        </p>
                        <p class="text-stone-700 mt-2">
                            <span class="font-bold">小練習：</span> 唸唸看：${createBopomofoChip('ㄩ')}、${createBopomofoChip('ㄩ')}、${createBopomofoChip('ㄩ')}（魚），${createBopomofoChip('(ㄩㄝ)')}、${createBopomofoChip('(ㄩㄝ)')}、${createBopomofoChip('(ㄩㄝ)')}（月亮）。
                        </p>
                    </div>

                    <button onclick="navigateToPage(6)" class="mt-8 bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded-lg shadow-md transition-all duration-300">
                        返回教學計畫 (第 4 週)
                    </button>
                </div>
            `;
        }

        /**
         * 渲染注音冒險王遊戲介紹頁面
         */
        function renderGameIntroduction() {
            gameIntroContentArea.innerHTML = `
                <div class="animate-fade-in">
                    <h2 class="text-3xl font-bold text-indigo-800 mb-6">🎮 注音冒險王遊戲介紹</h2>
                    <p class="text-lg text-stone-700 mb-4">注音冒險王為設計iOS與Android平板電腦使用的APP，主要使用者為幼兒園與國小孩童，而學校教師也可以透過本應用程式了解學生學習狀況。提醒推播通知，孩童每日使用時間約10~15分鐘學習注音符號。本應用程式結合多樣特殊教學方式，拼音結合、詞彙學習，讓孩童輕鬆上手，從遊戲中學習注音符號，透過示範教學，以及多樣練習遊戲達到學習效果。用注音符號遊戲示範教學與練習，以每日使用15分鐘為目標，提供學齡前孩童透過數位遊戲增加使用注音符號經驗，或針對環境因素較少接觸注音符號的族群兒童提供輔助教材。</p>

                    <h3 class="text-2xl font-bold text-indigo-700 mt-8 mb-4">🌟 遊戲特點</h3>
                    <p class="text-stone-700 mb-4">注音冒險王APP設計有學生功能以及學習獎勵機制功能，讓孩童更喜歡學習注音符號。</p>

                    <div class="bg-purple-50 border-l-4 border-purple-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-purple-800 mb-2">學生功能</h4>
                        <p class="text-stone-700 mb-2">遊戲中有示範教學搭配練習遊戲讓學生學習注音符號。注音示範教學共有4種類型，分別為注音示範、拼音結合示範、聲調示範、注音與聲調排列示範。每次播放完示範教學，可選擇再播一次，也可以直接選擇進入下個示範或練習遊戲。從遊戲練習當中檢視學習效果。注音冒險王練習遊戲共有14種。有單選、多選、詞彙題型測驗練習，以及能力檢測試題。每個遊戲畫面裡的選項都有回上一頁、重聽鍵、作答進度條。</p>
                    </div>

                        <div class="bg-purple-50 border-l-4 border-purple-400 p-4 rounded-r-lg mb-6">
                        <h4 class="text-xl font-semibold text-purple-800 mb-2">學習獎勵機制</h4>
                        <p class="text-stone-700 mb-2">遊戲關卡中設計機會卡片，通過數個關卡後會獲得一張機會卡片，機會卡片獎勵共有4種：獲得金幣(隨機金額)、商城商品5折(限購一次/一項商品)、獲得一件商品、無任何獎勵。透過角色造型，使用者可以依照自己想要的造型到商城購買配件更換造型。成就徽章分金牌、銀牌、銅牌，未達標著為顯示上鎖圖示，讓孩童更努力想完成闖關遊戲。</p>
                    </div>

                    <h3 class="text-2xl font-bold text-indigo-700 mt-8 mb-4">🗺️ 新增冒險地圖與小遊戲</h3>
                    <p class="text-stone-700 mb-4">新版注音冒險王將原本只有單一地圖增加到五個地圖，分別為南南冒險島、康康冒險島、翰翰冒險島、練功房與挑戰地圖，並增加6種不同的小遊戲，也增加練習的題數，讓孩子能夠更加熟練注音。</p>

                    <h3 class="text-2xl font-bold text-indigo-700 mt-8 mb-4">🚫 移除離線模式</h3>
                    <p class="text-stone-700 mb-4">新版地圖內容更豐富，註冊帳號即可以使用五個學習地圖</p>

                    <h3 class="text-2xl font-bold text-indigo-700 mt-8 mb-4">📝 重新註冊帳號</h3>
                    <p class="text-stone-700 mb-4">舊版的帳號已無法使用，需註冊新帳號才能使用新版的注音冒險王</p>

                    <h3 class="text-2xl font-bold text-indigo-700 mt-8 mb-4">📈 遊戲效應</h3>
                    <p class="text-stone-700 mb-4">對於學習中文的孩童在發展識字能力的過程中，我們預期注音冒險王遊戲能幫助孩童掌握注音符號，並提升其聲韻覺識能力。</p>

                    <h3 class="text-2xl font-bold text-indigo-700 mt-8 mb-4">🔑 登入說明</h3>
                    <ul class="list-disc list-inside text-stone-700 space-y-2 mb-6">
                        <li>進入學校帳號：<span class="font-bold">sses202501</span></li>
                        <li>輸入學校密碼：<span class="font-bold">sses202501</span></li>
                        <li>進入自己帳號：例如：<span class="font-bold">01</span> (座號)</li>
                        <li>輸入自己密碼：例如：<span class="font-bold">5401</span> (54座號)</li>
                        <li>請先配合課本進入康康冒險島挑戰</li>
                    </ul>

                    <h3 class="text-2xl font-bold text-indigo-700 mt-8 mb-4">  注音冒險王APP簡介</h3>
                    <h4 class="text-xl font-semibold text-indigo-800 mb-2">如何下載安裝?</h4>
                    <div class="flex flex-col md:flex-row items-center gap-8 mt-4">
                        <div class="text-center">
                            <p class="text-stone-700 mb-2">掃瞄QR code：</p>
                            <!-- Android QR Code image updated here -->
                            <img src="https://ball.ling.sinica.edu.tw/brain/images/zhuyin.png" alt="Android QR Code" class="mx-auto rounded-lg shadow-md mb-2">
                            <p class="font-bold text-indigo-700">Android 系統</p>
                            <ul class="list-disc list-inside text-stone-700 text-sm mt-1">
                                <li>從Google Play 搜尋「注音冒險王」，或點擊連結下載安裝</li>
                                <li>安裝需求: Android 最低版本需求6.0以上版本/ 注音冒險王APP 約需101MB空間</li>
                            </ul>
                        </div>
                        <div class="text-center">
                            <!-- iOS QR Code image updated here -->
                            <img src="https://scontent.ftpe7-1.fna.fbcdn.net/v/t39.30808-6/518272047_24615415774722975_5915507876987764739_n.jpg?_nc_cat=106&ccb=1-7&_nc_sid=127cfc&_nc_ohc=mkn3DfDd44AQ7kNvwGGE_gK&_nc_oc=Adm1gpFcqDhiglvKbosXmURkv3LCliusTKn2kNaADYkg7puuPd0wGwQsIPdyC_GpDVk&_nc_zt=23&_nc_ht=scontent.ftpe7-1.fna&_nc_gid=0swJYvAmgTICDaicI84s9Q&oh=00_AfS19FCc1oOywngFB66R2FkkeDy6fgBGg1r27066IUn73w&oe=68824C35" alt="iOS QR Code" class="mx-auto rounded-lg shadow-md mb-2">
                            <p class="font-bold text-indigo-700">iOS 系統</p>
                            <ul class="list-disc list-inside text-stone-700 text-sm mt-1">
                                <li>從App store搜尋「注音冒險王」，或點擊連結下載安裝</li>
                                <li>安裝需求: iOS13.0或以後版本 / 注音冒險王APP 約需298MB空間</li>
                                <li>相容裝置:
                                    <ul class="list-none ml-4">
                                        <li>iPad: 需要iPadOS 13.0 或以上版本</li>
                                        <li>iPhone: 需要iOS 13.0 或以上版本</li>
                                        <li>iPod touch: 需要iOS 13.0 或以上版本</li>
                                        <li>Mac: 需要macOS11.0 (或以上版本) 以及配備Apple M1(或以上版本)晶片的Mac</li>
                                    </ul>
                                </li>
                            </ul>
                        </div>
                    </div>

                    <button onclick="navigateToPage(4)" class="mt-8 bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded-lg shadow-md transition-all duration-300">
                        返回教學計畫 (第 2 週)
                    </button>
                </div>
            `;
        }
        
        /**
         * 更新步驟導航器的活躍和完成狀態
         * @param {number} activeStepperIndex - 當前活躍的步驟索引
         */
        function updateStepperState(activeStepperIndex) {
            const items = stepperNav.querySelectorAll('.stepper-item');
            items.forEach((item, index) => {
                item.classList.remove('active', 'completed'); // 移除所有狀態
                if (index < activeStepperIndex) {
                    item.classList.add('completed'); // 設置為已完成
                } else if (index === activeStepperIndex) {
                    item.classList.add('active'); // 設置為活躍
                }
            });
        }

        /**
         * 導航到指定的頁面 (週計畫、總覽或遊戲)
         * @param {number} stepperIndex - stepperConfig 中要導航到的項目索引
         */
        function navigateToPage(stepperIndex) {
            currentActiveStepperIndex = stepperIndex; // 更新當前活躍的步驟索引
            const item = stepperConfig[stepperIndex];

            // 隱藏所有內容區塊
            weeklyPlanContentArea.classList.add('hidden');
            bopomofoOverviewContentArea.classList.add('hidden');
            bopomofoGameContentArea.classList.add('hidden');
            bopomofoCompoundGameContentArea.classList.add('hidden');
            bopomofoMFYGameContentArea.classList.add('hidden'); 
            gameIntroContentArea.classList.add('hidden'); // 隱藏遊戲介紹區塊

            if (item.type === 'week') {
                // 顯示週計畫內容
                weeklyPlanContentArea.classList.remove('hidden');
                renderTeachingPlanWeek(item.teachingPlanIndex); // 渲染指定週的教學計畫
            } else if (item.type === 'overview') {
                // 顯示總覽內容
                bopomofoOverviewContentArea.classList.remove('hidden');
                renderBopomofoOverview(); // 渲染注音符號總覽
            } else if (item.type === 'game') {
                // 顯示形音大考驗遊戲內容
                bopomofoGameContentArea.classList.remove('hidden');
                renderBopomofoGame(); // 渲染注音符號遊戲
            } else if (item.type === 'compound-game') {
                // 顯示結合韻魔法練習課遊戲內容
                bopomofoCompoundGameContentArea.classList.remove('hidden');
                renderBopomofoCompoundGame(); // 渲染注音符號結合韻遊戲
            } else if (item.type === 'm-f-yu-game') {
                // 顯示 ㄇ、ㄈ、ㄩ 辨識特訓班遊戲內容
                bopomofoMFYGameContentArea.classList.remove('hidden');
                renderBopomofoMFYGame(); // 渲染注音符號 ㄇ、ㄈ、ㄩ 辨識特訓班遊戲
            } else if (item.type === 'game-intro') { // 處理遊戲介紹頁面
                gameIntroContentArea.classList.remove('hidden');
                renderGameIntroduction(); // 渲染遊戲介紹內容
            }
            updateStepperState(stepperIndex); // 更新步驟導航器的活躍狀態
        }

        // 頁面加載完成後執行
        document.addEventListener('DOMContentLoaded', () => {
            // 獲取 DOM 元素引用
            stepperNav = document.getElementById('stepper-nav');
            weeklyPlanContentArea = document.getElementById('weekly-plan-content-area');
            bopomofoOverviewContentArea = document.getElementById('bopomofo-overview-content-area');
            bopomofoGameContentArea = document.getElementById('bopomofo-game-content-area');
            bopomofoCompoundGameContentArea = document.getElementById('bopomofo-compound-game-content-area');
            bopomofoMFYGameContentArea = document.getElementById('bopomofo-m-f-yu-game-content-area'); 
            gameIntroContentArea = document.getElementById('game-intro-content-area'); // 獲取遊戲介紹區塊引用

            renderStepper(); // 渲染步驟導航器
            navigateToPage(0); // 默認顯示第一個項目 (第 1 週)
        });
    </script>
</body>
</html>
 
