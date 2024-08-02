<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ad Income App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #000;
            color: #fff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            padding: 10px;
        }

        .container {
            display: none;
            flex-direction: column;
            align-items: center;
            margin: 20px;
            width: 100%;
            max-width: 300px;
        }

        .header {
            display: flex;
            justify-content: space-between;
            width: 100%;
            align-items: center;
        }

        .menu-icon {
            cursor: pointer;
            font-size: 24px;
        }

        .menu {
            display: none;
            flex-direction: column;
            align-items: center;
            margin-top: 10px;
        }

        .menu-item {
            padding: 10px;
            margin: 5px;
            width: 100%;
            max-width: 300px;
            background-color: #007bff;
            color: white;
            text-align: center;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .menu-item:hover {
            background-color: #0056b3;
        }

        .contact-number {
            margin-top: 10px;
            font-size: 14px;
            color: #ccc;
        }

        .content {
            display: none;
            flex-direction: column;
            align-items: center;
            width: 100%;
            max-width: 300px;
            margin-top: 20px;
        }

        .button, .withdraw-option {
            padding: 10px 20px;
            margin: 10px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            background-color: #007bff;
            color: white;
            width: 100%;
            max-width: 300px;
        }

        .button:hover, .withdraw-option:hover {
            background-color: #0056b3;
        }

        .input-field {
            padding: 10px;
            margin: 10px;
            font-size: 16px;
            border-radius: 5px;
            border: 1px solid #007bff;
            width: 100%;
        }

        .error-message {
            color: red;
            margin-top: 10px;
        }
    </style>
</head>
<body>

    <!-- Welcome Screen -->
    <div class="container" id="welcome-container">
        <h1>Welcome to Ad Income App</h1>
        <button class="button" id="button_continue">Continue</button>
    </div>

    <!-- Login and Signup -->
    <div class="container" id="auth-container">
        <h2>Login / Signup</h2>
        <div id="button_google_login"></div>
        <input type="email" id="email_input" class="input-field" placeholder="Enter your email">
        <button class="button" id="button_email_login">Login with Email</button>
        <p id="error_message" class="error-message"></p>
    </div>

    <!-- Main App Screen -->
    <div class="container" id="main-container">
        <div class="header">
            <h2>Ad Income App</h2>
            <div class="menu-icon" id="menu-icon">⋮</div>
        </div>

        <div class="menu" id="menu">
            <button class="menu-item" id="menu_account">Account</button>
            <button class="menu-item" id="menu_play_ad">Play Ad</button>
            <button class="menu-item" id="menu_withdraw">Withdraw Method</button>
            <button class="menu-item" id="menu_logout">Logout</button>
            <p class="contact-number">Contact: 01704626946</p>
        </div>

        <div class="content" id="account-content">
            <h2>Account</h2>
            <p>Total Earnings: $<span id="earnings">0.00</span></p>
        </div>

        <div class="content" id="ad-content">
            <h2>Watch Ads and Earn Money</h2>
            <button class="button" id="button_watch_ad">Watch Ad</button>
        </div>

        <div class="content" id="withdraw-content">
            <h2>Withdrawal Options</h2>
            <button class="withdraw-option" id="button_bkash">Bkash</button>
            <button class="withdraw-option" id="button_nagad">Nagad</button>
            <button class="withdraw-option" id="button_rocket">Rocket</button>
            <input type="text" id="phone_number" class="input-field" placeholder="Enter your phone number">
            <input type="number" id="withdraw_amount" class="input-field" placeholder="Enter amount to withdraw">
            <button class="button" id="button_withdraw">Withdraw</button>
        </div>
    </div>

    <script src="https://accounts.google.com/gsi/client" async defer></script>
    <script>
        let totalEarnings = 0;
        let loggedIn = false;

        function showContainer(containerId) {
            document.querySelectorAll('.container').forEach(container => {
                container.style.display = 'none';
            });
            document.getElementById(containerId).style.display = 'flex';
        }

        function showContent(contentId) {
            document.querySelectorAll('.content').forEach(content => {
                content.style.display = 'none';
            });
            document.getElementById(contentId).style.display = 'flex';
        }

        function handleLogin() {
            loggedIn = true;
            alert('Logged in successfully.');
            showContainer('main-container');
        }

        function handleLogout() {
            loggedIn = false;
            alert('Logged out successfully.');
            showContainer('auth-container');
        }

        document.getElementById('button_watch_ad').addEventListener('click', function() {
            if (!loggedIn) {
                alert('Please log in first.');
                showContainer('auth-container');
                return;
            }
            alert('Ad will be shown here.');
            // এখানে অ্যাড দেখানোর কোড আসবে
            totalEarnings += 0.10;
            document.getElementById('earnings').textContent = totalEarnings.toFixed(2);
        });

        document.getElementById('button_withdraw').addEventListener('click', function() {
            if (!loggedIn) {
                alert('Please log in first.');
                showContainer('auth-container');
                return;
            }
            const phoneNumber = document.getElementById('phone_number').value;
            const amount = parseFloat(document.getElementById('withdraw_amount').value);
            if (phoneNumber === '' || isNaN(amount) || amount <= 0) {
                alert('Please enter valid phone number and amount.');
                return;
            }
            if (amount > totalEarnings) {
                alert('Insufficient funds.');
                return;
            }
            totalEarnings -= amount;
            document.getElementById('earnings').textContent = totalEarnings.toFixed(2);
            alert('Withdrawal request submitted for number: ' + phoneNumber + ' with amount: $' + amount.toFixed(2));
        });

        document.getElementById('button_bkash').addEventListener('click', function() {
            document.getElementById('phone_number').placeholder = 'Enter Bkash number';
        });

        document.getElementById('button_nagad').addEventListener('click', function() {
            document.getElementById('phone_number').placeholder = 'Enter Nagad number';
        });

        document.getElementById('button_rocket').addEventListener('click', function() {
            document.getElementById('phone_number').placeholder = 'Enter Rocket number';
        });

        document.getElementById('menu-icon').addEventListener('click', function() {
            const menu = document.getElementById('menu');
            menu.style.display = menu.style.display === 'flex' ? 'none' : 'flex';
        });

        document.getElementById('menu_account').addEventListener('click', function() {
            showContent('account-content');
        });

        document.getElementById('menu_play_ad').addEventListener('click', function() {
            showContent('ad-content');
        });

        document.getElementById('menu_withdraw').addEventListener('click', function() {
            showContent('withdraw-content');
        });

        document.getElementById('menu_logout').addEventListener('click', handleLogout);

        document.getElementById('button_continue').addEventListener('click', function() {
            showContainer('auth-container');
        });

        document.getElementById('button_email_login').addEventListener('click', function() {
            const email = document.getElementById('email_input').value;
            const errorMessage = document.getElementById('error_message');
            
            if (validateEmail(email)) {
                handleLogin();
            } else {
                errorMessage.textContent = 'Invalid email address. Please check and try again.';
            }
        });

        function validateEmail(email) {
            const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailPattern.test(email);
        }

        function handleGoogleLogin(response) {
            console.log(response);
            handleLogin();
        }

        window.onload = function() {
            google.accounts.id.initialize({
                client_id: 'YOUR_GOOGLE_CLIENT_ID',
                callback: handleGoogleLogin
            });
            google.accounts.id.renderButton(
                document.getElementById('button_google_login'),
                { theme: 'outline', size: 'large' }
            );
        };

        showContainer('welcome-container');
    </script>

</body>
</html># adsincome.io
