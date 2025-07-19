<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Digitalart98 - Temp Mail</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f1f1f1;
      margin: 0;
      padding: 20px;
      text-align: center;
    }
    .container {
      max-width: 600px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
    }
    .email-box {
      font-size: 18px;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      margin: 20px 0;
      background-color: #f9f9f9;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin: 10px;
      cursor: pointer;
    }
    .inbox {
      text-align: left;
      margin-top: 20px;
    }
    .inbox-item {
      padding: 10px;
      border-bottom: 1px solid #ddd;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>📧 Digitalart98 Temp Mail</h1>
    <div class="email-box" id="emailBox">Loading...</div>
    <button onclick="copyEmail()">📋 Copy Email</button>
    <button onclick="generateEmail()">🔄 New Email</button>
    <button onclick="fetchInbox()">📩 Refresh Inbox</button><div class="inbox" id="inbox">
  <h3>Inbox:</h3>
  <div id="inboxItems">No messages yet.</div>
</div>

  </div>  <script>
    let login = "";
    let domain = "";

    function generateEmail() {
      fetch("https://www.1secmail.com/api/v1/?action=genRandomMailbox&count=1")
        .then(res => res.json())
        .then(data => {
          const email = data[0];
          [login, domain] = email.split("@");
          document.getElementById("emailBox").innerText = email;
          document.getElementById("inboxItems").innerHTML = "No messages yet.";
        });
    }

    function copyEmail() {
      const email = document.getElementById("emailBox").innerText;
      navigator.clipboard.writeText(email);
      alert("Email copied: " + email);
    }

    function fetchInbox() {
      if (!login || !domain) return alert("Generate email first!");

      fetch(`https://www.1secmail.com/api/v1/?action=getMessages&login=${login}&domain=${domain}`)
        .then(res => res.json())
        .then(data => {
          const inbox = document.getElementById("inboxItems");
          if (data.length === 0) {
            inbox.innerHTML = "No messages yet.";
            return;
          }
          inbox.innerHTML = "";
          data.forEach(msg => {
            const div = document.createElement("div");
            div.className = "inbox-item";
            div.innerHTML = `<strong>From:</strong> ${msg.from}<br><strong>Subject:</strong> ${msg.subject}`;
            inbox.appendChild(div);
          });
        });
    }

    generateEmail();
  </script></body>
</html>
