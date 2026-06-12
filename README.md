<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Yaqeth AI | يقظ</title>
    <!-- استدعاء خط احترافي وأيقونات القائمة -->
    <link href="https://googleapis.com" rel="stylesheet">
    <link rel="stylesheet" href="https://cloudflare.com">
    
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Cairo', sans-serif;
        }
        body {
            background-color: #0b0f19;
            color: #ffffff;
            overflow-x: hidden;
        }
        /* شاشة البداية مع الخلفية وشعار السعودية */
        .welcome-screen {
            position: relative;
            width: 100%;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 20px;
            /* خلفية تعبيرية لأبراج الرياض ليلاً */
            background: linear-gradient(rgba(11, 15, 25, 0.85), rgba(11, 15, 25, 0.95)), 
                        url('https://unsplash.com') no-repeat center center/cover;
        }
        .saudi-logo {
            width: 100px;
            height: auto;
            margin-bottom: 20px;
            filter: drop-shadow(0 0 10px #00a86b);
        }
        .welcome-title {
            font-size: 2.5rem;
            color: #00a86b;
            margin-bottom: 15px;
            font-weight: 700;
        }
        .welcome-text {
            font-size: 1.2rem;
            max-width: 600px;
            margin-bottom: 30px;
            line-height: 1.8;
            color: #cbd5e1;
        }
        /* زر بدء تشغيل النظام مع شاشة الدفع والبريد */
        .btn-start {
            background: linear-gradient(135deg, #00a86b, #0284c7);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.3rem;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 20px rgba(0, 168, 107, 0.4);
            transition: 0.3s ease;
        }
        .btn-start:hover {
            transform: scale(1.05);
            box-shadow: 0 6px 25px rgba(0, 168, 107, 0.6);
        }
        
        /* القائمة الجانبية في جهة اليسار (3 خطوط) */
        .menu-btn {
            position: absolute;
            top: 25px;
            left: 25px;
            font-size: 1.8rem;
            color: white;
            cursor: pointer;
            z-index: 100;
            transition: 0.3s;
        }
        .menu-btn:hover {
            color: #00a86b;
        }
        .sidebar {
            position: fixed;
            top: 0;
            left: -350px; /* مخفية باليسار وتظهر عند الضغط */
            width: 320px;
            height: 100vh;
            background-color: #111827;
            box-shadow: 5px 0 25px rgba(0,0,0,0.5);
            z-index: 99;
            transition: 0.4s ease;
            padding: 80px 20px 20px 20px;
            overflow-y: auto;
            border-right: 2px solid #1f2937;
        }
        .sidebar.active {
            left: 0;
        }
        .close-btn {
            position: absolute;
            top: 25px;
            right: 25px;
            font-size: 1.5rem;
            cursor: pointer;
            color: #9ca3af;
        }
        
        /* خانات الاشتراكات داخل القائمة الجانبية */
        .menu-item {
            background-color: #1f2937;
            padding: 15px;
            border-radius: 12px;
            margin-bottom: 15px;
            border: 1px solid #374151;
            transition: 0.3s;
        }
        .menu-item:hover {
            border-color: #00a86b;
            background-color: #1e293b;
        }
        .item-title {
            font-weight: bold;
            font-size: 1.1rem;
            color: #f3f4f6;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .badge-free {
            background-color: #059669;
            color: white;
            font-size: 0.75rem;
            padding: 2px 8px;
            border-radius: 20px;
        }
        .badge-pro {
            background-color: #d97706;
            color: white;
            font-size: 0.75rem;
            padding: 2px 8px;
            border-radius: 20px;
        }
        .badge-plus {
            background-color: #2563eb;
            color: white;
            font-size: 0.75rem;
            padding: 2px 8px;
            border-radius: 20px;
        }
        .item-desc {
            font-size: 0.85rem;
            color: #9ca3af;
            margin-top: 5px;
            line-height: 1.5;
        }
        
        /* خانة اللغات */
        .lang-section {
            margin-top: 25px;
            border-top: 1px solid #374151;
            padding-top: 20px;
        }
        .lang-label {
            font-size: 1rem;
            font-weight: bold;
            color: #9ca3af;
            margin-bottom: 10px;
            display: block;
        }
        .lang-select {
            width: 100%;
            padding: 10px;
            background-color: #1f2937;
            color: white;
            border: 1px solid #4b5563;
            border-radius: 8px;
            font-size: 0.95rem;
            cursor: pointer;
            outline: none;
        }
        
        /* نافذة تسجيل الدخول لحفظ الأموال والاشتراكات */
        .login-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.8);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        .login-box {
            background-color: #111827;
            padding: 40px;
            border-radius: 16px;
            width: 90%;
            max-width: 400px;
            border: 1px solid #00a86b;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }
        .login-box h3 {
            margin-bottom: 15px;
            color: #ffffff;
        }
        .login-box p {
            font-size: 0.85rem;
            color: #9ca3af;
            margin-bottom: 20px;
        }
        .login-input {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            background-color: #1f2937;
            border: 1px solid #4b5563;
            border-radius: 8px;
            color: white;
            outline: none;
            text-align: center;
        }
        .login-input:focus {
            border-color: #00a86b;
        }
        .btn-submit-login {
            width: 100%;
            background-color: #00a86b;
            color: white;
            border: none;
            padding: 12px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            font-size: 1rem;
        }
    </style>
</head>
<body>

    <!-- زر الثلاثة خطوط في جهة اليسار -->
    <div class="menu-btn" onclick="toggleSidebar()">
        <i class="fas fa-bars"></i>
    </div>

    <!-- القائمة الجانبية -->
    <div class="sidebar" id="sidebar">
        <div class="close-btn" onclick="toggleSidebar()"><i class="fas fa-times"></i></div>
        
        <!-- الخانة 1: السائق الخاص -->
        <div class="menu-item">
            <div class="item-title">
                <span>السائق الخاص</span>
                <span class="badge-free">مجاني 60 د</span>
            </div>
            <div class="item-desc">مجاني لأول 60 دقيقة فقط، بعد ذلك يتطلب الاشتراك في <strong>Yaqeth Plus</strong> للحصول على تركيز غير محدود طوال اليوم.</div>
        </div>

        <!-- الخانة 2: سائق الشاحنات -->
        <div class="menu-item">
            <div class="item-title">
                <span>سائق الشاحنات</span>
                <span class="badge-pro">Yaqeth Pro</span>
            </div>
            <div class="item-desc">ربط كامل بين الشركة والسائق لمراقبة مؤشرات النعاس الفورية وإرسال كافة البيانات لمدير الأسطول مباشرة.</div>
        </div>

        <!-- الخانة 3: سائق التاكسي -->
        <div class="menu-item">
            <div class="item-title">
                <span>سائق التاكسي</span>
                <span class="badge-pro">Yaqeth Pro</span>
            </div>
            <div class="item-desc">اشتراك مخصص لحماية ركاب التاكسي وربط النظام بمركز العمليات لإرسال تنبيهات الأمان الفورية.</div>
        </div>

        <!-- الخانة 4: سائق باصات النقل -->
        <div class="menu-item">
            <div class="item-title">
                <span>سائق باصات النقل</span>
                <span class="badge-pro">Yaqeth Pro</span>
            </div>
            <div class="item-desc">نظام أمان متكامل مخصص للحافلات يضمن رقابة صارمة للسائق لحماية أرواح المسافرين.</div>
        </div>

        <!-- الخانة 5: اختيار اللغات -->
        <div class="lang-section">
            <label class="lang-label"><i class="fas fa-globe"></i> لغة التطبيق / Language</label>
            <select class="lang-select">
                <option value="ar">العربية (الأصلية)</option>
                <option value="en">English (الإنجليزية)</option>
                <option value="ur">اردو (الباكستانية)</option>
                <option value="hi">हिन्दी (الهندية)</option>
                <option value="tl">Tagalog (الفلبينية)</option>
            </select>
        </div>
    </div>

    <!-- شاشة البداية الرئيسية -->
    <div class="welcome-screen">
        <!-- شعار السعودية الافتراضي (يمكنك استبداله برابط صورتك لاحقاً) -->
        <img class="saudi-logo" src="https://wikimedia.org" alt="شعار السعودية">
        

