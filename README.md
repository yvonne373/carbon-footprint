<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>一日生活碳足跡盤查表</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang TC", "Microsoft JhengHei", sans-serif;
      background-color: #faf5f0;
      color: #2d3748;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 24px 16px;
    }
    .container {
      background: #ffffff;
      width: 100%;
      max-width: 520px;
      padding: 24px;
      border-radius: 18px;
      box-shadow: 0 8px 24px rgba(180, 83, 9, 0.08);
    }
    header {
      text-align: center;
      margin-bottom: 20px;
    }
    h1 {
      font-size: 22px;
      color: #b45309;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }
    .subtitle {
      font-size: 13px;
      color: #78350f;
      margin-top: 4px;
    }
    .input-card {
      background: #fffbeb;
      border: 1.5px solid #fde68a;
      padding: 16px;
      border-radius: 14px;
      margin-bottom: 20px;
    }
    .form-group {
      margin-bottom: 14px;
    }
    label {
      display: block;
      font-size: 14px;
      font-weight: bold;
      color: #374151;
      margin-bottom: 6px;
    }
    select, input {
      width: 100%;
      padding: 12px;
      font-size: 15px;
      border: 1.5px solid #d1d5db;
      border-radius: 8px;
      background: #ffffff;
      outline: none;
    }
    select:focus, input:focus {
      border-color: #d97706;
    }
    .btn-add {
      width: 100%;
      padding: 13px;
      font-size: 15px;
      font-weight: bold;
      color: #ffffff;
      background-color: #b45309;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.2s;
    }
    .btn-add:hover {
      background-color: #92400e;
    }

    /* 總排碳儀表板 */
    .dashboard {
      background: #fff7ed;
      border: 1.5px solid #ffedd5;
      border-radius: 14px;
      padding: 16px;
      margin-bottom: 20px;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      text-align: center;
    }
    .card-stat.full-width {
      grid-column: span 2;
      padding-bottom: 10px;
      border-bottom: 1px dashed #fed7aa;
    }
    .stat-title {
      font-size: 13px;
      font-weight: 600;
      color: #c2410c;
      margin-bottom: 4px;
    }
    .stat-value {
      font-size: 26px;
      font-weight: 800;
      color: #9a3412;
    }
    .unit {
      font-size: 12px;
      font-weight: normal;
      color: #7c2d12;
    }

    /* 歷史清單 */
    .list-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      padding: 0 4px;
    }
    .list-header h3 {
      font-size: 15px;
      color: #374151;
    }
    .btn-clear {
      background: none;
      border: none;
      color: #dc2626;
      font-size: 13px;
      cursor: pointer;
      text-decoration: underline;
    }
    .record-list {
      list-style: none;
      max-height: 200px;
      overflow-y: auto;
    }
    .record-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 12px;
      background: #fdfdfd;
      border: 1px solid #e5e7eb;
      border-radius: 8px;
      margin-bottom: 8px;
      font-size: 14px;
    }
    .record-name {
      font-weight: 600;
      color: #1f2937;
    }
    .record-carbon {
      color: #c2410c;
      font-weight: bold;
      margin-right: 8px;
    }
    .btn-del {
      background: #fee2e2;
      border: none;
      color: #dc2626;
      border-radius: 4px;
      padding: 3px 6px;
      font-size: 12px;
      cursor: pointer;
    }
    .empty-tip {
      text-align: center;
      color: #9ca3af;
      font-size: 13px;
      padding: 18px 0;
    }

    /* AI倫理宣告 */
    footer {
      margin-top: 24px;
      font-size: 11px;
      color: #6b7280;
      text-align: center;
      line-height: 1.6;
    }
  </style>
