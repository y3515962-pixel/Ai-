<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>شات ذكاء اصطناعي</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f4f4f4;
      padding: 20px;
      direction: rtl;
    }
    #chatbox {
      width: 100%;
      max-width: 600px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .message {
      margin: 10px 0;
    }
    .user {
      text-align: right;
      color: blue;
    }
    .bot {
      text-align: left;
      color: green;
    }
    input, button {
      padding: 10px;
      margin-top: 10px;
      width: 100%;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div id="chatbox">
    <h2> Ai تحدث مع دليل المعلومات</h2>
    <div id="messages"></div>
    <input type="text" id="userInput" placeholder="اكتب رسالتك هنا">
    <button onclick="sendMessage()">إرسال</button>
  </div>

  <script>
    const apiKey = "YOUR_OPENAI_API_KEY"; // ضع مفتاح API هنا

    async function sendMessage() {
      const input = document.getElementById("userInput");
      const message = input.value;
      if (!message) return;

      showMessage(message, "user");
      input.value = "";

      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${apiKey}`
        },
        body: JSON.stringify({
          model: "gpt-3.5-turbo",
          messages: [{ role: "user", content: message }]
        })
      });

      const data = await response.json();
      const reply = data.choices[0].message.content;
      showMessage(reply, "bot");
    }

    function showMessage(text, sender) {
      const messages = document.getElementById("messages");
      const msg = document.createElement("div");
      msg.className = `message ${sender}`;
      msg.textContent = text;
      messages.appendChild(msg);
    }
  </script>
</body>
</html>
