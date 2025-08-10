<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text-to-Speech Machine</title>
    <link rel="stylesheet" href="style.css">
  <style>
    body {
    font-family: Arial, sans-serif;
    background: #f5f5f5;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

h1 {
    color: #333;
}

textarea {
    width: 100%;
    max-width: 500px;
    height: 150px;
    padding: 10px;
    font-size: 1rem;
    resize: none;
    margin-bottom: 10px;
}

select, button {
    padding: 10px;
    font-size: 1rem;
    margin: 5px;
    cursor: pointer;
}

.controls {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

button {
    background-color: #4CAF50;
    border: none;
    color: white;
    border-radius: 5px;
}

button:hover {
    background-color: #45a049;
}

button:nth-child(3) {
    background-color: #e74c3c;
}

button:nth-child(3):hover {
    background-color: #c0392b;
}

  </style>
</head>
<body>
    <h1>🗣 Convert Your Text into Speech</h1>
    
    <textarea id="text" placeholder="Type something to speak..."></textarea>
    
    <div class="controls">
        <select id="voiceSelect"></select>
        <button onclick="speakText()">▶ Speak</button>
        <button onclick="stopSpeaking()">⏹ Stop</button>
    </div>

    <script>
        let synth = window.speechSynthesis;
        let voiceSelect = document.getElementById('voiceSelect');
        let voices = [];

        function populateVoices() {
            voices = synth.getVoices();
            voiceSelect.innerHTML = '';
            voices.forEach((voice, i) => {
                let option = document.createElement('option');
                option.value = i;
                option.textContent = `${voice.name} (${voice.lang})`;
                voiceSelect.appendChild(option);
            });
        }

        populateVoices();
        if (speechSynthesis.onvoiceschanged !== undefined) {
            speechSynthesis.onvoiceschanged = populateVoices;
        }

        function speakText() {
            let text = document.getElementById('text').value;
            if (!text.trim()) {
                alert("Please enter some text!");
                return;
            }
            let utterance = new SpeechSynthesisUtterance(text);
            utterance.voice = voices[voiceSelect.value];
            synth.cancel(); // stop previous speech
            synth.speak(utterance);
        }

        function stopSpeaking() {
            synth.cancel();
        }
    </script>
</body>
</html>
