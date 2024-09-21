

<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة تخمين الرقم المحسنة</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            text-align: center;
            padding: 50px;
            background-color: #f0f8ff;
            transition: background-color 0.5s ease;
        }
        h1 {
            color: #444;
            font-size: 2.5em;
            margin-bottom: 20px;
        }
        input {
            padding: 10px;
            font-size: 16px;
            border-radius: 10px;
            border: 2px solid #ccc;
            width: 200px;
            transition: border-color 0.3s ease;
        }
        input:focus {
            border-color: #ff9900;
            outline: none;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            margin: 10px;
            border-radius: 10px;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            opacity: 0.9;
        }
        #checkBtn {
            background-color: #ff9900;
            color: white;
        }
        #hintBtn {
            background-color: #33cc33;
            color: white;
        }
        #restartBtn {
            background-color: #3366ff;
            color: white;
        }
        p {
            font-size: 18px;
            color: #555;
        }
        #result {
            font-weight: bold;
            font-size: 20px;
            color: #d9534f;
        }
        #history {
            font-size: 14px;
            margin-top: 20px;
        }
        /* صناديق المحاولات */
        #attemptsLeft {
            font-weight: bold;
            color: #555;
        }
    </style>
</head>
<body>

<h1>لعبة تخمين الرقم المحسنة</h1>
<p>خمن رقمًا بين 1 إلى 100. لديك 10 محاولات فقط!</p>

<input type="number" id="guess" placeholder="أدخل تخمينك" min="1" max="100">
<button id="checkBtn" onclick="checkGuess()">تحقق</button>
<button id="hintBtn" onclick="hint()">تلميح</button>
<button id="restartBtn" onclick="restartGame()">إعادة التشغيل</button>

<p id="result"></p>
<p>المحاولات المتبقية: <span id="attemptsLeft">10</span></p>
<p id="history"></p>

<script>
    let numberToGuess = Math.floor(Math.random() * 100) + 1;
    let attempts = 0;
    let maxAttempts = 10;
    let history = [];

    // الألوان التي سيتم التبديل بينها
    const colors = ["#f0f8ff", "#ffe6e6", "#e6f7ff", "#ccf2ff", "#ffffcc"];
    const buttonColors = ["#ff9900", "#33cc33", "#3366ff", "#ff6666", "#ffcc00"];
    let colorIndex = 0;

    // دالة لتبديل الألوان تلقائيًا
    function changeColors() {
        colorIndex = (colorIndex + 1) % colors.length;
        document.body.style.backgroundColor = colors[colorIndex];
        document.getElementById("checkBtn").style.backgroundColor = buttonColors[colorIndex];
        document.getElementById("hintBtn").style.backgroundColor = buttonColors[(colorIndex + 1) % buttonColors.length];
        document.getElementById("restartBtn").style.backgroundColor = buttonColors[(colorIndex + 2) % buttonColors.length];
    }

    setInterval(changeColors, 5000); // تبديل الألوان كل 5 ثوانٍ

    function checkGuess() {
        let userGuess = parseInt(document.getElementById("guess").value);
        attempts++;
        let attemptsLeft = maxAttempts - attempts;

        if (isNaN(userGuess) || userGuess < 1 || userGuess > 100) {
            document.getElementById("result").innerHTML = "من فضلك، أدخل رقمًا صحيحًا بين 1 و 100!";
            return;
        }

        history.push(userGuess);
        document.getElementById("history").innerHTML = "سجل التخمينات: " + history.join(", ");

        if (attemptsLeft >= 0) {
            if (userGuess < numberToGuess) {
                document.getElementById("result").innerHTML = userGuess > numberToGuess - 10 ? "أنت قريب جدًا، الرقم أكبر!" : "الرقم أكبر!";
            } else if (userGuess > numberToGuess) {
                document.getElementById("result").innerHTML = userGuess < numberToGuess + 10 ? "أنت قريب جدًا، الرقم أصغر!" : "الرقم أصغر!";
            } else {
                document.getElementById("result").innerHTML = `تهانينا! لقد خمنت الرقم الصحيح ${numberToGuess} في ${attempts} محاولة.`;
                disableInput();
            }

            document.getElementById("attemptsLeft").innerHTML = attemptsLeft;

            if (attemptsLeft === 0 && userGuess !== numberToGuess) {
                document.getElementById("result").innerHTML = `لقد خسرت! الرقم الصحيح هو ${numberToGuess}.`;
                disableInput();
            }
        }
    }

    function disableInput() {
        document.getElementById("guess").disabled = true;
        document.getElementById("checkBtn").disabled = true;
        document.getElementById("hintBtn").disabled = true;
    }

    function restartGame() {
        numberToGuess = Math.floor(Math.random() * 100) + 1;
        attempts = 0;
        history = [];
        document.getElementById("guess").disabled = false;
        document.getElementById("checkBtn").disabled = false;
        document.getElementById("hintBtn").disabled = false;
        document.getElementById("result").innerHTML = "";
        document.getElementById("attemptsLeft").innerHTML = maxAttempts;
        document.getElementById("guess").value = "";
        document.getElementById("history").innerHTML = "";
    }

    function hint() {
        let rangeStart = Math.max(1, numberToGuess - 10);
        let rangeEnd = Math.min(100, numberToGuess + 10);
        document.getElementById("result").innerHTML = `التلميح: الرقم يقع بين ${rangeStart} و ${rangeEnd}.`;
    }
</script>

</body>
</html>
