<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>睡眠效率計算器</title>
  <style>
    body {
      font-family: Arial, "Microsoft JhengHei", "PingFang TC", sans-serif;
      max-width: 600px;
      margin: 40px auto;
      padding: 0 15px;
      line-height: 1.6;
    }
    h1 {
      text-align: center;
    }
    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
    }
    .time-row {
      display: flex;
      align-items: center;
      gap: 8px;
      margin-top: 5px;
    }
    .time-row input {
      width: 80px;
      padding: 6px;
      box-sizing: border-box;
      text-align: right;
    }
    button {
      margin-top: 20px;
      padding: 10px 15px;
      font-size: 16px;
      cursor: pointer;
    }
    .result {
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
    }
    .note {
      font-size: 14px;
      color: #555;
      margin-top: 10px;
    }
  </style>
</head>
<body>

  <h1>睡眠效率計算器</h1>

  <p>
    睡眠效率 ＝（實際睡眠時間 ÷ 臥床時間）× 100%
  </p>

  <form id="sleep-form" onsubmit="event.preventDefault(); calculateSleepEfficiency();">
    <label>
      臥床時間（小時:分鐘）
      <span style="font-weight: normal;">（由上床躺下到第二天起床離床的總時間）</span>
    </label>
    <div class="time-row">
      <input type="number" id="bedHours" min="0" placeholder="小時">
      <span>:</span>
      <input type="number" id="bedMinutes" min="0" max="59" placeholder="分鐘">
    </div>

    <label>
      實際睡眠時間（小時:分鐘）
      <span style="font-weight: normal;">（真正睡著的總時間，不包括清醒時間）</span>
    </label>
    <div class="time-row">
      <input type="number" id="sleepHours" min="0" placeholder="小時">
      <span>:</span>
      <input type="number" id="sleepMinutes" min="0" max="59" placeholder="分鐘">
    </div>

    <button type="submit">計算睡眠效率</button>
  </form>

  <div class="result" id="result"></div>
  <div class="note">
    一般來說，約 85% 或以上的睡眠效率通常被視為較理想，但會因人而異。<br>
    如對自己的睡眠有疑問或困擾，建議向醫護人員或專業人士查詢。
  </div>

  <script>
    function calculateSleepEfficiency() {
      const bedHours = parseFloat(document.getElementById('bedHours').value);
      const bedMinutes = parseFloat(document.getElementById('bedMinutes').value);
      const sleepHours = parseFloat(document.getElementById('sleepHours').value);
      const sleepMinutes = parseFloat(document.getElementById('sleepMinutes').value);
      const resultDiv = document.getElementById('result');

      // 轉換為分鐘（處理空白情況）
      const totalBedMinutes =
        (isNaN(bedHours) ? 0 : bedHours * 60) +
        (isNaN(bedMinutes) ? 0 : bedMinutes);

      const totalSleepMinutes =
        (isNaN(sleepHours) ? 0 : sleepHours * 60) +
        (isNaN(sleepMinutes) ? 0 : sleepMinutes);

      // 輸入檢查
      if (totalBedMinutes <= 0) {
        resultDiv.textContent = '請輸入有效的臥床時間（小時或分鐘必須大於 0）。';
        return;
      }

      if (totalSleepMinutes < 0) {
        resultDiv.textContent = '實際睡眠時間不能為負數。';
        return;
      }

      if (totalSleepMinutes > totalBedMinutes) {
        resultDiv.textContent = '實際睡眠時間不能大於臥床時間，請檢查數字是否正確。';
        return;
      }

      const efficiency = (totalSleepMinutes / totalBedMinutes) * 100;
      resultDiv.textContent = '你的睡眠效率為：' + efficiency.toFixed(1) + '%';
    }
  </script>

</body>
</html>
