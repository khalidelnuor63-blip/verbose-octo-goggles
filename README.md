<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KHALID ELNUOR - نظام الفواتير والعملات والعملاء</title>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --brand-dark: #0b132b;
            --brand-blue: #0066ff;
            --brand-cyan: #00d2ff;
            --brand-accent: #1c2541;
            --success-color: #10b981;
            --pending-color: #f59e0b;
            --danger-color: #ef4444;
            --bg-color: #f8fafc;
            --sidebar-width: 270px;
        }

        * { box-sizing: border-box; }

        body {
            font-family: 'Tajawal', sans-serif;
            background-color: var(--bg-color);
            margin: 0;
            display: flex;
            height: 100vh;
            color: #334155;
            overflow: hidden;
        }

        /* القائمة الجانبية */
        .sidebar {
            width: var(--sidebar-width);
            background: linear-gradient(180deg, var(--brand-dark), var(--brand-accent));
            color: white;
            padding: 25px 15px;
            display: flex;
            flex-direction: column;
            box-shadow: 4px 0 15px rgba(0,0,0,0.1);
            z-index: 10;
        }

        .brand-logo {
            text-align: center;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            margin-bottom: 25px;
        }

        .brand-logo h2 {
            margin: 0;
            font-size: 22px;
            font-weight: 800;
            background: linear-gradient(90deg, #fff, var(--brand-cyan));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .brand-logo p {
            margin: 5px 0 0;
            font-size: 10px;
            color: #94a3b8;
            letter-spacing: 1.5px;
        }

        .nav-menu { list-style: none; padding: 0; margin: 0; }

        .nav-item {
            padding: 14px 16px;
            border-radius: 8px;
            cursor: pointer;
            margin-bottom: 10px;
            font-weight: 500;
            display: flex;
            align-items: center;
            gap: 12px;
            transition: all 0.2s ease;
            color: #cbd5e1;
        }

        .nav-item:hover, .nav-item.active {
            background-color: var(--brand-blue);
            color: white;
            font-weight: 700;
        }

        /* المنطقة الرئيسية */
        .main-content { flex: 1; padding: 25px; overflow-y: auto; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }

        .page-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            background: white;
            padding: 16px 20px;
            border-radius: 10px;
            border: 1px solid #e2e8f0;
        }

        .page-header h2 { margin: 0; color: var(--brand-dark); font-size: 20px; }

        .table-card {
            background: white;
            border-radius: 10px;
            border: 1px solid #e2e8f0;
            overflow: hidden;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05);
        }

        table { width: 100%; border-collapse: collapse; text-align: right; font-size: 14px; }
        th { background-color: #f1f5f9; color: var(--brand-dark); padding: 14px 15px; font-weight: 700; border-bottom: 2px solid #e2e8f0; }
        td { padding: 12px 15px; border-bottom: 1px solid #e2e8f0; }
        tr:hover { background-color: #f8fafc; }

        .badge {
            padding: 4px 10px;
            border-radius: 15px;
            color: white;
            font-size: 12px;
            font-weight: bold;
            display: inline-block;
        }

        .status-paid { background-color: var(--success-color); }
        .status-pending { background-color: var(--pending-color); }

        .grid-2 { display: grid; grid-template-columns: 1.1fr 0.9fr; gap: 20px; }
        .card { background: white; padding: 22px; border-radius: 10px; border: 1px solid #e2e8f0; }
        .form-group { margin-bottom: 14px; }
        label { display: block; margin-bottom: 6px; font-weight: 700; font-size: 13px; color: var(--brand-dark); }
        input, select { width: 100%; padding: 10px; border: 1px solid #cbd5e1; border-radius: 6px; font-family: inherit; font-size: 14px; }

        .btn { padding: 10px 16px; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; transition: 0.2s; font-family: inherit; }
        .btn-primary { background-color: var(--brand-blue); color: white; }
        .btn-danger { background-color: var(--danger-color); color: white; }
        .btn-sm { padding: 6px 12px; font-size: 12px; }

        /* نموذج معاينة الفاتورة */
        .invoice-box {
            border: 2px solid var(--brand-dark);
            padding: 22px;
            background: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.03);
        }
        .invoice-brand-header { text-align: center; border-bottom: 2px solid var(--brand-dark); padding-bottom: 12px; margin-bottom: 15px; }
        .brand-title { font-size: 20px; font-weight: 800; color: var(--brand-dark); letter-spacing: 0.5px; }
        .brand-subtitle { font-size: 10px; color: var(--brand-blue); font-weight: 700; letter-spacing: 1px; }
        .invoice-title-badge { background: var(--brand-dark); color: white; padding: 5px 18px; display: inline-block; margin-top: 10px; font-size: 15px; font-weight: bold; border-radius: 4px; }
        .divider { border-top: 1px dashed #cbd5e1; margin: 12px 0; }
        .double-divider { border-top: 3px double var(--brand-dark); margin: 15px 0 10px; }
        .row { display: flex; justify-content: space-between; margin: 8px 0; font-size: 14px; }
        .total-box { background: #f1f5f9; padding: 12px; text-align: center; font-size: 19px; font-weight: 800; border: 2px solid var(--brand-dark); margin-top: 12px; border-radius: 6px; color: var(--brand-dark); }

        /* الطباعة والتصدير */
        @media print {
            .sidebar, .page-header, .no-print { display: none !important; }
            .main-content { padding: 0; overflow: visible; }
            .grid-2 { display: block; }
            .invoice-card-container { display: block; width: 100%; }
            .invoice-box { border: 2px solid #000; }
        }
    </style>
</head>
<body>

    <!-- الشريط الجانبي -->
    <div class="sidebar">
        <div class="brand-logo">
            <h2>KHALID ELNUOR</h2>
            <p>AI AUTOMATION & DIGITAL SOLUTIONS</p>
        </div>
        <ul class="nav-menu">
            <li class="nav-item active" onclick="switchTab('create-tab', this)">
                <span>📄</span> إصدار فاتورة جديدة
            </li>
            <li class="nav-item" onclick="switchTab('invoices-tab', this)">
                <span>📂</span> جميع الفواتير الصادرة
            </li>
            <li class="nav-item" onclick="switchTab('customers-tab', this)">
                <span>👥</span> كشف وحسابات العملاء
            </li>
        </ul>
    </div>

    <!-- المحتوى الرئيسي -->
    <div class="main-content">
        
        <!-- التبويب 1: إصدار الفاتورة والمعاينة -->
        <div id="create-tab" class="tab-content active">
            <div class="page-header">
                <h2>إصدار فاتورة سداد جديدة (جميع العملات)</h2>
            </div>
            <div class="grid-2">
                <div class="card no-print">
                    <form onsubmit="saveAndPrint(event)">
                        <div class="form-group">
                            <label>اسم العميل:</label>
                            <input type="text" id="inputName" value="أحمد محمد علي" required oninput="updatePreview()">
                        </div>
                        <div class="form-group">
                            <label>رقم الحساب (الكود):</label>
                            <input type="text" id="inputAcc" value="DF-1001" required oninput="updatePreview()">
                        </div>
                        <div class="form-group">
                            <label>حالة الفاتورة:</label>
                            <select id="inputStatus" onchange="updatePreview()">
                                <option value="Pending">مستحقة الدفع (Pending)</option>
                                <option value="Paid">مكتملة الدفع (Paid)</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>العملة الأجنبية:</label>
                            <select id="inputCurrency" onchange="updatePreview()">
                                <option value="ARS" data-symbol="ARS (بيزو أرجنتيني)" selected>بيزو أرجنتيني (ARS)</option>
                                <option value="USD" data-symbol="$ USD (دولار أمريكي)">دولار أمريكي (USD)</option>
                                <option value="EUR" data-symbol="€ EUR (يورو)">يورو (EUR)</option>
                                <option value="SAR" data-symbol="SAR (ريال سعودي)">ريال سعودي (SAR)</option>
                                <option value="AED" data-symbol="AED (درهم إماراتي)">درهم إماراتي (AED)</option>
                                <option value="QAR" data-symbol="QAR (ريال قطري)">ريال قطري (QAR)</option>
                                <option value="EGP" data-symbol="EGP (جنيه مصري)">جنيه مصري (EGP)</option>
                                <option value="GBP" data-symbol="£ GBP (جنيه إسترليني)">جنيه إسترليني (GBP)</option>
                                <option value="TRY" data-symbol="₺ TRY (ليرة تركية)">ليرة تركية (TRY)</option>
                                <option value="CAD" data-symbol="C$ CAD (دولار كندي)">دولار كندي (CAD)</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label id="lblForeignAmount">المبلغ بالعملة الأجنبية:</label>
                            <input type="number" id="inputForeign" value="112000" step="any" oninput="updatePreview()">
                        </div>
                        <div class="form-group">
                            <label>المبلغ المعادل بالدولار ($ USD):</label>
                            <input type="number" id="inputUSD" value="100" step="any" oninput="updatePreview()">
                        </div>
                        <div class="form-group">
                            <label id="lblRate">سعر الصرف (1 دولار = X جنيه سوداني):</label>
                            <input type="number" id="inputRate" value="8940" step="any" oninput="updatePreview()">
                        </div>
                        <button type="submit" class="btn btn-primary" style="width: 100%; margin-top: 10px; padding: 12px;">حفظ الفاتورة والطباعة المباشرة</button>
                    </form>
                </div>

                <div class="invoice-card-container">
                    <div class="invoice-box" id="printArea">
                        <div class="invoice-brand-header">
                            <div class="brand-title">KHALID ELNUOR</div>
                            <div class="brand-subtitle">AI AUTOMATION & DIGITAL SOLUTIONS</div>
                            <div class="invoice-title-badge">فــــاتــورة ســــداد</div>
                        </div>

                        <div class="row">
                            <span>اسم العميل:</span>
                            <strong id="viewName">---</strong>
                        </div>
                        <div class="row">
                            <span>رقم الحساب:</span>
                            <strong id="viewAcc">ACC-DF-1001</strong>
                        </div>
                        <div class="row">
                            <span>حالة الفاتورة:</span>
                            <span id="viewStatus" class="badge status-pending">Pending</span>
                        </div>

                        <div class="divider"></div>

                        <div class="row">
                            <span>الخدمة / البيان:</span>
                            <strong>تجديد اشتراك خدمة ستارلينك الشهري</strong>
                        </div>
                        <div class="row">
                            <span>المبلغ بالعملة الأصلية:</span>
                            <span id="viewForeign">112,000.00 ARS</span>
                        </div>

                        <div class="divider"></div>

                        <div class="row">
                            <span>المبلغ بالدولار:</span>
                            <span id="viewUSD">$100.00 USD</span>
                        </div>
                        <div class="row">
                            <span>سعر الصرف المعتمد:</span>
                            <span id="viewRate">1 دولار = 8,940 جنيه</span>
                        </div>

                        <div class="divider"></div>

                        <p style="margin: 6px 0 2px; font-weight: bold;">المبلغ الإجمالي المستحق:</p>
                        <div class="total-box">
                            ▶ <span id="viewTotal">894,000</span> جنيه سوداني
                        </div>

                        <div class="double-divider"></div>
                        <div style="text-align: center; font-size: 12px; color: #475569; font-weight: bold;">
                            شكراً لتعاملكم مع KHALID ELNUOR Solutions
                        </div>
                        <div class="double-divider"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- التبويب 2: جميع الفواتير الصادرة -->
        <div id="invoices-tab" class="tab-content">
            <div class="page-header">
                <h2>جميع الفواتير الصادرة</h2>
                <input type="text" id="searchInvoice" placeholder="بحث باسم العميل أو رقم الحساب..." style="width: 280px;" onkeyup="renderInvoices()">
            </div>
            <div class="table-card">
                <table>
                    <thead>
                        <tr>
                            <th>رقم الفاتورة</th>
                            <th>التاريخ</th>
                            <th>رقم الحساب</th>
                            <th>اسم العميل</th>
                            <th>المبلغ بالعملة</th>
                            <th>المبلغ ($)</th>
                            <th>الإجمالي (SDG)</th>
                            <th>الحالة</th>
                            <th>إجراءات</th>
                        </tr>
                    </thead>
                    <tbody id="invoicesTableBody"></tbody>
                </table>
            </div>
        </div>

        <!-- التبويب 3: كشف وحسابات العملاء -->
        <div id="customers-tab" class="tab-content">
            <div class="page-header">
                <h2>كشف وحسابات العملاء المجمعة</h2>
            </div>
            <div class="table-card">
                <table>
                    <thead>
                        <tr>
                            <th>رقم الحساب</th>
                            <th>اسم العميل</th>
                            <th>عدد الفواتير</th>
                            <th>إجمالي المدفوعات (SDG)</th>
                            <th>المبالغ المعلقة (SDG)</th>
                            <th>إجراءات</th>
                        </tr>
                    </thead>
                    <tbody id="customersTableBody"></tbody>
                </table>
            </div>
        </div>

    </div>

    <script>
        // التخزين المحلي للفواتير
        let invoicesData = JSON.parse(localStorage.getItem('khalid_invoices')) || [
            { id: 'INV-1001', date: '2026-10-08', acc: 'ACC-DF-1001', name: 'أحمد محمد علي', currency: 'ARS', foreignAmount: 112000, usd: 100, rate: 8940, total: 894000, status: 'Pending' }
        ];

        // التنقل بين الشاشات
        function switchTab(tabId, element) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
            element.classList.add('active');

            if(tabId === 'invoices-tab') renderInvoices();
            if(tabId === 'customers-tab') renderCustomers();
        }

        // تحديث المعاينة الحية للفاتورة
        function updatePreview() {
            const name = document.getElementById('inputName').value;
            const acc = document.getElementById('inputAcc').value;
            const status = document.getElementById('inputStatus').value;
            const currencySelect = document.getElementById('inputCurrency');
            const currencyCode = currencySelect.value;
            const currencyText = currencySelect.options[currencySelect.selectedIndex].getAttribute('data-symbol');
            
            const foreignAmount = parseFloat(document.getElementById('inputForeign').value) || 0;
            const usd = parseFloat(document.getElementById('inputUSD').value) || 0;
            const rate = parseFloat(document.getElementById('inputRate').value) || 0;
            
            const total = usd * rate; // حساب السعر بالجنيه بناء على الدولار وسعر الصرف

            document.getElementById('lblForeignAmount').innerText = `المبلغ بـ (${currencyCode}):`;
            document.getElementById('lblRate').innerText = `سعر الصرف (1 دولار USD = X جنيه سوداني):`;

            document.getElementById('viewName').innerText = name;
            document.getElementById('viewAcc').innerText = 'ACC-' + acc;
            document.getElementById('viewForeign').innerText = `${foreignAmount.toLocaleString('en-US', {minimumFractionDigits: 2})} ${currencyText}`;
            document.getElementById('viewUSD').innerText = `$${usd.toLocaleString('en-US', {minimumFractionDigits: 2})} USD`;
            document.getElementById('viewRate').innerText = `1 دولار = ${rate.toLocaleString('ar-SD')} جنيه`;
            document.getElementById('viewTotal').innerText = total.toLocaleString('ar-SD');

            const statusElem = document.getElementById('viewStatus');
            statusElem.innerText = status === 'Paid' ? 'مكتملة الدفع (Paid)' : 'مستحقة الدفع (Pending)';
            statusElem.className = status === 'Paid' ? 'badge status-paid' : 'badge status-pending';
        }

        // حفظ الفاتورة وطباعتها
        function saveAndPrint(e) {
            e.preventDefault();
            const currencyCode = document.getElementById('inputCurrency').value;
            const foreignAmount = parseFloat(document.getElementById('inputForeign').value) || 0;
            const usd = parseFloat(document.getElementById('inputUSD').value) || 0;
            const rate = parseFloat(document.getElementById('inputRate').value) || 0;
            
            const newInvoice = {
                id: 'INV-' + Math.floor(1000 + Math.random() * 9000),
                date: new Date().toISOString().split('T')[0],
                acc: 'ACC-' + document.getElementById('inputAcc').value,
                name: document.getElementById('inputName').value,
                currency: currencyCode,
                foreignAmount: foreignAmount,
                usd: usd,
                rate: rate,
                total: usd * rate,
                status: document.getElementById('inputStatus').value
            };

            invoicesData.unshift(newInvoice);
            localStorage.setItem('khalid_invoices', JSON.stringify(invoicesData));
            window.print();
        }

        // عرض جدول الفواتير الصادرة
        function renderInvoices() {
            const tbody = document.getElementById('invoicesTableBody');
            const keyword = document.getElementById('searchInvoice').value.toLowerCase();
            tbody.innerHTML = '';

            const filtered = invoicesData.filter(inv => 
                inv.name.toLowerCase().includes(keyword) || inv.acc.toLowerCase().includes(keyword)
            );

            filtered.forEach((inv, index) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td><strong>${inv.id}</strong></td>
                    <td>${inv.date}</td>
                    <td>${inv.acc}</td>
                    <td>${inv.name}</td>
                    <td>${(inv.foreignAmount || 0).toLocaleString()} ${inv.currency || 'USD'}</td>
                    <td>$${(inv.usd || 0).toLocaleString()}</td>
                    <td><strong>${(inv.total || 0).toLocaleString()} SDG</strong></td>
                    <td><span class="badge ${inv.status === 'Paid' ? 'status-paid' : 'status-pending'}">${inv.status}</span></td>
                    <td>
                        <button class="btn btn-primary btn-sm" onclick="toggleStatus(${index})">تغيير الحالة</button>
                        <button class="btn btn-danger btn-sm" onclick="deleteInvoice(${index})">حذف</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        // تغيير حالة الفاتورة
        function toggleStatus(index) {
            invoicesData[index].status = invoicesData[index].status === 'Paid' ? 'Pending' : 'Paid';
            localStorage.setItem('khalid_invoices', JSON.stringify(invoicesData));
            renderInvoices();
        }

        // حذف فاتورة
        function deleteInvoice(index) {
            if(confirm('هل أنت تأكد من حذف هذه الفاتورة؟')) {
                invoicesData.splice(index, 1);
                localStorage.setItem('khalid_invoices', JSON.stringify(invoicesData));
                renderInvoices();
            }
        }

        // عرض جدول العملاء
        function renderCustomers() {
            const tbody = document.getElementById('customersTableBody');
            tbody.innerHTML = '';
            const customerMap = {};

            invoicesData.forEach(inv => {
                if (!customerMap[inv.acc]) {
                    customerMap[inv.acc] = { name: inv.name, acc: inv.acc, count: 0, paidTotal: 0, pendingTotal: 0 };
                }
                customerMap[inv.acc].count++;
                if (inv.status === 'Paid') customerMap[inv.acc].paidTotal += (inv.total || 0);
                else customerMap[inv.acc].pendingTotal += (inv.total || 0);
            });

            Object.values(customerMap).forEach(cust => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td><strong>${cust.acc}</strong></td>
                    <td>${cust.name}</td>
                    <td>${cust.count} فاتورة</td>
                    <td style="color: var(--success-color); font-weight: bold;">${cust.paidTotal.toLocaleString()} SDG</td>
                    <td style="color: var(--pending-color); font-weight: bold;">${cust.pendingTotal.toLocaleString()} SDG</td>
                    <td><button class="btn btn-primary btn-sm" onclick="filterCustomerInvoices('${cust.acc}')">عرض الفواتير</button></td>
                `;
                tbody.appendChild(tr);
            });
        }

        // التصفية للعميل المحدد
        function filterCustomerInvoices(acc) {
            document.getElementById('searchInvoice').value = acc;
            switchTab('invoices-tab', document.querySelectorAll('.nav-item')[1]);
        }

        // التهيئة الابتدائية
        updatePreview();
    </script>
</body>
</html>
