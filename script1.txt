const users = JSON.parse(localStorage.getItem('users')) || {};

function showCreateUser() {
    document.getElementById('login-section').style.display = 'none';
    document.getElementById('create-user-section').style.display = 'block';
}

function showResetPassword() {
    document.getElementById('login-section').style.display = 'none';
    document.getElementById('reset-password-section').style.display = 'block';
}

function backToLogin() {
    document.getElementById('create-user-section').style.display = 'none';
    document.getElementById('reset-password-section').style.display = 'none';
    document.getElementById('login-section').style.display = 'block';
}

function createUser() {
    const username = document.getElementById('new-username').value;
    const password = document.getElementById('new-password').value;
    if (users.hasOwnProperty(username)) {
        alert('Username already exists!');
        return;
    }
    users[username] = { password, balance: 0 };
    localStorage.setItem('users', JSON.stringify(users));
    alert('User created successfully!');
    backToLogin();
}

function resetPassword() {
    const username = document.getElementById('reset-username').value;
    const newPassword = document.getElementById('reset-password').value;
    if (users.hasOwnProperty(username)) {
        users[username].password = newPassword;
        localStorage.setItem('users', JSON.stringify(users));
        alert('Password reset successfully!');
        backToLogin();
    } else {
        alert('Username does not exist!');
    }
}

function login() {
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;
    if (users.hasOwnProperty(username) && users[username].password === password) {
        sessionStorage.setItem('currentUser', username);
        document.getElementById('login-section').style.display = 'none';
        document.getElementById('main-menu').style.display = 'block';
    } else {
        alert('Invalid username or password!');
    }
}

function logout() {
    sessionStorage.removeItem('currentUser');
    document.getElementById('main-menu').style.display = 'none';
    document.getElementById('login-section').style.display = 'block';
}

function viewBalance() {
    const currentUser = sessionStorage.getItem('currentUser');
    alert('Your current balance is: $' + users[currentUser].balance);
}

function deposit() {
    const amount = parseFloat(prompt('Enter amount to deposit:'));
    const currentUser = sessionStorage.getItem('currentUser');
    if (amount > 0) {
        users[currentUser].balance += amount;
        localStorage.setItem('users', JSON.stringify(users));
        alert('Deposited successfully! New balance: $' + users[currentUser].balance);
    } else {
        alert('Invalid amount!');
    }
}

function withdraw() {
    const amount = parseFloat(prompt('Enter amount to withdraw:'));
    const currentUser = sessionStorage.getItem('currentUser');
    if (amount > 0 && users[currentUser].balance >= amount) {
        users[currentUser].balance -= amount;
        localStorage.setItem('users', JSON.stringify(users));
        alert('Withdrawn successfully! New balance: $' + users[currentUser].balance);
    } else {
        alert('Invalid amount or insufficient funds!');
    }
}

function transfer() {
    const recipient = prompt('Enter recipient username:');
    const amount = parseFloat(prompt('Enter amount to transfer:'));
    const currentUser = sessionStorage.getItem('currentUser');
    if (users.hasOwnProperty(recipient) && amount > 0 && users[currentUser].balance >= amount) {
        users[currentUser].balance -= amount;
        users[recipient].balance += amount;
        localStorage.setItem('users', JSON.stringify(users));
        alert('Transferred successfully! Your new balance: $' + users[currentUser].balance);
    } else {
        alert('Invalid recipient or amount, or insufficient funds!');
    }
}

function customerSupport() {
    alert('For customer support, please call 1-800-123-4567 or email support@example.com.');
}

function changeLanguage(lang) {
    alert('Language changed to ' + lang + '. Please implement the translation in your UI.');
}
