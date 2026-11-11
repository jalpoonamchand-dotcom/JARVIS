
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JARVIS AI</title>
<style>
body {
  margin: 0;
  background: #07111f;
  color: #e7f6ff;
  font-family: Arial, sans-serif;
  text-align: center;
  padding: 35px 15px;
}
h1 { color: #00d9ff; letter-spacing: 4px; }
.panel {
  max-width: 450px;
  margin: 25px auto;
  padding: 22px;
  border: 1px solid #00bde8;
  border-radius: 16px;
  background: #0c1d30;
}
button {
  display: block;
  width: 100%;
  padding: 14px;
  margin: 12px 0;
  border: 0;
  border-radius: 9px;
  background: #00bde8;
  color: #00121c;
  font-size: 16px;
  font-weight: bold;
}
#status, #result {
  overflow-wrap: anywhere;
  line-height: 1.5;
}
</style>
</head>
<body>

<h1>J.A.R.V.I.S</h1>
<p>Personal AI Assistant</p>

<div class="panel">
  <h2>Microphone Test</h2>
  <p id="status">Microphone not tested yet.</p>
  <button onclick="testMic()">Test Microphone</button>
  <h2>Voice Recognition</h2>
  <p id="result">Your voice text will appear here.</p>
  <button onclick="startVoice()">Start Listening</button>
</div>

<script>
async function testMic() {
  const status = document.getElementById("status");
  status.textContent = "Requesting microphone permission...";

  try {
    const stream = await navigator.mediaDevices.getUserMedia({
      audio: true
    });
    status.textContent = "Microphone permission granted!";
    stream.getTracks().forEach(track => track.stop());
  } catch (error) {
    status.textContent = "Microphone error: " + error.name;
  }
}

function startVoice() {
  const result = document.getElementById("result");
  const Speech = window.SpeechRecognition ||
                 window.webkitSpeechRecognition;

  if (!Speech) {
    result.textContent =
      "Voice recognition is not supported in this browser.";
    return;
  }

  const recognition = new Speech();
  recognition.lang = "hi-IN";
  recognition.interimResults = true;

  result.textContent = "Listening... Speak now.";

  recognition.onresult = (event) => {
    let text = "";
    for (let i = event.resultIndex; i < event.results.length; i++) {
      text += event.results[i][0].transcript;
    }
    result.textContent = text;
  };

  recognition.onerror = (event) => {
    result.textContent = "Voice error: " + event.error;
  };

  recognition.onend = () => {
    result.textContent += " (Listening stopped)";
  };

  try {
    recognition.start();
  } catch (error) {
    result.textContent = "Could not start: " + error.message;
  }
}
</script>

</body>
</html>