</head>
<body>

  <div class="container">
    <header>
      <h1>🏭 一日生活碳足跡盤查表</h1>
      <div class="subtitle">算算看，今天的各項生活活動產生了多少二氧化碳？</div>
    </header>

    <!-- 輸入區塊 -->
    <div class="input-card">
      <div class="form-group">
        <label for="activity">選擇生活行為：</label>
        <select id="activity" onchange="updateUnit()">
          <option value="1.50" data-name="吃一餐含牛肉餐點" data-unit="餐">吃一餐含牛肉餐點 (+1.50 kg)</option>
          <option value="0.70" data-name="吃一餐豬肉/雞肉便當" data-unit="餐">吃一餐豬肉/雞肉便當 (+0.70 kg)</option>
          <option value="0.15" data-name="買一瓶寶特瓶飲料" data-unit="瓶">買一瓶寶特瓶飲料 (+0.15 kg)</option>
          <option value="0.08" data-name="買一杯外帶手搖飲(紙杯+封口膜)" data-unit="杯">買一杯外帶手搖飲 (+0.08 kg)</option>
          <option value="0.06" data-name="使用一個塑膠袋" data-unit="個">索取一個塑膠提袋 (+0.06 kg)</option>
          <option value="0.02" data-name="使用一雙免洗筷" data-unit="雙">使用一雙免洗筷 (+0.02 kg)</option>
          <option value="0.45" data-name="吹冷氣一小時" data-unit="小時">吹冷氣一小時 (+0.45 kg)</option>
          <option value="0.05" data-name="看電視/玩電腦一小時" data-unit="小時">使用電腦/看電視一小時 (+0.05 kg)</option>
          <option value="0.25" data-name="家長開汽車接送(每公里)" data-unit="公里">開汽車代步 (每公里 +0.25 kg)</option>
          <option value="0.07" data-name="家長騎燃油機車接送(每公里)" data-unit="公里">騎機車代步 (每公里 +0.07 kg)</option>
        </select>
      </div>

      <div class="form-group">
        <label for="amount">活動數量（單位：<span id="unit-label">餐</span>）：</label>
        <input type="number" id="amount" placeholder="請輸入數值，例如：1" min="1" step="any">
      </div>

      <button type="button" class="btn-add" onclick="addRecord()">＋ 記錄生活碳排</button>
    </div>

    <!-- 總累計看板 -->
    <div class="dashboard">
      <div class="card-stat full-width">
        <div class="stat-title">累計總排放量</div>
        <div class="stat-value"><span id="total-carbon">0.00</span> <span class="unit">kg CO₂e</span></div>
      </div>
      <div class="card-stat">
        <div class="stat-title">需多少棵樹吸收一年</div>
        <div class="stat-value">🌲 <span id="need-trees">0.00</span></div>
        <div class="unit">棵成樹吸收才能平衡</div>
      </div>
      <div class="card-stat">
        <div class="stat-title">消耗地球碳預算</div>
        <div class="stat-value">⚠️ <span id="budget-time">0 分鐘</span></div>
        <div class="unit">佔個人每日平均碳額度</div>
      </div>
    </div>

    <!-- 紀錄清單 -->
    <div class="list-header">
      <h3>今日排放明細</h3>
      <button class="btn-clear" onclick="clearAllRecords()">清空重測</button>
    </div>
    <ul class="record-list" id="record-list">
      <li class="empty-tip" id="empty-tip">尚未記錄任何行為，快從上方選擇並加入！</li>
    </ul>
  </div>

  <footer>
    <p>🤖 本程式由師生團隊與 AI 協同設計，專供課堂碳盤查與探究學習使用。</p>
    <p>🔒 運算全於裝置端本機進行，不記錄、不蒐集任何個人隱私與使用數據。</p>
  </footer>

  <script>
    let records = [];

    function updateUnit() {
      const select = document.getElementById('activity');
      const unit = select.options[select.selectedIndex].getAttribute('data-unit');
      document.getElementById('unit-label').textContent = unit;
    }

    function addRecord() {
      const select = document.getElementById('activity');
      const factor = parseFloat(select.value);
      const name = select.options[select.selectedIndex].getAttribute('data-name');
      const unit = select.options[select.selectedIndex].getAttribute('data-unit');
      const amountInput = document.getElementById('amount');
      const amount = parseFloat(amountInput.value);

      if (isNaN(amount) || amount <= 0) {
        alert('請輸入大於 0 的有效數字！');
        return;
      }

      const carbon = factor * amount;

      records.push({
        id: Date.now(),
        name: name,
        amount: amount,
        unit: unit,
        carbon: carbon
      });

      amountInput.value = '';
      render();
    }

    function removeRecord(id) {
      records = records.filter(item => item.id !== id);
      render();
    }

    function clearAllRecords() {
      if (records.length === 0) return;
      if (confirm('確定要清空今日的排放紀錄嗎？')) {
        records = [];
        render();
      }
    }

    function render() {
      const listEl = document.getElementById('record-list');
      listEl.innerHTML = '';

      if (records.length === 0) {
        listEl.innerHTML = '<li class="empty-tip">尚未記錄任何行為，快從上方選擇並加入！</li>';
      } else {
        records.forEach(item => {
          const li = document.createElement('li');
          li.className = 'record-item';
          li.innerHTML = `
            <div>
              <span class="record-name">${item.name}</span>
              <span style="color:#6b7280; font-size:12px;">× ${item.amount} ${item.unit}</span>
            </div>
            <div>
              <span class="record-carbon">+${item.carbon.toFixed(2)} kg</span>
              <button class="btn-del" onclick="removeRecord(${item.id})">刪除</button>
            </div>
          `;
          listEl.appendChild(li);
        });
      }

      const totalCarbon = records.reduce((sum, item) => sum + item.carbon, 0);

      // 成樹年吸碳量約 12 公斤 (需幾棵樹吸收整整一年才能抵消)
      const needTrees = (totalCarbon / 12).toFixed(2);

      // 台灣個人每日生活平均碳足跡約 16.5 kg CO2e (以一天 1440 分鐘換算消耗量)
      const minutesEquivalent = Math.round((totalCarbon / 16.5) * 1440);
      let timeText = "0 分鐘";
      if (minutesEquivalent >= 1440) {
        timeText = `${(minutesEquivalent / 1440).toFixed(1)} 天額度`;
      } else if (minutesEquivalent >= 60) {
        timeText = `${(minutesEquivalent / 60).toFixed(1)} 小時額度`;
      } else if (totalCarbon > 0) {
        timeText = `${minutesEquivalent} 分鐘額度`;
      }

      document.getElementById('total-carbon').textContent = totalCarbon.toFixed(2);
      document.getElementById('need-trees').textContent = needTrees;
      document.getElementById('budget-time').textContent = timeText;
    }
  </script>
</body>
</html>
