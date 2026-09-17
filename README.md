<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>城堡美學 | 專業霧眉美學與臉部保養</title>
    <!-- 引入 Google Fonts (Noto Serif TC / Noto Sans TC) -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&family=Noto+Serif+TC:wght@400;600&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        *, *::before, *::after {
            box-sizing: border-box;
        }

        :root {
            --primary: #b89778;
            --primary-dark: #8c6d52;
            --secondary: #f4efe9;
            --text-main: #3a3836;
            --text-light: #706c68;
            --bg-color: #faf8f5;
            --white: #ffffff;
            --border-color: #e6dfd5;
        }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--bg-color);
            color: var(--text-main);
            font-family: 'Noto Sans TC', sans-serif;
            line-height: 1.7;
            scroll-behavior: smooth;
        }

        /* 導覽列 */
        header {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            background: rgba(250, 248, 245, 0.92);
            backdrop-filter: blur(8px);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 16px 40px;
            z-index: 1000;
        }

        .logo {
            font-family: 'Noto Serif TC', serif;
            font-size: 1.25rem;
            color: var(--primary-dark);
            letter-spacing: 2px;
            font-weight: 600;
        }

        nav {
            display: flex;
            gap: 30px;
        }

        nav a {
            text-decoration: none;
            color: var(--text-light);
            font-size: 0.95rem;
            font-weight: 500;
            transition: color 0.3s;
            cursor: pointer;
            padding-bottom: 2px;
            border-bottom: 2px solid transparent;
        }

        nav a:hover, nav a.active {
            color: var(--primary);
            border-bottom: 2px solid var(--primary);
        }

        /* 頁籤內容區塊控制 */
        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: fadeIn 0.4s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 內容容器 */
        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 120px 20px 80px 20px;
        }

        .section-header {
            text-align: center;
            margin-bottom: 50px;
        }

        .section-header h2 {
            font-family: 'Noto Serif TC', serif;
            font-size: 2rem;
            color: var(--text-main);
            letter-spacing: 2px;
            margin-bottom: 10px;
        }

        .section-header p {
            color: var(--text-light);
            font-size: 0.95rem;
            letter-spacing: 1px;
        }

        .divider {
            width: 40px;
            height: 2px;
            background-color: var(--primary);
            margin: 15px auto 0;
        }

        /* 卡片與網格佈局 */
        .grid-2 {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(440px, 1fr));
            gap: 30px;
        }

        .card {
            background: var(--white);
            padding: 35px;
            border-radius: 12px;
            border: 1px solid var(--border-color);
            box-shadow: 0 4px 20px rgba(0,0,0,0.02);
            position: relative;
        }

        .card h3 {
            font-family: 'Noto Serif TC', serif;
            font-size: 1.25rem;
            color: var(--primary-dark);
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
            border-bottom: 1px solid var(--secondary);
            padding-bottom: 12px;
        }

        .card ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .card li {
            margin-bottom: 12px;
            font-size: 0.95rem;
            color: var(--text-light);
            position: relative;
            padding-left: 18px;
            line-height: 1.6;
        }

        .card li::before {
            content: '•';
            color: var(--primary);
            font-weight: bold;
            font-size: 1.1rem;
            position: absolute;
            left: 0;
            top: -1px;
        }

        /* 預約須知專屬列表排版 */
        .notice-section {
            background-color: var(--white);
            border: 1px solid var(--border-color);
            border-radius: 16px;
            padding: 50px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.02);
        }

        .notice-category {
            margin-bottom: 35px;
        }

        .notice-category:last-child {
            margin-bottom: 0;
        }

        .notice-category h3 {
            font-family: 'Noto Serif TC', serif;
            font-size: 1.15rem;
            color: var(--primary-dark);
            margin-bottom: 15px;
            border-left: 3px solid var(--primary);
            padding-left: 10px;
        }

        .notice-category ul {
            list-style: none;
            padding-left: 13px;
        }

        .notice-category li {
            margin-bottom: 8px;
            font-size: 0.95rem;
            color: var(--text-light);
            position: relative;
            padding-left: 15px;
        }

        .notice-category li::before {
            content: '-';
            color: var(--primary);
            position: absolute;
            left: 0;
        }

        /* 臉部保養課程額外樣式 */
        .course-badge {
            display: inline-block;
            background-color: var(--secondary);
            color: var(--primary-dark);
            font-size: 0.8rem;
            padding: 3px 10px;
            border-radius: 4px;
            margin-bottom: 10px;
            font-weight: 500;
        }

        .price-tag {
            float: right;
            color: var(--primary-dark);
            font-weight: 600;
            font-size: 1.1rem;
        }

        /* 頁尾 */
        footer {
            background-color: #2f2d2b;
            color: var(--white);
            text-align: center;
            padding: 40px 20px;
            font-size: 0.85rem;
            letter-spacing: 1px;
        }

        footer p {
            opacity: 0.7;
            margin-bottom: 6px;
        }

        /* 浮動 Line */
        .floating-line {
            position: fixed;
            bottom: 30px;
            right: 30px;
            background-color: #06C755;
            color: white;
            width: 55px;
            height: 55px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 28px;
            box-shadow: 0 4px 15px rgba(6, 199, 85, 0.3);
            z-index: 999;
            transition: transform 0.3s;
            text-decoration: none;
        }

        .floating-line:hover {
            transform: scale(1.08);
        }

        @media (max-width: 768px) {
            header {
                padding: 15px 20px;
            }
            nav {
                display: none;
            }
            .grid-2 {
                grid-template-columns: 1fr;
            }
            .notice-section {
                padding: 25px;
            }
        }
    </style>
