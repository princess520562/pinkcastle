<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>城堡美學工作室 - 從成分解析開始，真正了解你的肌膚需求</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load Zen Maru Gothic -->
    <link href="https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic:wght@300;400;500;700;900&display=swap" rel="stylesheet">
    
    <!-- Tailwind Custom Configuration -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'bg-light': '#FFE2E2',       
                        'card-white': '#F6F6F6',     
                        'primary-pink': '#DC7292',   
                        'text-dark': '#DC7292',      
                        'accent-light': '#D4A5A5',   
                    },
                    fontFamily: {
                        'sans': ['Zen Maru Gothic', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    
    <style>
        body {
            box-sizing: border-box;
            background-color: #FFF5F5;
            color: #5A4A4A;
            line-height: 1.6;
        }

        .fade-in {
            animation: fadeIn 0.4s ease-in;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .tab-button {
            position: relative;
            transition: all 0.3s ease;
            cursor: pointer;
            border-radius: 9999px;
        }
        .tab-button.active {
            font-weight: 700;
            color: #DC7292;
        }
        .tab-button.active::after {
            content: '';
            position: absolute;
            bottom: -8px;
            left: 50%;
            transform: translateX(-50%);
            width: 80%;
            max-width: 100px;
            height: 2px;
            background: #DC7292;
            border-radius: 1px;
        }
        
        .service-card {
            transition: all 0.3s ease;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }
        .service-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.15) !important;
        }

        /* Toast Notification Styles */
        #toast {
            visibility: hidden;
            min-width: 250px;
            background-color: #DC7292;
            color: #fff;
            text-align: center;
            border-radius: 50px;
            padding: 12px 24px;
            position: fixed;
            z-index: 200;
            left: 50%;
            bottom: 30px;
            transform: translateX(-50%);
            box-shadow: 0 4px 15px rgba(220, 114, 146, 0.4);
        }
        #toast.show {
            visibility: visible;
            animation: fadein 0.5s, fadeout 0.5s 2.5s;
        }
        @keyframes fadein {
            from {bottom: 0; opacity: 0;}
            to {bottom: 30px; opacity: 1;}
        }
        @keyframes fadeout {
            from {bottom: 30px; opacity: 1;}
            to {bottom: 0; opacity: 0;}
        }

        /* Modal Styles */
        .modal-overlay {
            background-color: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(4px);
            transition: opacity 0.3s ease;
        }
        .modal-content {
            transform: translateY(20px);
            transition: all 0.3s ease;
        }
        .modal-active .modal-content {
            transform: translateY(0);
        }

        input[type="date"]::-webkit-calendar-picker-indicator {
            filter: invert(56%) sepia(21%) saturate(1142%) hue-rotate(301deg) brightness(92%) contrast(87%);
            cursor: pointer;
        }
    </style>
</head>

