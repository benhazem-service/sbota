<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>المدير الذكي 2026</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">

    <style>
        :root {
            --primary: #4361ee; --primary-dark: #3a0ca3;
            --bg: #f8f9fa; --surface: #ffffff;
            --text: #2b2d42; --text-light: #8d99ae;
            --work: #4caf50; --holiday: #ffc107; --sick: #ff9800;
            --absent: #f44336; --eid: #9c27b0; --recup: #00bcd4;
            --radius: 16px;
        }

        * { box-sizing: border-box; touch-action: manipulation; -webkit-tap-highlight-color: transparent; }
        body { font-family: 'Cairo', sans-serif; background-color: var(--bg); margin: 0; padding-bottom: 80px; color: var(--text); }

        /* Auth Screen Styles */
        #auth-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: linear-gradient(135deg, var(--primary), #4cc9f0);
            z-index: 9999; display: flex; justify-content: center; align-items: center; flex-direction: column;
        }
        .auth-card {
            background: rgba(255,255,255,0.98); padding: 30px; border-radius: 24px;
            width: 90%; max-width: 380px; text-align: center;
            box-shadow: 0 10px 40px rgba(0,0,0,0.2);
        }
        .auth-header h2 { color: var(--primary-dark); margin: 0 0 10px 0; }
        .input-group { position: relative; margin-bottom: 15px; }
        .app-input { width: 100%; padding: 12px; border: 2px solid #e0e0e0; border-radius: 12px; font-family: inherit; font-size: 1rem; outline: none; transition: 0.3s; }
        .app-input:focus { border-color: var(--primary); }
        .toggle-password { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #888; font-size: 1.2rem; }
        .btn-main { width: 100%; padding: 12px; background: var(--primary); color: white; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; margin-top: 10px; }
        .btn-secondary { background: transparent; color: var(--primary); border: 2px solid var(--primary); margin-top: 10px; }
        .btn-close-modal { width: 100%; padding: 12px; margin-top: 15px; background: #f1f5f9; color: #475569; border: 1px solid #e2e8f0; border-radius: 12px; font-weight: bold; cursor: pointer; display: flex; justify-content: center; align-items: center; gap: 8px; }
        .error-msg { color: #d32f2f; display: none; background: #ffebee; padding: 8px; border-radius: 8px; margin-top: 10px; font-size: 0.85rem; }
        .success-msg { color: #2e7d32; display: none; background: #e8f5e9; padding: 8px; border-radius: 8px; margin-top: 10px; font-size: 0.85rem; }
        .view-section { display: none; } .view-section.active { display: block; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .loading { position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(255,255,255,0.8); z-index:10000; display:none; justify-content:center; align-items:center; }
        
        /* App Layout */
        #app-container { display: none; padding: 15px; max-width: 600px; margin: 0 auto; }
        
        .header { 
            display: flex; flex-wrap: wrap; justify-content: space-between; align-items: center; 
            background: var(--surface); padding: 15px; border-radius: var(--radius); 
            box-shadow: 0 4px 20px rgba(0,0,0,0.05); margin-bottom: 20px; gap: 10px;
        }
        .header-info { display: flex; flex-direction: column; min-width: 120px; }
        
        /* App Title Style */
        .app-main-title { margin: 0; font-size: 1.2rem; color: var(--primary-dark); font-weight: bold; }
        .user-sub-title { font-size: 0.9rem; color: #7f8c8d; }
        .user-sub-title span { color: var(--primary); font-weight: 600; }

        .header-actions { display: flex; gap: 8px; }
        .action-btn { background: #f1f5f9; border: 1px solid #e2e8f0; width: 36px; height: 36px; border-radius: 10px; cursor: pointer; font-size: 1.1rem; display: flex; justify-content: center; align-items: center; color: #64748b; position: relative; }
        .badge-count { position: absolute; top: -5px; left: -5px; background: #f44336; color: white; font-size: 0.7rem; width: 18px; height: 18px; border-radius: 50%; display: none; justify-content: center; align-items: center; border: 2px solid white; }

        /* Stats & Calendar Styles */
        .stats-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-bottom: 20px; }
        .stat-card { background: var(--surface); padding: 15px; border-radius: var(--radius); text-align: center; box-shadow: 0 4px 20px rgba(0,0,0,0.05); cursor: pointer; }
        .stat-card h4 { margin: 0; font-size: 0.75rem; color: var(--text-light); }
        .stat-card .val { font-size: 1.3rem; font-weight: 700; color: var(--primary-dark); }
        .stat-card .sub { font-size: 0.6rem; color: #999; }
        .full-width { grid-column: span 2; }
        .txt-red { color: #f44336 !important; } .txt-green { color: #4caf50 !important; }

        .calendar-box { background: var(--surface); border-radius: var(--radius); padding: 15px; box-shadow: 0 4px 20px rgba(0,0,0,0.05); }
        .cal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        .days-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; }
        .day-name { font-size: 0.75rem; color: var(--text-light); text-align: center; font-weight: bold; }
        
        .day-cell { 
            aspect-ratio: 1; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; 
            padding-top: 5px; border-radius: 10px; background: #f8f9fa; cursor: pointer; position: relative; border: 1px solid transparent; 
        }
        .day-cell span { font-weight: bold; font-size: 0.9rem; color: #444; z-index: 2; position: absolute; top: 4px; right: 6px; line-height: 1; }
        .day-cell.today { border-color: var(--primary); background: #e3f2fd !important; }
        
        .day-cell.weekend { background-color: #E6E6FA; color: #4a4a4a; border: 1px dashed #d1c4e9; }
        .day-cell.future { opacity: 0.5; cursor: default; }

        /* Full Color Classes */
        .day-cell.st-work { background-color: var(--work) !important; color: white !important; }
        .day-cell.st-work span { color: white !important; }
        .day-cell.st-holiday { background-color: var(--holiday) !important; color: #333 !important; }
        .day-cell.st-holiday span { color: #333 !important; }
        .day-cell.st-sick { background-color: var(--sick) !important; color: white !important; }
        .day-cell.st-sick span { color: white !important; }
        .day-cell.st-absent { background-color: var(--absent) !important; color: white !important; }
        .day-cell.st-absent span { color: white !important; }
        .day-cell.st-eid { background-color: var(--eid) !important; color: white !important; }
        .day-cell.st-eid span { color: white !important; }
        .day-cell.st-recup { background-color: var(--recup) !important; color: white !important; }
        .day-cell.st-recup span { color: white !important; }

        /* --- National Holiday (Matching Color) --- */
        .day-cell.nat-holiday { 
            background-color: #f8bbd0 !important; /* لون وردي فاتح للخلفية */
            border: 2px solid #ec407a !important; /* إطار وردي غامق */
            color: #880e4f !important;            /* نص غامق */
        }
        .day-cell.nat-holiday span { color: #880e4f !important; }
        .day-cell.nat-holiday.future { cursor: pointer; opacity: 1; }

        /* Legend */
        .legend-container { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin-top: 20px; padding: 10px; background: var(--surface); border-radius: var(--radius); }
        .legend-dot { width: 20px; height: 20px; border-radius: 50%; cursor: pointer; border: 2px solid white; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        
        .lg-work { background-color: var(--work); }
        .lg-holiday { background-color: var(--holiday); }
        .lg-sick { background-color: var(--sick); }
        .lg-absent { background-color: var(--absent); }
        .lg-recup { background-color: var(--recup); }
        .lg-eid { background-color: var(--eid); }
        /* Matching National Holiday Legend */
        .lg-nat { background-color: #f8bbd0; border: 2px solid #ec407a; }

        #legend-toast { position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%); background: #333; color: white; padding: 8px 16px; border-radius: 20px; font-size: 0.85rem; opacity: 0; transition: opacity 0.3s; pointer-events: none; z-index: 3000; }
        .show-toast { opacity: 1 !important; }

        /* Modals */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 2000; display: none; justify-content: center; align-items: flex-end; }
        #confirmModal, #msgPopup { z-index: 99999 !important; align-items: center; } 
        .modal-content { background: var(--surface); width: 100%; max-width: 500px; border-radius: 24px 24px 0 0; padding: 25px; animation: slideUp 0.3s; max-height: 85vh; overflow-y: auto; }
        #confirmModal .modal-content, #msgPopup .modal-content { border-radius: 24px; max-width: 320px; text-align: center; }
        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
        .modal-btns { display: flex; gap: 10px; margin-top: 20px; }
        .btn-save { background: var(--primary); color: white; flex: 2; padding: 12px; border-radius: 10px; border: none; font-weight: bold; cursor: pointer; }
        .btn-del { background: #ffebee; color: #f44336; flex: 1; padding: 12px; border-radius: 10px; border: none; font-weight: bold; cursor: pointer; }
        .hidden { display: none; }
        
        /* Lists */
        .preset-item, .search-item, .detail-item, .msg-item { display: flex; justify-content: space-between; align-items: center; background: #f1f5f9; padding: 12px; border-radius: 10px; margin-bottom: 6px; font-size: 0.9rem; border-left: 4px solid transparent; }
        .detail-item.pos { border-left-color: var(--work); } .detail-item.neg { border-left-color: var(--absent); } .detail-item.neutral { border-left-color: var(--primary); }
        .msg-item { border-left-color: #ff9800; background: #fff8e1; flex-direction: column; align-items: flex-start; gap: 8px; }
        .msg-item .msg-body { font-size: 0.9rem; color: #333; }
        .msg-item .msg-footer { width: 100%; display: flex; justify-content: space-between; font-size: 0.7rem; color: #888; border-top: 1px solid rgba(0,0,0,0.05); padding-top: 5px; }

        .del-icon { color: red; font-weight: bold; padding: 5px 10px; cursor: pointer; background: #fff; border-radius: 5px; }
        .d-val { font-weight: bold; direction: ltr; font-family: monospace; font-size: 1rem; }
        .d-val.pos { color: var(--work); } .d-val.neg { color: var(--absent); } .d-val.neutral { color: #666; }
        .details-header { font-weight: bold; margin: 15px 0 10px; color: var(--primary-dark); font-size: 0.95rem; border-bottom: 2px solid #eee; padding-bottom: 5px; }
        .msg-popup-text { font-size: 1rem; color: #333; margin: 15px 0; background: #f9f9f9; padding: 15px; border-radius: 10px; border-right: 4px solid var(--primary); text-align: right; }
    </style>
</head>
<body>

    <div id="loader" class="loading"><div style="width:40px;height:40px;border:4px solid #ddd;border-top-color:var(--primary);border-radius:50%;animation:spin 1s infinite"></div></div>
    
    <div id="legend-toast"></div>

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
                <!-- App Title Dynamic -->
                <h2 id="header-title" class="app-main-title">نظام الحضور الذكي</h2>
                <div class="user-sub-title">مرحباً، <span id="u-name">...</span></div>
            </div>
            <div class="header-actions">
                <button class="action-btn" onclick="window.app.openInbox()">
                    🔔 <span id="msg-badge" class="badge-count">0</span>
                </button>
                <button class="action-btn" onclick="window.app.openSearchModal()">🔍</button>
                <button class="action-btn" id="btn-settings" onclick="window.app.openSettings()">⚙️</button>
                <button class="action-btn logout-btn" onclick="handleLogout()" style="color:#ef4444; background:#fee2e2; border-color:#fca5a5;">↪️</button>
            </div>
        </div>

        <div class="stats-grid">
            <div class="stat-card" onclick="window.app.showDetails('net')"><h4>رصيد الساعات</h4><div class="val" id="st-net">0</div><div class="sub">ميزان (+/- 8س)</div></div>
            <div class="stat-card" onclick="window.app.showDetails('sat')"><h4>رصيد السبت</h4><div class="val" id="st-sat">0</div><div class="sub">عمل (+4) / آخر (-4)</div></div>
            <div class="stat-card" onclick="window.app.showDetails('sunday')"><h4>الأحد والأعياد</h4><div class="val" id="st-sunday">0</div><div class="sub">يوم تعويض</div></div>
            <div class="stat-card" onclick="window.app.showDetails('leave')"><h4>رصيد العطلة</h4><div class="val" id="st-leave">0</div><div class="sub">تراكمي FIFO</div></div>
            <div class="stat-card" onclick="window.app.showDetails('week')"><h4>هذا الأسبوع</h4><div class="val" id="st-week">0</div></div>
            <div class="stat-card" onclick="window.app.showDetails('month')"><h4>هذا الشهر</h4><div class="val" id="st-month">0</div></div>
            <div class="stat-card full-width" onclick="window.app.showDetails('year')"><h4>مجموع السنة</h4><div class="val" id="st-year">0</div></div>
        </div>

        <div class="calendar-box">
            <div class="cal-header">
                <button class="action-btn" onclick="window.app.navMonth(-1)">&#10094;</button>
                <div style="font-weight:bold" id="cal-title"></div>
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
    </div>

    <!-- Modals -->
    
    <!-- Confirm Modal -->
    <div class="modal-overlay" id="confirmModal">
        <div class="modal-content">
            <h3 style="color:#f44336; margin-bottom:10px;">⚠️ تأكيد الحذف</h3>
            <p style="color:#666; margin-bottom:20px;">هل أنت متأكد من مسح تسجيل هذا اليوم نهائياً؟</p>
            <div class="modal-btns">
                <button class="btn-del" onclick="window.app.performDelete()">نعم، احذف</button>
                <button class="btn-save" style="background:#e0e0e0; color:#333;" onclick="document.getElementById('confirmModal').style.display='none'">تراجع</button>
            </div>
        </div>
    </div>

    <!-- Msg Popup -->
    <div class="modal-overlay" id="msgPopup">
        <div class="modal-content">
            <h3 style="color:var(--primary); margin-bottom:10px;">📩 رسالة إدارية</h3>
            <div id="live-msg-content" class="msg-popup-text"></div>
            <button class="btn-save" onclick="window.app.dismissMessage()">قراءة وإخفاء</button>
        </div>
    </div>

    <!-- Inbox -->
    <div class="modal-overlay" id="inboxModal">
        <div class="modal-content">
            <h3 style="text-align:center;">صندوق الرسائل</h3>
            <div id="inbox-list" style="margin-top:15px; max-height:400px; overflow-y:auto;"></div>
            <button class="btn-close-modal" onclick="document.getElementById('inboxModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <!-- Day Edit -->
    <div class="modal-overlay" id="dayModal">
        <div class="modal-content">
            <h3 id="modal-title" style="text-align:center; margin-bottom:20px;"></h3>
            <label class="form-label">نوع النشاط:</label>
            <select id="d-type" class="app-input" onchange="window.app.toggleFields()">
                <option value="work">✅ عمل عادي</option><option value="holiday">🏖️ عطلة سنوية</option><option value="eid">🎉 عيد / مناسبة</option>
                <option value="recup">🔄 استرجاع</option><option value="sick">💊 مرض</option><option value="absent">❌ غياب</option>
            </select>
            <div id="f-holiday" class="hidden" style="background:#e3f2fd; padding:10px; border-radius:8px; margin-bottom:10px;"><label>عدد الأيام:</label><input type="number" id="d-count" class="app-input" value="1" min="1"></div>
            <div id="f-eid" class="hidden" style="background:#f3e5f5; padding:10px; border-radius:8px; margin-bottom:10px;"><input type="text" id="d-eid-name" class="app-input" placeholder="اسم المناسبة"><select id="d-eid-status" class="app-input" onchange="window.app.toggleFields()"><option value="work">عملت</option><option value="rest">عطلة مدفوعة</option></select></div>
            <div id="f-recup" class="hidden"><label>تعويض عن:</label><select id="d-recup-target" class="app-input"></select></div>
            <div id="f-time">
                <label>التوقيت:</label><select id="d-preset" class="app-input" onchange="window.app.applyPreset()" style="margin-bottom:5px;"><option value="manual">-- اختيار توقيت --</option></select>
                <div style="display:flex; gap:10px;"><input type="time" id="d-start" class="app-input"><input type="time" id="d-end" class="app-input"></div>
            </div>
            <div class="modal-btns">
                <button class="btn-save" onclick="window.app.saveDay()">حفظ</button>
                <button class="btn-del" onclick="window.app.askDelete()">مسح</button>
            </div>
            <button class="btn-close-modal" onclick="document.getElementById('dayModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <!-- Search -->
    <div class="modal-overlay" id="searchModal">
        <div class="modal-content">
            <h3 id="search-title" style="text-align:center;">بحث / تفاصيل</h3>
            <div id="search-inputs">
                <label class="form-label">فلترة البحث:</label>
                <div style="display:flex; gap:5px; margin-bottom:10px; flex-wrap: wrap;">
                    <select id="search-month" class="app-input" style="flex:1;" onchange="window.app.performSearch()">
                        <option value="">الأشهر</option>
                        <option value="1">يناير</option><option value="2">فبراير</option><option value="3">مارس</option>
                        <option value="4">أبريل</option><option value="5">مايو</option><option value="6">يونيو</option>
                        <option value="7">يوليو</option><option value="8">أغسطس</option><option value="9">سبتمبر</option>
                        <option value="10">أكتوبر</option><option value="11">نوفمبر</option><option value="12">ديسمبر</option>
                    </select>
                    <select id="search-day-name" class="app-input" style="flex:1;" onchange="window.app.performSearch()">
                        <option value="">الأيام</option><option value="1">الإثنين</option><option value="2">الثلاثاء</option><option value="3">الأربعاء</option><option value="4">الخميس</option><option value="5">الجمعة</option><option value="6">السبت</option><option value="0">الأحد</option>
                    </select>
                    <select id="search-type" class="app-input" style="flex:1; width:100%;" onchange="window.app.performSearch()">
                        <option value="">الحالات</option>
                        <option value="work">✅ عمل</option><option value="holiday">🏖️ عطلة</option><option value="sick">💊 مرض</option>
                        <option value="eid">🎉 أعياد</option><option value="recup">🔄 تعويض</option><option value="absent">❌ غياب</option>
                    </select>
                </div>
            </div>
            <div id="search-results" style="max-height:400px; overflow-y:auto; margin-top:10px;"></div>
            <button class="btn-close-modal" onclick="document.getElementById('searchModal').style.display='none'">إغلاق</button>
        </div>
    </div>

    <!-- Settings -->
    <div class="modal-overlay" id="settingsModal">
        <div class="modal-content">
            <h3 style="text-align:center;">الإعدادات</h3>
            
            <div id="admin-section" style="display:none; margin-bottom:15px;">
                <!-- App Name Customization -->
                <div style="background:#e3f2fd; padding:10px; border-radius:10px; margin-bottom:10px;">
                     <label class="form-label" style="color:#1565c0; font-weight:bold;">اسم البرنامج (للكل):</label>
                     <input type="text" id="p-app-name" class="app-input" placeholder="مثال: شركة النور">
                </div>

                <div style="background:#fff3e0; padding:10px; border-radius:10px; border:1px solid #ffcc80; margin-bottom:10px;">
                    <label class="form-label" style="color:#ef6c00; font-weight:bold;">✉️ إرسال رسالة للموظفين:</label>
                    <textarea id="admin-msg-text" class="app-input" rows="2" placeholder="اكتب الرسالة هنا..."></textarea>
                    <button class="btn-main" style="background:#ff9800; margin-top:0;" onclick="window.app.sendBroadcast()">إرسال للكل</button>
                </div>

                <div style="background:#e3f2fd; padding:10px; border-radius:10px;">
                    <label class="form-label" style="color:#1565c0; font-weight:bold;">إدارة التوقيتات:</label>
                    <div style="display:flex; gap:5px;"><input type="text" id="p-name" class="app-input" placeholder="اسم" style="width:30%"><input type="time" id="p-start" class="app-input" style="width:30%"><input type="time" id="p-end" class="app-input" style="width:30%"></div>
                    <button class="btn-main" onclick="window.app.addPreset()" style="font-size:0.8rem; padding:8px;">+ إضافة</button>
                    <div id="presets-list" class="preset-list" style="margin-top:10px; max-height:100px; overflow-y:auto;"></div>
                </div>
            </div>

            <div style="background:#e8f5e9; padding:10px; border-radius:10px; margin-bottom:15px;">
                <label class="form-label">الاسم الكامل:</label><input type="text" id="s-name" class="app-input" placeholder="اسم الموظف">
                <label class="form-label">تاريخ التحاقي:</label><input type="date" id="s-join" class="app-input">
            </div>
            <div style="background:#fff3e0; padding:10px; border-radius:10px; margin-bottom:15px;">
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

        const firebaseConfig = {
          apiKey: "AIzaSyDhpORuBt8k6YWDLUgRrnqfC8lSS97LexQ",
          authDomain: "sbota-37391.firebaseapp.com",
          projectId: "sbota-37391",
          storageBucket: "sbota-37391.firebasestorage.app",
          messagingSenderId: "1049902061223",
          appId: "1:1049902061223:web:68e7c10c349025ca7ead82",
          measurementId: "G-3B4ESSJWJ9"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);

        // --- GLOBAL EXPORTS ---
        window.showLoader = (s) => document.getElementById('loader').style.display = s?'flex':'none';
        window.showError = (id, msg) => { const el=document.getElementById(id); el.textContent=msg; el.style.display='block'; };
        window.switchView = (id) => {
            document.querySelectorAll('.view-section').forEach(e=>e.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.error-msg,.success-msg').forEach(e=>e.style.display='none');
        };
        window.togglePass = (id) => { const el=document.getElementById(id); el.type = el.type==='password'?'text':'password'; };

        // --- Auth Logic ---
        window.handleLogin = async () => {
            const e = document.getElementById('login-email').value;
            const p = document.getElementById('login-pass').value;
            const rem = document.getElementById('remember-me').checked;
            if(!e || !p) return window.showError('login-error', 'يرجى ملء البيانات');
            window.showLoader(true);
            try {
                await setPersistence(auth, rem ? browserLocalPersistence : browserSessionPersistence);
                const cred = await signInWithEmailAndPassword(auth, e, p);
                if(!cred.user.emailVerified) {
                    await signOut(auth);
                    window.showError('login-error', 'يرجى تفعيل البريد الإلكتروني أولاً');
                    window.showLoader(false);
                }
            } catch(error) {
                window.showLoader(false);
                window.showError('login-error', "بيانات خاطئة أو الحساب غير موجود");
            }
        };

        window.handleSignup = async () => {
            const e = document.getElementById('reg-email').value;
            const p = document.getElementById('reg-pass').value;
            const c = document.getElementById('reg-confirm').value;
            if(!e || !p || !c) return window.showError('reg-error', 'املأ جميع الخانات');
            if(p !== c) return window.showError('reg-error', 'كلمات المرور غير متطابقة');
            if(p.length < 6) return window.showError('reg-error', 'كلمة المرور قصيرة');
            window.showLoader(true);
            try {
                const snap = await getDocs(collection(db, "users"));
                const role = snap.empty ? 'admin' : 'user';
                const cred = await createUserWithEmailAndPassword(auth, e, p);
                await sendEmailVerification(cred.user);
                await setDoc(doc(db, "users", cred.user.uid), { email: e, role: role });
                await setDoc(doc(db, "settings", cred.user.uid), { joinDate: '', fullName: '', adjustments: [], dismissedMsgs: [], deletedMsgs: [] });
                if(role === 'admin') await setDoc(doc(db, "config", "general"), { presets: [{label:'عادي', start:'08:00', end:'16:00'}] });
                await signOut(auth);
                document.getElementById('reg-success').textContent = "تم التسجيل! راجع بريدك للتفعيل.";
                document.getElementById('reg-success').style.display = 'block';
            } catch(err) { window.showError('reg-error', err.code==='auth/email-already-in-use'?'الإيميل مسجل':'خطأ في التسجيل'); } finally { window.showLoader(false); }
        };

        window.handleReset = async () => {
            const e = document.getElementById('reset-email').value;
            if(!e) return window.showError('reset-error', 'أدخل الإيميل');
            window.showLoader(true);
            try {
                await sendPasswordResetEmail(auth, e);
                document.getElementById('reset-msg').textContent = "تم الإرسال، تفقد بريدك";
                document.getElementById('reset-msg').style.display = 'block';
            } catch(err) { window.showError('reset-error', 'تأكد من الإيميل'); } finally { window.showLoader(false); }
        };

        window.handleLogout = async () => { await signOut(auth); window.location.reload(); };

        window.saveData = async (type, data) => {
            const u = auth.currentUser;
            if(!u) return;
            try {
                if(type === 'personal_settings') await setDoc(doc(db, 'settings', u.uid), data, {merge:true});
                else if(type === 'global_config') await setDoc(doc(db, 'config', 'general'), data, {merge:true});
                else if(type === 'events') await setDoc(doc(db, 'attendance', u.uid), {events: data}, {merge:true});
            } catch(e) { console.log(e); }
        };

        window.fbDeleteDay = async (dateKey) => {
            const u = auth.currentUser;
            if(!u) return;
            try {
                await updateDoc(doc(db, 'attendance', u.uid), { [`events.${dateKey}`]: deleteField() });
            } catch(e) { console.error("Error deleting:", e); }
        };

        window.sendAdminMessage = async (text) => {
            const u = auth.currentUser;
            if(!u) return;
            try {
                await addDoc(collection(db, "notifications"), {
                    content: text,
                    createdAt: serverTimestamp(),
                    sender: u.uid
                });
                alert("تم إرسال الرسالة بنجاح!");
            } catch(e) { alert("خطأ في الإرسال"); }
        };

        onAuthStateChanged(auth, async (user) => {
            if(user && user.emailVerified) {
                document.getElementById('auth-overlay').style.display = 'none';
                document.getElementById('app-container').style.display = 'block';
                document.getElementById('u-name').textContent = user.email.split('@')[0];
                window.showLoader(true);
                
                const uDoc = await getDoc(doc(db, 'users', user.uid));
                if(uDoc.exists()) {
                    window.appData.role = uDoc.data().role;
                    if(window.appData.role === 'admin') document.getElementById('admin-section').style.display = 'block';
                }

                // Listeners
                onSnapshot(doc(db, "attendance", user.uid), (doc) => {
                    if(doc.exists()) window.appData.events = doc.data().events || {};
                    window.app.renderCalendar();
                    window.app.checkAutoFill();
                });
                onSnapshot(doc(db, "settings", user.uid), (doc) => {
                    if(doc.exists()) window.appData.personal = doc.data() || {joinDate:'', fullName:'', adjustments:[], dismissedMsgs:[], deletedMsgs:[]};
                    // Ensure arrays exist
                    if(!window.appData.personal.dismissedMsgs) window.appData.personal.dismissedMsgs = [];
                    if(!window.appData.personal.deletedMsgs) window.appData.personal.deletedMsgs = [];
                    
                    const displayName = window.appData.personal.fullName || user.email.split('@')[0];
                    document.getElementById('u-name').textContent = displayName;
                    
                    window.app.calcStats();
                    window.app.checkMessages();
                });
                onSnapshot(doc(db, "config", "general"), (doc) => {
                    if(doc.exists()) {
                        window.appData.global = doc.data() || {presets:[], appName:'نظام الحضور الذكي'};
                        // Update App Name Dynamic
                        if(window.appData.global.appName) {
                            document.title = window.appData.global.appName;
                            document.getElementById('header-title').textContent = window.appData.global.appName;
                            document.getElementById('auth-title-text').textContent = window.appData.global.appName;
                        }
                    }
                });
                
                const q = query(collection(db, "notifications"), orderBy("createdAt", "desc"));
                onSnapshot(q, (snapshot) => {
                    let msgs = [];
                    snapshot.forEach((doc) => msgs.push({ id: doc.id, ...doc.data() }));
                    window.appData.messages = msgs;
                    window.app.checkMessages();
                });

                window.showLoader(false);
            } else {
                if(user) await signOut(auth);
                document.getElementById('auth-overlay').style.display = 'flex';
                document.getElementById('app-container').style.display = 'none';
                window.showLoader(false);
            }
        });
    </script>

    <!-- App Logic -->
    <script>
        const nationalHolidays = { "1-11":"وثيقة الاستقلال","1-14":"رأس السنة الأمازيغية","5-1":"عيد الشغل","7-30":"عيد العرش","8-14":"وادي الذهب","8-20":"ثورة الملك والشعب","8-21":"عيد الشباب","10-31":"عيد الوحدة","11-6":"المسيرة الخضراء","11-18":"عيد الاستقلال","12-9":"عيد الوساطة" };
        const dayNames = ["إثنين", "ثلاثاء", "أربعاء", "خميس", "جمعة", "سبت", "أحد"];
        const monthNames = ["يناير", "فبراير", "مارس", "أبريل", "مايو", "يونيو", "يوليو", "أغسطس", "سبتمبر", "أكتوبر", "نوفمبر", "ديسمبر"];
        let currentDate = new Date(2026, 0, 1);
        let selectedKey = null;
        let activeMsgId = null;
        let deleteType = null;
        let pendingMsgId = null;

        window.appData = {
            role: 'user', events: {}, 
            personal: { joinDate:'', fullName:'', adjustments:[], dismissedMsgs:[], deletedMsgs:[] }, 
            global: { appName:'نظام الحضور الذكي', presets:[{label:'عادي', start:'08:00', end:'16:00'}] },
            messages: []
        };

        window.app = {
            // Messages
            sendBroadcast: () => {
                const txt = document.getElementById('admin-msg-text').value;
                if(!txt) return alert("اكتب رسالة");
                window.sendAdminMessage(txt);
                document.getElementById('admin-msg-text').value = '';
            },

            checkMessages: () => {
                let unreadCount = 0;
                let activePopupMsg = null;
                window.appData.messages.forEach(msg => {
                    if(window.appData.personal.deletedMsgs.includes(msg.id)) return;
                    if(!window.appData.personal.dismissedMsgs.includes(msg.id)) {
                        unreadCount++;
                        if(!activePopupMsg) activePopupMsg = msg;
                    }
                });

                const badge = document.getElementById('msg-badge');
                if(unreadCount > 0) {
                    badge.textContent = unreadCount;
                    badge.style.display = 'flex';
                } else {
                    badge.style.display = 'none';
                }

                if(activePopupMsg) {
                    activeMsgId = activePopupMsg.id;
                    document.getElementById('live-msg-content').textContent = activePopupMsg.content;
                    document.getElementById('msgPopup').style.display = 'flex';
                } else {
                    document.getElementById('msgPopup').style.display = 'none';
                }
            },

            dismissMessage: () => {
                if(activeMsgId) {
                    window.appData.personal.dismissedMsgs.push(activeMsgId);
                    window.saveData('personal_settings', window.appData.personal);
                    document.getElementById('msgPopup').style.display = 'none';
                }
            },

            openInbox: () => {
                const list = document.getElementById('inbox-list');
                list.innerHTML = '';
                const visibleMsgs = window.appData.messages.filter(m => !window.appData.personal.deletedMsgs.includes(m.id));

                if(visibleMsgs.length === 0) list.innerHTML = '<div style="text-align:center; padding:20px; color:#999">لا رسائل</div>';
                else {
                    visibleMsgs.forEach(msg => {
                        let dateStr = "الآن";
                        if(msg.createdAt) dateStr = new Date(msg.createdAt.seconds * 1000).toLocaleDateString('ar-EG');
                        list.innerHTML += `<div class="msg-item"><div class="msg-body">${msg.content}</div><div class="msg-footer"><span>${dateStr}</span><span class="del-icon" onclick="window.app.askDelete('msg', '${msg.id}')">حذف</span></div></div>`;
                    });
                }
                document.getElementById('inboxModal').style.display = 'flex';
            },

            askDelete: (type, id) => {
                deleteType = type || 'day';
                if(type === 'msg') pendingMsgId = id;
                document.getElementById('confirmModal').style.display = 'flex';
            },

            performDelete: () => {
                if(deleteType === 'day') {
                    if(window.appData.events[selectedKey]) {
                        delete window.appData.events[selectedKey];
                        window.fbDeleteDay(selectedKey);
                    }
                    document.getElementById('dayModal').style.display = 'none';
                    window.app.renderCalendar();
                } else if(deleteType === 'msg') {
                    if(!window.appData.personal.deletedMsgs) window.appData.personal.deletedMsgs = [];
                    window.appData.personal.deletedMsgs.push(pendingMsgId);
                    window.saveData('personal_settings', window.appData.personal);
                    window.app.openInbox();
                }
                document.getElementById('confirmModal').style.display = 'none';
            },

            // --- Legend Toast ---
            showLegendToast: (msg) => {
                const toast = document.getElementById('legend-toast');
                toast.textContent = msg;
                toast.classList.add('show-toast');
                setTimeout(() => toast.classList.remove('show-toast'), 3000);
            },

            // Calendar
            checkAutoFill: () => {
                const today = new Date(); today.setHours(0,0,0,0);
                let startCheck = new Date(2026, 0, 1);
                if (today < startCheck) return;
                
                // Get Last Saved Time (Memory Feature)
                let lastStart = '08:00';
                let lastEnd = '16:00';
                if(window.appData.global.presets && window.appData.global.presets.length > 0) {
                    lastStart = window.appData.global.presets[0].start;
                    lastEnd = window.appData.global.presets[0].end;
                }

                let changes = false;
                let loopDate = new Date(startCheck);

                while (loopDate < today) {
                    const k = `${loopDate.getFullYear()}-${String(loopDate.getMonth()+1).padStart(2,'0')}-${String(loopDate.getDate()).padStart(2,'0')}`;
                    const evt = window.appData.events[k];
                    
                    // 1. If exists and is work, update memory
                    if (evt && evt.type === 'work' && evt.start && evt.end) {
                        lastStart = evt.start;
                        lastEnd = evt.end;
                    }
                    // 2. If missing and is work day (Mon-Fri), auto-fill using memory
                    else if (!evt) {
                        const dNum = loopDate.getDay();
                        if (dNum !== 0 && dNum !== 6) {
                            const [h1, m1] = lastStart.split(':').map(Number);
                            const [h2, m2] = lastEnd.split(':').map(Number);
                            let diff = (h2*60+m2) - (h1*60+m1);
                            if(diff < 0) diff += 24*60;
                            const hrs = parseFloat((diff/60).toFixed(2));

                            window.appData.events[k] = { 
                                type: 'work', start: lastStart, end: lastEnd, hours: hrs, autoFilled: true 
                            };
                            changes = true;
                        }
                    }
                    loopDate.setDate(loopDate.getDate() + 1);
                }
                if (changes) window.saveData('events', window.appData.events);
            },

            renderCalendar: () => {
                const grid = document.getElementById('cal-grid');
                grid.innerHTML = '';
                dayNames.forEach(d => grid.innerHTML += `<div class="day-name">${d}</div>`);
                const y = currentDate.getFullYear();
                const m = currentDate.getMonth();
                document.getElementById('cal-title').textContent = `${monthNames[m]} ${y}`;
                let firstDayIndex = new Date(y, m, 1).getDay();
                firstDayIndex = (firstDayIndex === 0) ? 6 : firstDayIndex - 1;
                const daysInMonth = new Date(y, m + 1, 0).getDate();
                for(let i=0; i<firstDayIndex; i++) grid.innerHTML += `<div></div>`;
                
                for(let i=1; i<=daysInMonth; i++) {
                    const key = `${y}-${String(m+1).padStart(2,'0')}-${String(i).padStart(2,'0')}`;
                    const evt = window.appData.events[key];
                    let cls = '', txt = '';
                    
                    const natKey = `${m+1}-${i}`;
                    const isNat = nationalHolidays[natKey];
                    let natClass = '';

                    if(evt) {
                        if(evt.type === 'work') { cls = 'st-work'; txt = ''; } 
                        else if(evt.type === 'holiday') { cls = 'st-holiday'; }
                        else if(evt.type === 'sick') { cls = 'st-sick'; }
                        else if(evt.type === 'absent') { cls = 'st-absent'; }
                        else if(evt.type === 'recup') { cls = 'st-recup'; }
                        else if(evt.type === 'eid') { cls = 'st-eid'; }
                    } else if (isNat) {
                        natClass = 'nat-holiday';
                    }

                    const currentLoopDate = new Date(y, m, i);
                    const now = new Date();
                    now.setHours(0,0,0,0);
                    const isFuture = currentLoopDate.setHours(0,0,0,0) > now.getTime();
                    
                    const isWeekend = (currentLoopDate.getDay() === 0 || currentLoopDate.getDay() === 6);
                    const weekendClass = isWeekend ? 'weekend' : '';
                    
                    const todayClass = (currentLoopDate.getTime() === now.getTime()) ? 'today' : '';
                    
                    const futureClass = isFuture ? 'future' : '';
                    
                    // Allow clicking future IF it is national holiday OR if it is NOT future
                    const isClickable = !isFuture || isNat;
                    const clickAction = isClickable ? `onclick="window.app.openDay('${key}')"` : '';

                    grid.innerHTML += `
                        <div class="day-cell ${todayClass} ${weekendClass} ${natClass} ${cls} ${futureClass}" ${clickAction}>
                            <span>${i}</span>
                        </div>
                    `;
                }
                window.app.calcStats();
            },

            navMonth: (s) => { currentDate.setMonth(currentDate.getMonth() + s); window.app.renderCalendar(); },

            openDay: (key) => {
                const dateObj = new Date(key);
                const hKey = `${dateObj.getMonth()+1}-${dateObj.getDate()}`;
                const natName = nationalHolidays[hKey];
                
                // Allow if not future OR is national holiday
                const today = new Date();
                today.setHours(0,0,0,0);
                if(new Date(key).setHours(0,0,0,0) > today.getTime() && !natName) return;

                selectedKey = key;
                document.getElementById('modal-title').textContent = key;
                document.getElementById('dayModal').style.display = 'flex';
                
                let evt = window.appData.events[key];
                
                // Pre-fill if national holiday and not set
                if (!evt && natName) {
                    evt = { type: 'eid', eidStatus: 'rest', eidName: natName };
                } else if (!evt) {
                    evt = { type: 'work', start: '', end: '', eidStatus: 'work' };
                }

                document.getElementById('d-type').value = evt.type;
                document.getElementById('d-start').value = evt.start || '';
                document.getElementById('d-end').value = evt.end || '';
                document.getElementById('d-eid-name').value = evt.eidName || '';
                document.getElementById('d-count').value = 1;
                
                if(natName && !window.appData.events[key]) document.getElementById('d-eid-status').value = 'rest';
                else document.getElementById('d-eid-status').value = evt.eidStatus || 'work';

                const pre = document.getElementById('d-preset');
                pre.innerHTML = '<option value="manual">-- اختر توقيت --</option>';
                if(window.appData.global.presets) {
                    window.appData.global.presets.forEach((p, i) => { pre.innerHTML += `<option value="${i}">${p.label} (${p.start}-${p.end})</option>`; });
                }
                const rec = document.getElementById('d-recup-target');
                rec.innerHTML = '<option value="">-- اختر يوماً --</option>';
                const used = Object.values(window.appData.events).filter(e => e.type === 'recup').map(e => e.recupTarget);
                for(let k in window.appData.events) {
                    const e = window.appData.events[k];
                    const d = new Date(k);
                    if((d.getDay()===0 && e.type==='work') || (e.type==='eid' && e.eidStatus==='work')) {
                        if(!used.includes(k) || evt.recupTarget === k) {
                            rec.innerHTML += `<option value="${k}" ${evt.recupTarget===k?'selected':''}>${k} (${e.eidName || 'أحد'})</option>`;
                        }
                    }
                }
                window.app.toggleFields();
            },

            toggleFields: () => {
                const t = document.getElementById('d-type').value;
                const es = document.getElementById('d-eid-status').value;
                ['f-holiday', 'f-eid', 'f-recup', 'f-time'].forEach(id => document.getElementById(id).classList.add('hidden'));
                if(t === 'work') document.getElementById('f-time').classList.remove('hidden');
                else if(t === 'holiday') document.getElementById('f-holiday').classList.remove('hidden');
                else if(t === 'recup') document.getElementById('f-recup').classList.remove('hidden');
                else if(t === 'eid') {
                    document.getElementById('f-eid').classList.remove('hidden');
                    if(es === 'work') document.getElementById('f-time').classList.remove('hidden');
                }
            },

            applyPreset: () => {
                const idx = document.getElementById('d-preset').value;
                if(idx !== 'manual') {
                    const p = window.appData.global.presets[idx];
                    document.getElementById('d-start').value = p.start;
                    document.getElementById('d-end').value = p.end;
                }
            },

            saveDay: () => {
                const type = document.getElementById('d-type').value;
                let targetKey = selectedKey;

                if(type === 'holiday') {
                    let count = parseInt(document.getElementById('d-count').value);
                    let loopD = new Date(selectedKey);
                    let added = 0;
                    while(added < count) {
                        if(loopD.getDay() !== 6 && loopD.getDay() !== 0) {
                            const k = `${loopD.getFullYear()}-${String(loopD.getMonth()+1).padStart(2,'0')}-${String(loopD.getDate()).padStart(2,'0')}`;
                            window.appData.events[k] = { type: 'holiday', hours: 0 };
                            added++;
                        }
                        loopD.setDate(loopD.getDate() + 1);
                    }
                } else {
                    let data = { type };
                    if(type === 'work' || (type === 'eid' && document.getElementById('d-eid-status').value === 'work')) {
                        const s = document.getElementById('d-start').value;
                        const e = document.getElementById('d-end').value;
                        if(s && e) {
                            data.start = s; data.end = e;
                            const [h1, m1] = s.split(':').map(Number);
                            const [h2, m2] = e.split(':').map(Number);
                            let diff = (h2*60+m2) - (h1*60+m1);
                            if(s > e) { // Night shift fix
                                diff += 24*60;
                                const currentD = new Date(selectedKey);
                                currentD.setDate(currentD.getDate() + 1);
                                targetKey = `${currentD.getFullYear()}-${String(currentD.getMonth()+1).padStart(2,'0')}-${String(currentD.getDate()).padStart(2,'0')}`;
                            }
                            data.hours = parseFloat((diff/60).toFixed(2));
                        }
                        if(type === 'eid') {
                            data.eidStatus = 'work';
                            data.eidName = document.getElementById('d-eid-name').value;
                        }
                    } else if (type === 'eid') {
                        data.eidStatus = 'rest';
                        data.eidName = document.getElementById('d-eid-name').value;
                    } else if (type === 'recup') {
                        data.recupTarget = document.getElementById('d-recup-target').value;
                    }
                    window.appData.events[targetKey] = data;
                }
                window.saveData('events', window.appData.events);
                document.getElementById('dayModal').style.display = 'none';
            },

            askDelete: (type, id) => {
                deleteType = type || 'day';
                if(type === 'msg') pendingMsgId = id;
                document.getElementById('confirmModal').style.display = 'flex';
            },

            performDelete: () => {
                if(deleteType === 'day') {
                    if(window.appData.events[selectedKey]) {
                        delete window.appData.events[selectedKey];
                        window.fbDeleteDay(selectedKey);
                    }
                    document.getElementById('dayModal').style.display = 'none';
                    window.app.renderCalendar();
                } else if(deleteType === 'msg') {
                    if(!window.appData.personal.deletedMsgs) window.appData.personal.deletedMsgs = [];
                    window.appData.personal.deletedMsgs.push(pendingMsgId);
                    window.saveData('personal_settings', window.appData.personal);
                    window.app.openInbox();
                }
                document.getElementById('confirmModal').style.display = 'none';
            },

            // --- Logic Hub ---
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
                let net = 0, sat = 0, leave = 0, pending = 0;
                let tWeek = 0, tMonth = 0, tYear = 0;
                const yr = currentDate.getFullYear();
                const mth = currentDate.getMonth();
                const today = new Date();
                const weekStart = new Date(today); weekStart.setDate(today.getDate() - today.getDay()); weekStart.setHours(0,0,0,0);
                const weekEnd = new Date(weekStart); weekEnd.setDate(weekStart.getDate() + 6); weekEnd.setHours(23,59,59,999);

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

                for (const [k, evt] of Object.entries(window.appData.events)) {
                    const d = new Date(k);
                    if(d.getFullYear() !== yr) continue;
                    let effective = 0;
                    if(evt.type === 'work' || (evt.type === 'eid' && evt.eidStatus === 'work')) effective = evt.hours;
                    else if(['holiday','sick'].includes(evt.type) || (evt.type === 'eid' && evt.eidStatus === 'rest')) effective = 8;
                    tYear += effective;
                    if(d.getMonth() === mth) tMonth += effective;
                    if(d >= weekStart && d <= weekEnd) tWeek += effective;
                    if(evt.type === 'work' || (evt.type === 'eid' && evt.eidStatus === 'work')) net += (evt.hours - 8);
                    else if(evt.type === 'absent') net -= 8;
                }

                const used = Object.values(window.appData.events).filter(e => e.type === 'recup').map(e => e.recupTarget);
                for(let k in window.appData.events) {
                    const e = window.appData.events[k];
                    const d = new Date(k);
                    if((d.getDay()===0 && e.type==='work') || (e.type==='eid' && e.eidStatus==='work')) {
                        if(!used.includes(k)) pending++;
                    }
                }
                document.getElementById('st-net').innerHTML = `<span class="${net>=0?'txt-green':'txt-red'}">${net.toFixed(1)}</span>`;
                document.getElementById('st-sat').innerHTML = `<span class="${sat>=0?'txt-green':'txt-red'}">${sat}</span>`;
                document.getElementById('st-leave').textContent = leave.toFixed(1);
                document.getElementById('st-sunday').textContent = pending;
                document.getElementById('st-week').textContent = tWeek.toFixed(1);
                document.getElementById('st-month').textContent = tMonth.toFixed(1);
                document.getElementById('st-year').textContent = tYear.toFixed(1);
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
                        if((d.getDay()===0 && evt.type==='work') || (evt.type==='eid' && evt.eidStatus==='work')) {
                            const isComp = used.includes(k);
                            tempList.push({ date: k, note: evt.eidName || 'عمل يوم أحد', val: isComp ? 'تم التعويض' : 'مستحق', type: isComp ? 'neutral' : 'pos' });
                        }
                     }
                } else if (['week', 'month', 'year'].includes(cat)) {
                    for(const [k, evt] of Object.entries(window.appData.events)) {
                        if(new Date(k).getFullYear() === yr) {
                            let h = 0;
                            if(evt.type==='work' || (evt.type==='eid' && evt.eidStatus==='work')) h = evt.hours;
                            else if(['holiday','sick'].includes(evt.type)) h = 8;
                            if(h>0) tempList.push({date:k, note:evt.type, val:h+'س', type:'pos'});
                        }
                    }
                }
                tempList.sort((a,b) => new Date(b.date) - new Date(a.date));
                if(tempList.length === 0) list.innerHTML = '<div style="text-align:center; padding:10px;">لا توجد بيانات</div>';
                tempList.forEach(item => {
                    list.innerHTML += `<div class="detail-item ${item.type}" onclick="window.app.openDay('${item.date}')"><span>${item.date} <small>(${item.note})</small></span><span class="d-val ${item.type}">${item.val}</span></div>`;
                });
                document.getElementById('searchModal').style.display = 'flex';
            },

            openSearchModal: () => {
                document.getElementById('search-inputs').style.display = 'block';
                document.getElementById('search-title').textContent = 'بحث بالأيام';
                document.getElementById('search-results').innerHTML = '';
                document.getElementById('searchModal').style.display = 'flex';
            },

            performSearch: () => {
                const dayVal = document.getElementById('search-day-name').value;
                const typeVal = document.getElementById('search-type').value;
                const monthVal = document.getElementById('search-month').value;
                const list = document.getElementById('search-results');
                list.innerHTML = '';
                if(dayVal === "" && typeVal === "" && monthVal === "") return;
                
                const results = [];
                for(let k in window.appData.events) {
                    const evt = window.appData.events[k];
                    const d = new Date(k);
                    let matchDay = (dayVal === "") || (d.getDay() == parseInt(dayVal));
                    let matchType = (typeVal === "") || (evt.type === typeVal);
                    let matchMonth = (monthVal === "") || (d.getMonth() + 1 == parseInt(monthVal));
                    if(matchDay && matchType && matchMonth) results.push({date:k, ...evt});
                }
                results.sort((a,b) => new Date(b.date) - new Date(a.date));
                if(results.length === 0) list.innerHTML = '<div style="text-align:center; padding:10px;">لا توجد نتائج</div>';
                else {
                    results.forEach(res => {
                        let typeText = { work:'عمل', holiday:'عطلة', sick:'مرض', absent:'غياب', recup:'تعويض', eid:'عيد' }[res.type];
                        if(res.hours) typeText += ` (${res.hours}س)`;
                        list.innerHTML += `<div class="search-item" onclick="window.app.openDay('${res.date}'); document.getElementById('searchModal').style.display='none'"><span>${res.date}</span><span>${typeText}</span></div>`;
                    });
                }
            },

            openSettings: () => {
                document.getElementById('s-join').value = window.appData.personal.joinDate || '';
                document.getElementById('s-name').value = window.appData.personal.fullName || '';
                window.app.renderSettingsLists();
                document.getElementById('settingsModal').style.display = 'flex';
            },

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
                }
            },

            delPreset: (i) => {
                window.appData.global.presets.splice(i, 1);
                window.app.renderSettingsLists();
            },

            addAdj: () => {
                const d = document.getElementById('adj-days').value;
                const r = document.getElementById('adj-note').value;
                if(d) {
                    if(!window.appData.personal.adjustments) window.appData.personal.adjustments = [];
                    window.appData.personal.adjustments.push({amount:d, reason:r});
                    document.getElementById('adj-days').value = '';
                    document.getElementById('adj-note').value = '';
                    window.app.renderSettingsLists();
                }
            },

            delAdj: (i) => {
                window.appData.personal.adjustments.splice(i, 1);
                window.app.renderSettingsLists();
            },

            renderSettingsLists: () => {
                const pl = document.getElementById('presets-list');
                pl.innerHTML = '';
                if(window.appData.global.presets) {
                    window.appData.global.presets.forEach((p, i) => {
                        pl.innerHTML += `<div class="preset-item"><span>${p.label} (${p.start}-${p.end})</span> <span class="del-icon" onclick="window.app.delPreset(${i})">X</span></div>`;
                    });
                }
                const al = document.getElementById('adj-list');
                al.innerHTML = '';
                if(window.appData.personal.adjustments) {
                    window.appData.personal.adjustments.forEach((a, i) => {
                        al.innerHTML += `<div class="preset-item"><span>+${a.amount} (${a.reason})</span> <span class="del-icon" onclick="window.app.delAdj(${i})">X</span></div>`;
                    });
                }
            },

            saveSettings: () => {
                window.appData.personal.joinDate = document.getElementById('s-join').value;
                window.appData.personal.fullName = document.getElementById('s-name').value;
                window.appData.global.appName = document.getElementById('p-app-name').value || window.appData.global.appName;
                
                window.saveData('personal_settings', window.appData.personal);
                if(window.appData.role === 'admin') window.saveData('global_config', window.appData.global);
                document.getElementById('settingsModal').style.display = 'none';
            },

            // --- Legend Toast ---
            showLegendToast: (msg) => {
                const toast = document.getElementById('legend-toast');
                toast.textContent = msg;
                toast.classList.add('show-toast');
                setTimeout(() => toast.classList.remove('show-toast'), 3000);
            }
        };
    </script>
</body>
</html>