</head>
<body>

    <!-- 導覽列 -->
    <header>
        <div class="logo">城堡美學</div>
        <nav>
            <a href="#notice" class="nav-link active" onclick="switchTab(event, 'notice')">預約須知</a>
            <a href="#care" class="nav-link" onclick="switchTab(event, 'care')">霧眉注意事項</a>
            <a href="#course" class="nav-link" onclick="switchTab(event, 'course')">臉部保養課程</a>
        </nav>
    </header>

    <!-- 預約須知區塊 (第一頁) -->
    <div id="notice" class="container tab-content active">
        <div class="section-header">
            <h2>預約須知</h2>
            <p>BOOKING NOTICES</p>
            <div class="divider"></div>
        </div>

        <div class="notice-section">
            <div class="notice-category">
                <h3>｜預約方式｜</h3>
                <ul>
                    <li>採完全預約制，不接受臨時來店。</li>
                    <li>預約時請提供：姓名、聯絡電話、服務項目及希望預約日期、時段。</li>
                    <li>可透過線上預約系統查看即時可預約時段。</li>
                </ul>
            </div>

            <div class="notice-category">
                <h3>｜更改／取消預約｜</h3>
                <ul>
                    <li>如需更改或取消預約，請於預約時間前 12 小時告知。</li>
                </ul>
            </div>

            <div class="notice-category">
                <h3>｜遲到規範｜</h3>
                <ul>
                    <li>遲到 10 分鐘以上，將依現場狀況調整服務時間，以免影響後續顧客權益。</li>
                    <li>遲到超過 15 分鐘，將視同取消本次預約。</li>
                </ul>
            </div>

            <div class="notice-category">
                <h3>｜當日注意事項｜</h3>
                <ul>
                    <li>請依預約時間準時抵達。</li>
                    <li>若有皮膚過敏、特殊疾病、懷孕、醫美術後，或近期使用酸類、A 酸等產品，請務必提前告知。</li>
                    <li>若當日有發燒、感冒、傳染性疾病或身體不適等情況，請提前聯繫改期，以維護您與其他顧客的健康。</li>
                </ul>
            </div>

            <div class="notice-category">
                <h3>｜付款方式｜</h3>
                <ul>
                    <li>現金、銀行轉帳、LINE Pay、全支付。</li>
                    <li>如需開立收據，請於付款前告知。</li>
                </ul>
            </div>

            <div class="notice-category">
                <h3>｜療程說明｜</h3>
                <ul>
                    <li>療程效果會因個人膚況、生活作息、保養習慣及體質不同而有所差異，實際效果依個人狀況為準。</li>
                    <li>夾粉刺後因個人膚況不同，局部可能出現泛紅、輕微腫脹、結痂或短暫敏感等情形，皆屬正常術後反應，請依照美容師建議加強保濕、防曬及術後保養。</li>
                    <li>術後 3～7 天內請避免使用酸類、去角質產品，並避免高溫環境（如三溫暖, 烤箱, 蒸氣室）及過度摩擦肌膚，以利肌膚修復。</li>
                    <li>如有任何疑問或不適，請立即告知美容師，以便適時調整服務內容。</li>
                </ul>
            </div>

            <div class="notice-category">
                <h3>｜其他事項｜</h3>
                <ul>
                    <li>為維護服務品質，請勿攜伴（特殊情況除外）。</li>
                    <li>工作室保留調整服務內容、價格及預約相關規範之權利。</li>
                    <li>完成預約即表示您已閱讀並同意以上預約須知，感謝您的配合與支持。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- 霧眉注意事項區塊 (第二頁) -->
    <div id="care" class="container tab-content">
        <div class="section-header">
            <h2>霧眉注意事項</h2>
            <p>BROW CARE INSTRUCTIONS</p>
            <div class="divider"></div>
        </div>

        <div class="grid-2">
            <div class="card">
                <h3><i class="fa-regular fa-calendar-check" style="color:var(--primary);"></i> 術前準備事項</h3>
                <ul>
                    <li>施作前請保持充足睡眠，切勿空腹前來。</li>
                    <li>若有蟹足腫、糖尿病、孕婦、嚴重過敏體質或傳染性疾病，請事先告知。</li>
                    <li>近期若有進行醫美微整（如雷射、打肉毒），請間隔一個月再預約。</li>
                    <li>眉毛若有嚴重痘痘、傷口或異位性皮膚炎，請等痊癒後再操作。</li>
                    <li>每個人的臉型都是不對稱的，完美主義者請勿預約。</li>
                </ul>
            </div>
            <div class="card">
                <h3><i class="fa-solid fa-droplet" style="color:var(--primary);"></i> 術後居家護理</h3>
                <ul>
                    <li>操作後前3天，早晚使用生理食鹽水輕輕擦拭眉毛分泌物。</li>
                    <li>結痂期間（約5-7天）請讓其<strong>自然脫落</strong>，絕對不可用手摳抓！</li>
                    <li>一週內避免游泳、三溫暖、泡溫泉或進行大爆汗運動。</li>
                    <li>保養品與防曬請避開眉毛區域，避免酸類美白成分影響留色。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- 臉部保養課程介紹區塊 (第三頁) -->
    <div id="course" class="container tab-content">
        <div class="section-header">
            <h2>城堡保養 · 臉部保養價目表</h2>
            <p>CASTLE SKIN CARE - FACIAL TREATMENTS</p>
            <div class="divider"></div>
        </div>

        <div class="grid-2">
            <div class="card">
                <span class="course-badge">01 基礎保養</span>
                <span class="price-tag">60分鐘 | $1,000</span>
                <h3><i class="fa-solid fa-spa" style="color:var(--primary);"></i> 基礎保養</h3>
                <ul>
                    <li>含清潔、手工清粉刺、敷面</li>
                    <li><strong>適合對象：</strong>油脂分泌旺盛、青春期、定期清潔者</li>
                </ul>
            </div>

            <div class="card">
                <span class="course-badge">02 臉部撥筋按摩</span>
                <span class="price-tag">60分鐘 | $1,000</span>
                <h3><i class="fa-solid fa-hands-bubbles" style="color:var(--primary);"></i> 臉部撥筋按摩</h3>
                <ul>
                    <li>含清潔、撥筋、按摩、導入、敷面</li>
                    <li><strong>適合對象：</strong>粉刺痘痘極少、喜歡按摩者</li>
                </ul>
            </div>

            <div class="card">
                <span class="course-badge">03 客製化深層護膚</span>
                <span class="price-tag">90分鐘 | $1,200</span>
                <h3><i class="fa-solid fa-wand-magic-sparkles" style="color:var(--primary);"></i> 客製化深層護膚</h3>
                <ul>
                    <li>含清潔、手工清粉刺、導入、照光、敷面</li>
                    <li><strong>適合對象：</strong>首次預約，可針對個人喜好搭配產品及內容</li>
                </ul>
            </div>

            <div class="card">
                <span class="course-badge">04 負壓離子膜</span>
                <span class="price-tag">90分鐘 | $1,500</span>
                <h3><i class="fa-solid fa-water" style="color:var(--primary);"></i> 負壓離子膜</h3>
                <ul>
                    <li>含清潔、負壓離子膜、手工清粉刺、導入、照光、敷面</li>
                    <li><strong>適合對象：</strong>想提升肌膚代謝與保養品吸收率者</li>
                </ul>
            </div>

            <div class="card">
                <span class="course-badge">05 特殊護理課程</span>
                <span class="price-tag">90分鐘 | $1,800</span>
                <h3><i class="fa-solid fa-sliders" style="color:var(--primary);"></i> 特殊護理課程任我配</h3>
                <ul>
                    <li>含清潔、手工清粉刺、導入、照光、敷面</li>
                    <li><strong>適合對象：</strong>針對個人喜好搭配產品及內容，導入可升級EMS</li>
                </ul>
            </div>

            <div class="card">
                <span class="course-badge">06 白鑽燈泡肌</span>
                <span class="price-tag">120分鐘 | $2,200</span>
                <h3><i class="fa-solid fa-sun" style="color:var(--primary);"></i> 白鑽燈泡肌</h3>
                <ul>
                    <li>含清潔、負壓離子膜、手工清粉刺、雙導入、照光、敷面</li>
                    <li><strong>適合對象：</strong>負壓離子膜+傳明酸亮白導入雙課程</li>
                </ul>
            </div>

            <div class="card" style="grid-column: 1 / -1; max-width: 500px; margin: 0 auto;">
                <span class="course-badge">07 班密琳亮白</span>
                <span class="price-tag">120分鐘 | $2,600</span>
                <h3><i class="fa-solid fa-gem" style="color:var(--primary);"></i> 班密琳亮白</h3>
                <ul>
                    <li>含清潔、負壓離子膜、手工清粉刺、四重導入、照光、敷面</li>
                    <li><strong>適合對象：</strong>美白淡斑首選，建議搭配產品使用效果更好</li>
                </ul>
            </div>
        </div>
        <div style="text-align: center; margin-top: 40px; font-family: 'Noto Serif TC', serif; color: var(--primary-dark); font-size: 1.1rem; letter-spacing: 2px;">
            認識肌膚捷徑從這裡開始 ♡
        </div>
    </div>

    <!-- 頁尾 -->
    <footer>
        <p>&copy; 2026 城堡美學 STUDIO. All Rights Reserved.</p>
        <p>專業霧眉美學 · 專業臉部保養護理</p>
    </footer>

    <!-- 浮動 Line 按鈕 -->
    <a href="https://line.me" target="_blank" class="floating-line" title="聯絡我們">
        <i class="fa-brands fa-line"></i>
    </a>

    <script>
        // 切換頁籤的 JavaScript 函式
        function switchTab(event, tabId) {
            event.preventDefault();

            // 移除所有頁籤內容的 active 狀態
            const contents = document.querySelectorAll('.tab-content');
            contents.forEach(content => content.classList.remove('active'));

            // 移除導覽列連結的 active 狀態
            const links = document.querySelectorAll('nav a');
            links.forEach(link => link.classList.remove('active'));

            // 啟動點選的目標頁籤內容
            document.getElementById(tabId).classList.add('active');
            
            // 讓點選的導覽列項目高亮
            event.currentTarget.classList.add('active');

            // 滾動回到頂部
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }
    </script>
</body>
</html>
