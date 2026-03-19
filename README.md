<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Kiki AI</title>
<style>
* { -webkit-text-size-adjust: 100%; }
body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: #0f172a;
  color: white;
}

/* HEADER */
.header {
  padding: 10px;
  background: #020617;
  text-align: center;
  border-bottom: 1px solid #1e293b;
}
.kiki-title {
  font-size: 17px;
  font-weight: bold;
}
.kiki-sub {
  font-size: 12px;
  color: #38bdf8;
  margin-top: 2px;
}
.kiki-sub::before { content: "● "; color: #22c55e; }

/* CHAT */
.chat {
  height: 75vh;
  overflow-y: auto;
  padding: 12px;
  display: flex;
  flex-direction: column;
}
.msg {
  padding: 12px;
  margin: 6px;
  border-radius: 12px;
  max-width: 80%;
  line-height: 1.5;
  font-size: 15px;
  white-space: pre-wrap;
}
.me { background: #3b82f6; align-self: flex-end; }
.ai { background: #1e293b; align-self: flex-start; }

/* IMAGE */
.chat-img { max-width: 220px; border-radius: 10px; margin-top: 5px; }

/* INPUT */
.input-box {
  position: fixed;
  bottom: 0;
  width: 100%;
  display: flex;
  align-items: center;
  background: #020617;
  padding: 8px 10px;
}
textarea {
  flex: 1;
  padding: 8px 10px;
  border-radius: 16px;
  border: none;
  font-size: 14px;
  line-height: 1.4;
  resize: none;
  outline: none;
  height: 34px;
  max-height: 70px;
}

/* BUTTON */
.plus, .send {
  width: 42px;
  height: 42px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 6px;
  font-size: 18px;
  cursor: pointer;
}
.plus { background: #1e293b; }
.send { background: #3b82f6; }

/* LOADING */
.loading { font-style: italic; opacity: 0.7; }

#imgInput { display: none; }
</style>
</head>

<body>

<div class="header">
  <div class="kiki-title">Kiki AI</div>
  <div class="kiki-sub">Online • Siap membantu</div>
</div>

<div class="chat" id="chat"></div>

<div class="input-box">
  <div class="plus" onclick="document.getElementById('imgInput').click()">+</div>
  <textarea id="msg" placeholder="Tanya apa saja..."></textarea>
  <div class="send" onclick="send()">➤</div>
  <input type="file" id="imgInput" accept="image/*">
</div>

<script>
let chat = document.getElementById("chat");
let textarea = document.getElementById("msg");

// AUTO HEIGHT TEXTAREA
textarea.addEventListener("input", function(){
  this.style.height = "34px";
  this.style.height = Math.min(this.scrollHeight, 70) + "px";
});

// ENTER KIRIM PESAN
textarea.addEventListener("keydown", function(e){
  if(e.key === "Enter" && !e.shiftKey){
    e.preventDefault();
    send();
  }
});

// SEND
function send(){
  let text = textarea.value;
  let file = document.getElementById("imgInput").files[0];

  if(text.trim() !== ""){
    addMsg(text, "me");
    textarea.value = "";
    textarea.style.height = "34px";

    let loading = addLoading();
    setTimeout(()=>{
      loading.remove();
      typeText(ai(text));
    },700);
  }

  if(file){
    let reader = new FileReader();
    reader.onload = function(e){
      addImage(e.target.result, "me");
      let loading = addLoading();
      setTimeout(()=>{
        loading.remove();
        typeText("Gambar ini terlihat seperti ilustrasi atau karakter bergaya gaming 🎮. Kemungkinan berasal dari game seperti Free Fire atau sejenisnya.");
      },700);
    };
    reader.readAsDataURL(file);
  }

  document.getElementById("imgInput").value = "";
}

// ADD TEXT MESSAGE
function addMsg(text, type){
  let div = document.createElement("div");
  div.className = "msg " + type;
  div.innerText = text;
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

// ADD IMAGE
function addImage(src, type){
  let div = document.createElement("div");
  div.className = "msg " + type;
  let img = document.createElement("img");
  img.src = src;
  img.className = "chat-img";
  div.appendChild(img);
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

// LOADING
function addLoading(){
  let div = document.createElement("div");
  div.className = "msg ai loading";
  div.innerText = "Mencari jawaban...";
  chat.appendChild(div);
  return div;
}

// TYPING EFFECT
function typeText(text){
  let div = document.createElement("div");
  div.className = "msg ai";
  chat.appendChild(div);

  let i = 0;
  function typing(){
    if(i < text.length){
      div.innerText += text[i];
      i++;
      setTimeout(typing, 10);
    }
  }
  typing();
}

// AI FUNCTION
function ai(text){
  text = text.toLowerCase();
  if(text.includes("halo") || text.includes("hai")){
    return "Halo! 👋\nAku Kiki AI. Tanyakan apa saja, aku akan jawab dengan lengkap dan rapi.";
  }
  if(text.includes("siapa kamu")){
    return "Aku Kiki AI 🤖, chatbot berbasis web untuk membantu menjawab pertanyaanmu.";
  }
  if(text.includes("apa itu hp")){
    return "HP (Handphone) adalah alat komunikasi modern.\nFungsi utama: Telepon, Chat, Internet.\nFungsi tambahan: Game, Video, Belajar.";
  }
  if(text.includes("apa itu ai")){
    return "AI (Artificial Intelligence) adalah kecerdasan buatan yang memungkinkan komputer berpikir seperti manusia.\nContoh: Chatbot, Game pintar, Rekomendasi video.";
  }
  if(text.includes("internet")){
    return "Internet adalah jaringan global untuk komunikasi, mencari informasi, dan hiburan.";
  }
  if(text.includes("game")){
    return "Game adalah permainan digital. Bisa berupa Action, Petualangan, atau Strategi. Selain hiburan, game melatih fokus dan strategi.";
  }
  if(text.includes("sekolah")){
    return "Sekolah adalah tempat belajar. Mata pelajaran utama: Matematika, Bahasa, Sains. Penting untuk masa depan.";
  }
  if(text.includes("makanan")){
    return "Makanan adalah kebutuhan utama manusia. Jenis: Karbohidrat, Protein, Vitamin. Penting untuk energi dan kesehatan.";
  }
  let jawaban = [
    "Pertanyaan kamu menarik 👍, coba jelaskan lebih detail supaya aku bisa jawab lebih lengkap.",
    "Aku mengerti maksudmu, tapi butuh sedikit info tambahan agar jawabannya lebih akurat.",
    "Topik ini luas 😊, coba perjelas pertanyaannya agar jawaban lebih tepat."
  ];
  return jawaban[Math.floor(Math.random() * jawaban.length)];
}
</script>

</body>
</html>
