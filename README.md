<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>المدير الذكي 2026</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.0.0"></script>
    
    <style>
        :root {
            --primary: #4361ee; 
            --primary-dark: #3a0ca3;
            --bg: #f8f9fa; --surface: #ffffff;
            --text: #2b2d42; --text-light: #8d99ae;
            --border: #e0e0e0;
            --work: #4caf50; --holiday: #ffc107; --sick: #ff9800;
            --absent: #f44336; --eid: #9c27b0; --recup: #00bcd4;
            --paid: #009688;
            --ramadan: #673ab7;
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

        @media print {
            body * { visibility: hidden; }
            #report-container, #report-container * { visibility: visible; }
            #report-container { position: absolute; left: 0; top: 0; width: 100%; margin: 0; padding: 0; background: white; color: black; z-index: 99999999; display: block !important; }
            @page { margin: 1cm; size: auto; }
            .no-print { display: none !important; }
        }

        #report-container { position: fixed; top: 0; left: 0; width: 100%; height: 100vh; background: #ffffff; color: #000000; padding: 20px; font-family: 'Cairo', sans-serif; z-index: -9999; opacity: 0; pointer-events: none; overflow-y: auto; }
        #report-container.active-print { z-index: 999999; opacity: 1; background: white; }
        .report-header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 10px; margin-bottom: 20px; }
        .report-summary { display: flex; gap: 10px; margin-bottom: 20px; flex-wrap: wrap; justify-content: center; direction: rtl; }
        .report-box { border: 1px solid #ccc; padding: 10px; border-radius: 5px; min-width: 100px; text-align: center; flex: 1; }
        .report-table { width: 100%; border-collapse: collapse; margin-top: 15px; font-size: 12px; direction: rtl; }
        .report-table th, .report-table td { border: 1px solid #ccc; padding: 8px; text-align: center; }
        .report-table th { background-color: #eee; font-weight: bold; }

        #auth-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: linear-gradient(135deg, var(--primary), #4cc9f0); z-index: 9999; display: flex; justify-content: center; align-items: center; flex-direction: column; transition: background 0.3s; }
        .auth-card { background: rgba(255,255,255,0.98); padding: 30px; border-radius: 24px; width: 90%; max-width: 380px; text-align: center; box-shadow: 0 10px 40px rgba(0,0,0,0.2); }
        body.dark-mode .auth-card { background: #1e1e1e; color: #fff; }
        .auth-header h2 { color: var(--primary); margin: 0 0 10px 0; }
        .input-group { position: relative; margin-bottom: 15px; }
        .app-input { width: 100%; padding: 12px; border: 2px solid var(--border); border-radius: 12px; font-family: inherit; font-size: 1rem; outline: none; transition: 0.3s; background: var(--surface); color: var(--text); }
        .app-input:focus { border-color: var(--primary); }
        .toggle-password { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #888; font-size: 1.2rem; }
        .btn-main { width: 100%; padding: 12px; background: var(--primary); color: white; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; margin-top: 10px; transition: background 0.3s; }
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
        .day-cell.st-paid { background-color: var(--paid) !important; color: white !important; }
        
        .day-cell[class*="st-"] span { color: white !important; }
        .day-cell.st-holiday span { color: #333 !important; }
        .day-cell.nat-holiday { background-color: #f8bbd0 !important; border: 2px solid #ec407a !important; color: #880e4f !important; }
        .day-cell.nat-holiday span { color: #880e4f !important; }
        body.dark-mode .day-cell.nat-holiday { background-color: #4a1c2d !important; color: #fce4ec !important; }
        
        /* FIX: Ensure WORK on a National Holiday takes precedence visually */
        .day-cell.nat-holiday.st-work {
            background-color: var(--work) !important;
            color: white !important;
            border: 3px solid #ec407a !important; /* Pink border to remind it's a holiday */
        }
        .day-cell.nat-holiday.st-work span { color: white !important; }

        /* FIX: Worked Eid Style */
        .day-cell.worked-eid { 
            background-color: var(--eid) !important;
            border: 3px solid var(--work) !important; 
            box-shadow: inset 0 0 8px rgba(0,0,0,0.3);
            color: white !important;
        }
        .day-cell.worked-eid span { color: white !important; }

        .day-cell.is-ramadan { 
            border: 2px solid var(--ramadan) !important;
            background-color: rgba(103, 58, 183, 0.05);
            position: relative;
            overflow: hidden;
        }
        .day-cell.is-ramadan::before {
            content: "";
            position: absolute;
            top: 0; left: 0; width: 0; height: 0; 
            border-top: 16px solid var(--ramadan);
            border-right: 16px solid transparent;
            z-index: 1;
        }

        .legend-container { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin-top: 20px; padding: 10px; background: var(--bg); border-radius: var(--radius); }
        .legend-dot { width: 20px; height: 20px; border-radius: 50%; cursor: pointer; border: 2px solid var(--surface); box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .lg-work { background-color: var(--work); } .lg-holiday { background-color: var(--holiday); } .lg-sick { background-color: var(--sick); } .lg-absent { background-color: var(--absent); } .lg-recup { background-color: var(--recup); } .lg-eid { background-color: var(--eid); } .lg-paid { background-color: var(--paid); } .lg-nat { background-color: #f8bbd0; border: 2px solid #ec407a; }

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
        
        .preset-item, .search-item, .detail-item, .msg-item { display: flex; justify-content: space-between; align-items: center; background: var(--bg); padding: 12px; border-radius: 10px; margin-bottom: 6px; font-size: 0.9rem; border-left: 4px solid transparent; cursor: pointer; }
        .detail-item.pos { border-left-color: var(--work); } .detail-item.neg { border-left-color: var(--absent); } .detail-item.neutral { border-left-color: var(--primary); }
        .msg-item { border-left-color: #ff9800; background: rgba(255, 152, 0, 0.1); flex-direction: column; align-items: flex-start; gap: 8px; }
        .del-icon { color: red; font-weight: bold; padding: 5px 10px; cursor: pointer; background: var(--surface); border-radius: 5px; }
        .d-val { font-weight: bold; direction: ltr; font-family: monospace; font-size: 1rem; }
        .d-val.pos { color: var(--work); } .d-val.neg { color: var(--absent); } .d-val.neutral { color: var(--text-light); }
        .details-header { font-weight: bold; margin: 15px 0 10px; color: var(--primary); font-size: 0.95rem; border-bottom: 2px solid var(--border); padding-bottom: 5px; }
        .msg-popup-text { font-size: 1rem; color: var(--text); margin: 15px 0; background: var(--bg); padding: 15px; border-radius: 10px; border-right: 4px solid var(--primary); text-align: right; }

        .nfc-scanning { animation: pulse 1.5s infinite; border-color: var(--primary); color: var(--primary); }
        .switch-container { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; background: var(--bg); padding: 10px; border-radius: 10px; }
        .switch { position: relative; display: inline-block; width: 40px; height: 24px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #ccc; transition: .4s; border-radius: 24px; }
        .slider:before { position: absolute; content: ""; height: 16px; width: 16px; left: 4px; bottom: 4px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--primary); }
        input:checked + .slider:before { transform: translateX(16px); }

        .search-item { flex-direction: column; align-items: flex-start; gap: 5px; border: 1px solid var(--border); }
        .search-item:hover { background-color: rgba(0,0,0,0.03); }
        body.dark-mode .search-item:hover { background-color: rgba(255,255,255,0.05); }
    </style>
</head>
<body>

    <div id="loader" class="loading"><div style="width:40px;height:40px;border:4px solid #ddd;border-top-color:var(--primary);border-radius:50%;animation:spin 1s infinite"></div></div>
    
    <div id="legend-toast"></div>
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
            <div class="stat-card" onclick="window.app.showDetails('sat')"><h4>رصيد السبت</h4><div class="val" id="st-sat">0</div><div class="sub">عمل (+4) / فارغ (-4)</div></div>
            <div class="stat-card" onclick="window.app.showDetails('sunday')"><h4>الأحد والأعياد</h4><div class="val" id="st-sunday">0</div><div class="sub">يوم تعويض</div></div>
            <div class="stat-card" onclick="window.app.showDetails('leave')"><h4>رصيد العطلة</h4><div class="val" id="st-leave">0</div><div class="sub">تراكمي FIFO</div></div>
            <div class="stat-card" onclick="window.app.showDetails('week')"><h4>هذا الأسبوع</h4><div class="val" id="st-week">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('month')"><h4>هذا الشهر</h4><div class="val" id="st-month">0</div></div>
            <div class="stat-card full-width" onclick="window.app.showDetails('year')"><h4>المجموع السنوي (ساعات)</h4><div class="val" id="st-year">0</div></div>
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
                <div class="legend-dot lg-paid" onclick="window.app.showLegendToast('يوم مدفوع')"></div>
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
            <p style="color:var(--text-light); margin-bottom:20px;">هل أنت متأكد؟</p>
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
                <option value="work">✅ عمل عادي</option>
                <option value="holiday">🏖️ عطلة سنوية</option>
                <option value="paid">💰 يوم مدفوع</option>
                <option value="eid">🎉 عيد / مناسبة</option>
                <option value="recup">🔄 استرجاع</option>
                <option value="sick">💊 مرض</option>
                <option value="absent">❌ غياب</option>
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
                    <select id="search-type" class="app-input" style="flex:1; width:100%;" onchange="window.app.performSearch()"><option value="">الحالات</option><option value="work">✅ عمل</option><option value="holiday">🏖️ عطلة</option><option value="paid">💰 يوم مدفوع</option><option value="sick">💊 مرض</option><option value="eid">🎉 أعياد</option><option value="recup">🔄 تعويض</option><option value="absent">❌ غياب</option></select>
                    <!-- Search by Note -->
                    <input type="text" id="search-note" class="app-input" placeholder="🔍 بحث في الملاحظات..." oninput="window.app.performSearch()" style="width:100%; margin-top:5px;">
                </div>
            </div>
            <div id="search-results" style="max-height:400px; overflow-y:auto; margin-top:10px;"></div>
            <button class="btn-close-modal" onclick="document.getElementById('searchModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <!-- Settings (with NFC & Ramadan & Color Picker) -->
    <div class="modal-overlay" id="settingsModal">
        <div class="modal-content">
            <h3 style="text-align:center;">الإعدادات</h3>
            
            <div id="admin-section" style="display:none; margin-bottom:15px;">
                <div style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:10px; margin-bottom:10px;">
                     <label class="form-label" style="color:var(--primary); font-weight:bold;">اسم البرنامج:</label>
                     <input type="text" id="p-app-name" class="app-input">
                </div>
                
                <div style="background:rgba(255, 152, 0, 0.1); padding:10px; border-radius:10px; margin-bottom:10px; border:1px solid #ff9800;">
                    <label class="form-label" style="color:#ef6c00; font-weight:bold;">👥 طلبات الاشتراك:</label>
                    <div id="pending-users-list" style="margin-top:5px; max-height:150px; overflow-y:auto;"></div>
                </div>

                <div style="background:rgba(255, 152, 0, 0.1); padding:10px; border-radius:10px; margin-bottom:10px;">
                    <label class="form-label" style="color:#ef6c00;">✉️ إرسال رسالة:</label>
                    <textarea id="admin-msg-text" class="app-input" rows="2"></textarea>
                    <button class="btn-main" style="background:#ff9800; margin-top:5px;" onclick="window.app.sendBroadcast()">إرسال</button>
                </div>
                
                <div style="background:rgba(67, 97, 238, 0.1); padding:10px; border-radius:10px; margin-bottom:10px;">
                    <label class="form-label" style="color:var(--primary);">إدارة التوقيتات العادية:</label>
                    <div style="display:flex; gap:5px;"><input type="text" id="p-name" class="app-input" placeholder="اسم"><input type="time" id="p-start" class="app-input"><input type="time" id="p-end" class="app-input"></div>
                    <button class="btn-main" onclick="window.app.addPreset('normal')" style="font-size:0.8rem; padding:8px;">+ إضافة</button>
                    <div id="presets-list" class="preset-list" style="margin-top:10px; max-height:100px; overflow-y:auto;"></div>
                </div>

                <div style="background:rgba(103, 58, 183, 0.1); padding:10px; border-radius:10px; border:1px solid var(--ramadan);">
                    <label class="form-label" style="color:var(--ramadan); font-weight:bold;">🌙 إعدادات رمضان:</label>
                    <div style="display:flex; gap:5px; margin-bottom:10px;">
                        <input type="date" id="p-ramadan-start" class="app-input" placeholder="البداية">
                        <input type="number" id="p-ramadan-days" class="app-input" placeholder="عدد الأيام">
                    </div>
                    <label class="form-label" style="font-size:0.85rem;">مواقيت رمضان الخاصة:</label>
                    <div style="display:flex; gap:5px;"><input type="text" id="pr-name" class="app-input" placeholder="اسم"><input type="time" id="pr-start" class="app-input"><input type="time" id="pr-end" class="app-input"></div>
                    <button class="btn-main" onclick="window.app.addPreset('ramadan')" style="background:var(--ramadan); font-size:0.8rem; padding:8px; margin-top:5px;">+ إضافة توقيت رمضان</button>
                    <div id="ramadan-presets-list" class="preset-list" style="margin-top:10px; max-height:100px; overflow-y:auto;"></div>
                </div>
            </div>

            <div style="background:var(--bg); padding:15px; border-radius:10px; margin-bottom:15px; border:1px solid var(--border);">
                <div class="switch-container">
                    <label style="font-weight:bold; color:var(--text)">NFC Tools</label>
                    <label class="switch"><input type="checkbox" id="s-nfc-toggle" onchange="window.app.toggleNFCConfig()"><span class="slider"></span></label>
                </div>
                <div id="nfc-config-area" class="hidden">
                    <div style="display:flex; gap:8px;"><input type="text" id="s-nfc-serial" class="app-input" placeholder="مثال: 04:a3:5b..."><button onclick="window.app.scanTagForSetup()" style="white-space:nowrap; background:var(--text-light); border:none; border-radius:10px; color:white; padding:0 10px;">مسح 📶</button></div>
                </div>
            </div>

            <div style="background:rgba(76, 175, 80, 0.1); padding:10px; border-radius:10px; margin-bottom:15px;">
                <label class="form-label">الاسم الكامل:</label><input type="text" id="s-name" class="app-input">
                <label class="form-label">تاريخ التحاقي:</label><input type="date" id="s-join" class="app-input">
                
                <!-- Color Picker Feature -->
                <div style="margin-top:15px; border-top:1px dashed #ccc; padding-top:10px;">
                    <label class="form-label">لون التطبيق المفضل:</label>
                    <div style="display:flex; align-items:center; gap:10px;">
                        <input type="color" id="s-theme-color" value="#4361ee" style="height:40px; border:none; background:none; cursor:pointer;">
                        <span style="font-size:0.8rem; color:var(--text-light)">اضغط لتغيير اللون</span>
                    </div>
                </div>
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
        import { getFirestore, doc, setDoc, getDoc, collection, getDocs, onSnapshot, updateDoc, deleteField, addDoc, serverTimestamp, query, orderBy, where, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
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
            try { 
                await setPersistence(auth, document.getElementById('remember-me').checked ? browserLocalPersistence : browserSessionPersistence); 
                const cred = await signInWithEmailAndPassword(auth, e, p); 
                
                const uDoc = await getDoc(doc(db, 'users', cred.user.uid));
                if(uDoc.exists()) {
                    const userData = uDoc.data();
                    if(userData.status === 'pending') {
                        await signOut(auth);
                        window.showError('login-error', 'الحساب قيد المراجعة. يرجى انتظار تفعيل الأدمن.');
                        window.showLoader(false);
                        return;
                    }
                    if(userData.status === 'rejected') {
                        await signOut(auth);
                        window.showError('login-error', 'تم رفض طلبك. يرجى التواصل مع الإدارة.');
                        window.showLoader(false);
                        return;
                    }
                }

                if(!cred.user.emailVerified) { 
                    await signOut(auth); window.showError('login-error', 'يرجى تفعيل البريد الإلكتروني أولاً'); window.showLoader(false); 
                } 
            } catch(error) { 
                window.showLoader(false); window.showError('login-error', "بيانات خاطئة"); 
            }
        };

        window.handleSignup = async () => {
            const e = document.getElementById('reg-email').value, p = document.getElementById('reg-pass').value, c = document.getElementById('reg-confirm').value;
            if(!e || !p || !c || p!==c || p.length<6) return window.showError('reg-error', 'يرجى ملء جميع الحقول');
            if(p !== c) return window.showError('reg-error', 'كلمات المرور غير متطابقة');
            if(p.length < 6) return window.showError('reg-error', 'كلمة المرور يجب أن تكون 6 أحرف على الأقل');

            window.showLoader(true);
            try { 
                // 1. إنشاء الحساب أولاً
                const cred = await createUserWithEmailAndPassword(auth, e, p); 
                
                // 2. التحقق من المستخدمين الموجودين
                const snap = await getDocs(collection(db, "users")); 
                const role = snap.empty ? 'admin' : 'user'; 
                const status = (role === 'admin') ? 'active' : 'pending';

                // 3. إرسال رابط التفعيل
                await sendEmailVerification(cred.user); 
                
                // 4. حفظ البيانات
                await setDoc(doc(db, "users", cred.user.uid), { 
                    email: e, 
                    role: role, 
                    status: status 
                }); 
                
                // 5. إنشاء الإعدادات
                await setDoc(doc(db, "settings", cred.user.uid), { 
                    joinDate: '', 
                    fullName: '', 
                    adjustments: [], 
                    dismissedMsgs: [], 
                    deletedMsgs: [], 
                    nfc: {enabled:false, serial:''}, 
                    themeColor: '#4361ee' 
                }); 
                
                // 6. إعدادات الأدمن
                if(role === 'admin') {
                    await setDoc(doc(db, "config", "general"), { 
                        presets: [
                            {label:'صباح', start:'08:00', end:'16:00'},
                            {label:'نصف يوم', start:'08:00', end:'12:00'}
                        ],
                        ramadanPresets: [
                            {label:'صباح رمضان', start:'09:00', end:'15:00'}
                        ],
                        ramadanStart: '', 
                        ramadanDays: 0,
                        appName: 'المدير الذكي'
                    }); 
                }
                
                // 7. تسجيل الخروج
                await signOut(auth); 
                
                const successMsg = (status === 'pending') 
                    ? "تم إنشاء الحساب بنجاح. يرجى انتظار تفعيل الأدمن لاشتراكك."
                    : "تم إنشاء حساب الأدمن! يرجى تفعيل البريد الإلكتروني ثم الدخول.";
                
                document.getElementById('reg-success').textContent = successMsg;
                document.getElementById('reg-success').style.display = 'block'; 
                
                document.getElementById('reg-email').value = '';
                document.getElementById('reg-pass').value = '';
                document.getElementById('reg-confirm').value = '';

            } catch(err) { 
                console.error("Signup Error:", err);
                let msg = 'حدث خطأ في التسجيل';
                if(err.code === 'auth/email-already-in-use') msg = 'البريد الإلكتروني مسجل مسبقاً';
                if(err.code === 'auth/invalid-email') msg = 'صيغة البريد الإلكتروني خاطئة';
                if(err.code === 'permission-denied') msg = 'لا تملك صلاحية الوصول لقاعدة البيانات (تأكد من Rules)';
                
                window.showError('reg-error', msg); 
            } finally { 
                window.showLoader(false); 
            }
        };

        window.handleReset = async () => { const e = document.getElementById('reset-email').value; if(!e) return; try { await sendPasswordResetEmail(auth, e); document.getElementById('reset-msg').textContent = "تم الإرسال"; document.getElementById('reset-msg').style.display = 'block'; } catch(err) {} };
        window.handleLogout = async () => { await signOut(auth); window.location.reload(); };

        window.saveData = async (type, data) => { const u = auth.currentUser; if(!u) return; try { if(type === 'personal_settings') await setDoc(doc(db, 'settings', u.uid), data, {merge:true}); else if(type === 'global_config') await setDoc(doc(db, 'config', 'general'), data, {merge:true}); else if(type === 'events') await setDoc(doc(db, 'attendance', u.uid), {events: data}, {merge:true}); } catch(e) {} };
        window.fbDeleteDay = async (dateKey) => { const u = auth.currentUser; if(!u) return; try { await updateDoc(doc(db, 'attendance', u.uid), { [`events.${dateKey}`]: deleteField() }); } catch(e) {} };
        window.sendAdminMessage = async (text) => { const u = auth.currentUser; if(!u) return; try { await addDoc(collection(db, "notifications"), { content: text, createdAt: serverTimestamp(), sender: u.uid }); alert("تم"); } catch(e) {} };

        window.approveUser = async (uid) => { try { await updateDoc(doc(db, 'users', uid), { status: 'active' }); alert("تم تفعيل المستخدم ✅"); window.app.openSettings(); } catch(e) { alert("خطأ"); } };
        window.rejectUser = async (uid) => { if(confirm("هل أنت متأكد من رفض هذا المستخدم؟ سيتم حظره.")) { try { await updateDoc(doc(db, 'users', uid), { status: 'rejected' }); alert("تم رفض المستخدم ❌"); window.app.openSettings(); } catch(e) { alert("خطأ"); } } };

        window.fetchPendingUsers = async () => {
            const list = document.getElementById('pending-users-list');
            if(!list) return;
            list.innerHTML = '<div style="text-align:center;">جاري التحميل...</div>';
            try {
                const q = query(collection(db, "users"), where("status", "==", "pending"));
                const querySnapshot = await getDocs(q);
                list.innerHTML = '';
                if(querySnapshot.empty) {
                    list.innerHTML = '<div style="text-align:center; color:#999; font-size:0.8rem;">لا توجد طلبات جديدة</div>';
                    return;
                }
                querySnapshot.forEach((doc) => {
                    const u = doc.data();
                    list.innerHTML += `<div style="display:flex; justify-content:space-between; align-items:center; background:white; padding:8px; border-radius:8px; margin-bottom:5px; border:1px solid #eee;"><span style="font-size:0.85rem; font-weight:bold;">${u.email}</span><div style="display:flex; gap:5px;"><button onclick="window.approveUser('${doc.id}')" style="background:#e8f5e9; color:#2e7d32; border:none; padding:4px 8px; border-radius:6px; cursor:pointer;">✅</button><button onclick="window.rejectUser('${doc.id}')" style="background:#ffebee; color:#c62828; border:none; padding:4px 8px; border-radius:6px; cursor:pointer;">❌</button></div></div>`;
                });
            } catch(e) { list.innerHTML = 'خطأ في التحميل'; }
        };

        onAuthStateChanged(auth, async (user) => {
            if(user) {
                const uDoc = await getDoc(doc(db, 'users', user.uid));
                if(uDoc.exists()) {
                    const data = uDoc.data();
                    if(data.status !== 'active') {
                        if(data.status === 'pending' || data.status === 'rejected') {
                            await signOut(auth);
                            document.getElementById('auth-overlay').style.display = 'flex';
                            return;
                        }
                    }
                    window.appData.role = data.role;
                    if(window.appData.role === 'admin') {
                        document.getElementById('admin-section').style.display = 'block';
                    }
                }

                if(user.emailVerified) {
                    document.getElementById('auth-overlay').style.display = 'none'; document.getElementById('app-container').style.display = 'block';
                    document.getElementById('u-name').textContent = user.email.split('@')[0];
                    window.app.initTheme(); window.showLoader(true);
                    
                    onSnapshot(doc(db, "attendance", user.uid), (doc) => { if(doc.exists()) window.appData.events = doc.data().events || {}; window.app.renderCalendar(); window.app.checkAutoFill(); });
                    onSnapshot(doc(db, "settings", user.uid), (doc) => { 
                        if(doc.exists()) window.appData.personal = doc.data() || {};
                        if(!window.appData.personal.nfc) window.appData.personal.nfc = {enabled: false, serial: ''};
                        if(!window.appData.personal.dismissedMsgs) window.appData.personal.dismissedMsgs = [];
                        if(!window.appData.personal.deletedMsgs) window.appData.personal.deletedMsgs = [];
                        
                        document.getElementById('u-name').textContent = window.appData.personal.fullName || user.email.split('@')[0];
                        document.getElementById('btn-nfc-scan').style.display = window.appData.personal.nfc.enabled ? 'flex' : 'none';
                        
                        if(window.appData.personal.themeColor) {
                            window.app.applyColor(window.appData.personal.themeColor);
                        }

                        window.app.calcStats(); window.app.checkMessages();
                    });
                    onSnapshot(doc(db, "config", "general"), (doc) => { if(doc.exists()) { window.appData.global = doc.data() || {}; if(window.appData.global.appName) { document.title = window.appData.global.appName; document.getElementById('header-title').textContent = window.appData.global.appName; } } });
                    const q = query(collection(db, "notifications"), orderBy("createdAt", "desc"));
                    onSnapshot(q, (snapshot) => { let msgs = []; snapshot.forEach((doc) => msgs.push({ id: doc.id, ...doc.data() })); window.appData.messages = msgs; window.app.checkMessages(); });
                    window.showLoader(false);
                } else { if(user) await signOut(auth); document.getElementById('auth-overlay').style.display = 'flex'; document.getElementById('app-container').style.display = 'none'; window.showLoader(false); }
            } else {
                document.getElementById('auth-overlay').style.display = 'flex'; document.getElementById('app-container').style.display = 'none'; window.showLoader(false);
            }
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

        window.appData = { role: 'user', events: {}, personal: {}, global: {}, messages: [] };

        window.app = {
            initTheme: () => { 
                const saved = localStorage.getItem('theme');
                if (saved === 'dark') {
                    document.body.classList.add('dark-mode');
                } else if (!saved && window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
                    document.body.classList.add('dark-mode');
                }
                
                window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', e => {
                    if (!localStorage.getItem('theme')) { 
                        if (e.matches) document.body.classList.add('dark-mode');
                        else document.body.classList.remove('dark-mode');
                    }
                });
            },
            toggleTheme: () => { 
                document.body.classList.toggle('dark-mode'); 
                localStorage.setItem('theme', document.body.classList.contains('dark-mode') ? 'dark' : 'light'); 
                window.app.renderChart(); 
            },
            
            applyColor: (color) => {
                if(!color) return;
                document.documentElement.style.setProperty('--primary', color);
                
                if(color.startsWith('#') && color.length === 7) {
                    let col = parseInt(color.slice(1), 16);
                    let amt = -20;
                    let r = (col >> 16) + amt;
                    let g = (col >> 8 & 0x00FF) + amt;
                    let b = (col & 0x0000FF) + amt;
                    let newColor = "#" + (0x1000000 + (r<255?r<1?0:r:255)*0x10000 + (g<255?g<1?0:g:255)*0x100 + (b<255?b<1?0:b:255)).toString(16).slice(1);
                    document.documentElement.style.setProperty('--primary-dark', newColor);
                }
                
                const picker = document.getElementById('s-theme-color');
                if(picker) picker.value = color;
                
                const overlay = document.getElementById('auth-overlay');
                if(overlay) overlay.style.background = `linear-gradient(135deg, ${color}, #4cc9f0)`;
            },

            isRamadan: (dateKey) => {
                const g = window.appData.global;
                if(!g || !g.ramadanStart || !g.ramadanDays) return false;
                const start = new Date(g.ramadanStart); start.setHours(0,0,0,0);
                const target = new Date(dateKey); target.setHours(0,0,0,0);
                const end = new Date(start); end.setDate(start.getDate() + parseInt(g.ramadanDays) - 1);
                return target >= start && target <= end;
            },

            // --- NFC ---
            toggleNFCConfig: () => { const isEnabled = document.getElementById('s-nfc-toggle').checked; document.getElementById('nfc-config-area').classList.toggle('hidden', !isEnabled); },
            handleNFCReading: async (mode) => {
                if (!('NDEFReader' in window)) return alert("المتصفح أو الجهاز لا يدعم NFC.");
                try {
                    const ndef = new NDEFReader(); await ndef.scan();
                    window.app.showLegendToast(mode === 'setup' ? "قرّب الشريحة..." : "جاري المسح...");
                    if(mode !== 'setup') document.getElementById('btn-nfc-scan').classList.add('nfc-scanning');
                    ndef.onreading = event => {
                        const serial = event.serialNumber;
                        if (!serial) return;
                        if (mode === 'setup') { document.getElementById('s-nfc-serial').value = serial; window.app.showLegendToast("تم!"); } 
                        else if (mode === 'checkin') {
                            const savedSerial = window.appData.personal.nfc.serial;
                            if (serial.toLowerCase() === savedSerial.toLowerCase()) { document.getElementById('btn-nfc-scan').classList.remove('nfc-scanning'); document.getElementById('nfcPopup').style.display = 'flex'; } 
                        }
                    };
                } catch (error) { alert("خطأ NFC: " + error); document.getElementById('btn-nfc-scan').classList.remove('nfc-scanning'); }
            },
            scanTagForSetup: () => window.app.handleNFCReading('setup'),
            startNFCScan: () => window.app.handleNFCReading('checkin'),
            confirmNFCAttendance: () => {
                const today = new Date();
                const key = `${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,'0')}-${String(today.getDate()).padStart(2,'0')}`;
                if(window.appData.events[key]) { alert("مسجل مسبقاً."); document.getElementById('nfcPopup').style.display = 'none'; return; }
                let start = '08:00', end = '16:00';
                window.appData.events[key] = { type: 'work', start: start, end: end, hours: 8, note: 'NFC' };
                window.saveData('events', window.appData.events);
                document.getElementById('nfcPopup').style.display = 'none'; window.app.showLegendToast("تم التسجيل ✅"); window.app.renderCalendar();
            },
            
            showExportOptions: () => { document.getElementById('exportModal').style.display = 'flex'; },
            generateReport: (type) => {
                document.getElementById('exportModal').style.display = 'none'; window.showLoader(true);
                const pc = document.getElementById('report-container'); pc.innerHTML = '';
                const yr = currentDate.getFullYear(); const mth = currentDate.getMonth();
                const appName = window.appData.global.appName || 'نظام الحضور';
                const userName = window.appData.personal.fullName || 'موظف';
                let periodStr = type === 'month' ? `${monthNames[mth]} ${yr}` : `سنة ${yr}`;
                let eventsList = [];
                for(const [k, evt] of Object.entries(window.appData.events)) {
                    const d = new Date(k);
                    if((type==='month' && d.getMonth()===mth && d.getFullYear()===yr) || (type==='year' && d.getFullYear()===yr)) eventsList.push({date:k, ...evt});
                }
                eventsList.sort((a,b) => new Date(a.date) - new Date(b.date));

                let net=0, sat=0, workedDays=0, absentDays=0;
                const breakdown = window.app.getLeaveBreakdown();
                const totalLeave = breakdown.pools.reduce((sum, pool) => sum + pool.remaining, 0);

                eventsList.forEach(e => {
                    const isRam = window.app.isRamadan(e.date);
                    if(e.type === 'work' || e.type === 'paid' || (e.type === 'eid' && e.eidStatus === 'work')) {
                        workedDays++;
                        // If paid day, treated as 8 hours. If work/Ramadan, normalize.
                        let effectiveHours = (e.type === 'paid') ? 8 : (isRam ? (e.hours + 1) : e.hours);
                        net += (effectiveHours - 8);
                        const d = new Date(e.date); if(d.getDay() === 6) sat += 4; 
                    }
                    if(e.type === 'absent') { absentDays++; net -= 8; }
                });

                let html = `<div class="report-header"><h1>${appName}</h1><h3>${periodStr}</h3><p>${userName}</p></div>
                    <div class="report-summary">
                        <div class="report-box"><h3>رصيد العطلة</h3><p>${totalLeave}</p></div>
                        <div class="report-box"><h3>الصافي</h3><p>${net.toFixed(1)}</p></div>
                        <div class="report-box"><h3>السبت</h3><p>${sat}</p></div>
                        <div class="report-box"><h3>أيام العمل</h3><p>${workedDays}</p></div>
                    </div>
                    <table class="report-table"><thead><tr><th>التاريخ</th><th>اليوم</th><th>الحالة</th><th>الساعات</th><th>ملاحظات</th></tr></thead><tbody>`;
                
                if(eventsList.length === 0) html += `<tr><td colspan="5">لا توجد بيانات</td></tr>`;
                else eventsList.forEach(e => {
                    const dObj = new Date(e.date);
                    const dayName = dayNames[dObj.getDay()===0?6:dObj.getDay()-1];
                    let typeAr = { work:'عمل', holiday:'عطلة', sick:'مرض', absent:'غياب', recup:'تعويض', eid:'عيد', paid:'يوم مدفوع' }[e.type] || e.type;
                    html += `<tr><td>${e.date}</td><td>${dayName}</td><td>${typeAr}</td><td>${e.hours||'-'}</td><td>${e.note||''}</td></tr>`;
                });
                html += `</tbody></table>`;
                pc.innerHTML = html; pc.classList.add('active-print'); 
                setTimeout(() => { window.showLoader(false); window.print(); setTimeout(() => { pc.classList.remove('active-print'); }, 500); }, 500);
            },

            renderChart: () => {
                const ctx = document.getElementById('myChart'); if(!ctx) return;
                let counts = { work:0, holiday:0, sick:0, absent:0, eid:0, paid:0 };
                Object.values(window.appData.events).forEach(e => { if(counts[e.type]!==undefined) counts[e.type]++; });
                const isDark = document.body.classList.contains('dark-mode');
                if(window.myChartInstance) window.myChartInstance.destroy();
                window.myChartInstance = new Chart(ctx, {
                    type: 'doughnut',
                    data: { 
                        labels: ['عمل', 'عطلة', 'مرض', 'غياب', 'عيد', 'يوم مدفوع'], 
                        datasets: [{ 
                            data: [counts.work, counts.holiday, counts.sick, counts.absent, counts.eid, counts.paid], 
                            backgroundColor: ['#4caf50', '#ffc107', '#ff9800', '#f44336', '#9c27b0', '#009688'], 
                            borderWidth: 0 
                        }] 
                    },
                    options: { 
                        responsive: true, maintainAspectRatio: false, 
                        plugins: { 
                            datalabels: { color: '#fff', font: { weight: 'bold' }, formatter: (v) => v > 0 ? v : '' },
                            legend: { position: 'right', labels: { color: isDark?'#e0e0e0':'#2b2d42', font:{family:'Cairo'} } } 
                        } 
                    },
                    plugins: [ChartDataLabels]
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
                    
                    // --- FIX: Don't auto-fill on National Holidays ---
                    const isNat = nationalHolidays[`${loopDate.getMonth()+1}-${loopDate.getDate()}`];
                    
                    if (evt && evt.type === 'work') { lastStart = evt.start; lastEnd = evt.end; }
                    else if (!evt && loopDate.getDay()!==0 && loopDate.getDay()!==6 && !isNat) {
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
                    const isRam = window.app.isRamadan(key);
                    let cls = '', natCls = isNat ? 'nat-holiday' : '', ramCls = isRam ? 'is-ramadan' : '', noteInd = (evt && evt.note) ? '<div class="note-dot"></div>' : '';
                    if(evt) {
                        cls = `st-${evt.type}`;
                        // Add specific style for worked holidays/eids
                        if(evt.type === 'eid' && evt.eidStatus === 'work') cls += ' worked-eid';
                    }
                    const dObj = new Date(y, m, i), now = new Date(); now.setHours(0,0,0,0);
                    const classes = `day-cell ${dObj.getTime()===now.getTime()?'today':''} ${dObj.getDay()===0||dObj.getDay()===6?'weekend':''} ${natCls} ${ramCls} ${cls}`;
                    grid.innerHTML += `<div class="${classes}" onclick="window.app.openDay('${key}')"><span>${i}</span>${noteInd}</div>`;
                }
                window.app.calcStats();
            },
            navMonth: (s) => { currentDate.setMonth(currentDate.getMonth() + s); window.app.renderCalendar(); },
            openDay: (key) => {
                const natName = nationalHolidays[`${new Date(key).getMonth()+1}-${new Date(key).getDate()}`];
                const today = new Date(); today.setHours(0,0,0,0); const tomorrow = new Date(today); tomorrow.setDate(tomorrow.getDate() + 1);
                if(new Date(key).setHours(0,0,0,0) > tomorrow.getTime() && !natName) return;

                selectedKey = key; document.getElementById('modal-title').textContent = key; document.getElementById('dayModal').style.display = 'flex';
                let evt = window.appData.events[key] || (natName ? {type:'eid',eidStatus:'rest',eidName:natName} : {type:'work',start:'',end:'',eidStatus:'work'});
                document.getElementById('d-type').value = evt.type;
                document.getElementById('d-start').value = evt.start||''; document.getElementById('d-end').value = evt.end||'';
                document.getElementById('d-eid-name').value = evt.eidName||''; document.getElementById('d-count').value = 1;
                document.getElementById('d-note').value = evt.note||'';
                document.getElementById('d-eid-status').value = evt.eidStatus || (natName ? 'rest' : 'work');
                
                const pre = document.getElementById('d-preset'); 
                pre.innerHTML = '<option value="manual">-- توقيت --</option>';
                const isRam = window.app.isRamadan(key);
                const presetsList = isRam ? (window.appData.global.ramadanPresets || []) : (window.appData.global.presets || []);
                presetsList.forEach((p,i) => pre.innerHTML+=`<option value="${i}">${p.label}</option>`);

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
            applyPreset: () => { 
                const idx = document.getElementById('d-preset').value; 
                if(idx!=='manual') { 
                    const isRam = window.app.isRamadan(selectedKey);
                    const list = isRam ? window.appData.global.ramadanPresets : window.appData.global.presets;
                    const p = list[idx]; 
                    document.getElementById('d-start').value=p.start; document.getElementById('d-end').value=p.end; 
                } 
            },
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
                            let diff=(h2*60+m2)-(h1*60+m1); if(s > e) { diff += 24*60; } 
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
                if(deleteType==='day') { 
                    if(window.appData.events[selectedKey]) { 
                        delete window.appData.events[selectedKey]; 
                        window.fbDeleteDay(selectedKey); 
                        
                        // Check if it was a national holiday
                        const d = new Date(selectedKey);
                        const isNat = nationalHolidays[`${d.getMonth()+1}-${d.getDate()}`];
                        if(isNat) {
                            window.app.showLegendToast("تم حذف العمل وعاد اليوم كعطلة رسمية");
                        } else {
                            window.app.showLegendToast("تم الحذف بنجاح");
                        }
                    } 
                    document.getElementById('dayModal').style.display='none'; 
                    window.app.renderCalendar(); 
                }
                else if(deleteType==='msg') { window.appData.personal.deletedMsgs.push(pendingMsgId); window.saveData('personal_settings', window.appData.personal); window.app.openInbox(); }
                document.getElementById('confirmModal').style.display='none';
            },
            
            // --- UPDATED CALCULATIONS ---
            getLeaveBreakdown: () => {
                const currentY = new Date(2026, 0, 1).getFullYear();
                const joinDateStr = window.appData.personal.joinDate;
                let pools = [];
                if(window.appData.personal.adjustments) {
                    window.appData.personal.adjustments.forEach((adj, i) => {
                        pools.push({ id: `adj_${i}`, label: `رصيد سابق/إضافي (${adj.reason})`, total: parseFloat(adj.amount), remaining: parseFloat(adj.amount), type: 'bonus' });
                    });
                }
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
                const holidays = Object.entries(window.appData.events).filter(([k, v]) => v.type === 'holiday').sort((a, b) => new Date(a[0]) - new Date(b[0]));
                let deductions = [];
                holidays.forEach(h => {
                    let consumed = false;
                    for(let pool of pools) {
                        if(pool.remaining > 0) {
                            pool.remaining--;
                            deductions.push({ date: h[0], note: `تم خصمه من ${pool.label}`, val: '-1', type: 'neg' });
                            consumed = true; break;
                        }
                    }
                    if(!consumed) deductions.push({ date: h[0], note: 'رصيد غير كافٍ', val: '-1', type: 'neg' });
                });
                return { pools, deductions };
            },
            
            calcStats: () => {
                let net=0, sat=0, tYearWorkHours=0, tSickDays=0, tWeek=0, tMonth=0;
                let yr=currentDate.getFullYear(), mth=currentDate.getMonth(), today=new Date();
                const weekStart=new Date(today); weekStart.setDate(today.getDate()-today.getDay()); weekStart.setHours(0,0,0,0);
                const weekEnd=new Date(weekStart); weekEnd.setDate(weekStart.getDate()+6); weekEnd.setHours(23,59,59,999);
                
                const breakdown = window.app.getLeaveBreakdown();
                const leave = breakdown.pools.reduce((sum, pool) => sum + pool.remaining, 0);
                let pending = 0;

                // --- 1. Loop through DATES from Jan 1 to Today for better implicit holiday handling ---
                for (let d = new Date(yr, 0, 1); d <= new Date(); d.setDate(d.getDate() + 1)) {
                    const k = `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
                    const evt = window.appData.events[k];
                    const isNat = nationalHolidays[`${d.getMonth()+1}-${d.getDate()}`];
                    const isRam = window.app.isRamadan(k);
                    const dayNum = d.getDay();
                    const isWeekday = (dayNum >= 1 && dayNum <= 5); // Mon-Fri

                    // ----------------------------------------
                    // A) Total Annual Hours (tYearWorkHours)
                    // ----------------------------------------
                    let dayHours = 0;
                    if (evt) {
                        if (evt.type === 'work' || (evt.type === 'eid' && evt.eidStatus === 'work')) {
                            // Actual work + Ramadan bonus
                            dayHours = (evt.type === 'paid') ? 8 : (isRam ? (evt.hours + 1) : evt.hours);
                            
                            // If it is a holiday/eid worked, add the base 8h bonus
                            if ((evt.type === 'eid' && evt.eidStatus === 'work') || (evt.type === 'work' && isNat)) {
                                dayHours += 8; 
                            }
                        } 
                        else if (evt.type === 'recup') {
                            dayHours = 8; // Request: +8 for Recup
                        }
                        else if (evt.type === 'paid' || evt.type === 'holiday') {
                            dayHours = 8;
                        }
                        else if (evt.type === 'eid' && evt.eidStatus !== 'work') {
                            // Saved religious holiday (rest). If weekday -> 8
                            if (isWeekday) dayHours = 8; 
                        }
                    } else {
                        // No event. Check Implicit National Holiday
                        if (isNat && isWeekday) {
                            dayHours = 8; // Request: +8 for Holiday inside week if not worked
                        }
                    }
                    tYearWorkHours += dayHours;

                    // ----------------------------------------
                    // B) Net Balance (Net) & Monthly/Weekly
                    // ----------------------------------------
                    if (evt) {
                        if (evt.type === 'work' || evt.type === 'paid' || (evt.type === 'eid' && evt.eidStatus === 'work')) {
                            let h = evt.hours || 0;
                            let effectiveHours = (evt.type === 'paid') ? 8 : (isRam ? (h + 1) : h);
                            
                            // Fix for Net Balance (Eid Work Logic)
                            if ((evt.type === 'eid' && evt.eidStatus === 'work') || (evt.type === 'work' && isNat)) {
                                // Working on a holiday (bonus) adds full hours to net
                                net += effectiveHours;
                            } else {
                                // Normal work deducts the standard 8h obligation
                                net += (effectiveHours - 8);
                            }

                            if (d.getMonth() === mth) tMonth += effectiveHours;
                            if (d >= weekStart && d <= weekEnd) tWeek += effectiveHours;
                        } else if (evt.type === 'absent') {
                            net -= 8;
                        }
                        if (evt.type === 'sick') tSickDays++;
                    }

                    // ----------------------------------------
                    // C) Saturday Balance (Sat)
                    // ----------------------------------------
                    if (dayNum === 6) { // Saturday
                        if (evt && (evt.type === 'work' || evt.type === 'sick' || evt.type === 'paid' || (evt.type === 'eid' && evt.eidStatus === 'work'))) {
                            sat += 4;
                        } else {
                            sat -= 4;
                        }
                    }
                }

                // ----------------------------------------
                // D) Sunday & Holiday Pending (Count)
                // ----------------------------------------
                const used = Object.values(window.appData.events).filter(e=>e.type==='recup').map(e=>e.recupTarget);
                Object.entries(window.appData.events).forEach(([k,evt]) => { 
                    const d = new Date(k);
                    if (d.getFullYear() === yr) {
                        const isWorkedSunday = (d.getDay() === 0 && evt.type === 'work');
                        const isWorkedEid = (evt.type === 'eid' && evt.eidStatus === 'work');
                        const isNat = nationalHolidays[`${d.getMonth()+1}-${d.getDate()}`];
                        const isWorkOnNatHoliday = (evt.type === 'work' && isNat); 

                        if (isWorkedSunday || isWorkedEid || isWorkOnNatHoliday) { 
                            if (!used.includes(k)) {
                                pending++; 
                            }
                        }
                    }
                });

                document.getElementById('st-net').innerHTML = `<span class="${net>=0?'txt-green':'txt-red'}">${net.toFixed(1)}</span>`;
                document.getElementById('st-sat').innerHTML = `<span class="${sat>=0?'txt-green':'txt-red'}">${sat}</span>`;
                document.getElementById('st-leave').textContent = leave.toFixed(1); 
                document.getElementById('st-sunday').textContent = pending;
                document.getElementById('st-week').textContent = tWeek.toFixed(1); 
                document.getElementById('st-month').textContent = tMonth.toFixed(1); 
                document.getElementById('st-year').innerHTML = `<div style="font-size:1.8rem; font-weight:bold; color:var(--primary)">${tYearWorkHours.toFixed(0)} <span style="font-size:1rem; color:var(--text-light)">ساعة</span></div>`;
                window.app.renderChart();
            },
            
            showDetails: (cat) => {
                document.getElementById('search-inputs').style.display = 'none';
                document.getElementById('search-title').textContent = 'التفاصيل';
                const list = document.getElementById('search-results'); list.innerHTML = '';
                const yr = currentDate.getFullYear();
                let tempList = [];

                if (cat === 'leave') {
                    const bd = window.app.getLeaveBreakdown();
                    list.innerHTML += `<div class="details-header">الأرصدة المتاحة (FIFO):</div>`;
                    bd.pools.forEach(p => {
                        if(p.remaining > 0) list.innerHTML += `<div class="detail-item pos"><span>${p.label}</span><span class="d-val">${p.remaining} يوم</span></div>`;
                    });
                    if(bd.deductions.length > 0) {
                        list.innerHTML += `<div class="details-header">سجل الاستهلاك:</div>`;
                        bd.deductions.reverse().forEach(d => list.innerHTML += `<div class="detail-item neg" onclick="window.app.openDay('${d.date}')"><span>${d.date} <small>(${d.note})</small></span><span class="d-val">-1</span></div>`);
                    } else { list.innerHTML += `<div style="text-align:center; padding:10px;">لم يتم استهلاك أي عطلة</div>`; }
                    document.getElementById('searchModal').style.display = 'flex'; return;
                }

                else if (cat === 'sunday') {
                    document.getElementById('search-title').textContent = 'تفاصيل الأحد والأعياد (عمل)';
                    const used = Object.values(window.appData.events).filter(e=>e.type==='recup').map(e=>e.recupTarget);
                    
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        const d = new Date(k);
                        if(d.getFullYear() === yr) {
                            const isWorkedSunday = (d.getDay() === 0 && evt.type === 'work');
                            const isWorkedEid = (evt.type === 'eid' && evt.eidStatus === 'work');
                            
                            const isNat = nationalHolidays[`${d.getMonth()+1}-${d.getDate()}`];
                            const isWorkOnNatHoliday = (evt.type === 'work' && isNat);

                            if (isWorkedSunday || isWorkedEid || isWorkOnNatHoliday) {
                                const isPending = !used.includes(k);
                                let reason = isWorkedSunday ? 'عمل يوم أحد' : `عمل يوم عيد (${evt.eidName || (isNat ? nationalHolidays[`${d.getMonth()+1}-${d.getDate()}`] : '')})`;
                                
                                tempList.push({
                                    date: k, 
                                    note: reason + (isPending ? ' (يستوجب التعويض)' : ' (تم التعويض)'), 
                                    val: evt.hours + 'س', 
                                    type: isPending ? 'neg' : 'pos' 
                                });
                            }
                        }
                    }
                }

                else if (cat === 'year') {
                    let summary = { work:{c:0,h:0,l:'عمل',i:'✅'}, holiday:{c:0,h:0,l:'عطلة',i:'🏖️'}, sick:{c:0,h:0,l:'مرض',i:'💊'}, absent:{c:0,h:0,l:'غياب',i:'❌'}, eid:{c:0,h:0,l:'أعياد',i:'🎉'}, recup:{c:0,h:0,l:'استرجاع',i:'🔄'}, paid:{c:0,h:0,l:'يوم مدفوع',i:'💰'} };
                    for(const [k, e] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() === yr) {
                            if(summary[e.type]) {
                                summary[e.type].c++;
                                if(e.type === 'work' || e.type === 'paid' || e.type === 'holiday' || (e.type === 'eid' && e.eidStatus === 'work')) {
                                    const isRam = window.app.isRamadan(k);
                                    let effectiveHours = (e.type === 'paid' || e.type === 'holiday') ? 8 : (isRam ? (e.hours + 1) : e.hours);
                                    summary[e.type].h += effectiveHours;
                                }
                            }
                        }
                    }
                    list.innerHTML = `<div class="details-header">ملخص سنة ${yr}</div>`;
                    for(const [key, data] of Object.entries(summary)) {
                        if(data.c > 0) {
                            let isHoursType = (key === 'work' || key === 'eid' || key === 'paid' || key === 'holiday');
                            let valStr = isHoursType ? `${data.h.toFixed(1)} ساعة` : `${data.c} يوم`;
                            list.innerHTML += `<div class="detail-item neutral" onclick="window.app.showDetails('year_sub_${key}')"><span>${data.i} ${data.l}</span><span class="d-val">${valStr}</span></div>`;
                        }
                    }
                    document.getElementById('searchModal').style.display = 'flex'; return;
                }

                else if (cat.startsWith('year_sub_')) {
                    const subType = cat.replace('year_sub_', '');
                    document.getElementById('search-title').textContent = 'تفاصيل: ' + subType;
                    list.innerHTML = `<div class="details-header" style="cursor:pointer; color:var(--text-light)" onclick="window.app.showDetails('year')">⬅️ عودة للملخص</div>`;
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() === yr && evt.type === subType) {
                            let val = 'يوم';
                            if(evt.type==='work' || evt.type==='paid' || evt.type==='holiday' || (evt.type==='eid' && evt.eidStatus==='work')) {
                                const isRam = window.app.isRamadan(k);
                                let effectiveHours = (evt.type === 'paid' || evt.type === 'holiday') ? 8 : (isRam ? (evt.hours + 1) : evt.hours);
                                val = effectiveHours.toFixed(1) + 'س';
                            }
                            tempList.push({date:k, note:evt.note||'', val:val, type: evt.type==='absent'?'neg':'pos'});
                        }
                    }
                } else if (cat === 'sat') {
                    const startOfYear = new Date(yr, 0, 1);
                    const calcUntil = new Date();
                    
                    for (let d = new Date(startOfYear); d <= calcUntil; d.setDate(d.getDate() + 1)) {
                        if (d.getDay() === 6) {
                            const k = `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
                            const evt = window.appData.events[k];
                            
                            let st = 'فارغ', val = -4, type = 'neg';
                            
                            if (evt) {
                                if (evt.type === 'work' || evt.type === 'sick' || evt.type === 'paid' || (evt.type === 'eid' && evt.eidStatus === 'work')) {
                                    st = (evt.type === 'work' ? 'عمل' : (evt.type === 'sick' ? 'مرض' : 'مدفوع'));
                                    val = 4;
                                    type = 'pos';
                                } else {
                                    st = (evt.type === 'absent' ? 'غياب' : evt.type);
                                }
                            }
                            
                            tempList.push({date:k, note:st, val:(val>0?'+':'')+val, type});
                        }
                    }
                } else if (cat === 'net') {
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() !== yr) continue;
                        let diff = 0, note = '';
                        const isRam = window.app.isRamadan(k);
                        if(evt.type==='work' || evt.type==='paid' || (evt.type==='eid' && evt.eidStatus==='work')) { 
                            let effectiveHours = (evt.type === 'paid') ? 8 : (isRam ? (evt.hours + 1) : evt.hours);
                            
                            // Net Calculation Fix for Eid
                            if (evt.type === 'eid' && evt.eidStatus === 'work') {
                                diff = effectiveHours; // Add full hours
                                note = 'عمل يوم عيد';
                            } else {
                                diff = effectiveHours - 8; 
                                note = 'عمل';
                            }
                        }
                        else if(evt.type==='absent') { diff = -8; note='غياب'; }
                        if(diff !== 0) tempList.push({date:k, note, val:(diff>0?'+':'')+diff.toFixed(1), type:diff>=0?'pos':'neg'});
                    }
                } else if (['week', 'month'].includes(cat)) {
                     const today = new Date();
                     const weekStart=new Date(today); weekStart.setDate(today.getDate()-today.getDay()); weekStart.setHours(0,0,0,0);
                     const weekEnd=new Date(weekStart); weekEnd.setDate(weekStart.getDate()+6); weekEnd.setHours(23,59,59,999);
                     
                     for(const [k, evt] of Object.entries(window.appData.events)) {
                        const d = new Date(k);
                        if(d.getFullYear() === yr) {
                            let include = false;
                            if(cat === 'month' && d.getMonth() === currentDate.getMonth()) include = true;
                            if(cat === 'week' && d >= weekStart && d <= weekEnd) include = true;
                            
                            if(include && (evt.type==='work' || evt.type==='paid' || (evt.type==='eid' && evt.eidStatus==='work'))) {
                                const isRam = window.app.isRamadan(k);
                                let effectiveHours = (evt.type === 'paid') ? 8 : (isRam ? (evt.hours + 1) : evt.hours);
                                tempList.push({date:k, note:evt.type, val:effectiveHours.toFixed(1)+'س', type:'pos'});
                            }
                        }
                     }
                }

                tempList.sort((a,b) => new Date(b.date) - new Date(a.date));
                if(tempList.length === 0 && !cat.startsWith('year_sub')) list.innerHTML += '<div style="text-align:center; padding:10px;">لا توجد بيانات</div>';
                tempList.forEach(item => {
                    list.innerHTML += `<div class="detail-item ${item.type}" onclick="window.app.openDay('${item.date}')"><span>${item.date} <small>(${item.note})</small></span><span class="d-val ${item.type}">${item.val}</span></div>`;
                });
                document.getElementById('searchModal').style.display = 'flex';
            },

            openSearchModal: () => { 
                document.getElementById('search-inputs').style.display = 'block'; 
                document.getElementById('search-title').textContent = 'بحث'; 
                // Reset inputs
                document.getElementById('search-month').value = '';
                document.getElementById('search-day-name').value = '';
                document.getElementById('search-type').value = '';
                document.getElementById('search-note').value = '';
                // Clear results and show message
                document.getElementById('search-results').innerHTML = '<div style="text-align:center; padding:20px; color:#999;">المرجو تحديد نوع البحث أو كتابة ملاحظة لعرض النتائج</div>';
                document.getElementById('searchModal').style.display = 'flex'; 
            },

            performSearch: () => {
                const sMonth = document.getElementById('search-month').value;
                const sDay = document.getElementById('search-day-name').value;
                const sType = document.getElementById('search-type').value;
                const sNote = document.getElementById('search-note').value.trim().toLowerCase();
                
                const list = document.getElementById('search-results');
                
                // If all fields are empty, do not search
                if (sMonth === "" && sDay === "" && sType === "" && sNote === "") {
                    list.innerHTML = '<div style="text-align:center; padding:20px; color:#999;">المرجو تحديد نوع البحث...</div>';
                    return;
                }

                list.innerHTML = '';
                let found = false;
                const eventsArray = Object.entries(window.appData.events).sort((a, b) => new Date(b[0]) - new Date(a[0]));

                for(const [dateKey, evt] of eventsArray) {
                    const dateObj = new Date(dateKey);
                    
                    if (sMonth !== "" && (dateObj.getMonth() + 1) != sMonth) continue;
                    if (sDay !== "" && dateObj.getDay() != sDay) continue;
                    if (sType !== "" && evt.type !== sType) continue;

                    const evtNote = (evt.note || "").toLowerCase();
                    const eidName = (evt.eidName || "").toLowerCase();
                    const combinedNote = evtNote + " " + eidName;
                    if (sNote !== "" && !combinedNote.includes(sNote)) continue;

                    found = true;
                    const dayName = ["أحد", "إثنين", "ثلاثاء", "أربعاء", "خميس", "جمعة", "سبت"][dateObj.getDay()];
                    let typeAr = { work:'عمل', holiday:'عطلة', sick:'مرض', absent:'غياب', recup:'تعويض', eid:'عيد', paid:'يوم مدفوع' }[evt.type] || evt.type;
                    
                    list.innerHTML += `
                        <div class="search-item" onclick="window.app.openDay('${dateKey}')">
                            <div style="display:flex; justify-content:space-between; width:100%;">
                                <span style="font-weight:bold;">${dateKey} <span style="font-size:0.8em; color:#777;">(${dayName})</span></span>
                                <span style="font-size:0.85em; background:#eee; padding:2px 6px; border-radius:4px;">${typeAr}</span>
                            </div>
                            <div style="font-size:0.85rem; color:#555; margin-top:4px;">${evt.note || evt.eidName || ''}</div>
                        </div>
                    `;
                }

                if(!found) {
                    list.innerHTML = '<div style="text-align:center; padding:20px; color:#999;">لا توجد نتائج تطابق بحثك</div>';
                }
            },

            openSettings: () => {
                if(!window.appData.personal) window.appData.personal = {};
                document.getElementById('s-join').value = window.appData.personal.joinDate||''; document.getElementById('s-name').value = window.appData.personal.fullName||'';
                const nfc = window.appData.personal.nfc || {enabled:false, serial:''};
                document.getElementById('s-nfc-toggle').checked = nfc.enabled; document.getElementById('s-nfc-serial').value = nfc.serial;
                window.app.toggleNFCConfig();
                
                if(window.appData.role === 'admin') {
                    const g = window.appData.global;
                    document.getElementById('p-app-name').value = g.appName || '';
                    document.getElementById('p-ramadan-start').value = g.ramadanStart || '';
                    document.getElementById('p-ramadan-days').value = g.ramadanDays || '';
                    window.fetchPendingUsers();
                }
                
                // Color Picker
                if(window.appData.personal.themeColor) {
                    document.getElementById('s-theme-color').value = window.appData.personal.themeColor;
                }
                
                window.app.renderSettingsLists(); document.getElementById('settingsModal').style.display='flex';
            },
            
            addPreset: (type) => {
                const nameId = type === 'ramadan' ? 'pr-name' : 'p-name';
                const startId = type === 'ramadan' ? 'pr-start' : 'p-start';
                const endId = type === 'ramadan' ? 'pr-end' : 'p-end';
                const listName = type === 'ramadan' ? 'ramadanPresets' : 'presets';

                const n = document.getElementById(nameId).value;
                const s = document.getElementById(startId).value;
                const e = document.getElementById(endId).value;
                
                if(n && s && e) {
                    if(!window.appData.global[listName]) window.appData.global[listName] = [];
                    window.appData.global[listName].push({label:n, start:s, end:e});
                    document.getElementById(nameId).value = ''; document.getElementById(startId).value = ''; document.getElementById(endId).value = '';
                    window.app.renderSettingsLists();
                } else { alert("يرجى ملء جميع الحقول"); }
            },
            delPreset: (type, i) => { 
                const listName = type === 'ramadan' ? 'ramadanPresets' : 'presets';
                window.appData.global[listName].splice(i, 1); window.app.renderSettingsLists(); 
            },

            addAdj: () => { 
                const d = document.getElementById('adj-days').value; const n = document.getElementById('adj-note').value;
                if(d && n) {
                    if(!window.appData.personal.adjustments) window.appData.personal.adjustments = [];
                    window.appData.personal.adjustments.push({amount:d, reason:n});
                    document.getElementById('adj-days').value=''; document.getElementById('adj-note').value='';
                    window.app.renderSettingsLists();
                }
            },
            delAdj: (i) => { window.appData.personal.adjustments.splice(i, 1); window.app.renderSettingsLists(); },
            
            renderSettingsLists: () => {
                const pl = document.getElementById('presets-list'); pl.innerHTML = ''; 
                (window.appData.global.presets || []).forEach((p,i)=>pl.innerHTML+=`<div class="preset-item"><span>${p.label} (${p.start}-${p.end})</span> <span class="del-icon" onclick="window.app.delPreset('normal', ${i})">X</span></div>`);
                
                const rpl = document.getElementById('ramadan-presets-list'); rpl.innerHTML = ''; 
                (window.appData.global.ramadanPresets || []).forEach((p,i)=>rpl.innerHTML+=`<div class="preset-item"><span>${p.label} (${p.start}-${p.end})</span> <span class="del-icon" onclick="window.app.delPreset('ramadan', ${i})">X</span></div>`);
                
                const al = document.getElementById('adj-list'); al.innerHTML = ''; 
                (window.appData.personal.adjustments || []).forEach((a,i)=>al.innerHTML+=`<div class="preset-item"><span>${a.reason} (${a.amount})</span><span class="del-icon" onclick="window.app.delAdj(${i})">X</span></div>`);
            },
            
            saveSettings: () => {
                window.appData.personal.joinDate = document.getElementById('s-join').value;
                window.appData.personal.fullName = document.getElementById('s-name').value;
                window.appData.personal.nfc = { enabled: document.getElementById('s-nfc-toggle').checked, serial: document.getElementById('s-nfc-serial').value };
                window.appData.personal.themeColor = document.getElementById('s-theme-color').value;
                
                window.app.applyColor(window.appData.personal.themeColor);
                window.saveData('personal_settings', window.appData.personal);
                
                if(window.appData.role === 'admin') {
                    window.appData.global.appName = document.getElementById('p-app-name').value;
                    window.appData.global.ramadanStart = document.getElementById('p-ramadan-start').value;
                    window.appData.global.ramadanDays = document.getElementById('p-ramadan-days').value;
                    window.saveData('global_config', window.appData.global);
                }
                document.getElementById('settingsModal').style.display = 'none';
                document.getElementById('u-name').textContent = window.appData.personal.fullName || 'مستخدم';
                document.getElementById('btn-nfc-scan').style.display = window.appData.personal.nfc.enabled ? 'flex' : 'none';
                window.app.renderCalendar();
            },
            showLegendToast: (msg) => { const t = document.getElementById('legend-toast'); t.textContent = msg; t.classList.add('show-toast'); setTimeout(() => t.classList.remove('show-toast'), 3000); }
        };
    </script>
</body>
</html>
