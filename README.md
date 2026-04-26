<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تكميل الوردية اليومي</title>
    
    <style>
        /* إعدادات الصفحة والطباعة النظيفة */
        @page { 
            size: A4 landscape; 
            margin: 10mm; /* هامش متوازن */
        }

        /* منع المتصفح من كتابة اسم الملف والتاريخ في الهوامش */
        @media print {
            header, footer, .no-print { display: none !important; }
            body { -webkit-print-color-adjust: exact; margin: 0; padding: 0; }
            .app-wrapper { border: none !important; width: 100% !important; transform: scale(1); }
        }

        :root { --border: #000; --bg: #fdfdfd; }
        body { font-family: 'Times New Roman', Times, serif; margin: 0; padding: 15px; background-color: #f0f0f0; }
        
        .app-wrapper { 
            background: white; 
            width: 277mm; /* عرض الورقة A4 بالعرض تقريباً */
            margin: auto; 
            padding: 15px; 
            box-sizing: border-box;
            min-height: 190mm;
            border: 1px solid #ccc;
        }

        /* لوحة التحكم العلوية */
        .admin-header { background: #2c3e50; color: white; padding: 12px; border-radius: 5px; margin-bottom: 15px; display: flex; gap: 15px; align-items: center; }
        .admin-header select, .btn-print { padding: 8px 15px; border-radius: 4px; border: none; font-weight: bold; }
        .btn-print { background: #27ae60; color: white; cursor: pointer; }

        /* ترويسة الخطاب */
        .doc-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; border-bottom: 2px solid var(--border); padding-bottom: 10px; }
        .header-side { width: 30%; font-size: 14px; font-weight: bold; line-height: 1.5; }
        .header-center { width: 40%; text-align: center; }
        .header-center h1 { margin: 0; font-size: 26px; text-decoration: underline; }

        /* بيانات المسؤول */
        .info-grid { display: grid; grid-template-columns: 1.5fr 1fr 1fr; gap: 10px; margin-bottom: 15px; }
        .info-cell { border: 1.5px solid var(--border); padding: 10px; background: #fafafa; font-weight: bold; font-size: 15px; }

        /* الجداول - توحيد العرض */
        table { width: 100%; border-collapse: collapse; margin-bottom: 15px; table-layout: fixed; }
        th, td { border: 1.5px solid var(--border); text-align: center; font-weight: bold; padding: 8px; }

        /* جدول الإحصائية - متساوي مع الجدول الرئيسي */
        .stats-table th { background: #f2f2f2; font-size: 14px; }
        .stats-table td { font-size: 16px; height: 35px; }
        .wardia-title { background: #e9ecef; font-size: 18px; padding: 10px; }

        /* جدول الأفراد الرئيسي */
        .main-table th { background: #e9ecef; font-size: 15px; }
        .main-table td { font-size: 14px; height: 38px; }
        .main-table input { width: 100%; border: none; text-align: center; font-size: 14px; font-weight: bold; background: transparent; outline: none; }

        /* التعليمات والختم */
        .instructions { border: 1.5px solid #000; padding: 12px; margin-top: 15px; background: #fff; }
        .instr-head { font-size: 14px; font-weight: bold; text-decoration: underline; margin-bottom: 5px; display: block; }
        .instr-list { margin: 0; padding-right: 25px; font-size: 13px; line-height: 1.6; font-weight: bold; }
        
        .footer-sig { text-align: left; margin-top: 40px; font-weight: bold; font-size: 15px; padding-left: 60px; }
    </style>
</head>
<body>

<div class="admin-header no-print">
    <label>المسؤول:</label>
    <select id="sel_name" onchange="sync()">
        <option value="">-- اختر الاسم --</option>
        <option value="سلطان بن فهد العتيبي">سلطان بن فهد العتيبي</option>
        <option value="محمد بن عبدالله">محمد بن عبدالله</option>
        <option value="خالد الحربي">خالد الحربي</option>
    </select>
    <label>الرتبة:</label>
    <select id="sel_rank" onchange="sync()">
        <option value="عريف">عريف</option>
        <option value="رقيب">رقيب</option>
        <option value="وكيل رقيب">وكيل رقيب</option>
    </select>
    <button class="btn-print" onclick="window.print()">🖨️ طباعة النسخة النهائية</button>
</div>

<div class="app-wrapper">
    <header class="doc-header">
        <div class="header-side">المملكة العربية السعودية<br>القوات الخاصة للأمن الدبلوماسي<br>أمن وحماية الشخصيات</div>
        <div class="header-center">
            <h1>تكميل الوردية اليومي</h1>
            <p style="margin-top:5px;">التاريخ: <span id="date_text"></span> هـ</p>
        </div>
        <div class="header-side" style="text-align: left;">الفترة: ...................</div>
    </header>

    <section class="info-grid">
        <div class="info-cell">الاسم: <span id="view_name">...........................</span></div>
        <div class="info-cell">الرتبة: <span id="view_rank">............</span></div>
        <div class="info-cell">التوقيع: .................................</div>
    </section>

    <table class="stats-table">
        <thead>
            <tr>
                <th colspan="9" class="wardia-title">
                    تكميل الوردية: 
                    <select style="font-size:18px; font-weight:bold; border:none; background:transparent;">
                        <option>الأولى</option><option>الثانية</option><option>الثالثة</option><option selected>الرابعة</option>
                    </select>
                </th>
            </tr>
            <tr>
                <th>إجمالي القوة</th><th>موجود</th><th>خارج</th><th>دورة</th><th>رخصة</th><th>إجازة</th><th>توقيف</th><th>مرضية</th><th>عرضية</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td id="t_c">9</td><td id="s_p">-</td><td id="s_a">-</td><td id="s_c">-</td><td id="s_l">-</td><td id="s_v">-</td><td id="s_ar">-</td><td id="s_s">-</td><td id="s_ca">-</td>
            </tr>
        </tbody>
    </table>

    <table class="main-table">
        <thead>
            <tr>
                <th width="35">م</th><th width="90">الرتبة</th><th width="180">الاسم</th><th width="100">رقم الهوية</th><th width="100">الجوال</th><th width="150">الموقع</th><th width="90">الحالة</th><th>ملاحظات</th>
            </tr>
        </thead>
        <tbody id="staff_rows"></tbody>
    </table>

    <div class="instructions">
        <span class="instr-head">التوجيهات والتعليمات الميدانية الرسمية:</span>
        <ol class="instr-list">
            <li>الالتزام التام بالقيافة العسكرية والمظهر اللائق طوال فترة الاستلام الميداني للوردية.</li>
            <li>يمنع مغادرة الموقع المحدد للفرد إلا ببلاغ رسمي وإذن مسبق من مشرف العمليات.</li>
            <li>التأكد من جاهزية العهدة المسلمة والتبليغ الفوري عن أي ملاحظات رُصدت.</li>
            <li>التعامل الراقي والمهني مع الجمهور ورفع التقارير الميدانية دورياً.</li>
        </ol>
    </div>

    <div class="footer-sig">قايد مجموعة الحراسات: ........................................</div>
</div>

<script>
    const data = [
        { r: "رقيب", n: "محمد بن عبدالله", id: "10233441", p: "050111" },
        { r: "وكيل رقيب", n: "فهد العتيبي", id: "10334452", p: "050222" },
        { r: "عريف", n: "خالد الحربي", id: "10445563", p: "050333" },
        { r: "عريف", n: "سلطان المطيري", id: "10556674", p: "050444" },
        { r: "جندي أول", n: "نايف السبيعي", id: "10667785", p: "050555" },
        { r: "جندي أول", n: "ماجد الدوسري", id: "10778896", p: "050666" },
        { r: "جندي أول", n: "بندر القحطاني", id: "10889907", p: "050777" },
        { r: "جندي", n: "وليد الشمري", id: "10990018", p: "050888" },
        { r: "جندي", n: "صالح الشهري", id: "11001129", p: "050999" }
    ];

    document.getElementById('date_text').innerText = new Date().toLocaleDateString('ar-SA');

    function sync() {
        document.getElementById('view_name').innerText = document.getElementById('sel_name').value || "...........................";
        document.getElementById('view_rank').innerText = document.getElementById('sel_rank').value;
    }

    function init() {
        const tbody = document.getElementById('staff_rows');
        tbody.innerHTML = data.map((s, i) => `
            <tr>
                <td>${i+1}</td><td>${s.r}</td><td align="right" style="padding-right:10px;">${s.n}</td>
                <td>${s.id}</td><td>${s.p}</td><td><input type="text" placeholder="..."></td>
                <td>
                    <select onchange="calc()" class="st-sel" style="width:100%; border:none; font-weight:bold; font-size:12px; background:transparent;">
                        <option value="p">موجود</option><option value="c">دورة</option>
                        <option value="l">رخصة</option><option value="v">إجازة</option>
                        <option value="ar">توقيف</option><option value="s">مرضية</option>
                        <option value="ca">عرضية</option>
                    </select>
                </td>
                <td><input type="text" placeholder="-"></td>
            </tr>
        `).join('');
        calc();
    }

    function calc() {
        const sels = document.querySelectorAll('.st-sel');
        let r = { p:0, c:0, l:0, v:0, ar:0, s:0, ca:0 };
        sels.forEach(s => r[s.value]++);
        const set = (id, v) => document.getElementById(id).innerText = v || "-";
        document.getElementById('t_c').innerText = data.length;
        set('s_p', r.p); set('s_a', data.length - r.p);
        set('s_c', r.c); set('s_l', r.l); set('s_v', r.v);
        set('s_ar', r.ar); set('s_s', r.s); set('s_ca', r.ca);
    }
    window.onload = init;
</script>
</body>
</html>

