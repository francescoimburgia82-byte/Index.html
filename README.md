
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JARVIS AI</title>
<style>
*{box-sizing:border-box}
body{
 margin:0;background:#050d18;color:#00d9ff;
 font-family:Arial,sans-serif;text-align:center;
}
header{padding:22px;font-size:28px;letter-spacing:5px}
.core{
 width:180px;height:180px;margin:30px auto;
 border:5px solid #00d9ff;border-radius:50%;
 box-shadow:0 0 25px #00d9ff,inset 0 0 25px #00d9ff;
 display:flex;align-items:center;justify-content:center;
 font-size:35px;animation:pulse 2s infinite;
}
@keyframes pulse{
 50%{box-shadow:0 0 45px #00d9ff,inset 0 0 35px #00d9ff}
}
#status{color:#8beeff;margin:15px}
#chat{
 margin:20px auto;padding:12px;width:90%;max-width:500px;
 height:240px;overflow:auto;border:1px solid #087b9b;
 text-align:left;border-radius:12px;
}
.message{margin:10px 0;line-height:1.4}
input{
 width:65%;padding:14px;border-radius:20px;
 border:1px solid #00d9ff;background:#102235;color:white;
}
button{
 padding:13px 17px;margin:5px;border:0;border-radius:20px;
 background:#00bfe8;color:#00121d;font-weight:bold;
}
</style>
</head>
<body>
<header>J.A.R.V.I.S.</header>
<div class="core">AI</div>
<div id="status">SISTEMA ONLINE</div>
<div id="chat">
 <div class="message">JARVIS: Buongiorno. Sono pronto a servirti.</div>
</div>
<input id="msg" placeholder="Scrivi un messaggio...">
<button onclick="send()">INVIA</button>
<button onclick="listen()">🎤</button>
<script>
const chat=document.getElementById("chat");
const statusText=document.getElementById("status");

function speak(text){
 if(!("speechSynthesis" in window))return;
 speechSynthesis.cancel();
 let voice=new SpeechSynthesisUtterance(text);
 voice.lang="it-IT";
 voice.rate=0.95;
 speechSynthesis.speak(voice);
}

function reply(text){
 let t=text.toLowerCase();
 if(t.includes("ciao")||t.includes("buongiorno"))
  return "Buongiorno. È un piacere sentirti. Come posso aiutarti?";
 if(t.includes("come stai"))
  return "Tutto perfettamente funzionante. Grazie per avermelo chiesto.";
 if(t.includes("chi sei"))
  return "Sono JARVIS, il tuo assistente virtuale.";
 if(t.includes("ora")||t.includes("orario"))
  return "Sono le "+new Date().toLocaleTimeString("it-IT");
 if(t.includes("data"))
  return "Oggi è "+new Date().toLocaleDateString("it-IT");
 if(t.includes("grazie"))
  return "È un piacere aiutarti.";
 if(t.includes("apri chatgpt"))
  {window.open("https://chatgpt.com","_blank");return "Apro ChatGPT.";}
 return "Ho ricevuto il tuo messaggio. Per risposte intelligenti complete, in futuro potremo collegarmi a un modello AI.";
}

function addMessage(who,text){
 let div=document.createElement("div");
 div.className="message";
 div.textContent=who+": "+text;
 chat.appendChild(div);
 chat.scrollTop=chat.scrollHeight;
}

function send(){
 let input=document.getElementById("msg");
 let text=input.value.trim();
 if(!text)return;
 addMessage("TU",text);
 input.value="";
 let answer=reply(text);
 addMessage("JARVIS",answer);
 speak(answer);
}

function listen(){
 const Recognition=window.SpeechRecognition||window.webkitSpeechRecognition;
 if(!Recognition){
  statusText.textContent="RICONOSCIMENTO VOCALE NON DISPONIBILE";
  return;
 }
 let recognition=new Recognition();
 recognition.lang="it-IT";
 recognition.onstart=()=>statusText.textContent="TI STO ASCOLTANDO...";
 recognition.onresult=(event)=>{
  document.getElementById("msg").value=event.results[0][0].transcript;
  send();
 };
 recognition.onerror=()=>statusText.textContent="MICROFONO NON DISPONIBILE";
 recognition.onend=()=>statusText.textContent="SISTEMA ONLINE";
 recognition.start();
}
document.getElementById("msg").addEventListener("keydown",e=>{
 if(e.key==="Enter")send();
});
</script>
</body>
</html>
