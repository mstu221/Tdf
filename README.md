<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Investment Dashboard Project</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: 'Arial', sans-serif;
            background-color: #001f3f;
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        /* মেইন ড্যাশবোর্ড */
        .header { width: 100%; display: flex; justify-content: space-between; padding: 20px; box-sizing: border-box; }
        .btn { padding: 12px 25px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; font-size: 16px; }
        .deposit-btn { background-color: #28a745; color: white; }
        .withdraw-btn { background-color: #dc3545; color: white; }

        .timer { margin-top: 40px; font-size: 3rem; background: #007bff; padding: 10px 40px; border-radius: 50px; box-shadow: 0 0 20px rgba(0, 123, 255, 0.5); }

        .main-container { margin-top: 60px; display: flex; flex-direction: column; align-items: center; gap: 20px; }
        .balance { font-size: 5rem; font-weight: bold; color: #ffcc00; }
        .income-status { font-size: 1.8rem; color: #00ffcc; border: 1px dashed #00ffcc; padding: 10px 30px; border-radius: 10px; }

        /* পপ-আপ (Full Page Modal) */
        .modal {
            display: none; 
            position: fixed; z-index: 10; left: 0; top: 0; 
            width: 100%; height: 100%; background-color: #002b1b; /* ভিডিওর মতো সবুজ আবাহন */
            overflow-y: auto;
        }

        .modal-header { display: flex; align-items: center; padding: 15px; background: #003d27; }
        .back-btn { font-size: 24px; cursor: pointer; margin-right: 15px; }

        .modal-body { padding: 20px; text-align: center; }
        .amount-box {
            background: #004d32; padding: 20px; border-radius: 15px; margin-top: 20px;
            font-size: 2rem; font-weight: bold; border: 2px solid #00ffcc;
        }

        .input-field {
            width: 90%; padding: 15px; margin-top: 20px; border-radius: 10px;
            border: none; font-size: 16px; background: #005a3b; color: white;
        }

        .action-submit {
            width: 90%; padding: 15px; margin-top: 30px; border-radius: 30px;
            border: none; background: #ffcc00; color: #002b1b; font-weight: bold; font-size: 18px; cursor: pointer;
        }

        .instruction { text-align: left; margin-top: 30px; font-size: 14px; color: #ccc; line-height: 1.6; }
    </style>
</head>
<body>

    <div class="header">
        <button class="btn deposit-btn" onclick="showPage('depositModal')">ডিপোজিট</button>
        <button class="btn withdraw-btn" onclick="showPage('withdrawModal')">উইথড্র</button>
    </div>

    <div class="timer" id="timer">24:00:00</div>

    <div class="main-container">
        <div class="balance" id="mainBalance">১০০০</div>
        <div class="income-status">মোট আয়: <span id="totalIncome">০</span> টাকা</div>
    </div>

    <div id="depositModal" class="modal">
        <div class="modal-header">
            <span class="back-btn" onclick="hidePage('depositModal')">←</span>
            <h3>Recharge (ডিপোজিট)</h3>
        </div>
        <div class="modal-body">
            <p>আপনার ডিপোজিট পরিমাণ</p>
            <div class="amount-box">১০০০ টাকা</div>
            <input type="text" class="input-field" placeholder="টাকার পরিমাণ লিখুন">
            <button class="action-submit">রিচার্জ নিশ্চিত করুন</button>
            <div class="instruction">
                <p>১. সঠিক পরিমাণ টাকা লিখুন।</p>
                <p>২. বিকাশ বা নগদ ব্যবহার করে পেমেন্ট করুন।</p>
            </div>
        </div>
    </div>

    <div id="withdrawModal" class="modal">
        <div class="modal-header">
            <span class="back-btn" onclick="hidePage('withdrawModal')">←</span>
            <h3>Withdrawal (উইথড্র)</h3>
        </div>
        <div class="modal-body">
            <p>আপনার উইথড্র পরিমাণ</p>
            <div class="amount-box">৭০০ টাকা</div>
            <input type="text" class="input-field" placeholder="আপনার একাউন্ট নম্বর দিন">
            <button class="action-submit" style="background: #28a745; color: white;">উইথড্র নিশ্চিত করুন</button>
            <div class="instruction">
                <p>১. ভুল নম্বর দিলে টাকা পাবেন না।</p>
                <p>২. উইথড্র হতে ১-২৪ ঘণ্টা সময় লাগতে পারে।</p>
            </div>
        </div>
    </div>

    <script>
        // টাইমার এবং টাকা যোগ হওয়ার সিস্টেম
        let balance = 1000;
        let income = 0;
        let timeLeft = 24 * 60 * 60;

        function updateEverything() {
            let h = Math.floor(timeLeft / 3600);
            let m = Math.floor((timeLeft % 3600) / 60);
            let s = timeLeft % 60;
            document.getElementById('timer').textContent = 
                `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;

            if (timeLeft <= 0) {
                income += 225;
                balance += 225;
                document.getElementById('totalIncome').textContent = income;
                document.getElementById('mainBalance').textContent = balance;
                timeLeft = 24 * 60 * 60;
            } else {
                timeLeft--;
            }
        }
        setInterval(updateEverything, 1000);

        // পেজ শো এবং হাইড ফাংশন
        function showPage(id) { document.getElementById(id).style.display = "block"; }
        function hidePage(id) { document.getElementById(id).style.display = "none"; }
    </script>

</body>
</html>
<script>
    // --- ১. মোডাল (Modal) কন্ট্রোল ---
    function showPage(id) {
        document.getElementById(id).style.display = "block";
    }

    function hidePage(id) {
        document.getElementById(id).style.display = "none";
    }

    // --- ২. ২৪ ঘণ্টার টাইমার লজিক ---
    function startTimer(duration, display) {
        let timer = duration, hours, minutes, seconds;
        setInterval(function () {
            hours = parseInt(timer / 3600, 10);
            minutes = parseInt((timer % 3600) / 60, 10);
            seconds = parseInt(timer % 60, 10);

            hours = hours < 10 ? "0" + hours : hours;
            minutes = minutes < 10 ? "0" + minutes : minutes;
            seconds = seconds < 10 ? "0" + seconds : seconds;

            display.textContent = hours + ":" + minutes + ":" + seconds;

            if (--timer < 0) {
                timer = duration; // সময় শেষ হলে আবার শুরু হবে
            }
        }, 1000);
    }

    window.onload = function () {
        let twentyFourHours = 24 * 60 * 60; // সেকেন্ডে হিসাব
        let display = document.querySelector('#timer');
        startTimer(twentyFourHours, display);
    };

    // --- ৩. ব্যালেন্স আপডেট (উদাহরণ) ---
    // যদি আপনার ইনপুট ফিল্ডের আইডি 'amountInput' হয়:
    function depositMoney() {
        let amount = document.querySelector('.input-field').value;
        let currentBalance = document.getElementById('mainBalance').innerText;
