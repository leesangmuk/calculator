<style>
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
</style>

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

<script>
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
</script>
