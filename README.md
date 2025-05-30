<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>건강 계산기</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 0 auto; max-width: 900px; padding: 2em; }
    .tab-buttons { display: flex; gap: 10px; margin-bottom: 20px; }
    .tab-btn { padding: 12px 20px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; background-color: #ccc; color: #333; transition: all 0.2s; }
    .tab-btn.active { background-color: #0059a5; color: white; }
    .tab-section { display: none; }
    .tab-section.active { display: block; }
    .vat-container { display: flex; flex-wrap: wrap; gap: 40px; background: #0059a5; padding: 30px; border-radius: 12px; color: white; }
    .vat-box { flex: 1; min-width: 300px; background: #006bbf; padding: 20px; border-radius: 12px; }
    .vat-box h3 { font-size: 18px; margin-bottom: 10px; }
    .vat-box label { display: block; margin-top: 10px; font-size: 14px; }
    .vat-box input { width: 100%; padding: 8px; margin-top: 5px; border-radius: 6px; border: none; font-size: 16px; }
    .vat-box button { margin-top: 15px; padding: 10px 15px; font-weight: bold; border: none; border-radius: 6px; background: white; color: #0059a5; cursor: pointer; }
    .vat-box .result { margin-top: 20px; background: #e0f2ff; padding: 10px; border-radius: 8px; color: #003b71; font-weight: bold; }
  </style>
</head>
<body>

  <div class="tab-buttons">
    <button class="tab-btn active" onclick="switchTab(event, 'vat')">부가세계산기</button>
    <button class="tab-btn" onclick="switchTab(event, 'bmi')">학생 비만도 계산기</button>
  </div>

  <div class="tab-section active" id="tab-vat">
    <div class="vat-container">
      <div class="vat-box">
        <h3>합계금액을 알고 계신 경우</h3>
        <label>합계금액(총액)</label>
        <input id="totalInput" type="number" placeholder="총금액 입력 (원)">
        <div class="button-group">
          <button onclick="calcFromTotal()">계산하기</button>
          <button onclick="resetVatForm()">다시하기</button>
        </div>
        <div class="result" id="totalResult"></div>
      </div>
      <div class="vat-box">
        <h3>공급가액을 알고 계신 경우</h3>
        <label>공급가액</label>
        <input id="supplyInput" type="number" placeholder="공급가액 입력 (원)">
        <div class="button-group">
          <button onclick="calcFromSupply()">계산하기</button>
          <button onclick="resetVatForm()">다시하기</button>
        </div>
        <div class="result" id="supplyResult"></div>
      </div>
    </div>
    <footer style="margin-top: 3em; text-align: center;">
      <p style="color: #555; font-size: 1.8em; font-weight: bold;">종합검진 예약은 검진라인에서 간편하게!</p>
      <a href="https://www.sjcore.co.kr" target="_blank" style="display: inline-block; padding: 10px 20px; background-color: #0059ff; color: white; border-radius: 6px; text-decoration: none; font-weight: bold; margin-top: 0.5em;">검진라인 바로가기</a>
    </footer>
  </div>

  <div class="tab-section" id="tab-bmi">
    <div style="background: #eef2f9; padding: 20px; border-radius: 12px;">
      <h2>BMI 계산기</h2>
      <label>키 (cm): <input type="number" id="bmiHeight" /></label><br><br>
      <label>몸무게 (kg): <input type="number" id="bmiWeight" /></label><br><br>
      <div class="button-group">
        <button onclick="calcBMI()">계산하기</button>
        <button onclick="resetBMI()">다시하기</button>
      </div>
      <div class="result" id="bmiResult"></div>
    </div>
    <footer style="margin-top: 3em; text-align: center;">
      <p style="color: #555; font-size: 1.8em; font-weight: bold;">종합검진 예약은 검진라인에서 간편하게!</p>
      <a href="https://www.sjcore.co.kr" target="_blank" style="display: inline-block; padding: 10px 20px; background-color: #0059ff; color: white; border-radius: 6px; text-decoration: none; font-weight: bold; margin-top: 0.5em;">검진라인 바로가기</a>
    </footer>
  </div>

<script>
  function switchTab(event, type) {
    const tabs = document.querySelectorAll(".tab-section");
    const buttons = document.querySelectorAll(".tab-btn");
    tabs.forEach(tab => tab.classList.remove("active"));
    buttons.forEach(btn => btn.classList.remove("active"));
    document.getElementById("tab-" + type).classList.add("active");
    event.target.classList.add("active");
  }

  function formatWon(value) {
    return value.toLocaleString('ko-KR') + ' 원';
  }

  function calcFromTotal() {
    const total = parseFloat(document.getElementById("totalInput").value);
    if (isNaN(total) || total <= 0) {
      document.getElementById("totalResult").innerText = "올바른 총금액을 입력하세요.";
      return;
    }
    const supply = total / 1.1;
    const tax = total - supply;
    document.getElementById("totalResult").innerHTML =
      `공급가액: ${formatWon(Math.floor(supply))}<br>부가세액: ${formatWon(Math.floor(tax))}`;
  }

  function calcFromSupply() {
    const supply = parseFloat(document.getElementById("supplyInput").value);
    if (isNaN(supply) || supply <= 0) {
      document.getElementById("supplyResult").innerText = "올바른 공급가액을 입력하세요.";
      return;
    }
    const tax = supply * 0.1;
    const total = supply + tax;
    document.getElementById("supplyResult").innerHTML =
      `부가세액: ${formatWon(Math.floor(tax))}<br>합계금액: ${formatWon(Math.floor(total))}`;
  }

  function resetVatForm() {
    document.getElementById("totalInput").value = "";
    document.getElementById("supplyInput").value = "";
    document.getElementById("totalResult").innerHTML = "";
    document.getElementById("supplyResult").innerHTML = "";
  }

  function calcBMI() {
    const height = parseFloat(document.getElementById("bmiHeight").value);
    const weight = parseFloat(document.getElementById("bmiWeight").value);
    if (isNaN(height) || isNaN(weight) || height <= 0 || weight <= 0) {
      document.getElementById("bmiResult").innerText = "올바른 값을 입력해주세요.";
      return;
    }
    const bmi = weight / ((height / 100) ** 2);
    let category = "";
    if (bmi < 18.5) category = "저체중";
    else if (bmi < 23) category = "정상";
    else if (bmi < 25) category = "과체중";
    else category = "비만";
    document.getElementById("bmiResult").innerText = `BMI 지수는 ${bmi.toFixed(1)}로, ${category}입니다.`;
  }

  function resetBMI() {
    document.getElementById("bmiHeight").value = "";
    document.getElementById("bmiWeight").value = "";
    document.getElementById("bmiResult").innerText = "";
  }
</script>

</body>
</html>


    document.getElementById("bmiResult").innerText =
      `BMI 지수는 ${bmi.toFixed(1)}로, ${category}입니다.`;
  }
</script>
