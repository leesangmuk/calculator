<style>
  /* 공통 스타일 */
  .tab-buttons {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }

  .tab-btn {
    padding: 12px 20px;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    cursor: pointer;
    background-color: #cccccc;
    color: #333;
    transition: all 0.2s;
  }

  .tab-btn.active {
    background-color: #0059a5;
    color: white;
  }

  .tab-section {
    display: none;
  }

  .tab-section.active {
    display: block;
  }

  .vat-container {
    display: flex;
    flex-wrap: wrap;
    gap: 40px;
    background: #0059a5;
    padding: 30px;
    border-radius: 12px;
    color: white;
    font-family: sans-serif;
  }

  .vat-box {
    flex: 1;
    min-width: 300px;
    background: #006bbf;
    padding: 20px;
    border-radius: 12px;
  }

  .vat-box h3 {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .vat-box label {
    display: block;
    margin-top: 10px;
    font-size: 14px;
  }

  .vat-box input {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    border-radius: 6px;
    border: none;
    font-size: 16px;
  }

  .vat-box button {
    margin-top: 15px;
    padding: 10px 15px;
    font-weight: bold;
    border: none;
    border-radius: 6px;
    background: white;
    color: #0059a5;
    cursor: pointer;
  }

  .vat-box .result {
    margin-top: 20px;
    background: #e0f2ff;
    padding: 10px;
    border-radius: 8px;
    color: #003b71;
    font-weight: bold;
  }

  .bmi-box {
    background-color: #eef5ff;
    padding: 30px;
    border-radius: 12px;
    font-family: sans-serif;
    color: #222;
  }

</style>

<!-- 탭 선택 버튼 -->
<div class="tab-buttons">
  <button class="tab-btn active" onclick="switchTab('vat')">부가세계산기</button>
  <button class="tab-btn" onclick="switchTab('bmi')">학생 비만도 계산기</button>
</div>

<!-- 부가세계산기 영역 -->
<div class="tab-section active" id="tab-vat">
  <div class="vat-container">
    <!-- 합계금액 기준 계산 -->
    <div class="vat-box">
      <h3>합계금액을 알고 계신 경우</h3>
      <label>합계금액(총액)</label>
      <input id="totalInput" type="number" placeholder="총금액 입력 (원)">
      <button onclick="calcFromTotal()">계산하기</button>
      <div class="result" id="totalResult"></div>
    </div>

    <!-- 공급가액 기준 계산 -->
    <div class="vat-box">
      <h3>공급가액을 알고 계신 경우</h3>
      <label>공급가액</label>
      <input id="supplyInput" type="number" placeholder="공급가액 입력 (원)">
      <button onclick="calcFromSupply()">계산하기</button>
      <div class="result" id="supplyResult"></div>
    </div>
  </div>
</div>

<!-- 학생 비만도 계산기 영역 -->
<div class="tab-section" id="tab-bmi">
  <div class="bmi-box">
    <h3>학생 비만도 계산기</h3>
    <label>키 (cm):</label>
    <input type="number" id="height" placeholder="예: 160"><br><br>
    <label>몸무게 (kg):</label>
    <input type="number" id="weight" placeholder="예: 55"><br><br>
    <button onclick="calcBMI()">계산하기</button>
    <p id="bmiResult" style="margin-top: 15px; font-weight: bold;"></p>
  </div>
</div>

<script>
  // 탭 스위칭
  function switchTab(type) {
    const tabs = document.querySelectorAll(".tab-section");
    const buttons = document.querySelectorAll(".tab-btn");

    tabs.forEach(tab => tab.classList.remove("active"));
    buttons.forEach(btn => btn.classList.remove("active"));

    document.getElementById("tab-" + type).classList.add("active");
    event.target.classList.add("active");
  }

  // 부가세 계산 함수
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

  // BMI 계산
  function calcBMI() {
    const h = parseFloat(document.getElementById("height").value);
    const w = parseFloat(document.getElementById("weight").value);
    if (isNaN(h) || isNaN(w) || h <= 0 || w <= 0) {
      document.getElementById("bmiResult").innerText = "올바른 키와 몸무게를 입력하세요.";
      return;
    }

    const m = h / 100;
    const bmi = w / (m * m);
    let category = "";

    if (bmi < 18.5) category = "저체중";
    else if (bmi < 23) category = "정상";
    else if (bmi < 25) category = "과체중";
    else category = "비만";

    document.getElementById("bmiResult").innerText =
      `BMI 지수는 ${bmi.toFixed(1)}로, ${category}입니다.`;
  }
</script>
