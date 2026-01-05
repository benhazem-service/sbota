<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>المدير الذكي 2026</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- مكتبة PDF اختيارية، سنعتمد على الطباعة المباشرة لدقتها -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        :root {
            --primary: #4361ee; --primary-dark: #3a0ca3;
            --bg: #f8f9fa; --surface: #ffffff;
            --text: #2b2d42; --text-light: #8d99ae;
            --border: #e0e0e0;
            --work: #4caf50; --holiday: #ffc107; --sick: #ff9800;
            --absent: #f44336; --eid: #9c27b0; --recup: #00bcd4;
            --radius: 16px;
        }

        body.dark-mode {
            --bg: #121212; --surface: #1e1e1e;
            --text: #e0e0e0; --text-light: #b0b0b0;
            --border: #333;
            --primary: #5c7cfa;
        }

        * { box-sizing: border-box; touch-action: manipulation; -webkit-tap-highlight-color: transparent; }
        body { font-family: 'Cairo', sans-serif; background-color: var(--bg); margin: 0; padding-bottom: 80px; color: var(--text); transition: background 0.3s, color 0.3s; }

        /* --- إعدادات الطباعة (CSS Print) --- */
        @media print {
            body * { visibility: hidden; }
            #report-container, #report-container * { visibility: visible; }
            #report-container {
                position: absolute; left: 0; top: 0; width: 100%; margin: 0; padding: 0;
                background: white; color: black; z-index: 99999999; display: block !important;
            }
            @page { size: A4; margin: 5mm; } /* هوامش صغيرة لضمان صفحة واحدة */
            .no-print { display: none !important; }
        }

        /* --- حاوية التقرير --- */
        #report-container {
            position: fixed; top: 0; left: 0; width: 100%; height: 100vh;
            background: #ffffff; color: #000000; padding: 15px;
            font-family: 'Cairo', sans-serif;
            z-index: -9999; opacity: 0; pointer-events: none; overflow-y: auto;
        }
        #report-container.active-print { z-index: 999999; opacity: 1; background: white; }

        /* تنسيقات التقارير */
        .report-header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 5px; margin-bottom: 10px; }
        .report-header h1 { font-size: 18px; margin: 0; }
        .report-header h3 { font-size: 14px; margin: 2px 0; }
        .report-header p { font-size: 12px; margin: 0; }

        /* تنسيق التقرير الشهري (جدول مضغوط) */
        .report-table { width: 100%; border-collapse: collapse; margin-top: 5px; font-size: 10px; direction: rtl; }
        .report-table th, .report-table td { border: 1px solid #666; padding: 3px; text-align: center; height: 18px; }
        .report-table th { background-color: #eee; font-weight: bold; }

        /* تنسيق التقرير السنوي (شبكة المربعات) */
        .year-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr); /* 3 أعمدة */
            gap: 10px;
            margin-top: 10px;
        }
        .month-box {
            border: 1px solid #333;
            border-radius: 8px;
            padding: 8px;
            text-align: center;
            background-color: #fcfcfc;
            page-break-inside: avoid; /* منع انقسام المربع بين الصفحات */
        }
        .month-box h4 { margin: 0 0 5px 0; background: #eee; padding: 2px; border-radius: 4px; font-size: 12px; }
        .month-stat-row { display: flex; justify-content: space-between; font-size: 10px; margin-bottom: 2px; border-bottom: 1px dotted #ccc; }
        .month-stat-row:last-child { border-bottom: none; }
        
        /* ملخصات علوية */
        .report-summary { display: flex; gap: 5px; margin-bottom: 10px; flex-wrap: wrap; justify-content: center; direction: rtl; }
        .report-box { border: 1px solid #ccc; padding: 5px; border-radius: 5px; min-width: 80px; text-align: center; flex: 1; }
        .report-box h3 { margin: 0; font-size: 10px; color: #555; }
        .report-box p { margin: 2px 0 0 0; font-size: 12px; font-weight: bold; }

        /* باقي الستايلات الافتراضية */
        .input-group { position: relative; margin-bottom: 15px; }
        .app-input { width: 100%; padding: 12px; border: 2px solid var(--border); border-radius: 12px; font-family: inherit; font-size: 1rem; outline: none; transition: 0.3s; background: var(--surface); color: var(--text); }
        .app-input:focus { border-color: var(--primary); }
        .toggle-password { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #888; font-size: 1.2rem; }
        .btn-main { width: 100%; padding: 12px; background: var(--primary); color: white; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; margin-top: 10px; }
        .btn-secondary { background: transparent; color: var(--primary); border: 2px solid var(--primary); margin-top: 10px; }
        .btn-close-modal { width: 100%; padding: 12px; margin-top: 15px; background: var(--bg); color: var(--text-light); border: 1px solid var(--border); border-radius: 12px; font-weight: bold; cursor: pointer; display: flex; justify-content: center; align-items: center; gap: 8px; }
        .error-msg { color: #d32f2f; display: none; background: #ffebee; padding: 8px; border-radius: 8px; margin-top: 10px; font-size: 0.85rem; }
        .success-msg { color: #2e7d32; display: none; background: #e8f5e9; padding: 8px; border-radius: 8px; margin-top: 10px; font-size: 0.85rem; }
        .view-section { display: none; } .view-section.active { display: block; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .loading { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); z-index:10000; display:none; justify-content:center; align-items:center; }
        
        #app-container { display: none; padding: 15px; max-width: 600px; margin: 0 auto; }
        .header { display: flex; flex-wrap: wrap; justify-content: space-between; align-items: center; background: var(--surface); padding: 15px; border-radius: var(--radius); box-shadow: 0 4px 20px rgba(0,0,0,0.05); margin-bottom: 20px; gap: 10px; }
        .header-info { display: flex; flex-direction: column; min-width: 120px; }
        .app-main-title { margin: 0; font-size: 1.2rem; color: var(--text); font-weight: bold; }
        .user-sub-title { font-size: 0.9rem; color: var(--text-light); }
        .header-actions { display: flex; gap: 8px; }
        .action-btn { background: var(--bg); border: 1px solid var(--border); width: 36px; height: 36px; border-radius: 10px; cursor: pointer; font-size: 1.1rem; display: flex; justify-content: center; align-items: center; color: var(--text-light); position: relative; }
        .badge-count { position: absolute; top: -5px; left: -5px; background: #f44336; color: white; font-size: 0.7rem; width: 18px; height: 18px; border-radius: 50%; display: none; justify-content: center; align-items: center; border: 2px solid white; }

        .stats-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-bottom: 20px; }
        .stat-card { background: var(--surface); padding: 15px; border-radius: var(--radius); text-align: center; box-shadow: 0 4px 20px rgba(0,0,0,0.05); cursor: pointer; }
        .stat-card h4 { margin: 0; font-size: 0.75rem; color: var(--text-light); }
        .stat-card .val { font-size: 1.3rem; font-weight: 700; color: var(--text); }
        .stat-card .sub { font-size: 0.6rem; color: #999; }
        .full-width { grid-column: span 2; }
        .txt-red { color: #f44336 !important; } .txt-green { color: #4caf50 !important; }
        
        .chart-box { background: var(--surface); padding: 15px; border-radius: var(--radius); box-shadow: 0 4px 20px rgba(0,0,0,0.05); margin-top: 20px; margin-bottom: 20px; height: 260px; position: relative; }

        .calendar-box { background: var(--surface); border-radius: var(--radius); padding: 15px; box-shadow: 0 4px 20px rgba(0,0,0,0.05); }
        .cal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        .days-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; }
        .day-name { font-size: 0.75rem; color: var(--text-light); text-align: center; font-weight: bold; }
        .day-cell { aspect-ratio: 1; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; padding-top: 5px; border-radius: 10px; background: var(--bg); cursor: pointer; position: relative; border: 1px solid transparent; }
        .day-cell span { font-weight: bold; font-size: 0.9rem; color: var(--text); z-index: 2; position: absolute; top: 4px; right: 6px; line-height: 1; }
        .day-cell.today { border-color: var(--primary); background: rgba(67, 97, 238, 0.1) !important; }
        .day-cell.weekend { background-color: rgba(200,200,255,0.15); border: 1px dashed var(--border); }
        body.dark-mode .day-cell.weekend { background-color: rgba(255,255,255,0.05); }
        .day-cell.future { opacity: 0.5; cursor: default; }
        .note-dot { width: 6px; height: 6px; background-color: var(--text); border-radius: 50%; position: absolute; bottom: 5px; left: 50%; transform: translateX(-50%); opacity: 0.6; }
        
        .day-cell.st-work { background-color: var(--work) !important; color: white !important; }
        .day-cell.st-holiday { background-color: var(--holiday) !important; color: #333 !important; }
        .day-cell.st-sick { background-color: var(--sick) !important; color: white !important; }
        .day-cell.st-absent { background-color: var(--absent) !important; color: white !important; }
        .day-cell.st-eid { background-color: var(--eid) !important; color: white !important; }
        .day-cell.st-recup { background-color: var(--recup) !important; color: white !important; }
        .day-cell[class*="st-"] span { color: white !important; }
        .day-cell.st-holiday span { color: #333 !important; }
        .day-cell.nat-holiday { background-color: #f8bbd0 !important; border: 2px solid #ec407a !important; color: #880e4f !important; }
        .day-cell.nat-holiday span { color: #880e4f !important; }
        body.dark-mode .day-cell.nat-holiday { background-color: #4a1c2d !important; color: #fce4ec !important; }
        body.dark-mode .day-cell.nat-holiday span { color: #fce4ec !important; }

        .legend-container { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin-top: 20px; padding: 10px; background: var(--bg); border-radius: var(--radius); }
        .legend-dot { width: 20px; height: 20px; border-radius: 50%; cursor: pointer; border: 2px solid var(--surface); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .lg-work { background-color: var(--work); } .lg-holiday { background-color: var(--holiday); } .lg-sick { background-color: var(--sick); } .lg-absent { background-color: var(--absent); } .lg-recup { background-color: var(--recup); } .lg-eid { background-color: var(--eid); } .lg-nat { background-color: #f8bbd0; border: 2px solid #ec407a; }

        #legend-toast { position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%); background: #333; color: white; padding: 8px 16px; border-radius: 20px; font-size: 0.85rem; opacity: 0; transition: opacity 0.3s; pointer-events: none; z-index: 3000; }
        .show-toast { opacity: 1 !important; }

        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 2000; display: none; justify-content: center; align-items: flex-end; backdrop-filter: blur(2px); }
        #confirmModal, #msgPopup, #nfcPopup, #exportModal { z-index: 99999 !important; align-items: center; } 
        .modal-content { background: var(--surface); width: 100%; max-width: 500px; border-radius: 24px 24px 0 0; padding: 25px; animation: slideUp 0.3s; max-height: 85vh; overflow-y: auto; color: var(--text); }
        #confirmModal .modal-content, #msgPopup .modal-content, #nfcPopup .modal-content, #exportModal .modal-content { border-radius: 24px; max-width: 320px; text-align: center; }
        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
        
        .modal-btns { display: flex; gap: 10px; margin-top: 20px; }
        .btn-save { background: var(--primary); color: white; flex: 2; padding: 12px; border-radius: 10px; border: none; font-weight: bold; cursor: pointer; }
        .btn-del { background: #ffebee; color: #f44336; flex: 1; padding: 12px; border-radius: 10px; border: none; font-weight: bold; cursor: pointer; }
        body.dark-mode .btn-del { background: #4a1c1c; }
        .hidden { display: none; }
        
        .preset-item, .search-item, .detail-item, .msg-item { display: flex; justify-content: space-between; align-items: center; background: var(--bg); padding: 12px; border-radius: 10px; margin-bottom: 6px; font-size: 0.9rem; border-left: 4px solid transparent; }
        .detail-item.pos { border-left-color: var(--work); } .detail-item.neg { border-left-color: var(--absent); } .detail-item.neutral { border-left-color: var(--primary); }
        .msg-item { border-left-color: #ff9800; background: rgba(255, 152, 0, 0.1); flex-direction: column; align-items: flex-start; gap: 8px; }
        .del-icon { color: red; font-weight: bold; padding: 5px 10px; cursor: pointer; background: var(--surface); border-radius: 5px; }
        .d-val { font-weight: bold; direction: ltr; font-family: monospace; font-size: 1rem; }
        .d-val.pos { color: var(--work); } .d-val.neg { color: var(--absent); } .d-val.neutral { color: var(--text-light); }
        .details-header { font-weight: bold; margin: 15px 0 10px; color: var(--primary); font-size: 0.95rem; border-bottom: 2px solid var(--border); padding-bottom: 5px; }
        .msg-popup-text { font-size: 1rem; color: var(--text); margin: 15px 0; background: var(--bg); padding: 15px; border-radius: 10px; border-right: 4px solid var(--primary); text-align: right; }

        .nfc-scanning { animation: pulse 1.5s infinite; border-color: var(--primary); color: var(--primary); }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(67, 97, 238, 0.4); } 70% { box-shadow: 0 0 0 10px rgba(67, 97, 238, 0); } 100% { box-shadow: 0 0 0 0 rgba(67, 97, 238, 0); } }
        .switch-container { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; background: var(--bg); padding: 10px; border-radius: 10px; }
        .switch { position: relative; display: inline-block; width: 40px; height: 24px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #ccc; transition: .4s; border-radius: 24px; }
        .slider:before { position: absolute; content: ""; height: 16px; width: 16px; left: 4px; bottom: 4px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--primary); }
        input:checked + .slider:before { transform: translateX(16px); }
    </style>
</head>
<body>

    <div id="loader" class="loading"><div style="width:40px;height:40px;border:4px solid #ddd;border-top-color:var(--primary);border-radius:50%;animation:spin 1s infinite"></div></div>
    
    <div id="legend-toast"></div>

    <!-- Hidden Report Container for PDF (Fixed) -->
    <div id="report-container"></div>

    <!-- Auth System -->
    <div id="auth-overlay">
        <div class="auth-card">
            <div id="view-login" class="view-section active">
                <div class="auth-header">
                    <h2 id="auth-title-text">نظام الحضور الذكي</h2>
                    <p>تسجيل الدخول</p>
                </div>
                <div class="input-group"><input type="email" id="login-email" class="app-input" placeholder="البريد الإلكتروني"></div>
                <div class="input-group"><input type="password" id="login-pass" class="app-input" placeholder="كلمة المرور"><span class="toggle-password" onclick="togglePass('login-pass')">👁️</span></div>
                <div style="display:flex; align-items:center; margin-bottom:15px; font-size:0.9rem;"><input type="checkbox" id="remember-me" style="margin-left:8px;"> <label for="remember-me">تذكرني</label></div>
                <button class="btn-main" onclick="handleLogin()">دخول</button>
                <button class="btn-main btn-secondary" onclick="switchView('view-signup')">إنشاء حساب جديد</button>
                <div style="margin-top:15px;"><span style="color:var(--primary); cursor:pointer; font-size:0.9rem;" onclick="switchView('view-reset')">نسيت كلمة المرور؟</span></div>
                <div id="login-error" class="error-msg"></div>
            </div>
            <div id="view-signup" class="view-section">
                <div class="auth-header"><h2>إنشاء حساب جديد</h2><p>سيصلك رابط تفعيل على الإيميل</p></div>
                <div class="input-group"><input type="email" id="reg-email" class="app-input" placeholder="البريد الإلكتروني"></div>
                <div class="input-group"><input type="password" id="reg-pass" class="app-input" placeholder="كلمة المرور"><span class="toggle-password" onclick="togglePass('reg-pass')">👁️</span></div>
                <div class="input-group"><input type="password" id="reg-confirm" class="app-input" placeholder="تأكيد كلمة المرور"></div>
                <button class="btn-main" onclick="handleSignup()">تسجيل</button>
                <button class="btn-main btn-secondary" onclick="switchView('view-login')">عودة للدخول</button>
                <div id="reg-error" class="error-msg"></div><div id="reg-success" class="success-msg"></div>
            </div>
            <div id="view-reset" class="view-section">
                <div class="auth-header"><h2>استعادة كلمة المرور</h2><p>أدخل بريدك لاستلام رابط التعيين</p></div>
                <div class="input-group"><input type="email" id="reset-email" class="app-input" placeholder="البريد الإلكتروني"></div>
                <button class="btn-main" onclick="handleReset()">إرسال الرابط</button>
                <button class="btn-main btn-secondary" onclick="switchView('view-login')">عودة</button>
                <div id="reset-msg" class="success-msg"></div><div id="reset-error" class="error-msg"></div>
            </div>
        </div>
    </div>

    <!-- App -->
    <div id="app-container">
        <div class="header">
            <div class="header-info">
                <h2 id="header-title" class="app-main-title">نظام الحضور الذكي</h2>
                <div class="user-sub-title">مرحباً، <span id="u-name">...</span></div>
            </div>
            <div class="header-actions">
                <button id="btn-nfc-scan" class="action-btn" onclick="window.app.startNFCScan()" style="display:none; color:var(--primary); font-weight:bold;">📡</button>
                <button class="action-btn" onclick="window.app.openInbox()">🔔 <span id="msg-badge" class="badge-count">0</span></button>
                <button class="action-btn" onclick="window.app.toggleTheme()">🌓</button>
                <button class="action-btn" onclick="window.app.showExportOptions()" style="color:var(--work)">🖨️</button>
                <button class="action-btn" onclick="window.app.openSearchModal()">🔍</button>
                <button class="action-btn" id="btn-settings" onclick="window.app.openSettings()">⚙️</button>
                <button class="action-btn logout-btn" onclick="handleLogout()" style="color:#ef4444; background:rgba(239,68,68,0.1); border-color:rgba(239,68,68,0.2);">↪️</button>
            </div>
        </div>

        <div class="stats-grid">
            <div class="stat-card" onclick="window.app.showDetails('net')"><h4>رصيد الساعات</h4><div class="val" id="st-net">0</div><div class="sub">ميزان (+/- 8س)</div></div>
            <div class="stat-card" onclick="window.app.showDetails('sat')"><h4>رصيد السبت</h4><div class="val" id="st-sat">0</div><div class="sub">عمل (+4) / آخر (-4)</div></div>
            <div class="stat-card" onclick="window.app.showDetails('sunday')"><h4>الأحد والأعياد</h4><div class="val" id="st-sunday">0</div><div class="sub">يوم تعويض</div></div>
            <div class="stat-card" onclick="window.app.showDetails('leave')"><h4>رصيد العطلة</h4><div class="val" id="st-leave">0</div><div class="sub">تراكمي FIFO</div></div>
            <div class="stat-card" onclick="window.app.showDetails('week')"><h4>هذا الأسبوع</h4><div class="val" id="st-week">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('month')"><h4>هذا الشهر</h4><div class="val" id="st-month">0</div></div>
            <div class="stat-card full-width" onclick="window.app.showDetails('year')"><h4>المجموع السنوي</h4><div class="val" id="st-year">0</div></div>
        </div>

        <div class="calendar-box">
            <div class="cal-header">
                <button class="action-btn" onclick="window.app.navMonth(-1)">&#10094;</button>
                <div style="font-weight:bold; color:var(--text)" id="cal-title"></div>
                <button class="action-btn" onclick="window.app.navMonth(1)">&#10095;</button>
            </div>
            <div class="days-grid" id="cal-grid"></div>
            
            <div class="legend-container">
                <div class="legend-dot lg-work" onclick="window.app.showLegendToast('عمل عادي')"></div>
                <div class="legend-dot lg-holiday" onclick="window.app.showLegendToast('عطلة سنوية')"></div>
                <div class="legend-dot lg-sick" onclick="window.app.showLegendToast('مرض')"></div>
                <div class="legend-dot lg-absent" onclick="window.app.showLegendToast('غياب')"></div>
                <div class="legend-dot lg-recup" onclick="window.app.showLegendToast('تعويض (Recuperation)')"></div>
                <div class="legend-dot lg-eid" onclick="window.app.showLegendToast('عيد / مناسبة')"></div>
                <div class="legend-dot lg-nat" onclick="window.app.showLegendToast('عيد وطني')"></div>
            </div>
        </div>

        <div class="chart-box">
            <canvas id="myChart"></canvas>
        </div>
    </div>

    <!-- Modals -->
    <div class="modal-overlay" id="exportModal">
        <div class="modal-content">
            <h3 style="text-align:center;">اختر نوع الطباعة</h3>
            <div style="display:flex; flex-direction:column; gap:10px; margin-top:20px;">
                <button class="btn-main" onclick="window.app.generateReport('month')">📄 طباعة تقرير شهري</button>
                <button class="btn-main btn-secondary" onclick="window.app.generateReport('year')">📑 طباعة تقرير سنوي</button>
            </div>
            <button class="btn-close-modal" onclick="document.getElementById('exportModal').style.display='none'">إلغاء</button>
        </div>
    </div>

    <div class="modal-overlay" id="confirmModal">
        <div class="modal-content">
            <h3 style="color:#f44336; margin-bottom:10px;">⚠️ تأكيد الحذف</h3>
            <p style="color:var(--text-light); margin-bottom:20px;">هل أنت متأكد من مسح تسجيل هذا اليوم نهائياً؟</p>
            <div class="modal-btns">
                <button class="btn-del" onclick="window.app.performDelete()">نعم، احذف</button>
                <button class="btn-save" style="background:var(--border); color:var(--text);" onclick="document.getElementById('confirmModal').style.display='none'">تراجع</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="msgPopup">
        <div class="modal-content">
            <h3 style="color:var(--primary); margin-bottom:10px;">📩 رسالة إدارية</h3>
            <div id="live-msg-content" class="msg-popup-text"></div>
            <button class="btn-save" onclick="window.app.dismissMessage()">قراءة وإخفاء</button>
        </div>
    </div>

    <!-- NFC Success Popup -->
    <div class="modal-overlay" id="nfcPopup">
        <div class="modal-content">
            <h3 style="color:var(--work); margin-bottom:10px;">📡 تم التعرف على الشريحة</h3>
            <p style="color:var(--text); margin-bottom:20px;">تمت مطابقة السيريال بنجاح.<br>هل تريد تسجيل الحضور لليوم؟</p>
            <div class="modal-btns">
                <button class="btn-save" onclick="window.app.confirmNFCAttendance()">نعم، تسجيل</button>
                <button class="btn-del" onclick="document.getElementById('nfcPopup').style.display='none'">إلغاء</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="inboxModal">
        <div class="modal-content">
            <h3 style="text-align:center;">صندوق الرسائل</h3>
            <div id="inbox-list" style="margin-top:15px; max-height:400px; overflow-y:auto;"></div>
            <button class="btn-close-modal" onclick="document.getElementById('inboxModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <div class="modal-overlay" id="dayModal">
        <div class="modal-content">
            <h3 id="modal-title" style="text-align:center; margin-bottom:20px;"></h3>
            <label class="form-label">نوع النشاط:</label>
            <select id="d-type" class="app-input" onchange="window.app.toggleFields()">
                <option value="work">✅ عمل عادي</option><option value="holiday">🏖️ عطلة سنوية</option><option value="eid">🎉 عيد / مناسبة</option>
                <option value="recup">🔄 استرجاع</option><option value="sick">💊 مرض</option><option value="absent">❌ غياب</option>
            </select>
            <div id="f-holiday" class="hidden" style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:8px; margin-bottom:10px;"><label>عدد الأيام:</label><input type="number" id="d-count" class="app-input" value="1" min="1"></div>
            <div id="f-eid" class="hidden" style="background:rgba(156, 39, 176, 0.1); padding:10px; border-radius:8px; margin-bottom:10px;"><input type="text" id="d-eid-name" class="app-input" placeholder="اسم المناسبة"><select id="d-eid-status" class="app-input" onchange="window.app.toggleFields()"><option value="work">عملت</option><option value="rest">عطلة مدفوعة</option></select></div>
            <div id="f-recup" class="hidden"><label>تعويض عن:</label><select id="d-recup-target" class="app-input"></select></div>
            <div id="f-time">
                <label>التوقيت:</label><select id="d-preset" class="app-input" onchange="window.app.applyPreset()" style="margin-bottom:5px;"><option value="manual">-- اختيار توقيت --</option></select>
                <div style="display:flex; gap:10px;"><input type="time" id="d-start" class="app-input"><input type="time" id="d-end" class="app-input"></div>
            </div>
            <div style="margin-top:10px;"><label>ملاحظة:</label><textarea id="d-note" class="app-input" rows="2" placeholder="ملاحظات إضافية..."></textarea></div>
            <div class="modal-btns">
                <button class="btn-save" onclick="window.app.saveDay()">حفظ</button>
                <button class="btn-del" onclick="window.app.askDelete()">مسح</button>
            </div>
            <button class="btn-close-modal" onclick="document.getElementById('dayModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <div class="modal-overlay" id="searchModal">
        <div class="modal-content">
            <h3 id="search-title" style="text-align:center;">بحث / تفاصيل</h3>
            <div id="search-inputs">
                <label class="form-label">فلترة البحث:</label>
                <div style="display:flex; gap:5px; margin-bottom:10px; flex-wrap: wrap;">
                    <select id="search-month" class="app-input" style="flex:1;" onchange="window.app.performSearch()"><option value="">الأشهر</option><option value="1">يناير</option><option value="2">فبراير</option><option value="3">مارس</option><option value="4">أبريل</option><option value="5">مايو</option><option value="6">يونيو</option><option value="7">يوليو</option><option value="8">أغسطس</option><option value="9">سبتمبر</option><option value="10">أكتوبر</option><option value="11">نوفمبر</option><option value="12">ديسمبر</option></select>
                    <select id="search-day-name" class="app-input" style="flex:1;" onchange="window.app.performSearch()"><option value="">الأيام</option><option value="1">الإثنين</option><option value="2">الثلاثاء</option><option value="3">الأربعاء</option><option value="4">الخميس</option><option value="5">الجمعة</option><option value="6">السبت</option><option value="0">الأحد</option></select>
                    <select id="search-type" class="app-input" style="flex:1; width:100%;" onchange="window.app.performSearch()"><option value="">الحالات</option><option value="work">✅ عمل</option><option value="holiday">🏖️ عطلة</option><option value="sick">💊 مرض</option><option value="eid">🎉 أعياد</option><option value="recup">🔄 تعويض</option><option value="absent">❌ غياب</option></select>
                </div>
            </div>
            <div id="search-results" style="max-height:400px; overflow-y:auto; margin-top:10px;"></div>
            <button class="btn-close-modal" onclick="document.getElementById('searchModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <!-- Settings (with NFC) -->
    <div class="modal-overlay" id="settingsModal">
        <div class="modal-content">
            <h3 style="text-align:center;">الإعدادات</h3>
            
            <div id="admin-section" style="display:none; margin-bottom:15px;">
                <div style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:10px; margin-bottom:10px;">
                     <label class="form-label" style="color:var(--primary); font-weight:bold;">اسم البرنامج:</label>
                     <input type="text" id="p-app-name" class="app-input">
                </div>
                <div style="background:rgba(255, 152, 0, 0.1); padding:10px; border-radius:10px; margin-bottom:10px;">
                    <label class="form-label" style="color:#ef6c00;">✉️ إرسال رسالة:</label>
                    <textarea id="admin-msg-text" class="app-input" rows="2"></textarea>
                    <button class="btn-main" style="background:#ff9800; margin-top:5px;" onclick="window.app.sendBroadcast()">إرسال</button>
                </div>
                <div style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:10px;">
                    <label class="form-label" style="color:var(--primary);">إدارة التوقيتات:</label>
                    <div style="display:flex; gap:5px;"><input type="text" id="p-name" class="app-input" placeholder="اسم"><input type="time" id="p-start" class="app-input"><input type="time" id="p-end" class="app-input"></div>
                    <button class="btn-main" onclick="window.app.addPreset()" style="font-size:0.8rem; padding:8px;">+ إضافة</button>
                    <div id="presets-list" class="preset-list" style="margin-top:10px; max-height:100px; overflow-y:auto;"></div>
                </div>
            </div>

            <!-- NFC Section -->
            <div style="background:var(--bg); padding:15px; border-radius:10px; margin-bottom:15px; border:1px solid var(--border);">
                <div class="switch-container">
                    <label style="font-weight:bold; color:var(--text)">NFC Tools</label>
                    <label class="switch">
                        <input type="checkbox" id="s-nfc-toggle" onchange="window.app.toggleNFCConfig()">
                        <span class="slider"></span>
                    </label>
                </div>
                <div id="nfc-config-area" class="hidden">
                    <p style="font-size:0.85rem; color:var(--text-light); margin-bottom:10px;">أدخل Serial Number الخاص بالشريحة:</p>
                    <div style="display:flex; gap:8px;">
                        <input type="text" id="s-nfc-serial" class="app-input" placeholder="مثال: 04:a3:5b...">
                        <button onclick="window.app.scanTagForSetup()" style="white-space:nowrap; background:var(--text-light); border:none; border-radius:10px; color:white; padding:0 10px;">مسح 📶</button>
                    </div>
                </div>
            </div>

            <div style="background:rgba(76, 175, 80, 0.1); padding:10px; border-radius:10px; margin-bottom:15px;">
                <label class="form-label">الاسم الكامل:</label><input type="text" id="s-name" class="app-input">
                <label class="form-label">تاريخ التحاقي:</label><input type="date" id="s-join" class="app-input">
            </div>
            <div style="background:rgba(255, 152, 0, 0.1); padding:10px; border-radius:10px; margin-bottom:15px;">
                <label class="form-label">رصيد عطلة إضافي:</label>
                <div style="display:flex; gap:5px;"><input type="number" id="adj-days" class="app-input" placeholder="أيام"><input type="text" id="adj-note" class="app-input" placeholder="سبب"></div>
                <button class="btn-main" onclick="window.app.addAdj()" style="background:#ff9800; font-size:0.8rem; padding:8px;">+ إضافة</button>
                <div id="adj-list" style="margin-top:10px;"></div>
            </div>
            <div class="modal-btns"><button class="btn-save" onclick="window.app.saveSettings()">حفظ الكل</button></div>
            <button class="btn-close-modal" onclick="document.getElementById('settingsModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, onSnapshot, updateDoc, deleteField, addDoc, serverTimestamp, query, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut, onAuthStateChanged, sendPasswordResetEmail, sendEmailVerification, setPersistence, browserLocalPersistence, browserSessionPersistence } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

        const firebaseConfig = { apiKey: "AIzaSyDhpORuBt8k6YWDLUgRrnqfC8lSS97LexQ", authDomain: "sbota-37391.firebaseapp.com", projectId: "sbota-37391", storageBucket: "sbota-37391.firebasestorage.app", messagingSenderId: "1049902061223", appId: "1:1049902061223:web:68e7c10c349025ca7ead82", measurementId: "G-3B4ESSJWJ9" };
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);

        // Exports
        window.showLoader = (s) => document.getElementById('loader').style.display = s?'flex':'none';
        window.showError = (id, msg) => { const el=document.getElementById(id); el.textContent=msg; el.style.display='block'; };
        window.switchView = (id) => { document.querySelectorAll('.view-section').forEach(e=>e.classList.remove('active')); document.getElementById(id).classList.add('active'); document.querySelectorAll('.error-msg,.success-msg').forEach(e=>e.style.display='none'); };
        window.togglePass = (id) => { const el=document.getElementById(id); el.type = el.type==='password'?'text':'password'; };

        window.handleLogin = async () => {
            const e = document.getElementById('login-email').value, p = document.getElementById('login-pass').value;
            if(!e || !p) return window.showError('login-error', 'يرجى ملء البيانات');
            window.showLoader(true);
            try { await setPersistence(auth, document.getElementById('remember-me').checked ? browserLocalPersistence : browserSessionPersistence); const cred = await signInWithEmailAndPassword(auth, e, p); if(!cred.user.emailVerified) { await signOut(auth); window.showError('login-error', 'يرجى تفعيل البريد الإلكتروني أولاً'); window.showLoader(false); } } catch(error) { window.showLoader(false); window.showError('login-error', "بيانات خاطئة"); }
        };

        window.handleSignup = async () => {
            const e = document.getElementById('reg-email').value, p = document.getElementById('reg-pass').value, c = document.getElementById('reg-confirm').value;
            if(!e || !p || !c || p!==c || p.length<6) return window.showError('reg-error', 'خطأ في البيانات');
            window.showLoader(true);
            try { 
                const snap = await getDocs(collection(db, "users")); 
                const role = snap.empty ? 'admin' : 'user'; 
                const cred = await createUserWithEmailAndPassword(auth, e, p); 
                await sendEmailVerification(cred.user); 
                await setDoc(doc(db, "users", cred.user.uid), { email: e, role: role }); 
                await setDoc(doc(db, "settings", cred.user.uid), { joinDate: '', fullName: '', adjustments: [], dismissedMsgs: [], deletedMsgs: [], nfc: {enabled:false, serial:''} }); 
                if(role === 'admin') await setDoc(doc(db, "config", "general"), { 
                    presets: [
                        {label:'صباح', start:'06:00', end:'14:00'},
                        {label:'مساء', start:'14:00', end:'22:00'},
                        {label:'ليل', start:'22:00', end:'06:00'}
                    ] 
                }); 
                await signOut(auth); document.getElementById('reg-success').textContent = "تم التسجيل!"; document.getElementById('reg-success').style.display = 'block'; 
            } catch(err) { window.showError('reg-error', 'خطأ في التسجيل'); } finally { window.showLoader(false); }
        };
        window.handleReset = async () => { const e = document.getElementById('reset-email').value; if(!e) return; try { await sendPasswordResetEmail(auth, e); document.getElementById('reset-msg').textContent = "تم الإرسال"; document.getElementById('reset-msg').style.display = 'block'; } catch(err) {} };
        window.handleLogout = async () => { await signOut(auth); window.location.reload(); };

        window.saveData = async (type, data) => { const u = auth.currentUser; if(!u) return; try { if(type === 'personal_settings') await setDoc(doc(db, 'settings', u.uid), data, {merge:true}); else if(type === 'global_config') await setDoc(doc(db, 'config', 'general'), data, {merge:true}); else if(type === 'events') await setDoc(doc(db, 'attendance', u.uid), {events: data}, {merge:true}); } catch(e) {} };
        window.fbDeleteDay = async (dateKey) => { const u = auth.currentUser; if(!u) return; try { await updateDoc(doc(db, 'attendance', u.uid), { [`events.${dateKey}`]: deleteField() }); } catch(e) {} };
        window.sendAdminMessage = async (text) => { const u = auth.currentUser; if(!u) return; try { await addDoc(collection(db, "notifications"), { content: text, createdAt: serverTimestamp(), sender: u.uid }); alert("تم"); } catch(e) {} };

        onAuthStateChanged(auth, async (user) => {
            if(user && user.emailVerified) {
                document.getElementById('auth-overlay').style.display = 'none'; document.getElementById('app-container').style.display = 'block';
                document.getElementById('u-name').textContent = user.email.split('@')[0];
                window.app.initTheme(); window.showLoader(true);
                const uDoc = await getDoc(doc(db, 'users', user.uid));
                if(uDoc.exists()) { window.appData.role = uDoc.data().role; if(window.appData.role === 'admin') document.getElementById('admin-section').style.display = 'block'; }
                onSnapshot(doc(db, "attendance", user.uid), (doc) => { if(doc.exists()) window.appData.events = doc.data().events || {}; window.app.renderCalendar(); window.app.checkAutoFill(); });
                onSnapshot(doc(db, "settings", user.uid), (doc) => { 
                    if(doc.exists()) window.appData.personal = doc.data() || {};
                    // Defaults
                    if(!window.appData.personal.nfc) window.appData.personal.nfc = {enabled: false, serial: ''};
                    if(!window.appData.personal.dismissedMsgs) window.appData.personal.dismissedMsgs = [];
                    if(!window.appData.personal.deletedMsgs) window.appData.personal.deletedMsgs = [];
                    
                    document.getElementById('u-name').textContent = window.appData.personal.fullName || user.email.split('@')[0];
                    // Update NFC Button Visibility
                    document.getElementById('btn-nfc-scan').style.display = window.appData.personal.nfc.enabled ? 'flex' : 'none';
                    window.app.calcStats(); window.app.checkMessages();
                });
                onSnapshot(doc(db, "config", "general"), (doc) => { if(doc.exists()) { window.appData.global = doc.data() || {}; if(window.appData.global.appName) { document.title = window.appData.global.appName; document.getElementById('header-title').textContent = window.appData.global.appName; } } });
                const q = query(collection(db, "notifications"), orderBy("createdAt", "desc"));
                onSnapshot(q, (snapshot) => { let msgs = []; snapshot.forEach((doc) => msgs.push({ id: doc.id, ...doc.data() })); window.appData.messages = msgs; window.app.checkMessages(); });
                window.showLoader(false);
            } else { if(user) await signOut(auth); document.getElementById('auth-overlay').style.display = 'flex'; document.getElementById('app-container').style.display = 'none'; window.showLoader(false); }
        });
    </script>

    <script>
        const nationalHolidays = { "1-11":"وثيقة الاستقلال","1-14":"رأس السنة الأمازيغية","5-1":"عيد الشغل","7-30":"عيد العرش","8-14":"وادي الذهب","8-20":"ثورة الملك والشعب","8-21":"عيد الشباب","10-31":"عيد الوحدة","11-6":"المسيرة الخضراء","11-18":"عيد الاستقلال","12-9":"عيد الوساطة" };
        const dayNames = ["إثنين", "ثلاثاء", "أربعاء", "خميس", "جمعة", "سبت", "أحد"];
        const monthNames = ["يناير", "فبراير", "مارس", "أبريل", "مايو", "يونيو", "يوليو", "أغسطس", "سبتمبر", "أكتوبر", "نوفمبر", "ديسمبر"];
        let currentDate = new Date(2026, 0, 1);
        let selectedKey = null;
        let activeMsgId = null;
        let deleteType = null;
        let pendingMsgId = null;
        window.myChartInstance = null;
        let nfcReader = null; 

        window.appData = { role: 'user', events: {}, personal: {}, global: {}, messages: [] };

        window.app = {
            initTheme: () => { if(localStorage.getItem('theme') === 'dark') document.body.classList.add('dark-mode'); },
            toggleTheme: () => { document.body.classList.toggle('dark-mode'); localStorage.setItem('theme', document.body.classList.contains('dark-mode') ? 'dark' : 'light'); window.app.renderChart(); },

            // --- NFC LOGIC ---
            toggleNFCConfig: () => {
                const isEnabled = document.getElementById('s-nfc-toggle').checked;
                document.getElementById('nfc-config-area').classList.toggle('hidden', !isEnabled);
            },
            handleNFCReading: async (mode) => {
                if (!('NDEFReader' in window)) return alert("المتصفح أو الجهاز لا يدعم NFC. يرجى استخدام Chrome على Android.");
                try {
                    const ndef = new NDEFReader(); await ndef.scan();
                    window.app.showLegendToast(mode === 'setup' ? "قرّب الشريحة لحفظها..." : "قرّب الشريحة لتسجيل الحضور...");
                    if(mode !== 'setup') document.getElementById('btn-nfc-scan').classList.add('nfc-scanning');
                    ndef.onreading = event => {
                        const serial = event.serialNumber;
                        if (!serial) return window.app.showLegendToast("لم يتم العثور على سيريال");
                        if (mode === 'setup') { document.getElementById('s-nfc-serial').value = serial; window.app.showLegendToast("تم قراءة السيريال بنجاح!"); } 
                        else if (mode === 'checkin') {
                            const savedSerial = window.appData.personal.nfc.serial;
                            if (serial.toLowerCase() === savedSerial.toLowerCase()) { document.getElementById('btn-nfc-scan').classList.remove('nfc-scanning'); document.getElementById('nfcPopup').style.display = 'flex'; } 
                            else { window.app.showLegendToast("خطأ: الشريحة غير مطابقة!"); }
                        }
                    };
                    ndef.onreadingerror = () => { window.app.showLegendToast("حدث خطأ أثناء القراءة."); document.getElementById('btn-nfc-scan').classList.remove('nfc-scanning'); };
                } catch (error) { alert("تعذر بدء مسح NFC: " + error); document.getElementById('btn-nfc-scan').classList.remove('nfc-scanning'); }
            },
            scanTagForSetup: () => window.app.handleNFCReading('setup'),
            startNFCScan: () => window.app.handleNFCReading('checkin'),
            confirmNFCAttendance: () => {
                const today = new Date();
                const key = `${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,'0')}-${String(today.getDate()).padStart(2,'0')}`;
                if(window.appData.events[key]) { alert("يوجد تسجيل مسبق لهذا اليوم."); document.getElementById('nfcPopup').style.display = 'none'; return; }
                let start = '08:00', end = '16:00';
                if(window.appData.global.presets && window.appData.global.presets.length > 0) { start = window.appData.global.presets[0].start; end = window.appData.global.presets[0].end; }
                const [h1, m1] = start.split(':').map(Number), [h2, m2] = end.split(':').map(Number);
                let diff = (h2*60+m2) - (h1*60+m1); if(diff < 0) diff += 24*60;
                window.appData.events[key] = { type: 'work', start: start, end: end, hours: parseFloat((diff/60).toFixed(2)), note: 'تسجيل تلقائي عبر NFC 📡' };
                window.saveData('events', window.appData.events);
                document.getElementById('nfcPopup').style.display = 'none'; window.app.showLegendToast("تم تسجيل الحضور بنجاح! ✅"); window.app.renderCalendar();
            },
            // --- END NFC LOGIC ---

            // --- EXPORT & PRINT LOGIC ---
            showExportOptions: () => { document.getElementById('exportModal').style.display = 'flex'; },
            
            generateReport: (type) => {
                document.getElementById('exportModal').style.display = 'none';
                window.showLoader(true);
                const pc = document.getElementById('report-container');
                pc.innerHTML = '';
                pc.classList.add('active-print'); 
                
                // 1. Gather Data
                const yr = currentDate.getFullYear();
                const mth = currentDate.getMonth();
                const appName = window.appData.global.appName || 'نظام الحضور الذكي';
                const userName = window.appData.personal.fullName || 'موظف';
                let periodStr = '';
                
                // --- Yearly Grid Report Logic ---
                if(type === 'year') {
                    periodStr = `سنة ${yr}`;
                    
                    // Init 12 months buckets
                    let monthBuckets = [];
                    for(let i=0; i<12; i++) {
                        monthBuckets.push({ work:0, sick:0, absent:0, leave:0, eid:0 });
                    }

                    // Fill Data
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        const d = new Date(k);
                        if(d.getFullYear() === yr) {
                            let mIndex = d.getMonth();
                            if(evt.type==='work'||(evt.type==='eid'&&evt.eidStatus==='work')) {
                                monthBuckets[mIndex].work += evt.hours || 0;
                            }
                            if(evt.type==='sick') monthBuckets[mIndex].sick++;
                            if(evt.type==='absent') monthBuckets[mIndex].absent++;
                            if(evt.type==='holiday') monthBuckets[mIndex].leave++;
                        }
                    }

                    // Build HTML Grid
                    let gridHtml = `<div class="year-grid">`;
                    monthNames.forEach((name, i) => {
                        gridHtml += `
                            <div class="month-box">
                                <h4>${name}</h4>
                                <div class="month-stat-row"><span>عمل:</span> <b>${monthBuckets[i].work.toFixed(1)}س</b></div>
                                <div class="month-stat-row"><span>مرض:</span> <b>${monthBuckets[i].sick}ي</b></div>
                                <div class="month-stat-row"><span>غياب:</span> <b>${monthBuckets[i].absent}ي</b></div>
                                <div class="month-stat-row"><span>عطلة:</span> <b>${monthBuckets[i].leave}ي</b></div>
                            </div>
                        `;
                    });
                    gridHtml += `</div>`;

                    pc.innerHTML = `
                        <div class="report-header">
                            <h1>${appName}</h1>
                            <h3>التقرير السنوي الشامل - ${periodStr}</h3>
                            <p>الموظف: ${userName}</p>
                        </div>
                        ${gridHtml}
                        <div style="margin-top:20px; font-size:10px; text-align:center;">تاريخ الاستخراج: ${new Date().toLocaleDateString('ar-EG')}</div>
                    `;

                } 
                // --- Monthly Detail Report Logic ---
                else {
                    periodStr = `${monthNames[mth]} ${yr}`;
                    let eventsList = [];
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        const d = new Date(k);
                        if(d.getFullYear() === yr && d.getMonth() === mth) eventsList.push({date:k, ...evt});
                    }
                    eventsList.sort((a,b) => new Date(a.date) - new Date(b.date));

                    // Stats
                    let net=0, sat=0, workedDays=0, absentDays=0;
                    const breakdown = window.app.getLeaveBreakdown();
                    const totalLeave = breakdown.pools.reduce((sum, pool) => sum + pool.remaining, 0);

                    eventsList.forEach(e => {
                        if(e.type === 'work' || (e.type === 'eid' && e.eidStatus === 'work')) {
                            workedDays++;
                            net += (e.hours - 8);
                            const d = new Date(e.date);
                            if(d.getDay() === 6) sat += 4; 
                        }
                        if(e.type === 'absent') { absentDays++; net -= 8; }
                    });

                    let html = `
                        <div class="report-header">
                            <h1>${appName}</h1>
                            <h3>تقرير شهري - ${periodStr}</h3>
                            <p>الموظف: ${userName}</p>
                        </div>

                        <div class="report-summary">
                            <div class="report-box"><h3>رصيد العطلة</h3><p>${totalLeave}</p></div>
                            <div class="report-box"><h3>صافي الساعات</h3><p>${net.toFixed(1)}</p></div>
                            <div class="report-box"><h3>رصيد السبت</h3><p>${sat}</p></div>
                            <div class="report-box"><h3>أيام العمل</h3><p>${workedDays}</p></div>
                        </div>

                        <table class="report-table">
                            <thead>
                                <tr>
                                    <th>التاريخ</th>
                                    <th>اليوم</th>
                                    <th>الحالة</th>
                                    <th>التوقيت</th>
                                    <th>ملاحظات</th>
                                </tr>
                            </thead>
                            <tbody>
                    `;

                    if(eventsList.length === 0) {
                        html += `<tr><td colspan="5" style="text-align:center;">لا سجلات</td></tr>`;
                    } else {
                        eventsList.forEach(e => {
                            const dObj = new Date(e.date);
                            const dayName = dayNames[dObj.getDay()===0?6:dObj.getDay()-1];
                            let typeAr = { work:'عمل', holiday:'عطلة', sick:'مرض', absent:'غياب', recup:'تعويض', eid:'عيد' }[e.type] || e.type;
                            let timeInfo = (e.hours) ? `${e.hours}س` : '-';
                            if(e.start && e.end) timeInfo += ` (${e.start}-${e.end})`;
                            
                            html += `
                                <tr>
                                    <td>${e.date}</td>
                                    <td>${dayName}</td>
                                    <td>${typeAr}</td>
                                    <td style="direction:ltr">${timeInfo}</td>
                                    <td>${e.note || ''}</td>
                                </tr>
                            `;
                        });
                    }
                    html += `</tbody></table>`;
                    pc.innerHTML = html;
                }

                // 4. Trigger Print
                setTimeout(() => {
                    window.showLoader(false);
                    window.print();
                    setTimeout(() => { pc.classList.remove('active-print'); }, 1000);
                }, 500);
            },

            renderChart: () => {
                const ctx = document.getElementById('myChart'); if(!ctx) return;
                let counts = { work:0, holiday:0, sick:0, absent:0, eid:0 };
                Object.values(window.appData.events).forEach(e => { if(counts[e.type]!==undefined) counts[e.type]++; });
                const isDark = document.body.classList.contains('dark-mode');
                if(window.myChartInstance) window.myChartInstance.destroy();
                window.myChartInstance = new Chart(ctx, {
                    type: 'doughnut',
                    data: { labels: ['عمل', 'عطلة', 'مرض', 'غياب', 'عيد'], datasets: [{ data: Object.values(counts), backgroundColor: ['#4caf50', '#ffc107', '#ff9800', '#f44336', '#9c27b0'], borderWidth: 0 }] },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'right', labels: { color: isDark?'#e0e0e0':'#2b2d42', font:{family:'Cairo'} } } } }
                });
            },

            sendBroadcast: () => { const txt = document.getElementById('admin-msg-text').value; if(txt) { window.sendAdminMessage(txt); document.getElementById('admin-msg-text').value=''; } },
            checkMessages: () => {
                let unread=0; window.appData.messages.forEach(msg => { if(!window.appData.personal.deletedMsgs.includes(msg.id) && !window.appData.personal.dismissedMsgs.includes(msg.id)) { unread++; activeMsgId=msg.id; document.getElementById('live-msg-content').textContent=msg.content; document.getElementById('msgPopup').style.display='flex'; } });
                const b=document.getElementById('msg-badge'); b.textContent=unread; b.style.display=unread>0?'flex':'none'; if(unread===0) document.getElementById('msgPopup').style.display='none';
            },
            dismissMessage: () => { if(activeMsgId) { window.appData.personal.dismissedMsgs.push(activeMsgId); window.saveData('personal_settings', window.appData.personal); document.getElementById('msgPopup').style.display='none'; } },
            openInbox: () => {
                const list = document.getElementById('inbox-list'); list.innerHTML = '';
                const msgs = window.appData.messages.filter(m => !window.appData.personal.deletedMsgs.includes(m.id));
                if(msgs.length === 0) list.innerHTML = '<div style="text-align:center;color:#999">لا رسائل</div>';
                msgs.forEach(msg => list.innerHTML += `<div class="msg-item"><div class="msg-body">${msg.content}</div><div class="msg-footer"><span>${msg.createdAt?new Date(msg.createdAt.seconds*1000).toLocaleDateString('ar-EG'):'الآن'}</span><span class="del-icon" onclick="window.app.askDelete('msg','${msg.id}')">حذف</span></div></div>`);
                document.getElementById('inboxModal').style.display = 'flex';
            },
            
            checkAutoFill: () => {
                const today = new Date(); today.setHours(0,0,0,0);
                let lastStart = '08:00', lastEnd = '16:00';
                if(window.appData.global.presets && window.appData.global.presets.length > 0) { lastStart = window.appData.global.presets[0].start; lastEnd = window.appData.global.presets[0].end; }
                let loopDate = new Date(2026, 0, 1), changes = false;
                while (loopDate < today) {
                    const k = `${loopDate.getFullYear()}-${String(loopDate.getMonth()+1).padStart(2,'0')}-${String(loopDate.getDate()).padStart(2,'0')}`;
                    const evt = window.appData.events[k];
                    if (evt && evt.type === 'work') { lastStart = evt.start; lastEnd = evt.end; }
                    else if (!evt && loopDate.getDay()!==0 && loopDate.getDay()!==6) {
                        const [h1, m1] = lastStart.split(':').map(Number), [h2, m2] = lastEnd.split(':').map(Number);
                        let diff = (h2*60+m2)-(h1*60+m1); if(diff<0) diff+=24*60;
                        window.appData.events[k] = { type: 'work', start: lastStart, end: lastEnd, hours: parseFloat((diff/60).toFixed(2)), autoFilled: true };
                        changes = true;
                    }
                    loopDate.setDate(loopDate.getDate() + 1);
                }
                if (changes) window.saveData('events', window.appData.events);
            },

            renderCalendar: () => {
                const grid = document.getElementById('cal-grid'); grid.innerHTML = ''; dayNames.forEach(d => grid.innerHTML += `<div class="day-name">${d}</div>`);
                const y = currentDate.getFullYear(), m = currentDate.getMonth();
                document.getElementById('cal-title').textContent = `${monthNames[m]} ${y}`;
                let firstDay = new Date(y, m, 1).getDay(); firstDay = (firstDay === 0) ? 6 : firstDay - 1;
                for(let i=0; i<firstDay; i++) grid.innerHTML += `<div></div>`;
                const days = new Date(y, m + 1, 0).getDate();
                for(let i=1; i<=days; i++) {
                    const key = `${y}-${String(m+1).padStart(2,'0')}-${String(i).padStart(2,'0')}`;
                    const evt = window.appData.events[key], isNat = nationalHolidays[`${m+1}-${i}`];
                    let cls = '', natCls = isNat ? 'nat-holiday' : '', noteInd = (evt && evt.note) ? '<div class="note-dot"></div>' : '';
                    if(evt) cls = `st-${evt.type}`;
                    const dObj = new Date(y, m, i), now = new Date(); now.setHours(0,0,0,0);
                    // Update: Allow 1 day into future for visual style
                    const tomorrow = new Date(now); tomorrow.setDate(tomorrow.getDate() + 1);
                    const isFuture = dObj > tomorrow && !isNat;
                    
                    const classes = `day-cell ${dObj.getTime()===now.getTime()?'today':''} ${dObj.getDay()===0||dObj.getDay()===6?'weekend':''} ${natCls} ${cls} ${isFuture?'future':''}`;
                    // Update: Click action allows today + tomorrow
                    const action = (!isFuture) ? `onclick="window.app.openDay('${key}')"` : '';
                    grid.innerHTML += `<div class="${classes}" ${action}><span>${i}</span>${noteInd}</div>`;
                }
                window.app.calcStats();
            },
            navMonth: (s) => { currentDate.setMonth(currentDate.getMonth() + s); window.app.renderCalendar(); },
            openDay: (key) => {
                const natName = nationalHolidays[`${new Date(key).getMonth()+1}-${new Date(key).getDate()}`];
                // Update: Logic to allow today + 1 day (tomorrow)
                const today = new Date(); today.setHours(0,0,0,0);
                const tomorrow = new Date(today); tomorrow.setDate(tomorrow.getDate() + 1);
                if(new Date(key).setHours(0,0,0,0) > tomorrow.getTime() && !natName) return;

                selectedKey = key; document.getElementById('modal-title').textContent = key; document.getElementById('dayModal').style.display = 'flex';
                let evt = window.appData.events[key] || (natName ? {type:'eid',eidStatus:'rest',eidName:natName} : {type:'work',start:'',end:'',eidStatus:'work'});
                document.getElementById('d-type').value = evt.type;
                document.getElementById('d-start').value = evt.start||''; document.getElementById('d-end').value = evt.end||'';
                document.getElementById('d-eid-name').value = evt.eidName||''; document.getElementById('d-count').value = 1;
                document.getElementById('d-note').value = evt.note||'';
                document.getElementById('d-eid-status').value = evt.eidStatus || (natName ? 'rest' : 'work');
                const pre = document.getElementById('d-preset'); pre.innerHTML = '<option value="manual">-- توقيت --</option>'; window.appData.global.presets.forEach((p,i)=>pre.innerHTML+=`<option value="${i}">${p.label}</option>`);
                const rec = document.getElementById('d-recup-target'); rec.innerHTML = '<option value="">-- يوم --</option>';
                const used = Object.values(window.appData.events).filter(e=>e.type==='recup').map(e=>e.recupTarget);
                for(let k in window.appData.events) { const e=window.appData.events[k],d=new Date(k); if((d.getDay()===0&&e.type==='work')||(e.type==='eid'&&e.eidStatus==='work')) { if(!used.includes(k)||evt.recupTarget===k) rec.innerHTML+=`<option value="${k}" ${evt.recupTarget===k?'selected':''}>${k}</option>`; } }
                window.app.toggleFields();
            },
            toggleFields: () => {
                const t = document.getElementById('d-type').value, es = document.getElementById('d-eid-status').value;
                ['f-holiday','f-eid','f-recup','f-time'].forEach(id=>document.getElementById(id).classList.add('hidden'));
                if(t==='work') document.getElementById('f-time').classList.remove('hidden');
                else if(t==='holiday') document.getElementById('f-holiday').classList.remove('hidden');
                else if(t==='recup') document.getElementById('f-recup').classList.remove('hidden');
                else if(t==='eid') { document.getElementById('f-eid').classList.remove('hidden'); if(es==='work') document.getElementById('f-time').classList.remove('hidden'); }
            },
            applyPreset: () => { const idx = document.getElementById('d-preset').value; if(idx!=='manual') { const p=window.appData.global.presets[idx]; document.getElementById('d-start').value=p.start; document.getElementById('d-end').value=p.end; } },
            saveDay: () => {
                const type = document.getElementById('d-type').value, note = document.getElementById('d-note').value;
                let targetKey = selectedKey;

                if(type === 'holiday') {
                    let count = parseInt(document.getElementById('d-count').value), loopD = new Date(selectedKey), added = 0;
                    while(added < count) { if(loopD.getDay()!==6&&loopD.getDay()!==0) { const k = `${loopD.getFullYear()}-${String(loopD.getMonth()+1).padStart(2,'0')}-${String(loopD.getDate()).padStart(2,'0')}`; window.appData.events[k] = {type:'holiday',hours:0,note:note}; added++; } loopD.setDate(loopD.getDate()+1); }
                } else {
                    let data = {type, note};
                    if(type==='work' || (type==='eid'&&document.getElementById('d-eid-status').value==='work')) {
                        const s=document.getElementById('d-start').value, e=document.getElementById('d-end').value;
                        if(s&&e) { 
                            data.start=s; data.end=e; 
                            const [h1,m1]=s.split(':').map(Number),[h2,m2]=e.split(':').map(Number); 
                            let diff=(h2*60+m2)-(h1*60+m1); 
                            // Night Shift Logic (Shift to next day)
                            if(s > e) { 
                                diff += 24*60;
                                const dObj = new Date(selectedKey);
                                dObj.setDate(dObj.getDate() + 1);
                                targetKey = `${dObj.getFullYear()}-${String(dObj.getMonth()+1).padStart(2,'0')}-${String(dObj.getDate()).padStart(2,'0')}`;
                            } 
                            data.hours=parseFloat((diff/60).toFixed(2)); 
                        }
                        if(type==='eid') { data.eidStatus='work'; data.eidName=document.getElementById('d-eid-name').value; }
                    } else if(type==='eid') { data.eidStatus='rest'; data.eidName=document.getElementById('d-eid-name').value; }
                    else if(type==='recup') data.recupTarget=document.getElementById('d-recup-target').value;
                    
                    window.appData.events[targetKey] = data;
                }
                window.saveData('events', window.appData.events); document.getElementById('dayModal').style.display='none';
            },
            askDelete: (type, id) => { deleteType=type||'day'; if(type==='msg') pendingMsgId=id; document.getElementById('confirmModal').style.display='flex'; },
            performDelete: () => {
                if(deleteType==='day') { if(window.appData.events[selectedKey]) { delete window.appData.events[selectedKey]; window.fbDeleteDay(selectedKey); } document.getElementById('dayModal').style.display='none'; window.app.renderCalendar(); }
                else if(deleteType==='msg') { window.appData.personal.deletedMsgs.push(pendingMsgId); window.saveData('personal_settings', window.appData.personal); window.app.openInbox(); }
                document.getElementById('confirmModal').style.display='none';
            },
            // --- Logic Hub ---
            getLeaveBreakdown: () => {
                const currentY = new Date(2026, 0, 1).getFullYear();
                const joinDateStr = window.appData.personal.joinDate;
                let pools = [];
                // Adjustments
                if(window.appData.personal.adjustments) {
                    window.appData.personal.adjustments.forEach((adj, i) => {
                        pools.push({ id: `adj_${i}`, label: `رصيد سابق/إضافي (${adj.reason})`, total: parseFloat(adj.amount), remaining: parseFloat(adj.amount), type: 'bonus' });
                    });
                }
                // Yearly Balance
                if(joinDateStr) {
                    const joinD = new Date(joinDateStr);
                    const joinY = joinD.getFullYear();
                    const startCalc = Math.max(joinY, 2026);
                    for(let y = startCalc; y <= 2026; y++) {
                        let months = 12;
                        if(y === joinY) months = 12 - joinD.getMonth();
                        let seniority = Math.floor((y - joinY)/5) * 1.5;
                        let amount = Math.min((months * 1.5) + seniority, 30);
                        if(amount > 0) pools.push({ id: y, label: `رصيد سنة ${y}`, total: amount, remaining: amount, type: 'year' });
                    }
                }
                // Deductions logic
                const holidays = Object.entries(window.appData.events)
                    .filter(([k, v]) => v.type === 'holiday')
                    .sort((a, b) => new Date(a[0]) - new Date(b[0]));

                let deductions = [];
                holidays.forEach(h => {
                    let consumed = false;
                    for(let pool of pools) {
                        if(pool.remaining > 0) {
                            pool.remaining--;
                            deductions.push({ date: h[0], note: `تم خصمه من ${pool.label}`, val: '-1', type: 'neg' });
                            consumed = true;
                            break;
                        }
                    }
                    if(!consumed) deductions.push({ date: h[0], note: 'رصيد غير كافٍ', val: '-1', type: 'neg' });
                });
                return { pools, deductions };
            },
            calcStats: () => {
                let net=0, sat=0, leave=0, pending=0, tWeek=0, tMonth=0, tYear=0, tYearWorkHours=0, tSickDays=0, tYearWorkDays=0;
                let yr=currentDate.getFullYear(), mth=currentDate.getMonth(), today=new Date();
                const weekStart=new Date(today); weekStart.setDate(today.getDate()-today.getDay()); weekStart.setHours(0,0,0,0);
                const weekEnd=new Date(weekStart); weekEnd.setDate(weekStart.getDate()+6); weekEnd.setHours(23,59,59,999);
                
                const breakdown = window.app.getLeaveBreakdown();
                leave = breakdown.pools.reduce((sum, pool) => sum + pool.remaining, 0);

                const startLoop = new Date(yr, 0, 1);
                const limitLoop = (yr === today.getFullYear()) ? today : new Date(yr, 11, 31);
                for (let d = new Date(startLoop); d <= limitLoop; d.setDate(d.getDate() + 1)) {
                     const k = `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
                     const evt = window.appData.events[k];
                     const dayNum = d.getDay();
                     if(dayNum === 6) {
                         if(evt && (evt.type === 'work' || (evt.type === 'eid' && evt.eidStatus === 'work'))) sat += 4;
                         else sat -= 4;
                     }
                }

                Object.entries(window.appData.events).forEach(([k, evt]) => {
                     const d = new Date(k);
                     if(d.getFullYear()===yr) {
                         let h=0; 
                         // Only work hours count for week/month/year totals. Sick is excluded.
                         if(evt.type==='work'||(evt.type==='eid'&&evt.eidStatus==='work')) {
                             h=evt.hours;
                             tYear += h;
                             if(d.getMonth()===mth) tMonth += h;
                             if(d>=weekStart&&d<=weekEnd) tWeek += h;
                             
                             // Stats for Annual Card
                             tYearWorkHours += h;
                         }
                         
                         // Net hours balance calculation
                         if(evt.type==='work'||(evt.type==='eid'&&evt.eidStatus==='work')) net+=(evt.hours-8); else if(evt.type==='absent') net-=8;
                         
                         if(evt.type==='sick') {
                             tSickDays++;
                         }
                     }
                });
                
                // Sunday Logic
                const used = Object.values(window.appData.events).filter(e=>e.type==='recup').map(e=>e.recupTarget);
                Object.entries(window.appData.events).forEach(([k,e]) => { 
                    const d=new Date(k);
                    const isNat = nationalHolidays[`${d.getMonth()+1}-${d.getDate()}`];
                    const isEid = e.type === 'eid';
                    if(d.getDay()===0 && !isNat && !isEid) { if(e.type==='work') { if(!used.includes(k)) pending++; } }
                });

                document.getElementById('st-net').innerHTML = `<span class="${net>=0?'txt-green':'txt-red'}">${net.toFixed(1)}</span>`;
                document.getElementById('st-sat').innerHTML = `<span class="${sat>=0?'txt-green':'txt-red'}">${sat}</span>`;
                document.getElementById('st-leave').textContent = leave.toFixed(1); document.getElementById('st-sunday').textContent = pending;
                document.getElementById('st-week').textContent = tWeek.toFixed(1); document.getElementById('st-month').textContent = tMonth.toFixed(1); 
                
                // Updated Annual Total Layout: Work | Sick | Total (Work Hours)
                document.getElementById('st-year').innerHTML = `
                    <div style="display:flex; justify-content:space-around; align-items:center; width:100%;">
                        <div style="display:flex; flex-direction:column;">
                            <span style="font-size:0.7rem; color:#888;">عمل</span>
                            <span style="font-size:1.1rem; font-weight:bold; color:var(--work)">${tYearWorkHours.toFixed(0)}</span>
                        </div>
                        <div style="width:1px; height:25px; background:#e0e0e0;"></div>
                        <div style="display:flex; flex-direction:column;">
                            <span style="font-size:0.7rem; color:#888;">مرض</span>
                            <span style="font-size:1.1rem; font-weight:bold; color:var(--sick)">${tSickDays}</span>
                        </div>
                        <div style="width:1px; height:25px; background:#e0e0e0;"></div>
                        <div style="display:flex; flex-direction:column;">
                            <span style="font-size:0.7rem; color:#888;">مجموع</span>
                            <span style="font-size:1.1rem; font-weight:bold; color:var(--text)">${tYearWorkHours.toFixed(0)}</span>
                        </div>
                    </div>
                `;
                
                window.app.renderChart();
            },
            
            showDetails: (cat) => {
                document.getElementById('search-inputs').style.display = 'none';
                document.getElementById('search-title').textContent = 'التفاصيل';
                const list = document.getElementById('search-results');
                list.innerHTML = '';
                const yr = currentDate.getFullYear();
                const today = new Date();
                let tempList = [];

                if (cat === 'leave') {
                    const bd = window.app.getLeaveBreakdown();
                    list.innerHTML += `<div class="details-header">الأرصدة المتاحة (FIFO):</div>`;
                    bd.pools.forEach(p => {
                        if(p.remaining > 0) list.innerHTML += `<div class="detail-item pos"><span>${p.label}</span><span class="d-val">${p.remaining} يوم</span></div>`;
                    });

                    if(bd.deductions.length > 0) {
                        list.innerHTML += `<div class="details-header">سجل الاستهلاك:</div>`;
                        bd.deductions.reverse().forEach(d => {
                            list.innerHTML += `<div class="detail-item neg" onclick="window.app.openDay('${d.date}')"><span>${d.date} <small>(${d.note})</small></span><span class="d-val">-1</span></div>`;
                        });
                    } else {
                        list.innerHTML += `<div style="text-align:center; padding:10px;">لم يتم استهلاك أي عطلة</div>`;
                    }
                    document.getElementById('searchModal').style.display = 'flex';
                    return;
                }

                if (cat === 'sat') {
                    const start = new Date(yr, 0, 1);
                    const limit = (yr === today.getFullYear()) ? today : new Date(yr, 11, 31);
                    for (let d = new Date(start); d <= limit; d.setDate(d.getDate() + 1)) {
                        if(d.getDay() === 6) {
                            const k = `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
                            const evt = window.appData.events[k];
                            let st = 'غياب/فارغ', val = -4, type = 'neg';
                            if(evt && (evt.type==='work' || (evt.type==='eid' && evt.eidStatus==='work'))) { st='عمل'; val=4; type='pos'; }
                            tempList.push({date:k, note:st, val:(val>0?'+':'')+val, type});
                        }
                    }
                } else if (cat === 'net') {
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() !== yr) continue;
                        let diff = 0, note = '';
                        if(evt.type==='work' || (evt.type==='eid' && evt.eidStatus==='work')) { diff = evt.hours-8; note='عمل'; }
                        else if(evt.type==='absent') { diff = -8; note='غياب'; }
                        if(diff !== 0) tempList.push({date:k, note, val:(diff>0?'+':'')+diff.toFixed(1), type:diff>=0?'pos':'neg'});
                    }
                } else if (cat === 'sunday') {
                     const used = Object.values(window.appData.events).filter(e => e.type === 'recup').map(e => e.recupTarget);
                     for(const [k, evt] of Object.entries(window.appData.events)) {
                        const d = new Date(k);
                        const isNat = nationalHolidays[`${d.getMonth()+1}-${d.getDate()}`];
                        const isEid = evt.type === 'eid';

                        if((d.getDay()===0 && !isNat && !isEid && evt.type==='work') || (evt.type==='eid' && evt.eidStatus==='work')) {
                            const isComp = used.includes(k);
                            tempList.push({ date: k, note: evt.eidName || 'عمل يوم أحد', val: isComp ? 'تم التعويض' : 'مستحق', type: isComp ? 'neutral' : 'pos' });
                        }
                     }
                } else if (cat === 'year') {
                    // Specific detail view for Yearly Total
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() === yr) {
                            if(evt.type==='work' || (evt.type==='eid' && evt.eidStatus==='work')) {
                                tempList.push({date:k, note:'عمل', val:evt.hours+'س', type:'pos'});
                            }
                            if(evt.type==='sick') {
                                // Display as 'Day' but doesn't add to hours sum in main stat
                                tempList.push({date:k, note:'مرض', val:'يوم', type:'neutral', style:'color:var(--sick)'});
                            }
                        }
                    }
                } else if (['week', 'month'].includes(cat)) {
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() === yr) {
                            let h = 0;
                            // Only include work hours
                            if(evt.type==='work' || (evt.type==='eid' && evt.eidStatus==='work')) h = evt.hours;
                            
                            if(h>0) tempList.push({date:k, note:evt.type, val:h+'س', type:'pos'});
                        }
                    }
                }
                
                tempList.sort((a,b) => new Date(b.date) - new Date(a.date));
                if(tempList.length === 0) list.innerHTML = '<div style="text-align:center; padding:10px;">لا توجد بيانات</div>';
                tempList.forEach(item => {
                    let customStyle = item.style ? `style="${item.style}"` : '';
                    list.innerHTML += `<div class="detail-item ${item.type}" onclick="window.app.openDay('${item.date}')"><span>${item.date} <small>(${item.note})</small></span><span class="d-val ${item.type}" ${customStyle}>${item.val}</span></div>`;
                });
                document.getElementById('searchModal').style.display = 'flex';
            },

            openSearchModal: () => { document.getElementById('search-inputs').style.display = 'block'; document.getElementById('search-title').textContent = 'بحث'; document.getElementById('search-results').innerHTML = ''; document.getElementById('searchModal').style.display = 'flex'; },
            performSearch: () => { /* Same as before */ },
            openSettings: () => {
                document.getElementById('s-join').value = window.appData.personal.joinDate||''; document.getElementById('s-name').value = window.appData.personal.fullName||'';
                const nfc = window.appData.personal.nfc || {enabled:false, serial:''};
                document.getElementById('s-nfc-toggle').checked = nfc.enabled;
                document.getElementById('s-nfc-serial').value = nfc.serial;
                window.app.toggleNFCConfig();
                
                window.app.renderSettingsLists(); document.getElementById('settingsModal').style.display='flex';
            },
            
            // --- FIX FOR ADMIN PRESETS BUTTON ---
            addPreset: () => {
                const n = document.getElementById('p-name').value;
                const s = document.getElementById('p-start').value;
                const e = document.getElementById('p-end').value;
                if(n && s && e) {
                    if(!window.appData.global.presets) window.appData.global.presets = [];
                    window.appData.global.presets.push({label:n, start:s, end:e});
                    document.getElementById('p-name').value = '';
                    document.getElementById('p-start').value = '';
                    document.getElementById('p-end').value = '';
                    window.app.renderSettingsLists();
                } else {
                    alert("يرجى ملء جميع الحقول (الاسم، البداية، النهاية)");
                }
            },
            
            delPreset: (i) => { window.appData.global.presets.splice(i, 1); window.app.renderSettingsLists(); },
            addAdj: () => { /* Same */ window.app.renderSettingsLists(); }, delAdj: (i) => { window.appData.personal.adjustments.splice(i, 1); window.app.renderSettingsLists(); },
            renderSettingsLists: () => {
                const pl = document.getElementById('presets-list'); pl.innerHTML = ''; window.appData.global.presets.forEach((p,i)=>pl.innerHTML+=`<div class="preset-item"><span>${p.label} (${p.start}-${p.end})</span> <span class="del-icon" onclick="window.app.delPreset(${i})">X</span></div>`);
                const al = document.getElementById('adj-list'); al.innerHTML = ''; window.appData.personal.adjustments.forEach((a,i)=>al.innerHTML+=`<div class="preset-item"><span>+${a.amount}</span><span class="del-icon" onclick="window.app.delAdj(${i})">X</span></div>`);
            },
            saveSettings: () => {
                window.appData.personal.joinDate = document.getElementById('s-join').value;
                window.appData.personal.fullName = document.getElementById('s-name').value;
                window.appData.global.appName = document.getElementById('p-app-name').value || window.appData.global.appName;
                // Save NFC
                window.appData.personal.nfc = {
                    enabled: document.getElementById('s-nfc-toggle').checked,
                    serial: document.getElementById('s-nfc-serial').value
                };
                
                window.saveData('personal_settings', window.appData.personal);
                if(window.appData.role === 'admin') window.saveData('global_config', window.appData.global);
                document.getElementById('settingsModal').style.display = 'none';
            },
            showLegendToast: (msg) => { const t = document.getElementById('legend-toast'); t.textContent = msg; t.classList.add('show-toast'); setTimeout(() => t.classList.remove('show-toast'), 3000); }
        };
    </script>
</body>
</html>
