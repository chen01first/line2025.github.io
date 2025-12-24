<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>遺產稅與贈與稅試算工具 (106年新制基準)</title>
    <style>
        :root {
            --header-blue: #004a99;
            --header-text: white;
            --bg-grey: #e6e6e6;
            --bg-yellow: #fff2cc;
            --border-color: #b3b3b3;
            --highlight-red: #d9534f;
        }

        body {
            font-family: "Microsoft JhengHei", Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: #fff;
            padding: 15px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        .grid-container {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
        }

        @media (min-width: 992px) {
            .grid-container {
                grid-template-columns: 5fr 5fr; 
            }
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        th, td {
            border: 1px solid var(--border-color);
            padding: 10px;
            text-align: center;
        }

        .header-blue { background-color: var(--header-blue); color: var(--header-text); font-weight: bold; }
        .bg-grey { background-color: var(--bg-grey); }
        .bg-yellow { background-color: var(--bg-yellow); font-weight: bold; }
        .text-right { text-align: right !important; }
        .text-red { color: var(--highlight-red); font-weight: bold; }
        .text-large { font-size: 1.4em; font-weight: bold; }

        /* 輸入框樣式 */
        input[type="number"] {
            padding: 8px;
            border: 1px solid #ccc;
            text-align: right;
            border-radius: 4px;
        }

        .input-large { 
            width: 300px; /* 依照要求拉長為2倍寬 */
            max-width: 95%;
            font-size: 1.2em; 
            font-weight: bold; 
            color: var(--header-blue);
        }

        .input-count { width: 60px; text-align: center; }
        
        .tax-bracket-title {
            background-color: #545454;
            color: white;
            padding: 5px;
            font-size: 0.9em;
        }

        .bracket-table td { font-size: 0.85em; padding: 5px; }
    </style>
</head>
<body>

<div class="container">
    <div class="grid-container">
        
        <div class="left-column">
            <table>
                <tr>
                    <th class="header-blue" width="25%">預計<br>淨資產</th>
                    <td class="bg-yellow text-right">
                        <input type="number" id="totalAssets" class="input-large" value="140000000"> 元
                    </td>
                </tr>
                <tr>
                    <th class="bg-grey">扣除免稅額<br>(已扣除)</th>
                    <td class="bg-grey text-large"><span id="displayTotalDeduction">2136</span> 萬</td>
                </tr>
                <tr>
                    <th class="bg-grey">遺產稅<br><span id="estateTaxRateDisplay">20%</span></th>
                    <td class="bg-grey text-large"><span id="estateTaxResult">1623</span> 萬</td>
                </tr>
            </table>

            <table>
                <tr>
                    <th class="header-blue" width="25%">預計<br>規劃稅源</th>
                    <td colspan="3" class="bg-yellow text-right">
                        <input type="number" id="planAmount" class="input-large" value="10000000"> 元
                    </td>
                </tr>
                <tr class="header-blue">
                    <th>規劃項目</th>
                    <th>贈與</th>
                    <th>有規劃<br>(無列入遺產)</th>
                    <th>有規劃<br>(有列入遺產)</th>
                </tr>
                <tr>
                    <th class="bg-grey">保障倍數</th>
                    <td>0</td>
                    <td class="bg-yellow text-red">
                        <input type="number" id="insuranceMultiplier" class="input-count" value="2.3" step="0.1">
                    </td>
                    <td class="bg-grey"><span id="displayMultiplier">2.3</span></td>
                </tr>
                <tr>
                    <th class="bg-grey">免稅額</th>
                    <td>244萬</td>
                    <td>-</td>
                    <td class="bg-grey"><span id="planEstateExemption">2136</span>萬</td>
                </tr>
                <tr>
                    <th class="bg-grey">稅率/<br>應納稅額</th>
                    <td>贈與 10%<br><span id="giftTaxResult">76萬</span></td>
                    <td>-</td>
                    <td class="bg-grey">遺產 <span id="planEstateTaxRate">20%</span><br><span id="planEstateTaxResult">460萬</span></td>
                </tr>
                <tr>
                    <th class="bg-grey">最後<br>預留金額</th>
                    <td class="text-large"><span id="giftFinalAmount">925萬</span></td>
                    <td class="bg-grey text-large text-red"><span id="planNoEstateFinalAmount">2300萬</span></td>
                    <td class="bg-grey text-large"><span id="planYesEstateFinalAmount">1840萬</span></td>
                </tr>
            </table>
        </div>

        <div class="right-column">
             <table>
                <thead>
                    <tr class="header-blue">
                        <th colspan="4">遺產稅扣除額明細</th>
                    </tr>
                    <tr class="bg-grey">
                        <th>項目</th>
                        <th>基準(萬)</th>
                        <th>人數</th>
                        <th>小計(萬)</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>免稅額</td>
                        <td>1333</td>
                        <td>基準</td>
                        <td>1333</td>
                    </tr>
                    <tr>
                        <td>配偶扣除</td>
                        <td>553</td>
                        <td class="bg-yellow">
                            <input type="number" id="countSpouse" class="input-count" value="1" min="0" max="1">
                        </td>
                        <td><span id="subSpouse">553</span></td>
                    </tr>
                    <tr>
                        <td>直系卑親屬</td>
                        <td>56</td>
                        <td class="bg-yellow">
                             <input type="number" id="countChild" class="input-count" value="2" min="0">
                        </td>
                        <td><span id="subChild">112</span></td>
                    </tr>
                    <tr>
                        <td>父母扣除</td>
                        <td>138</td>
                        <td class="bg-yellow">
                             <input type="number" id="countParent" class="input-count" value="0" min="0" max="2">
                        </td>
                        <td><span id="subParent">0</span></td>
                    </tr>
                    <tr>
                        <td>喪葬費</td>
                        <td>138</td>
                        <td>基準</td>
                        <td>138</td>
                    </tr>
                    <tr class="header-blue">
                        <td colspan="3" class="text-right">總計扣除免稅額(萬)：</td>
                        <td class="text-large"><span id="totalDeductionCalc">2136</span></td>
                    </tr>
                </tbody>
            </table>

            <table class="bracket-table">
                <thead>
                    <tr class="tax-bracket-title">
                        <th colspan="4">課稅級距對照表 (106年5月12日起適用)</th>
                    </tr>
                    <tr class="bg-grey">
                        <th>稅目</th>
                        <th>課稅級距</th>
                        <th>稅率</th>
                        <th>累進差額</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td rowspan="3" class="bg-grey">遺產稅</td>
                        <td class="text-left">5,000 萬元以下</td>
                        <td>10%</td>
                        <td>0</td>
                    </tr>
                    <tr>
                        <td class="text-left">超過 5,000 萬 ~ 1 億元</td>
                        <td>15%</td>
                        <td>250 萬</td>
                    </tr>
                    <tr>
                        <td class="text-left">超過 1 億元</td>
                        <td>20%</td>
                        <td>750 萬</td>
                    </tr>
                    <tr style="border-top: 2px solid #545454;">
                        <td rowspan="3" class="bg-grey">贈與稅</td>
                        <td class="text-left">2,500 萬元以下</td>
                        <td>10%</td>
                        <td>0</td>
                    </tr>
                    <tr>
                        <td class="text-left">超過 2,500 萬 ~ 5,000 萬元</td>
                        <td>15%</td>
                        <td>125 萬</td>
                    </tr>
                    <tr>
                        <td class="text-left">超過 5,000 萬元</td>
                        <td>20%</td>
                        <td>375 萬</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</div>

<script>
    const ESTATE_EXEMPTION = 13330000;
    const FUNERAL_DEDUCTION = 1380000;
    const GIFT_EXEMPTION = 2440000;
    const D_SPOUSE = 5530000;
    const D_CHILD = 560000;
    const D_PARENT = 1380000;

    function toWan(num) { return Math.round(num / 10000); }

    function calculate() {
        const totalAssets = parseFloat(document.getElementById('totalAssets').value) || 0;
        const planAmount = parseFloat(document.getElementById('planAmount').value) || 0;
        const multiplier = parseFloat(document.getElementById('insuranceMultiplier').value) || 0;
        
        const cSpouse = parseInt(document.getElementById('countSpouse').value) || 0;
        const cChild = parseInt(document.getElementById('countChild').value) || 0;
        const cParent = parseInt(document.getElementById('countParent').value) || 0;

        // 更新小計
        document.getElementById('subSpouse').innerText = toWan(cSpouse * D_SPOUSE);
        document.getElementById('subChild').innerText = toWan(cChild * D_CHILD);
        document.getElementById('subParent').innerText = toWan(cParent * D_PARENT);

        // 總扣除額計算
        let totalD = ESTATE_EXEMPTION + FUNERAL_DEDUCTION + (cSpouse * D_SPOUSE) + (cChild * D_CHILD) + (cParent * D_PARENT);
        const totalDWan = toWan(totalD);
        document.getElementById('totalDeductionCalc').innerText = totalDWan;
        document.getElementById('displayTotalDeduction').innerText = totalDWan;
        document.getElementById('planEstateExemption').innerText = totalDWan;

        // 106年新制遺產稅邏輯
        const netEstate = totalAssets - totalD;
        let eRate = 0.1, eDiff = 0;
        if (netEstate > 100000000) { eRate = 0.2; eDiff = 7500000; }
        else if (netEstate > 50000000) { eRate = 0.15; eDiff = 2500000; }
        
        const eTax = netEstate > 0 ? (netEstate * eRate - eDiff) : 0;
        document.getElementById('estateTaxRateDisplay').innerText = (eRate * 100) + '%';
        document.getElementById('estateTaxResult').innerText = toWan(eTax);

        // 106年新制贈與稅邏輯 (規劃部分)
        document.getElementById('displayMultiplier').innerText = multiplier;
        const leveragedVal = planAmount * multiplier;
        
        const netGift = planAmount - GIFT_EXEMPTION;
        let gRate = 0.1, gDiff = 0;
        if (netGift > 50000000) { gRate = 0.2; gDiff = 3750000; }
        else if (netGift > 25000000) { gRate = 0.15; gDiff = 1250000; }

        const gTax = netGift > 0 ? (netGift * gRate - gDiff) : 0;
        document.getElementById('giftTaxResult').innerText = toWan(gTax) + '萬';
        document.getElementById('giftFinalAmount').innerText = toWan(planAmount - gTax) + '萬';

        // 規劃試算
        document.getElementById('planNoEstateFinalAmount').innerText = toWan(leveragedVal) + '萬';
        const planETax = leveragedVal * eRate; // 依圖表採用資產所在之邊際稅率計算
        document.getElementById('planEstateTaxRate').innerText = (eRate * 100) + '%';
        document.getElementById('planEstateTaxResult').innerText = toWan(planETax) + '萬';
        document.getElementById('planYesEstateFinalAmount').innerText = toWan(leveragedVal - planETax) + '萬';
    }

    const inputs = document.querySelectorAll('input');
    inputs.forEach(input => input.addEventListener('input', calculate));
    calculate();
</script>

</body>
</html>
