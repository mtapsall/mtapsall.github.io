
Savings calculators

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IC++ & Recovery Breakdown Calculator</title>
    <style>
        /* CSS STYLES */
        :root {
            --primary-color: #2563eb; /* Blue for Revenue */
            --success-color: #16a34a; /* Green for Savings */
            --danger-color: #ef4444;  /* Red for Costs/Losses */
            --bg-color: #f8fafc;
            --card-bg: #ffffff;
            --text-color: #1e293b;
            --border-radius: 12px;
            --border-color: #e2e8f0;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            display: flex;
            justify-content: center;
            padding: 20px;
        }

        .calc-container {
            background: var(--card-bg);
            width: 100%;
            max-width: 1000px;
            padding: 40px;
            border-radius: var(--border-radius);
            box-shadow: 0 10px 25px rgba(0,0,0,0.05);
            display: flex;
            flex-wrap: wrap;
            gap: 40px;
        }

        /* --- COLUMNS --- */
        .col-inputs { flex: 1; min-width: 300px; }
        .col-results { 
            flex: 1.2; 
            min-width: 350px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        /* --- TYPOGRAPHY --- */
        h2 { margin-top: 0; color: var(--text-color); font-size: 1.4rem; margin-bottom: 20px;}
        h3 { font-size: 0.85rem; text-transform: uppercase; color: #64748b; margin-top: 0; margin-bottom: 15px; letter-spacing: 0.5px; border-bottom: 1px solid var(--border-color); padding-bottom: 8px; }

        /* --- INPUT STYLES --- */
        .input-group { margin-bottom: 15px; }
        label { display: block; font-weight: 600; margin-bottom: 6px; color: var(--text-color); font-size: 0.85rem; }
        
        .input-wrapper { position: relative; }
        .input-prefix, .input-suffix { position: absolute; top: 50%; transform: translateY(-50%); color: #94a3b8; font-weight: 500; font-size: 0.9rem; }
        .input-prefix { left: 10px; }
        .input-suffix { right: 10px; }

        input[type="number"] {
            width: 100%; padding: 10px 10px 10px 25px; border: 1px solid #cbd5e1; border-radius: 6px; font-size: 0.95rem; outline: none; transition: border-color 0.2s; box-sizing: border-box;
        }
        input[type="number"]:focus { border-color: var(--primary-color); }
        .ic-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }

        /* --- RESULT BOX STYLES --- */
        .result-box {
            background: #fff;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            padding: 20px;
        }
        
        .result-header {
            font-weight: 700;
            color: var(--text-color);
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Grid for Results */
        .res-grid { display: grid; grid-template-columns: 2fr 1fr 1fr; gap: 10px; align-items: center; font-size: 0.9rem; margin-bottom: 8px;}
        .res-head { color: #64748b; font-size: 0.75rem; text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600; margin-bottom: 5px; }
        .res-val { font-weight: 600; color: var(--text-color); }
        .res-row { border-bottom: 1px dashed #e2e8f0; padding-bottom: 8px; margin-bottom: 8px; }
        .res-row:last-child { border-bottom: none; }
        
        .highlight-green { color: var(--success-color); background: #f0fdf4; padding: 5px; border-radius: 4px; }
        .highlight-blue { color: var(--primary-color); background: #eff6ff; padding: 5px; border-radius: 4px; }
        .text-red { color: var(--danger-color); }

        .total-banner {
            background: var(--text-color);
            color: #fff;
            padding: 20px;
            border-radius: var(--border-radius);
            text-align: center;
            margin-top: 10px;
        }
        .total-banner-label { font-size: 0.85rem; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; margin-bottom: 5px; }
        .total-banner-amount { font-size: 2.2rem; font-weight: 800; line-height: 1; }

    </style>
</head>
<body>

<div class="calc-container">
    
    <div class="col-inputs">
        <h2>Parameters</h2>

        <h3>1. Merchant Profile</h3>
        <div class="input-group">
            <label>Monthly Processing Volume</label>
            <div class="input-wrapper">
                <span class="input-prefix">£</span>
                <input type="number" id="monthlyVol" value="50000" step="1000">
            </div>
        </div>
        <div class="input-group">
            <label>Avg Ticket Size</label>
            <div class="input-wrapper">
                <span class="input-prefix">£</span>
                <input type="number" id="ticketSize" value="45">
            </div>
        </div>
        <div class="ic-grid">
            <div class="input-group">
                <label>Current Rate %</label>
                <div class="input-wrapper">
                    <input type="number" id="currentRate" value="1.4" step="0.1" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
            <div class="input-group">
                <label>Current Fee (p)</label>
                <div class="input-wrapper">
                    <span class="input-prefix">£</span>
                    <input type="number" id="currentTxnFee" value="0.20" step="0.01">
                </div>
            </div>
        </div>

        <h3 style="margin-top: 20px;">2. IC++ Proposal</h3>
        <div class="ic-grid">
            <div class="input-group">
                <label>Interchange %</label>
                <div class="input-wrapper">
                    <input type="number" id="icRate" value="0.30" step="0.01" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
            <div class="input-group">
                <label>Scheme Fee %</label>
                <div class="input-wrapper">
                    <input type="number" id="schemeRate" value="0.12" step="0.01" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
        </div>
        <div class="ic-grid">
            <div class="input-group">
                <label>Your Markup %</label>
                <div class="input-wrapper">
                    <input type="number" id="markupRate" value="0.40" step="0.01" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
            <div class="input-group">
                <label>Gateway Fee (p)</label>
                <div class="input-wrapper">
                    <span class="input-prefix">£</span>
                    <input type="number" id="gatewayFee" value="0.05" step="0.01">
                </div>
            </div>
        </div>

        <h3 style="margin-top: 20px;">3. Failure Recovery</h3>
        <div class="ic-grid">
            <div class="input-group">
                <label>Current Fail Rate %</label>
                <div class="input-wrapper">
                    <input type="number" id="currentFailRate" value="8.0" step="0.5" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
            <div class="input-group">
                <label>Target Fail Rate %</label>
                <div class="input-wrapper">
                    <input type="number" id="targetFailRate" value="5.0" step="0.5" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
        </div>
    </div>

    <div class="col-results">
        <h2>Breakdown</h2>

        <div class="result-box" style="border-left: 4px solid var(--success-color);">
            <div class="result-header">
                <span>1. Card Cost & Savings</span>
            </div>
            
            <div class="res-grid">
                <div></div>
                <div class="res-head">Monthly</div>
                <div class="res-head">Yearly</div>
            </div>

            <div class="res-grid res-row">
                <div class="res-val" style="font-weight:400">Current Cost</div>
                <div class="res-val text-red" id="moCurrentCost">£0</div>
                <div class="res-val text-red" id="yrCurrentCost">£0</div>
            </div>

            <div class="res-grid res-row">
                <div class="res-val" style="font-weight:400">New IC++ Cost</div>
                <div class="res-val" id="moNewCost">£0</div>
                <div class="res-val" id="yrNewCost">£0</div>
            </div>

            <div class="res-grid highlight-green">
                <div class="res-val">NET SAVINGS</div>
                <div class="res-val" id="moFeeSavings">£0</div>
                <div class="res-val" id="yrFeeSavings">£0</div>
            </div>
        </div>

        <div class="result-box" style="border-left: 4px solid var(--primary-color);">
            <div class="result-header">
                <span>2. Revenue Recovery</span>
            </div>

            <div class="res-grid">
                <div></div>
                <div class="res-head">Monthly</div>
                <div class="res-head">Yearly</div>
            </div>

            <div class="res-grid res-row">
                <div class="res-val" style="font-weight:400">Current Lost Sales</div>
                <div class="res-val text-red" id="moLostRev">£0</div>
                <div class="res-val text-red" id="yrLostRev">£0</div>
            </div>

            <div class="res-grid highlight-blue">
                <div class="res-val">RECOVERED</div>
                <div class="res-val" id="moRecovered">£0</div>
                <div class="res-val" id="yrRecovered">£0</div>
            </div>
            
            <div style="font-size: 0.75rem; color: #64748b; margin-top: 5px;">
                *Based on reducing failure rate from <span id="txtCurFail">8%</span> to <span id="txtTgtFail">5%</span>.
            </div>
        </div>

        <div class="total-banner">
            <div class="total-banner-label">Total Annual Benefit (Savings + Recovery)</div>
            <div class="total-banner-amount" id="grandTotal">£0</div>
        </div>

    </div>
</div>

<script>
    // --- HELPERS ---
    const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
    const setTxt = (id, val) => document.getElementById(id).innerText = val;
    
    // Formatter: £1,234.56
    const fmt = new Intl.NumberFormat('en-GB', {
        style: 'currency', currency: 'GBP', minimumFractionDigits: 2, maximumFractionDigits: 2
    });
    // Formatter: £1,234 (No pence for large annual totals)
    const fmtInt = new Intl.NumberFormat('en-GB', {
        style: 'currency', currency: 'GBP', minimumFractionDigits: 0, maximumFractionDigits: 0
    });

    function calculate() {
        // --- INPUTS ---
        const vol = getVal('monthlyVol');
        const ticket = getVal('ticketSize');
        if (vol === 0 || ticket === 0) return;

        const txnCount = vol / ticket;

        // 1. FEE CALCULATIONS
        // Current
        const currentRate = getVal('currentRate') / 100;
        const currentTxnFee = getVal('currentTxnFee');
        const costCurrent = (vol * currentRate) + (txnCount * currentTxnFee);

        // New IC++
        const icPlusRate = (getVal('icRate') + getVal('schemeRate') + getVal('markupRate')) / 100;
        const gatewayFee = getVal('gatewayFee');
        const costNew = (vol * icPlusRate) + (txnCount * gatewayFee);

        // Savings
        const saveMo = Math.max(0, costCurrent - costNew);
        const saveYr = saveMo * 12;

        // 2. RECOVERY CALCULATIONS
        const failCur = getVal('currentFailRate') / 100;
        const failTgt = getVal('targetFailRate') / 100;

        const lostVolMo = vol * failCur;
        const targetLostMo = vol * failTgt;
        
        // Recovered is the difference between what they lose now vs what they will lose later
        const recMo = Math.max(0, lostVolMo - targetLostMo);
        const recYr = recMo * 12;

        // 3. UPDATING UI
        
        // Box 1: Fees
        setTxt('moCurrentCost', fmt.format(costCurrent));
        setTxt('yrCurrentCost', fmtInt.format(costCurrent * 12));
        setTxt('moNewCost', fmt.format(costNew));
        setTxt('yrNewCost', fmtInt.format(costNew * 12));
        setTxt('moFeeSavings', "+" + fmt.format(saveMo));
        setTxt('yrFeeSavings', "+" + fmtInt.format(saveYr));

        // Box 2: Recovery
        setTxt('moLostRev', fmt.format(lostVolMo));
        setTxt('yrLostRev', fmtInt.format(lostVolMo * 12));
        setTxt('moRecovered', "+" + fmt.format(recMo));
        setTxt('yrRecovered', "+" + fmtInt.format(recYr));

        // Update Text Spans
        setTxt('txtCurFail', (failCur * 100) + "%");
        setTxt('txtTgtFail', (failTgt * 100) + "%");

        // Grand Total
        setTxt('grandTotal', fmtInt.format(saveYr + recYr));
    }

    // --- LISTENERS ---
    const ids = [
        'monthlyVol', 'ticketSize', 
        'currentRate', 'currentTxnFee',
        'icRate', 'schemeRate', 'markupRate', 'gatewayFee',
        'currentFailRate', 'targetFailRate'
    ];
    ids.forEach(id => document.getElementById(id).addEventListener('input', calculate));
    
    // Init
    calculate();
</script>

</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IC++ Switch Calculator</title>
    <style>
        /* CSS STYLES */
        :root {
            --primary-color: #2563eb; /* Revenue Blue */
            --success-color: #16a34a; /* Savings Green */
            --danger-color: #ef4444;  /* Cost Red */
            --bg-color: #f8fafc;
            --card-bg: #ffffff;
            --text-color: #1e293b;
            --border-radius: 12px;
            --border-color: #e2e8f0;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            display: flex;
            justify-content: center;
            padding: 20px;
        }

        .calc-container {
            background: var(--card-bg);
            width: 100%;
            max-width: 1000px;
            padding: 40px;
            border-radius: var(--border-radius);
            box-shadow: 0 10px 25px rgba(0,0,0,0.05);
            display: flex;
            flex-wrap: wrap;
            gap: 40px;
        }

        /* --- COLUMNS --- */
        .col-inputs { flex: 1; min-width: 300px; }
        .col-results { 
            flex: 1.2; 
            min-width: 350px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        /* --- TYPOGRAPHY --- */
        h2 { margin-top: 0; color: var(--text-color); font-size: 1.4rem; margin-bottom: 20px;}
        h3 { font-size: 0.85rem; text-transform: uppercase; color: #64748b; margin-top: 0; margin-bottom: 15px; letter-spacing: 0.5px; border-bottom: 1px solid var(--border-color); padding-bottom: 8px; }

        /* --- INPUT STYLES --- */
        .input-group { margin-bottom: 15px; }
        label { display: block; font-weight: 600; margin-bottom: 6px; color: var(--text-color); font-size: 0.85rem; }
        
        .input-wrapper { position: relative; }
        .input-prefix, .input-suffix { position: absolute; top: 50%; transform: translateY(-50%); color: #94a3b8; font-weight: 500; font-size: 0.9rem; }
        .input-prefix { left: 10px; }
        .input-suffix { right: 10px; }

        input[type="number"] {
            width: 100%; padding: 10px 10px 10px 25px; border: 1px solid #cbd5e1; border-radius: 6px; font-size: 0.95rem; outline: none; transition: border-color 0.2s; box-sizing: border-box;
        }
        input[type="number"]:focus { border-color: var(--primary-color); }
        
        .ic-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        
        /* Comparison Grid specifically for HEAD to HEAD */
        .comp-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            background: #f1f5f9;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
        }
        .comp-col h4 { margin: 0 0 10px 0; font-size: 0.8rem; text-transform: uppercase; color: #64748b; }

        /* --- RESULT BOX STYLES --- */
        .result-box {
            background: #fff;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            padding: 20px;
        }
        
        .result-header {
            font-weight: 700;
            color: var(--text-color);
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .res-grid { display: grid; grid-template-columns: 2fr 1fr 1fr; gap: 10px; align-items: center; font-size: 0.9rem; margin-bottom: 8px;}
        .res-head { color: #64748b; font-size: 0.75rem; text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600; margin-bottom: 5px; }
        .res-val { font-weight: 600; color: var(--text-color); }
        .res-row { border-bottom: 1px dashed #e2e8f0; padding-bottom: 8px; margin-bottom: 8px; }
        .res-row:last-child { border-bottom: none; }
        
        .highlight-green { color: var(--success-color); background: #f0fdf4; padding: 5px; border-radius: 4px; }
        .highlight-blue { color: var(--primary-color); background: #eff6ff; padding: 5px; border-radius: 4px; }
        .text-red { color: var(--danger-color); }

        .total-banner {
            background: var(--text-color);
            color: #fff;
            padding: 20px;
            border-radius: var(--border-radius);
            text-align: center;
            margin-top: 10px;
        }
        .total-banner-label { font-size: 0.85rem; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; margin-bottom: 5px; }
        .total-banner-amount { font-size: 2.2rem; font-weight: 800; line-height: 1; }

    </style>
</head>
<body>

<div class="calc-container">
    
    <div class="col-inputs">
        <h2>Parameters</h2>

        <h3>1. Merchant Profile</h3>
        <div class="input-group">
            <label>Monthly Processing Volume</label>
            <div class="input-wrapper">
                <span class="input-prefix">£</span>
                <input type="number" id="monthlyVol" value="50000" step="1000">
            </div>
        </div>
        <div class="input-group">
            <label>Avg Ticket Size</label>
            <div class="input-wrapper">
                <span class="input-prefix">£</span>
                <input type="number" id="ticketSize" value="45">
            </div>
        </div>

        <h3>2. Base Costs (Pass-Through)</h3>
        <div class="ic-grid">
            <div class="input-group">
                <label>Interchange %</label>
                <div class="input-wrapper">
                    <input type="number" id="icRate" value="0.30" step="0.01" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
            <div class="input-group">
                <label>Scheme Fee %</label>
                <div class="input-wrapper">
                    <input type="number" id="schemeRate" value="0.12" step="0.01" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
        </div>

        <h3 style="margin-top: 20px;">3. The Comparison (Markup)</h3>
        <div class="comp-grid">
            <div class="comp-col">
                <h4>Current Provider</h4>
                <div class="input-group">
                    <label>Markup %</label>
                    <div class="input-wrapper">
                        <input type="number" id="curMarkup" value="0.65" step="0.01" style="padding-left:10px; border-color:#cbd5e1;">
                        <span class="input-suffix">%</span>
                    </div>
                </div>
                <div class="input-group">
                    <label>Gateway (p)</label>
                    <div class="input-wrapper">
                        <span class="input-prefix">£</span>
                        <input type="number" id="curGateway" value="0.10" step="0.01">
                    </div>
                </div>
            </div>

            <div class="comp-col">
                <h4>New Proposal</h4>
                <div class="input-group">
                    <label>Markup %</label>
                    <div class="input-wrapper">
                        <input type="number" id="newMarkup" value="0.40" step="0.01" style="padding-left:10px; border-color:var(--success-color);">
                        <span class="input-suffix">%</span>
                    </div>
                </div>
                <div class="input-group">
                    <label>Gateway (p)</label>
                    <div class="input-wrapper">
                        <span class="input-prefix">£</span>
                        <input type="number" id="newGateway" value="0.05" step="0.01">
                    </div>
                </div>
            </div>
        </div>

        <h3 style="margin-top: 20px;">4. Failure Recovery</h3>
        <div class="ic-grid">
            <div class="input-group">
                <label>Current Fail Rate %</label>
                <div class="input-wrapper">
                    <input type="number" id="currentFailRate" value="8.0" step="0.5" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
            <div class="input-group">
                <label>Target Fail Rate %</label>
                <div class="input-wrapper">
                    <input type="number" id="targetFailRate" value="5.0" step="0.5" style="padding-left:10px">
                    <span class="input-suffix">%</span>
                </div>
            </div>
        </div>
    </div>

    <div class="col-results">
        <h2>Breakdown</h2>

        <div class="result-box" style="border-left: 4px solid var(--success-color);">
            <div class="result-header">
                <span>1. Card Cost & Savings</span>
            </div>
            
            <div class="res-grid">
                <div></div>
                <div class="res-head">Monthly</div>
                <div class="res-head">Yearly</div>
            </div>

            <div class="res-grid res-row">
                <div class="res-val" style="font-weight:400">Current Cost</div>
                <div class="res-val text-red" id="moCurrentCost">£0</div>
                <div class="res-val text-red" id="yrCurrentCost">£0</div>
            </div>

            <div class="res-grid res-row">
                <div class="res-val" style="font-weight:400">New IC++ Cost</div>
                <div class="res-val" id="moNewCost">£0</div>
                <div class="res-val" id="yrNewCost">£0</div>
            </div>

            <div class="res-grid highlight-green">
                <div class="res-val">NET SAVINGS</div>
                <div class="res-val" id="moFeeSavings">£0</div>
                <div class="res-val" id="yrFeeSavings">£0</div>
            </div>
        </div>

        <div class="result-box" style="border-left: 4px solid var(--primary-color);">
            <div class="result-header">
                <span>2. Revenue Recovery</span>
            </div>

            <div class="res-grid">
                <div></div>
                <div class="res-head">Monthly</div>
                <div class="res-head">Yearly</div>
            </div>

            <div class="res-grid res-row">
                <div class="res-val" style="font-weight:400">Current Lost Sales</div>
                <div class="res-val text-red" id="moLostRev">£0</div>
                <div class="res-val text-red" id="yrLostRev">£0</div>
            </div>

            <div class="res-grid highlight-blue">
                <div class="res-val">RECOVERED</div>
                <div class="res-val" id="moRecovered">£0</div>
                <div class="res-val" id="yrRecovered">£0</div>
            </div>
            
            <div style="font-size: 0.75rem; color: #64748b; margin-top: 5px;">
                *Based on reducing failure rate from <span id="txtCurFail">8%</span> to <span id="txtTgtFail">5%</span>.
            </div>
        </div>

        <div class="total-banner">
            <div class="total-banner-label">Total Annual Benefit</div>
            <div class="total-banner-amount" id="grandTotal">£0</div>
        </div>

    </div>
</div>

<script>
    // --- HELPERS ---
    const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
    const setTxt = (id, val) => document.getElementById(id).innerText = val;
    
    const fmt = new Intl.NumberFormat('en-GB', {
        style: 'currency', currency: 'GBP', minimumFractionDigits: 2, maximumFractionDigits: 2
    });
    const fmtInt = new Intl.NumberFormat('en-GB', {
        style: 'currency', currency: 'GBP', minimumFractionDigits: 0, maximumFractionDigits: 0
    });

    function calculate() {
        // --- INPUTS ---
        const vol = getVal('monthlyVol');
        const ticket = getVal('ticketSize');
        if (vol === 0 || ticket === 0) return;

        const txnCount = vol / ticket;

        // Base Costs (Apply to both)
        const baseRate = (getVal('icRate') + getVal('schemeRate')) / 100;
        const baseCost = vol * baseRate;

        // 1. FEE COMPARISON
        // Current Provider
        const curMarkup = getVal('curMarkup') / 100;
        const curGateway = getVal('curGateway');
        const costCurrent = baseCost + (vol * curMarkup) + (txnCount * curGateway);

        // New Proposal
        const newMarkup = getVal('newMarkup') / 100;
        const newGateway = getVal('newGateway');
        const costNew = baseCost + (vol * newMarkup) + (txnCount * newGateway);

        // Savings
        const saveMo = Math.max(0, costCurrent - costNew);
        const saveYr = saveMo * 12;

        // 2. RECOVERY CALCULATIONS
        const failCur = getVal('currentFailRate') / 100;
        const failTgt = getVal('targetFailRate') / 100;

        const lostVolMo = vol * failCur;
        const targetLostMo = vol * failTgt;
        
        // Recovered
        const recMo = Math.max(0, lostVolMo - targetLostMo);
        const recYr = recMo * 12;

        // 3. UPDATING UI
        
        // Box 1
        setTxt('moCurrentCost', fmt.format(costCurrent));
        setTxt('yrCurrentCost', fmtInt.format(costCurrent * 12));
        setTxt('moNewCost', fmt.format(costNew));
        setTxt('yrNewCost', fmtInt.format(costNew * 12));
        setTxt('moFeeSavings', "+" + fmt.format(saveMo));
        setTxt('yrFeeSavings', "+" + fmtInt.format(saveYr));

        // Box 2
        setTxt('moLostRev', fmt.format(lostVolMo));
        setTxt('yrLostRev', fmtInt.format(lostVolMo * 12));
        setTxt('moRecovered', "+" + fmt.format(recMo));
        setTxt('yrRecovered', "+" + fmtInt.format(recYr));

        // Text Spans
        setTxt('txtCurFail', (failCur * 100) + "%");
        setTxt('txtTgtFail', (failTgt * 100) + "%");

        // Grand Total
        setTxt('grandTotal', fmtInt.format(saveYr + recYr));
    }

    // --- LISTENERS ---
    const ids = [
        'monthlyVol', 'ticketSize', 
        'icRate', 'schemeRate',
        'curMarkup', 'curGateway',
        'newMarkup', 'newGateway',
        'currentFailRate', 'targetFailRate'
    ];
    ids.forEach(id => document.getElementById(id).addEventListener('input', calculate));
    
    // Init
    calculate();
</script>

</body>
</html>