<body class="w-full min-h-screen font-sans bg-bg-light">
    <div id="app" class="w-full min-h-screen flex flex-col">
        <!-- Content will be injected by JavaScript -->
    </div>

    <!-- Booking Modal -->
    <div id="bookingModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 modal-overlay opacity-0 pointer-events-none">
        <div class="bg-white w-full max-w-md rounded-3xl overflow-hidden shadow-2xl modal-content">
            <div class="p-6 bg-primary-pink text-white flex justify-between items-center">
                <h3 class="text-xl font-bold">預約資訊</h3>
                <button onclick="closeBookingModal()" class="text-white/80 hover:text-white">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>
            <div class="p-6 space-y-4">
                <div>
                    <label class="block text-xs font-bold text-accent-light mb-1 uppercase tracking-wider">預約項目</label>
                    <div id="modalServiceName" class="text-lg font-medium text-text-dark"></div>
                </div>

                <div>
                    <label class="block text-xs font-bold text-accent-light mb-1 uppercase tracking-wider">姓名 (暱稱)</label>
                    <input type="text" id="userName" oninput="validateForm()" placeholder="請輸入您的稱呼" class="w-full p-3 rounded-xl border border-pink-100 bg-pink-50/30 text-text-dark focus:outline-none focus:ring-2 focus:ring-primary-pink/50 placeholder:text-accent-light/50">
                </div>
                
                <div>
                    <label class="block text-xs font-bold text-accent-light mb-1 uppercase tracking-wider">選擇日期</label>
                    <input type="date" id="bookingDate" onchange="handleDateChange()" class="w-full p-3 rounded-xl border border-pink-100 bg-pink-50/30 text-text-dark focus:outline-none focus:ring-2 focus:ring-primary-pink/50">
                </div>

                <div>
                    <label class="block text-xs font-bold text-accent-light mb-1 uppercase tracking-wider">選擇時段</label>
                    <div id="timeSlots" class="grid grid-cols-2 gap-2 mt-2">
                        <!-- Time slots will be injected here -->
                    </div>
                    <p class="text-[11px] text-accent-light mt-2 italic">* 如想微調時段，可直接於 LINE 詢問</p>
                </div>

                <button onclick="confirmBooking()" id="confirmBtn" class="w-full py-4 mt-2 rounded-2xl bg-primary-pink text-white font-bold shadow-lg shadow-pink-200 hover:brightness-105 active:scale-95 transition-all disabled:opacity-30 disabled:grayscale disabled:pointer-events-none">
                    一鍵複製訊息到LINE
                </button>
            </div>
        </div>
    </div>

    <!-- Toast Notification Element -->
    <div id="toast">已複製預約格式，請至 LINE 貼上</div>

    <script>
        const contentData = {
            about: {
                title: '關於城堡',
                sections: [
                    {
                        heading: '預約須知',
                        items: [
                            '預約統一使用官方LINE，請提供：時間/姓名/項目',
                            '取消或更改請提前一天告知',
                            '身體按摩只接女性',
                            '工作室位於3樓，會有家人在家',
                            '需配合拍攝紀錄',
                            '空間有限，個人預約請勿攜伴',
                            '每週僅開放1位散客預約身體按摩',
                            '平日固定只有18:00的時段，準時抵達即可',
                            '預約沒有保留10分鐘，遲到仍同意服務，會縮減療程時間，以保障其他顧客權益'
                        ]
                    },
                    {
                        heading: '定金說明',
                        items: [
                            '無故遲到10分鐘當次預約取消，下次預約須付三成定金',
                            '預約當日臨時取消者下次預約須付定金三成，若再次無法前來定金不退還'
                        ]
                    },
                    {
                        heading: '付款方式',
                        items: [
                            '接受現金、轉帳、行動支付',
                            '可開立收據'
                        ]
                    }
                ]
            },
            pricing: {
                title: '價目表',
                categories: [
                    {
                        name: '臉部保養',
                        services: [
                            { name: '基礎保養', price: '1,000', duration: '60分鐘' },
                            { name: '臉部撥筋按摩', price: '1,000', duration: '60分鐘 (不含手工清粉刺)' },
                            { name: '客製化深層護膚', price: '1,200', duration: '90分鐘' },
                            { name: '負壓離子膜', price: '1,500', duration: '90分鐘' },
                            { name: '白鑽燈泡肌', price: '2,200', duration: '120分鐘 (含負壓離子膜)' },
                            { name: 'MTS管理', price: '2,200', duration: '120分鐘(痘疤、毛孔)' },
                            { name: '特殊課程任我配', price: '1,800', duration: '90分鐘' }
                        ]
                    },
                    {
                        name: '身體保養',
                        services: [
                            { name: '精油按摩', price: '1,000', duration: '60分鐘' },
                            { name: '頭部舒壓', price: '700', duration: '50分鐘' },
                            { name: '艾草溫罐', price: '900', duration: '40分鐘' },
                            { name: '全身去角質', price: '900', duration: '40分鐘' },
                            { name: '美背煥膚', price: '1,500', duration: '90分鐘' }
                        ]
                    },
                    {
                        name: '訂製霧眉',
                        services: [
                            { name: '手工霧眉', price: '5,000', duration: '2-3小時 (四個月內補色一次)' },
                            { name: '薄霧點刺眉', price: '6,000', duration: '3-4小時 (四個月內補色一次)' },
                            { name: '補色服務', price: '一年內回補7折', duration: '' }
                        ]
                    },
                    {
                        name: '睫毛管理',
                        services: [
                            { name: '精油睫毛管理', price: '1,200', duration: '50分鐘' }
                        ]
                    }
                ]
            },
            packages: {
                title: '儲值方案',
                plans: [
                    { name: '臉部保養儲值', amount: '10,000', description: '全項享9折優惠', bonus: '適合定期保養的您', icon: '🌟' },
                    { name: '客製化護膚5堂', amount: '5,500', sessions: '5堂', description: '量身打造專屬療程', bonus: '每堂省200元', icon: '✨' },
                    { name: '特殊課程10堂', amount: '15,000', sessions: '10堂', description: '深度保養完整方案', bonus: '再送一組保養品', icon: '🎁' },
                    { name: '精油按摩10堂', amount: '8,000', sessions: '10堂', description: '舒壓放鬆優惠組｜限女性', bonus: '拔罐、刮痧、滑罐任選一', icon: '💆‍♀️' }
                ]
            },
      eyebrow: {

        title: '霧眉須知',

        sections: [

          {

            heading: '以下情形不適合預約',

            items: [

              '❶ 懷孕與哺乳期',

              '❷ 抗凝血藥物使用者',

              '❸ 癌症或免疫力低下者',

              '❹ 糖尿病控制不佳者',

              '❺ 蟹足腫體質',

              '❻ 眉部皮膚病變或傷口',

              '❼ 身體不適（感冒、發燒）',

              '❽ 高度焦慮完美主義者',

              '❾ 麻藥、色料或金屬過敏者',

              '❿ 剛做醫美尚未間隔足夠時間'

            ]

          },

          {

            heading: '術前準備',

            items: [

              '前一週避免使用酸類保養品',

              '前三天停止使用美白產品',

              '當天請勿化妝，保持眉部清潔',

              '避免熬夜，保持良好作息',

              '如有服用抗凝血藥物請提前告知'

            ]

          },

          {

            heading: '術後保養｜當天',

            items: [

              '每小時可用乾淨衛生紙按壓吸附組織液',

              '晚上洗������可搓眉毛，碰到水請用按壓方式吸乾'

            ]

          },

          {

            heading: '術後保養｜連續七天',

            items: [

              '如有需要可塗抹修護膏，保持濕潤',

              '早晚清潔後濕敷5-10分鐘'

            ]

          },

          {

            heading: '術後保養｜一個月',

            items: [

              '避免游泳、大量流汗、溫泉',

              '期間自然脫痂，不可強行剝除',

              '眉毛暫停使用酸類及美白產品',

              '防曬工作要做好'

            ]

          },

          {

            heading: '術後保養｜後續',

            items: [

              '一個月後四個月內預約補色時間',

              '顏色穩定需四至六週',

              '保持正常清潔保養',

              '可恢復正常化妝',

              '建議每年補色以維持效果',

              '有任何問題隨時聯繫我們'

            ]

          }

        ]

      }

    };
        
        let currentTab = 'about';
        let selectedBookingItem = '';
        let selectedTime = '';

        // --- Booking Logic ---

        function openBookingModal(serviceName) {
            selectedBookingItem = serviceName;
            selectedTime = '';
            document.getElementById('modalServiceName').innerText = serviceName;
            document.getElementById('userName').value = '';
            document.getElementById('bookingDate').value = '';
            document.getElementById('timeSlots').innerHTML = '<p class="col-span-2 text-center py-4 text-xs text-accent-light">請先選擇日期</p>';
            validateForm();

            const modal = document.getElementById('bookingModal');
            modal.classList.remove('pointer-events-none', 'opacity-0');
            modal.classList.add('modal-active');
        }

        function closeBookingModal() {
            const modal = document.getElementById('bookingModal');
            modal.classList.add('pointer-events-none', 'opacity-0');
            modal.classList.remove('modal-active');
        }

        function validateForm() {
            const name = document.getElementById('userName').value.trim();
            const date = document.getElementById('bookingDate').value;
            const btn = document.getElementById('confirmBtn');
            
            if (name && date && selectedTime) {
                btn.disabled = false;
            } else {
                btn.disabled = true;
            }
        }

        function handleDateChange() {
            const dateVal = document.getElementById('bookingDate').value;
            if (!dateVal) return;

            const date = new Date(dateVal);
            const day = date.getDay(); // 0 is Sunday, 6 is Saturday
            const isWeekend = (day === 0 || day === 6);

            const weekdaySlots = ['18:00'];
            const weekendSlots = ['09:30', '11:00', '13:30', '15:30'];
            
            const slots = isWeekend ? weekendSlots : weekdaySlots;
            renderTimeSlots(slots);
            selectedTime = '';
            validateForm();
        }

        function renderTimeSlots(slots) {
            const container = document.getElementById('timeSlots');
            container.innerHTML = slots.map(time => `
                <button 
                    onclick="selectTime('${time}')" 
                    id="slot-${time.replace(':', '')}"
                    class="time-slot-btn py-2 px-4 rounded-xl border-2 border-pink-100 text-text-dark font-medium hover:bg-pink-50 transition-all text-sm"
                >
                    ${time}
                </button>
            `).join('');
        }

        function selectTime(time) {
            selectedTime = time;
            // Reset styles
            document.querySelectorAll('.time-slot-btn').forEach(btn => {
                btn.classList.remove('bg-primary-pink', 'text-white', 'border-primary-pink');
                btn.classList.add('border-pink-100', 'text-text-dark');
            });
            // Highlight selected
            const activeBtn = document.getElementById(`slot-${time.replace(':', '')}`);
            activeBtn.classList.remove('border-pink-100', 'text-text-dark');
            activeBtn.classList.add('bg-primary-pink', 'text-white', 'border-primary-pink');
            
            validateForm();
        }

        function confirmBooking() {
            const name = document.getElementById('userName').value.trim();
            const date = document.getElementById('bookingDate').value;
            const message = `您好，我想預約：\n姓名：${name}\n項目：${selectedBookingItem}\n日期：${date}\n時段：${selectedTime}`;
            
            copyToClipboard(message);
            closeBookingModal();
        }

        function copyToClipboard(text) {
            const el = document.createElement('textarea');
            el.value = text;
            document.body.appendChild(el);
            el.select();
            document.execCommand('copy');
            document.body.removeChild(el);
            
            showToast();
        }

        function showToast() {
            const toast = document.getElementById('toast');
            toast.className = "show";
            setTimeout(() => { toast.className = toast.className.replace("show", ""); }, 3000);
        }

        // --- UI Rendering ---

        function renderHeader() {
            const castleIcon = `
                <svg class="w-16 h-16 md:w-20 md:h-20 text-primary-pink" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M10 1v3M14 1v3M1 21h22M3 21v-3h18v3M4 18V9h16v9M7 9v3M17 9v3M12 9v3M5 14h14M8 17h8M10 5h4l-2-2zM12 14v4"></path>
                </svg>
            `;

            return `
                <header class="w-full bg-card-white shadow-md">
                    <div class="max-w-6xl mx-auto px-4 py-10 md:py-16">
                        <div class="flex justify-center mb-6">${castleIcon}</div>
                        <h1 class="text-center mb-1 text-text-dark text-3xl md:text-4xl font-semibold tracking-wide">城堡美學</h1>
                        <p class="text-center text-accent-light text-base tracking-wider">從成分解析開始，真正了解你的肌膚需求</p>
                        <p class="text-center text-text-dark text-opacity-80 text-sm mt-1">認識肌膚捷徑課 歡迎詢問</p>
                        <div class="flex justify-center items-center mt-8 gap-3">
                            <div class="w-12 h-px bg-accent-light"></div>
                            <div class="w-2 h-2 rounded-full bg-primary-pink shadow-md"></div>
                            <div class="w-12 h-px bg-accent-light"></div>
                        </div>
                    </div>
                </header>
            `;
        }

        function renderNavigation() {
            const tabs = [
                { tab: 'about', label: '預約須知' },
                { tab: 'pricing', label: '價目表' },
                { tab: 'packages', label: '儲值方案' },
                { tab: 'eyebrow', label: '霧眉必看' }
            ];

            return `
                <nav class="w-full bg-card-white sticky top-0 z-10 border-b border-gray-100">
                    <div class="max-w-6xl mx-auto px-4">
                        <div class="tab-nav flex justify-center gap-2 py-4 overflow-x-auto whitespace-nowrap">
                            ${tabs.map(t => `
                                <button class="tab-button px-4 py-2 text-text-dark text-sm md:text-base ${currentTab === t.tab ? 'active' : ''}" onclick="switchTab('${t.tab}')">
                                    ${t.label}
                                </button>
                            `).join('')}
                        </div>
                    </div>
                </nav>
            `;
        }

        function renderPricing(content) {
            return content.categories.map(category => `
                <div class="mb-12">
                    <div class="flex justify-between items-end mb-6 pb-2 border-b-2 border-primary-pink/30">
                        <h3 class="text-text-dark text-xl font-bold">${category.name}</h3>
                        ${category.name === '臉部保養' ? '<span class="text-[10px] text-primary-pink bg-primary-pink/10 px-3 py-1 rounded-full mb-1">💡 點擊右側按鈕可預約</span>' : ''}
                    </div>
                    <div class="grid grid-cols-1 gap-4">
                        ${category.services.map(service => `
                            <div class="service-card p-4 rounded-xl bg-card-white shadow-sm flex justify-between items-center border border-pink-50">
                                <div class="flex-grow">
                                    <h4 class="text-text-dark text-lg font-medium">${service.name}</h4>
                                    <p class="text-accent-light text-xs mt-0.5">${service.duration}</p>
                                    <p class="text-primary-pink font-bold mt-1">$${service.price}</p>
                                </div>
                                <button 
                                    onclick="openBookingModal('${service.name}')"
                                    class="ml-4 flex flex-col items-center justify-center w-12 h-12 rounded-full bg-primary-pink/10 text-primary-pink hover:bg-primary-pink hover:text-white transition-all active:scale-95 shadow-sm"
                                    title="開啟預約選單"
                                >
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
                                    </svg>
                                    <span class="text-[9px] font-bold mt-0.5">預約</span>
                                </button>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `).join('');
        }

        function renderAbout(content) {
            return content.sections.map(section => `
                <div class="service-card mb-8 p-6 rounded-xl bg-card-white border border-pink-50">
                    <h3 class="mb-4 text-primary-pink text-lg font-bold">${section.heading}</h3>
                    <ul class="space-y-3">
                        ${section.items.map(item => `<li class="text-sm text-text-dark/80 pl-5 relative"><span class="absolute left-0 text-primary-pink">•</span>${item}</li>`).join('')}
                    </ul>
                </div>
            `).join('');
        }

        function renderPackages(content) {
            return `<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                ${content.plans.map(plan => `
                    <div class="service-card p-6 rounded-2xl bg-card-white border border-primary-pink/20">
                        <div class="text-3xl mb-3">${plan.icon}</div>
                        <h3 class="text-lg font-bold text-text-dark">${plan.name}</h3>
                        <p class="text-xs text-text-dark/70 my-2">${plan.description}</p>
                        <div class="flex justify-between items-center mt-4 pt-4 border-t border-dotted border-accent-light">
                            <span class="text-2xl font-black text-primary-pink">$${plan.amount}</span>
                            <span class="text-[10px] bg-bg-light px-2 py-1 rounded text-primary-pink">${plan.bonus}</span>
                        </div>
                    </div>
                `).join('')}
            </div>`;
        }

        function renderEyebrow(content) {
            return content.sections.map(section => `
                <div class="service-card mb-8 p-6 rounded-xl bg-card-white border border-pink-50">
                    <h3 class="mb-4 text-primary-pink text-lg font-bold">${section.heading}</h3>
                    <ul class="space-y-3">
                        ${section.items.map(item => `<li class="text-sm text-text-dark/80 pl-5 relative"><span class="absolute left-0 text-primary-pink">•</span>${item}</li>`).join('')}
                    </ul>
                </div>
            `).join('');
        }

        function switchTab(tab) {
            currentTab = tab;
            renderApp();
            window.scrollTo({ top: document.querySelector('nav').offsetTop, behavior: 'smooth' });
        }

        function renderApp() {
            const app = document.getElementById('app');
            let mainHtml = '';
            const content = contentData[currentTab];

            if (currentTab === 'pricing') mainHtml = renderPricing(content);
            else if (currentTab === 'about') mainHtml = renderAbout(content);
            else if (currentTab === 'packages') mainHtml = renderPackages(content);
            else if (currentTab === 'eyebrow') mainHtml = renderEyebrow(content);
            else mainHtml = renderAbout(content); 

            app.innerHTML = `
                ${renderHeader()}
                ${renderNavigation()}
                <main class="max-w-4xl mx-auto px-4 py-8 fade-in flex-grow">
                    ${mainHtml}
                </main>
                <footer class="w-full mt-auto py-10 bg-card-white border-t border-accent-light/20">
                    <div class="flex flex-col items-center gap-4">
                        <div class="flex flex-wrap justify-center gap-3">
                            <a href="https://www.instagram.com/pink.castle_?igsh=NjJnZWhvNmVtZm15&utm_source=qr" 
                               target="_blank" 
                               class="flex items-center gap-2 px-5 py-2.5 rounded-full bg-white text-primary-pink border border-primary-pink/30 text-sm font-bold shadow-sm hover:shadow-md transition-all active:scale-95">
                                <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                                    <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"></path>
                                </svg>
                                Instagram
                            </a>
                            <a href="https://maps.app.goo.gl/v1MJLD9nA5FqPNdLA?g_st=ipc" 
                               target="_blank" 
                               class="flex items-center gap-2 px-5 py-2.5 rounded-full bg-white text-primary-pink border border-primary-pink/30 text-sm font-bold shadow-sm hover:shadow-md transition-all active:scale-95">
                                <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                                </svg>
                                地圖導航
                            </a>
                        </div>
                        <a href="" target="_blank" class="px-10 py-3 rounded-full bg-primary-pink text-white text-base font-bold shadow-lg hover:bg-primary-pink/90 transition-all active:scale-95">
                            官方 LINE 預約
                        </a>
                        <p class="text-center text-[10px] text-accent-light mt-2">&copy; 2025 城堡美學. All rights reserved.</p>
                    </div>
                </footer>
            `;
        }

        window.onload = renderApp;
    </script>
</body>
</html>```

