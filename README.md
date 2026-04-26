<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تكميل الوردية اليومي - نسخة محلية</title>
    
    <style>
        /* إعدادات الطباعة للآيفون لضمان عدم القص */
        @page { 
            size: A4 landscape; 
            margin: 3mm; /* تقليل الهوامش لإعطاء مساحة أكبر */
        }
        
        :root { --primary: #2c3e50; --border: #000; --bg: #f8f9fa; }

        body { font-family: 'Times New Roman', Times, serif; margin: 0; padding: 5px; background-color: #eee; }
        
        .app-wrapper { 
            background: white; 
            max-width: 100%; /* جعل العرض مرن */
            margin: auto; 
            padding: 10px; 
            box-sizing: border-box;
        }

        /* لوحة التحكم */
        .admin-header { background: var(--primary); color: white; padding: 10px; border-radius: 5px; margin-bottom: 8px; display: flex; gap: 15px; align-items: center; }
        .admin-header select { padding: 4px; border-radius: 3px; font-weight: bold; border: none; }
        .btn-print { background: #27ae60; color: white; border: none; padding: 7px 20px; border-radius: 4px; cursor: pointer; font-weight: bold; }

        /* الترويسة */
        .doc-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid var(--border); padding-bottom: 5px; margin-bottom: 8px; }
        .header-side { width: 30%; font-size: 10px; font-weight: bold; line-height: 1.2; }
        .header-center { width: 40%; text-align: center; }
        .header-center h1 { margin: 0; font-size: 18px; text-decoration: underline; }

        /* شريط المعلومات */
        .info-grid { display: grid; grid-template-columns: 1.5fr 1fr 1fr; gap: 5px; margin-bottom: 8px; }
        .info-cell { border: 1px solid var(--border); padding: 5px 8px; background: var(--bg); font-weight: bold; font-size: 11px; }

        /* الجداول */
        table { width: 100%; border-collapse: collapse; margin-bottom: 5px; table-layout: fixed; } /* تثبيت عرض الجدول */
        th, td { border: 1px solid var(--border); text-align: center; font-weight: bold; overflow: hidden; word-wrap: break-word; }
        
        .stats-table th { background: #eee; font-size: 9px; padding: 2px; }
        .stats-table td { font-size: 10px; height: 18px; }
        .wardia-row { background: #f0f0f0; font-size: 12px; padding: 4px; }

        /* جدول القوة */
        .main-table th { background: #e6e6e6; font-size: 10px; padding: 4px; }
        .main-table td { font-size: 9.5px; height: 20px; }
        .main-table input { width: 100%; border: none; text-align: center; font-size: 9px; outline: none; background: transparent; }

        /* التعليمات */
        .instructions-box { border: 1px solid #ddd; padding: 4px; background: #fff; margin-top: 5px; }
        .instr-head { font-size: 7.5px; font-weight: bold; text-decoration: underline; margin-bottom: 2px; display: block; }
        .instr-list { margin: 0; padding-right: 12px; font-size: 7px; line-height: 1.1; color: #444; }

        @media print {
            .no-print { display: none !important; }
            body { background: white; padding: 0; }
            /* كود تصغير المحتوى ليتناسب مع الصفحة */
            .app-wrapper { 
                transform: scale(0.98); 
                transform-origin: top center;
                width: 100% !important;
            }
            select { appearance: none; -webkit-appearance: none; border: none !important; }
        }
    </style>
</head>
<body>

<div class="admin-header no-print">
    <label>الاسم:</label>
    <select id="sel_name" onchange="sync()">
        <option value="">-- اختر الاسم --</option>
        <option value="أحمد بن صالح المحمدي">أحمد بن صالح المحمدي</option>
        <option value="خالد بن وليد الشمري">خالد بن وليد الشمري</option>
        <option value="سلطان بن فهد العتيبي">سلطان بن فهد العتيبي</option>
        <option value="بندر بن نايف الحربي">بندر بن نايف الحربي</option>
        <option value="محمد بن عبدالله القحطاني">محمد بن عبدالله القحطاني</option>
        <option value="فيصل بن جابر الدوسري">فيصل بن جابر الدوسري</option>
    </select>

    <label>الرتبة:</label>
    <select id="sel_rank" onchange="sync()">
        <option value="رقيب">رقيب</option>
        <option value="وكيل رقيب">وكيل رقيب</option>
        <option value="عريف">عريف</option>
        <option value="جندي أول">جندي أول</option>
    </select>

    <button class="btn-print" onclick="window.print()">🖨️ طباعة التكميل</button>
</div>

<div class="app-wrapper">
    <header class="doc-header">
        <div class="header-side">المملكة العربية السعودية<br>القوات الخاصة للامن الدبلماسي <br> امن وحماية الشخصيات</div>
        <div class="header-center">
            <h1>تكميل الوردية اليومي</h1>
            <p style="font-size: 9px; margin-top: 3px;">التاريخ: <span id="date_text"></span></p>
        </div>
        <div class="header-side" style="text-align: left;">الفترة: ...........</div>
    </header>

    <section class="info-grid">
        <div class="info-cell">الاسم: <span id="view_name">...........................</span></div>
        <div class="info-cell">الرتبة: <span id="view_rank">............</span></div>
        <div class="info-cell">التوقيع: .................................</div>
    </section>

    <table class="stats-table">
        <thead>
            <tr>
                <th colspan="9" class="wardia-row">
                    تكميل الوردية: 
                    <select style="font-family: inherit; font-weight: bold; border: none; background: transparent; font-size: 13px;">
                        <option>الأولى</option>
                        <option>الثانية</option>
                        <option>الثالثة</option>
                        <option>الرابعة</option>
                        <option>الخامسة</option>
                    </select>
                </th>
            </tr>
            <tr>
                <th>إجمالي القوة</th><th>النازل</th><th>غير نازل</th><th>دورة</th><th>رخصة</th><th>إجازة</th><th>توقيف</th><th>مرضية</th><th>عرضية</th>
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
                <th width="25">م</th><th width="160">الرتبة والاسم</th><th width="85">رقم الهوية</th><th width="85">الجوال</th><th width="150">الموقع </th><th width="75">الحالة</th><th>ملاحظات</th>
            </tr>
        </thead>
        <tbody id="staff_rows"></tbody>
    </table>

    <footer class="instructions-box">
        <span class="instr-head">التوجيهات والتعليمات الميدانية الرسمية:</span>
        <ol class="instr-list">
            <li>الالتزام التام بالقيافة العسكرية والمظهر اللائق طوال فترة الاستلام الميداني للوردية.</li>
            <li>يمنع مغادرة الموقع المحدد للفرد إلا ببلاغ رسمي وإذن مسبق من مشرف العمليات.</li>
            <li>التأكد من جاهزية العهدة المسلمة والتبليغ الفوري عن أي ملاحظات رُصدت.</li>
            <li>التعامل الراقي والمهني مع الجمهور ورفع التقارير الميدانية دورياً.</li>
        </ol>
    </footer>

    <div style="text-align: left; margin-top: 25px; font-weight: bold; font-size: 10px; padding-left: 40px;">
        قايد مجموعة الحراسات : ........................................
    </div>
</div>

<script>
    const data = [
        { n: "رقيب/ محمد بن عبدالله", id: "10233441", p: "050111" },
        { n: "وكيل رقيب/ فهد العتيبي", id: "10334452", p: "050222" },
        { n: "عريف/ خالد الحربي", id: "10445563", p: "050333" },
        { n: "عريف/ سلطان المطيري", id: "10556674", p: "050444" },
        { n: "جندي أول/ نايف السبيعي", id: "10667785", p: "050555" },
        { n: "جندي أول/ ماجد الدوسري", id: "10778896", p: "050666" },
        { n: "جندي أول/ بندر القحطاني", id: "10889907", p: "050777" },
        { n: "جندي/ وليد الشمري", id: "10990018", p: "050888" },
        { n: "جندي/ صالح الشهري", id: "11001129", p: "050999" }
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
                <td>${i+1}</td>
                <td align="right"><b>${s.n}</b></td>
                <td>${s.id}</td>
                <td>${s.p}</td>
                <td><input type="text" placeholder="..."></td>
                <td>
                    <select onchange="calc()" class="st-sel" style="width:100%; border:none; font-weight:bold; font-size:9px; background:transparent;">
                        <option value="p">حاضر</option><option value="c">دورة</option>
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

