<!DOCTYPE html>  
<html lang="ja">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>あなたのタイプ診断</title>  
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&display=swap" rel="stylesheet">  
<style>  
:root {  
  --line-green: #06C755;  
  --line-green-dark: #04a844;  
  --line-green-pale: #e8f9ef;  
  --bg: #efeff4;  
  --white: #ffffff;  
  --text: #1a1a1a;  
  --text-mid: #555555;  
  --text-light: #999999;  
  --header-bg: #06C755;  
  --bubble-ai: #ffffff;  
  --bubble-user: #06C755;  
  --shadow-sm: 0 1px 4px rgba(0,0,0,0.10);  
  --shadow-md: 0 4px 16px rgba(0,0,0,0.10);  
}  
  
* { box-sizing: border-box; margin: 0; padding: 0; }  
  
body {  
  font-family: 'Noto Sans JP', sans-serif;  
  background: var(--bg);  
  color: var(--text);  
  min-height: 100vh;  
  overflow-x: hidden;  
}  
  
.app {  
  max-width: 480px;  
  margin: 0 auto;  
  min-height: 100vh;  
  display: flex;  
  flex-direction: column;  
  background: var(--bg);  
}  
  
/* ── SCREEN管理 ── */  
.screen { display: none; }  
.screen.active { display: flex; flex-direction: column; }  
  
/* ═══════════════════════════  
   共通ヘッダー  
══════════════════════════════ */  
.line-header {  
  background: var(--line-green);  
  padding: 14px 16px;  
  display: flex;  
  align-items: center;  
  gap: 12px;  
  position: sticky;  
  top: 0;  
  z-index: 100;  
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);  
  flex-shrink: 0;  
}  
.line-header-icon {  
  width: 40px; height: 40px;  
  background: white;  
  border-radius: 50%;  
  display: flex; align-items: center; justify-content: center;  
  font-size: 22px;  
  flex-shrink: 0;  
}  
.line-header-info { flex: 1; }  
.line-header-name { font-size: 15px; font-weight: 700; color: white; }  
.line-header-status { font-size: 11px; color: rgba(255,255,255,0.85); }  
  
/* ═══════════════════════════  
   トップ画面（チャット風）  
══════════════════════════════ */  
#screen-top {  
  min-height: 100vh;  
}  
#screen-top.active { display: flex; flex-direction: column; }  
  
.top-chat-area {  
  flex: 1;  
  padding: 20px 16px;  
  display: flex;  
  flex-direction: column;  
  gap: 16px;  
  overflow-y: auto;  
}  
  
.date-chip {  
  text-align: center;  
  font-size: 11px;  
  color: var(--text-light);  
  background: rgba(0,0,0,0.07);  
  display: inline-block;  
  margin: 0 auto 4px;  
  padding: 3px 14px;  
  border-radius: 10px;  
}  
  
.msg-row {  
  display: flex;  
  align-items: flex-end;  
  gap: 8px;  
  animation: fadeUp 0.4s ease both;  
}  
.msg-row.user-row { flex-direction: row-reverse; }  
  
.av {  
  width: 38px; height: 38px;  
  border-radius: 50%;  
  background: var(--line-green);  
  display: flex; align-items: center; justify-content: center;  
  font-size: 20px;  
  flex-shrink: 0;  
}  
.av.user-av { background: #c8e6d8; }  
  
.bubble {  
  max-width: 78%;  
  padding: 10px 14px;  
  border-radius: 18px;  
  font-size: 14px;  
  line-height: 1.7;  
  box-shadow: var(--shadow-sm);  
}  
.bubble.ai-bubble {  
  background: var(--bubble-ai);  
  color: var(--text);  
  border-bottom-left-radius: 4px;  
}  
.bubble.user-bubble {  
  background: var(--bubble-user);  
  color: white;  
  border-bottom-right-radius: 4px;  
}  
  
.top-types-grid {  
  display: grid;  
  grid-template-columns: 1fr 1fr;  
  gap: 8px;  
  margin: 4px 0 4px 46px;  
  animation: fadeUp 0.4s ease 0.3s both;  
}  
.type-card-mini {  
  background: white;  
  border-radius: 14px;  
  padding: 12px 10px;  
  text-align: center;  
  box-shadow: var(--shadow-sm);  
  cursor: default;  
}  
.type-card-mini .tc-emoji { font-size: 26px; }  
.type-card-mini .tc-name { font-size: 11px; color: var(--text-mid); margin-top: 4px; font-weight: 500; }  
  
.top-start-area {  
  padding: 12px 16px 32px;  
  display: flex;  
  flex-direction: column;  
  gap: 10px;  
}  
  
.start-big-btn {  
  background: var(--line-green);  
  color: white;  
  border: none;  
  border-radius: 24px;  
  padding: 15px;  
  font-size: 16px;  
  font-family: 'Noto Sans JP', sans-serif;  
  font-weight: 700;  
  cursor: pointer;  
  letter-spacing: 0.04em;  
  box-shadow: 0 4px 16px rgba(6,199,85,0.35);  
  transition: all 0.2s ease;  
}  
.start-big-btn:hover { background: var(--line-green-dark); transform: translateY(-2px); }  
.start-big-btn:active { transform: scale(0.98); }  
  
.start-note {  
  text-align: center;  
  font-size: 12px;  
  color: var(--text-light);  
}  
  
/* ═══════════════════════════  
   質問画面  
══════════════════════════════ */  
#screen-quiz { min-height: 100vh; }  
#screen-quiz.active { display: flex; flex-direction: column; }  
  
.quiz-chat-area {  
  flex: 1;  
  padding: 16px 16px 8px;  
  display: flex;  
  flex-direction: column;  
  gap: 14px;  
  overflow-y: auto;  
}  
  
.progress-bar-wrap {  
  padding: 10px 16px 0;  
  display: flex;  
  align-items: center;  
  gap: 10px;  
  flex-shrink: 0;  
}  
.progress-label { font-size: 12px; color: var(--text-light); white-space: nowrap; }  
.progress-track {  
  flex: 1;  
  height: 4px;  
  background: rgba(0,0,0,0.12);  
  border-radius: 2px;  
  overflow: hidden;  
}  
.progress-fill {  
  height: 100%;  
  background: var(--line-green);  
  border-radius: 2px;  
  transition: width 0.5s cubic-bezier(0.4,0,0.2,1);  
}  
  
.choices-bottom {  
  padding: 8px 16px 24px;  
  display: flex;  
  flex-direction: column;  
  gap: 8px;  
  flex-shrink: 0;  
}  
  
.choice-btn {  
  background: white;  
  border: 2px solid var(--line-green);  
  color: var(--line-green);  
  border-radius: 24px;  
  padding: 12px 18px;  
  font-size: 14px;  
  font-family: 'Noto Sans JP', sans-serif;  
  font-weight: 500;  
  cursor: pointer;  
  text-align: left;  
  display: flex;  
  align-items: center;  
  gap: 10px;  
  transition: all 0.18s ease;  
  box-shadow: var(--shadow-sm);  
  animation: fadeUp 0.3s ease both;  
}  
.choice-btn:nth-child(1) { animation-delay: 0.04s; }  
.choice-btn:nth-child(2) { animation-delay: 0.08s; }  
.choice-btn:nth-child(3) { animation-delay: 0.12s; }  
.choice-btn:nth-child(4) { animation-delay: 0.16s; }  
.choice-btn:nth-child(5) { animation-delay: 0.20s; }  
  
.choice-btn:hover {  
  background: var(--line-green);  
  color: white;  
  transform: translateY(-1px);  
  box-shadow: 0 4px 14px rgba(6,199,85,0.3);  
}  
.choice-btn.selected {  
  background: var(--line-green);  
  color: white;  
  opacity: 0.8;  
}  
.choice-btn-emoji { font-size: 20px; flex-shrink: 0; }  
  
/* タイピングインジケーター */  
.typing-row {  
  display: flex;  
  align-items: flex-end;  
  gap: 8px;  
  animation: fadeUp 0.3s ease both;  
}  
.typing-bubble {  
  background: white;  
  border-radius: 18px;  
  border-bottom-left-radius: 4px;  
  padding: 12px 16px;  
  display: flex;  
  gap: 5px;  
  align-items: center;  
  box-shadow: var(--shadow-sm);  
}  
.typing-dot {  
  width: 7px; height: 7px;  
  background: #bbb;  
  border-radius: 50%;  
  animation: bounce 1.1s infinite ease-in-out;  
}  
.typing-dot:nth-child(2) { animation-delay: 0.18s; }  
.typing-dot:nth-child(3) { animation-delay: 0.36s; }  
  
@keyframes bounce {  
  0%,80%,100% { transform: translateY(0); }  
  40% { transform: translateY(-7px); }  
}  
  
/* ═══════════════════════════  
   ローディング画面  
══════════════════════════════ */  
#screen-loading { min-height: 100vh; }  
#screen-loading.active {  
  display: flex;  
  flex-direction: column;  
  align-items: center;  
  justify-content: center;  
  gap: 20px;  
  background: var(--bg);  
}  
  
.loading-emoji {  
  font-size: 58px;  
  animation: pulse 0.9s ease infinite;  
}  
.loading-text {  
  font-size: 16px;  
  color: var(--text-mid);  
  font-weight: 500;  
}  
.loading-dots span { animation: blink 1.2s infinite ease-in-out; }  
.loading-dots span:nth-child(2) { animation-delay: 0.2s; }  
.loading-dots span:nth-child(3) { animation-delay: 0.4s; }  
  
@keyframes pulse { 0%,100% { transform: scale(1); } 50% { transform: scale(1.14); } }  
@keyframes blink { 0%,80%,100% { opacity: 0.3; } 40% { opacity: 1; } }  
  
/* ═══════════════════════════  
   結果画面  
══════════════════════════════ */  
#screen-result { min-height: 100vh; }  
#screen-result.active { display: flex; flex-direction: column; }  
  
.result-scroll {  
  flex: 1;  
  overflow-y: auto;  
  padding-bottom: 32px;  
}  
  
/* 結果ヒーロー：LINE風チャットバナー */  
.result-hero-chat {  
  padding: 16px 16px 8px;  
  display: flex;  
  flex-direction: column;  
  gap: 14px;  
}  
  
.result-announce {  
  display: flex;  
  align-items: flex-end;  
  gap: 8px;  
  animation: fadeUp 0.4s ease both;  
}  
.result-announce-bubble {  
  background: white;  
  border-radius: 18px;  
  border-bottom-left-radius: 4px;  
  padding: 12px 16px;  
  font-size: 14px;  
  line-height: 1.6;  
  color: var(--text);  
  box-shadow: var(--shadow-sm);  
  max-width: 80%;  
}  
  
.result-main-card {  
  background: white;  
  border-radius: 20px;  
  box-shadow: var(--shadow-md);  
  overflow: hidden;  
  margin: 0 16px;  
  animation: fadeUp 0.5s ease 0.2s both;  
}  
.result-main-header {  
  padding: 24px 20px 16px;  
  text-align: center;  
  border-bottom: 1px solid #f0f0f0;  
}  
.result-main-label {  
  font-size: 10px;  
  letter-spacing: 0.18em;  
  color: var(--line-green);  
  text-transform: uppercase;  
  font-weight: 700;  
  margin-bottom: 10px;  
}  
.result-main-animal {  
  font-size: 64px;  
  display: block;  
  margin-bottom: 8px;  
  animation: popIn 0.6s cubic-bezier(0.34,1.56,0.64,1) 0.3s both;  
}  
.result-main-name {  
  font-size: 22px;  
  font-weight: 900;  
  color: var(--text);  
  margin-bottom: 4px;  
}  
.result-main-tagline {  
  font-size: 13px;  
  color: var(--text-mid);  
  margin-bottom: 10px;  
}  
.result-main-motto {  
  display: inline-block;  
  background: var(--line-green);  
  color: white;  
  font-size: 12px;  
  font-weight: 700;  
  padding: 5px 18px;  
  border-radius: 20px;  
}  
  
/* 詳細カード群 */  
.result-cards {  
  padding: 12px 16px;  
  display: flex;  
  flex-direction: column;  
  gap: 10px;  
}  
  
.r-card {  
  background: white;  
  border-radius: 16px;  
  padding: 18px;  
  box-shadow: var(--shadow-sm);  
  animation: fadeUp 0.4s ease both;  
}  
.r-card:nth-child(1) { animation-delay: 0.35s; }  
.r-card:nth-child(2) { animation-delay: 0.42s; }  
.r-card:nth-child(3) { animation-delay: 0.49s; }  
.r-card:nth-child(4) { animation-delay: 0.56s; }  
  
.r-card-title {  
  font-size: 11px;  
  letter-spacing: 0.12em;  
  color: var(--line-green);  
  font-weight: 700;  
  text-transform: uppercase;  
  margin-bottom: 10px;  
}  
  
.trait-grid {  
  display: flex;  
  flex-wrap: wrap;  
  gap: 6px;  
}  
.trait-chip {  
  background: var(--line-green-pale);  
  color: #047a38;  
  border-radius: 20px;  
  font-size: 12px;  
  padding: 5px 12px;  
  font-weight: 500;  
}  
  
.desc-text, .growth-text {  
  font-size: 14px;  
  line-height: 1.8;  
  color: var(--text-mid);  
}  
  
.compat-row {  
  display: flex;  
  gap: 10px;  
}  
.compat-item {  
  flex: 1;  
  background: var(--bg);  
  border-radius: 12px;  
  padding: 12px 8px;  
  text-align: center;  
}  
.compat-emoji { font-size: 26px; }  
.compat-name { font-size: 11px; color: var(--text-light); margin-top: 4px; }  
  
/* コンテンツタイプカード */  
.content-type-card {  
  background: linear-gradient(135deg, #f0f9ff 0%, #e8f4fd 100%);  
  border: 1.5px solid #b3d9f5;  
}  
.content-type-label {  
  font-size: 16px;  
  font-weight: 700;  
  color: #1a6fa8;  
  margin-bottom: 10px;  
  padding: 6px 14px;  
  background: white;  
  border-radius: 20px;  
  display: inline-block;  
  box-shadow: 0 1px 4px rgba(0,0,0,0.08);  
}  
.content-type-desc {  
  font-size: 13.5px;  
  line-height: 1.8;  
  color: #2a4a6a;  
  margin-bottom: 10px;  
}  
.content-type-note {  
  font-size: 11px;  
  color: #888;  
  line-height: 1.6;  
  border-top: 1px dashed #c0d8ee;  
  padding-top: 8px;  
  margin-top: 4px;  
}  
  
/* ループカード */  
.loop-card {  
  border: 2px solid #ffd54f;  
  background: #fffde7;  
}  
.loop-card-title { color: #e65100 !important; }  
.loop-intro {  
  font-size: 12px;  
  color: #888;  
  margin-bottom: 12px;  
}  
.loop-chain {  
  display: flex;  
  flex-direction: column;  
  gap: 2px;  
}  
.loop-step {  
  display: flex;  
  align-items: flex-start;  
  gap: 8px;  
  padding: 7px 0;  
  border-bottom: 1px dashed rgba(0,0,0,0.07);  
  position: relative;  
}  
.loop-step:last-child { border-bottom: none; }  
.loop-step-num {  
  width: 20px; height: 20px;  
  background: #ffc107;  
  color: white;  
  border-radius: 50%;  
  font-size: 10px;  
  font-weight: 700;  
  display: flex; align-items: center; justify-content: center;  
  flex-shrink: 0;  
  margin-top: 1px;  
}  
.loop-step-text {  
  font-size: 13.5px;  
  line-height: 1.5;  
  color: #4a3800;  
}  
.loop-step-arrow {  
  display: block;  
  text-align: left;  
  padding-left: 28px;  
  font-size: 11px;  
  color: #ffc107;  
  margin: -4px 0 2px;  
}  
.loop-step-last {  
  background: #fff3e0;  
  border-radius: 10px;  
  padding: 10px 12px;  
  margin-top: 6px;  
  border-bottom: none !important;  
}  
.loop-step-last .loop-step-text {  
  font-weight: 700;  
  color: #bf360c;  
  font-size: 14px;  
}  
  
/* リトライ */  
.retry-wrap {  
  padding: 4px 16px 40px;  
}  
.retry-btn-main {  
  width: 100%;  
  background: var(--line-green);  
  color: white;  
  border: none;  
  border-radius: 24px;  
  padding: 15px;  
  font-size: 15px;  
  font-family: 'Noto Sans JP', sans-serif;  
  font-weight: 700;  
  cursor: pointer;  
  letter-spacing: 0.04em;  
  box-shadow: 0 4px 16px rgba(6,199,85,0.3);  
  transition: all 0.2s ease;  
}  
.retry-btn-main:hover { background: var(--line-green-dark); transform: translateY(-2px); }  
.retry-btn-main:active { transform: scale(0.98); }  
  
/* ── アニメーション ── */  
@keyframes fadeUp {  
  from { opacity: 0; transform: translateY(14px); }  
  to   { opacity: 1; transform: translateY(0); }  
}  
@keyframes fadeIn {  
  from { opacity: 0; } to { opacity: 1; }  
}  
@keyframes popIn {  
  from { opacity: 0; transform: scale(0.4); }  
  to   { opacity: 1; transform: scale(1); }  
}  
  
  
/* ── タイプ画像 ── */  
.result-main-animal-img {  
  width: 140px;  
  height: 140px;  
  border-radius: 50%;  
  object-fit: cover;  
  margin-bottom: 12px;  
  box-shadow: 0 4px 20px rgba(0,0,0,0.18);  
  animation: popIn 0.6s cubic-bezier(0.34,1.56,0.64,1) 0.3s both;  
}  
.type-card-mini img {  
  width: 48px;  
  height: 48px;  
  border-radius: 50%;  
  object-fit: cover;  
}  
  
/* スクロールバー */  
::-webkit-scrollbar { width: 4px; }  
::-webkit-scrollbar-track { background: transparent; }  
::-webkit-scrollbar-thumb { background: #ccc; border-radius: 2px; }  
</style>  
</head>  
<body>  
<div class="app">  
  
  <!-- ── TOP ── -->  
  <div id="screen-top" class="screen active">  
    <div class="line-header">  
      <div class="line-header-icon">🤖</div>  
      <div class="line-header-info">  
        <div class="line-header-name">タイプ診断 AI</div>  
        <div class="line-header-status">オンライン</div>  
      </div>  
    </div>  
    <div class="top-chat-area">  
      <div style="text-align:center"><span class="date-chip">今日</span></div>  
      <div class="msg-row" style="animation-delay:0.1s">  
        <div class="av">🤖</div>  
        <div class="bubble ai-bubble">こんにちは！✨<br>いくつかの質問に答えるだけで、あなたの「動き方のタイプ」がわかります。</div>  
      </div>  
      <div class="msg-row" style="animation-delay:0.4s">  
        <div class="av">🤖</div>  
        <div class="bubble ai-bubble">5つのタイプがあります👇</div>  
      </div>  
      <div class="top-types-grid">  
        <div class="type-card-mini"><div class="tc-emoji">🐢</div><div class="tc-name">長距離タイプ</div></div>  
        <div class="type-card-mini"><div class="tc-emoji">🐇</div><div class="tc-name">短距離タイプ</div></div>  
        <div class="type-card-mini"><div class="tc-emoji">🌿</div><div class="tc-name">ネギタイプ</div></div>  
        <div class="type-card-mini"><div class="tc-emoji">🐯</div><div class="tc-name">タイガータイプ</div></div>  
        <div class="type-card-mini" style="grid-column:1/-1;max-width:50%;margin:0 auto;"><div class="tc-emoji">🐒</div><div class="tc-name">第五タイプ</div></div>  
      </div>  
      <div class="msg-row" style="animation-delay:0.7s">  
        <div class="av">🤖</div>  
        <div class="bubble ai-bubble">直感で正直に答えてください♡<br>全12問・約3〜4分で完了します！</div>  
      </div>  
    </div>  
    <div class="top-start-area">  
      <button class="start-big-btn" onclick="startQuiz()">診断スタート →</button>  
      <p class="start-note">全12問 ・ 約4分</p>  
    </div>  
  </div>  
  
  <!-- ── QUIZ ── -->  
  <div id="screen-quiz" class="screen">  
    <div class="line-header">  
      <div class="line-header-icon">🤖</div>  
      <div class="line-header-info">  
        <div class="line-header-name">タイプ診断 AI</div>  
        <div class="line-header-status" id="quizStatusText">質問 1 / 12</div>  
      </div>  
    </div>  
    <div class="progress-bar-wrap">  
      <span class="progress-label" id="progressLabel">1/6</span>  
      <div class="progress-track">  
        <div class="progress-fill" id="progressFill" style="width:0%"></div>  
      </div>  
    </div>  
    <div class="quiz-chat-area" id="quizChatArea"></div>  
    <div class="choices-bottom" id="choicesBottom"></div>  
  </div>  
  
  <!-- ── LOADING ── -->  
  <div id="screen-loading" class="screen">  
    <div class="line-header">  
      <div class="line-header-icon">🤖</div>  
      <div class="line-header-info">  
        <div class="line-header-name">タイプ診断 AI</div>  
        <div class="line-header-status">診断中...</div>  
      </div>  
    </div>  
    <div class="loading-emoji" id="loadingEmoji">🔍</div>  
    <div class="loading-text">診断中<span class="loading-dots"><span>.</span><span>.</span><span>.</span></span></div>  
  </div>  
  
  <!-- ── RESULT ── -->  
  <div id="screen-result" class="screen">  
    <div class="line-header">  
      <div class="line-header-icon" id="resultHeaderIcon">🤖</div>  
      <div class="line-header-info">  
        <div class="line-header-name">診断結果</div>  
        <div class="line-header-status">完了しました！</div>  
      </div>  
    </div>  
    <div class="result-scroll">  
      <div class="result-hero-chat">  
        <div class="result-announce">  
          <div class="av">🤖</div>  
          <div class="result-announce-bubble" id="resultAnnounce">診断が完了しました！🎉<br>あなたのタイプはこちらです👇</div>  
        </div>  
      </div>  
      <div class="result-main-card">  
        <div class="result-main-header">  
          <div class="result-main-label">YOUR TYPE</div>  
          <span class="result-main-animal" id="resultAnimal"></span>  
          <div class="result-main-name" id="resultName"></div>  
          <div class="result-main-tagline" id="resultTagline"></div>  
          <div class="result-main-motto" id="resultMotto"></div>  
        </div>  
      </div>  
      <div class="result-cards" id="resultCards"></div>  
      <div class="retry-wrap">  
        <button class="retry-btn-main" onclick="reset()">🔄 もう一度診断する</button>  
      </div>  
    </div>  
  </div>  
  
</div>  
  
<script>  
// ══════════════════════════  
  
const typeImages = {  
  long:  null,  
  short: null,  
  negi:  null,  
  tiger: null,  
  daigo: null,  
};  
  
// 質問データ（PDF「アルゴリズムが映し出すトレーダーの脳内」視点を組み込み）  
// ══════════════════════════  
const questions = [  
  // Q1: 基本行動パターン  
  {  
    text: "グループで何かを決めるとき、あなたはどうする？",  
    choices: [  
      { emoji: "🎯", text: "自分の意見をはっきり伝える",              score: { tiger: 2 } },  
      { emoji: "🤝", text: "みんなの意見をまとめようとする",          score: { negi: 2 } },  
      { emoji: "📋", text: "計画を立てて、責任を持って進める",        score: { long: 2 } },  
      { emoji: "⚡", text: "場の雰囲気を盛り上げて決める",            score: { short: 2 } },  
      { emoji: "💎", text: "自分の得意なことで貢献できる方向を選ぶ", score: { daigo: 2 } },  
    ]  
  },  
  // Q2: モチベーション源泉  
  {  
    text: "モチベーションが上がるのはどんなとき？",  
    choices: [  
      { emoji: "🏆", text: "「あなただから頼みたい」と直接言われたとき",  score: { tiger: 2 } },  
      { emoji: "🌸", text: "チームみんなが仲良く楽しそうなとき",          score: { negi: 2 } },  
      { emoji: "📈", text: "結果が出て、次の目標が見えたとき",            score: { long: 2 } },  
      { emoji: "🎉", text: "周りが盛り上がって、自分もスイッチが入ったとき", score: { short: 2 } },  
      { emoji: "✨", text: "自分の能力や成果をしっかり認められたとき",    score: { daigo: 2 } },  
    ]  
  },  
  // Q3: イベント参加判断（集会・誘い）  
  {  
    text: "イベントや集まりに誘われたとき、行くかどうかは？",  
    choices: [  
      { emoji: "🔥", text: "「来てほしい」と直接言われると燃える",        score: { tiger: 2 } },  
      { emoji: "👥", text: "誰が来るか確認して、知ってる人がいれば行く",  score: { negi: 2 } },  
      { emoji: "📅", text: "スケジュールと目的を確認してから判断する",    score: { long: 2 } },  
      { emoji: "📞", text: "テンション高く誘ってもらえれば行く！",        score: { short: 2 } },  
      { emoji: "🌟", text: "自分が価値を感じるかどうかで決める",          score: { daigo: 2 } },  
    ]  
  },  
  // Q4: 問題発生時の行動  
  {  
    text: "チームで問題が起きたとき、あなたは？",  
    choices: [  
      { emoji: "🗣", text: "「こうすべきだ」と率直に言う",              score: { tiger: 2 } },  
      { emoji: "🕊", text: "みんなが揉めないよう間に入る",              score: { negi: 2 } },  
      { emoji: "🔍", text: "原因を分析して、対策を考える",              score: { long: 2 } },  
      { emoji: "😄", text: "明るく「なんとかなる！」と場を和ませる",   score: { short: 2 } },  
      { emoji: "💡", text: "自分のスキルで解決できる方法を提案する",   score: { daigo: 2 } },  
    ]  
  },  
  // Q5: ストレス発散・休息スタイル（PDF分類1・2を反映）  
  {  
    text: "疲れたとき・ストレスがたまったとき、自然と何をしたくなる？",  
    choices: [  
      { emoji: "🎵", text: "BGMや自然音を流して静かに集中を取り戻す",    score: { long: 2 } },  
      { emoji: "😂", text: "お笑い・面白い動画を見て一瞬で気持ちをリセット", score: { short: 2 } },  
      { emoji: "🫂", text: "仲間に話しかけて「大丈夫」という空気が欲しい",  score: { negi: 2 } },  
      { emoji: "🏅", text: "すごい人の動画を見てインスピレーションをもらう", score: { tiger: 2 } },  
      { emoji: "🔬", text: "知的な雑学や科学の話に没頭して頭を切り替える",  score: { daigo: 2 } },  
    ]  
  },  
  // Q6: 贈り物・お礼スタイル  
  {  
    text: "誰かに何かを贈るとき（お礼・プレゼントなど）、どうする？",  
    choices: [  
      { emoji: "💝", text: "相手の想いに応えたくて、すごく考える",    score: { tiger: 2 } },  
      { emoji: "🎁", text: "細やかに気を遣って、喜んでもらいたい",    score: { long: 2 } },  
      { emoji: "😅", text: "正直あまり得意じゃない…つい後回しに",    score: { daigo: 1, negi: 1 } },  
      { emoji: "🌈", text: "楽しいものを贈りたい！ノリで選ぶ",       score: { short: 2 } },  
      { emoji: "🌿", text: "みんなに合わせて無難なものを選ぶ",       score: { negi: 2 } },  
    ]  
  },  
  // Q7: 頑張れる根本理由（コア）  
  {  
    text: "「自分が頑張れる理由」に一番近いのは？",  
    choices: [  
      { emoji: "❤️", text: "想い・熱い気持ちがあるから",             score: { tiger: 3 } },  
      { emoji: "🛡",  text: "安心できる仲間・環境があるから",         score: { negi: 3 } },  
      { emoji: "📌",  text: "責任と使命感があるから",                 score: { long: 3 } },  
      { emoji: "🎊",  text: "周りの雰囲気が盛り上がってるから",       score: { short: 3 } },  
      { emoji: "💫",  text: "自分の輝きやキラキラを感じられるから",   score: { daigo: 3 } },  
    ]  
  },  
  // Q8: 落ち込んだときの回復法  
  {  
    text: "落ち込んだり、調子が出ないとき、どうやって回復する？",  
    choices: [  
      { emoji: "🔥", text: "「絶対やってやる」と自分に言い聞かせる",        score: { tiger: 2 } },  
      { emoji: "🫂", text: "仲の良い人と話してほっとする",                  score: { negi: 2 } },  
      { emoji: "📊", text: "何がいけなかったか振り返り、計画を立て直す",    score: { long: 2 } },  
      { emoji: "🎵", text: "好きな音楽・場所でテンションを上げる",          score: { short: 2 } },  
      { emoji: "🏅", text: "過去の自分の成果・実績を見返して自信を取り戻す", score: { daigo: 2 } },  
    ]  
  },  
  // Q9: 新環境での優先事項  
  {  
    text: "新しいチームや環境に入ったとき、まず何を気にする？",  
    choices: [  
      { emoji: "👀", text: "自分の実力をいつ・どこで発揮できるか",          score: { tiger: 2 } },  
      { emoji: "🤗", text: "みんなと早く仲良くなれるかどうか",              score: { negi: 2 } },  
      { emoji: "📐", text: "ルールや役割分担、目標がはっきりしているか",    score: { long: 2 } },  
      { emoji: "🌈", text: "雰囲気が明るくて楽しそうかどうか",              score: { short: 2 } },  
      { emoji: "💼", text: "自分のスキルを正当に評価してもらえる環境か",    score: { daigo: 2 } },  
    ]  
  },  
  // Q10: コンテンツ消費パターン（PDF視点を直接組み込み）  
  {  
    text: "YouTube・SNSで動画を見るとき、何を求めてる？",  
    choices: [  
      { emoji: "🏆", text: "すごい人の圧倒的なスキル・パフォーマンスに刺激をもらいたい", score: { tiger: 2 } },  
      { emoji: "🌿", text: "みんなが見てる話題の動画で場の話題についていきたい",         score: { negi: 2 } },  
      { emoji: "🎵", text: "長時間BGMを流して集中力や神経を整えたい",                    score: { long: 2 } },  
      { emoji: "😂", text: "面白い・癒し系で瞬時に気分をリセットしたい",                 score: { short: 2 } },  
      { emoji: "🔬", text: "知的な雑学・科学・社会ネタで知識欲を満たしたい",             score: { daigo: 2 } },  
    ]  
  },  
  // Q11: リーダーへの感じ方  
  {  
    text: "リーダーや上の立場の人に対して、あなたはどう感じる？",  
    choices: [  
      { emoji: "🔥", text: "本気・熱量が見えないと物足りなくなる",            score: { tiger: 2 } },  
      { emoji: "🌿", text: "みんなと仲良くしてくれる人が好き",                score: { negi: 2 } },  
      { emoji: "📋", text: "責任感があって、ちゃんと筋を通す人が信頼できる",  score: { long: 2 } },  
      { emoji: "😊", text: "場を明るくして引っ張ってくれる人が好き",          score: { short: 2 } },  
      { emoji: "💡", text: "自分の能力を認めて使いこなしてくれる人がいい",   score: { daigo: 2 } },  
    ]  
  },  
  // Q12: 最大の行動エンジン（コア・重みづけ最大）  
  {  
    text: "目標に向けて動くとき、一番エンジンになるものは？",  
    choices: [  
      { emoji: "❤️‍🔥", text: "強烈な「やりたい」「悔しい」という感情",   score: { tiger: 3 } },  
      { emoji: "🤝",    text: "一緒に頑張れる安心できる仲間の存在",        score: { negi: 3 } },  
      { emoji: "🎯",    text: "明確な目標とそこへの責任・約束",            score: { long: 3 } },  
      { emoji: "🎊",    text: "その場の熱狂・盛り上がり・ノリ",            score: { short: 3 } },  
      { emoji: "✨",    text: "自分がキラキラ輝けるビジョンやイメージ",    score: { daigo: 3 } },  
    ]  
  },  
];  
  
// ══════════════════════════  
// 5タイプ定義  
// ══════════════════════════  
const types = {  
  long: {  
    animal: "🐢",  
    name: "長距離タイプ",  
    tagline: "責任と使命感で動くリーダー",  
    motto: "責任で頑張れる♡",  
    heroBg: "🐢",  
    heroColor: "#f0f8ff",  
    traits: ["責任感が強い", "長期目線", "手土産・お礼が細やか", "目標への執着", "計画的", "結果へのフォーカス"],  
    desc: "長い視点で物事を見て、責任を持って最後まで動き続けられるタイプ。手土産やお礼の細やかさが、信頼を積み重ねる武器になっています。\n\n心理学的には「責任感」が自律神経を安定させる錨になっており、長時間BGMを流しながら集中を維持する視聴スタイルが脳のパフォーマンスを最大化します。外部のノイズを遮断し、深い集中状態（フロー）に入ることが得意なタイプです。\n\nスケジュールで動けるため、チームの中で「段取りの人」として信頼を集めます。ただし、自分の結果より他者の承認を優先しすぎると燃え尽きるリスクがあります。",  
    growth: "自分の成果と結果にどんどんフォーカスしていくと、あなたの責任感がさらに強力な推進力に変わります♡\n\n認知負荷の低い「集中と神経調整」コンテンツ（環境音・バイノーラルBGM）を活用して、ゾーンに入る習慣を作ると、さらに長距離を走れる体になります。",  
    contentType: "🎵 集中と神経調整型",  
    contentDesc: "長時間BGM・自律神経を整える音楽・環境映像。相場という混乱の中で脳波をチューニングし、ゾーンへ導く「音のインフラ」を本能的に求めています。",  
    compat: [  
      { emoji: "🐯", name: "タイガー" },  
      { emoji: "🌿", name: "ネギ" },  
    ]  
  },  
  short: {  
    animal: "🐇",  
    name: "短距離タイプ",  
    tagline: "雰囲気と勢いで場を動かす存在",  
    motto: "雰囲気で頑張れる♡",  
    heroBg: "🐇",  
    heroColor: "#f0fff4",  
    traits: ["場の空気を読む", "テンション高め", "感情を動かす力", "勢いで突破", "瞬発力抜群", "場の雰囲気を作る"],  
    desc: "その場の雰囲気やテンションで一気に動けるタイプ。電話で勢いよく誘われると動けるように、自分もそのエネルギーで周りを巻き込む力があります。\n\n心理学的には「ドーパミンの瞬間的なスパイク」で行動エンジンがかかるタイプ。お笑い・面白い動画・癒し系コンテンツで張り詰めた緊張の糸を意図的に緩める「感情のリセットボタン」を本能的に求めています。笑いと癒しがドーパミンを再分配し、次の行動への準備が整います。\n\n「感情を抑えられるようになる」という成長が、さらに大きな結果を生みます。",  
    growth: "感情をコントロールする力を身につけると、雰囲気作りの才能がさらに活きてきます♡\n\n短時間・高刺激の「現実からの逃避とエンタメ」コンテンツ（5〜20分）を意識的に使い、感情リセット後に行動モードに入る習慣を作ると、あなたの瞬発力がさらに武器になります。",  
    contentType: "😂 現実からの逃避とエンタメ型",  
    contentDesc: "お笑い・癒し動物・短時間コメディ。張り詰めた緊張を一瞬で解放するリセットボタン。5〜20分の高刺激コンテンツでドーパミンを補充し、次の行動エネルギーを作ります。",  
    compat: [  
      { emoji: "🐢", name: "長距離" },  
      { emoji: "🐯", name: "タイガー" },  
    ]  
  },  
  negi: {  
    animal: "🌿",  
    name: "ネギタイプ",  
    tagline: "安心と仲間が原動力の平和主義者",  
    motto: "安心で頑張れる♡",  
    heroBg: "🌿",  
    heroColor: "#f5fff0",  
    traits: ["平和主義", "みんなに合わせる", "覚えてもらえると嬉しい", "お揃いが好き", "環境に左右されやすい", "話が長い"],  
    desc: "争いを好まず、みんなが仲良くいられる環境で最大限の力を発揮します。強く誘ってもらえれば動ける行動力がある一方、意見を求められると困ることも。チームの雰囲気に左右されやすく、楽しい♡で組織が伸びていた頃が懐かしいと感じることも。\n\n心理学的には「社会的安全」が自律神経の安定に直結しているタイプ。チームの雰囲気がそのままパフォーマンスに出ます。話題のコンテンツを積極的に見るのは、「場の話題についていきたい」という帰属欲求の表れです。\n\nミーティングで質問・意見を求められると困る傾向がありますが、それは「賢くない」のではなく「嫌われることへの恐怖」が意思決定を止めているためです。",  
    growth: "自分なりの立ち回り方を身につけると、チームで最強のサポーターになれます♡\n\n「社会と現実」コンテンツ（他者の日常・ドキュメンタリー）を意識的に見て、「自分以外の人の生き方」にアンカーを打つ習慣が、判断力と自己信頼を育てます。",  
    contentType: "👥 社会と現実・帰属確認型",  
    contentDesc: "話題の動画・みんなが見てるコンテンツ・他者の日常。「仲間と同じものを共有したい」帰属欲求を満たすコンテンツを求めます。場の雰囲気に安心感を見出すタイプです。",  
    loop: [  
      "賢くない",  
      "嫌われたくない",  
      "みんなの意見を聞く",  
      "嫌われたくないから…",  
      "満場一致を求める",  
      "決まらない",  
      "決まらないことに時間を使う",  
      "みんなイライラする",  
      "イライラしている空気が苦手",  
      "→ だからリーダーをしたくない‼️",  
    ],  
    compat: [  
      { emoji: "🐇", name: "短距離" },  
      { emoji: "🐢", name: "長距離" },  
    ]  
  },  
  tiger: {  
    animal: "🐯",  
    name: "タイガータイプ",  
    tagline: "想いと熱量で突き動かす実力者",  
    motto: "想いで頑張れる♡",  
    heroBg: "🐯",  
    heroColor: "#fffaf0",  
    traits: ["直接言われると燃える", "想い・熱量が強い", "本気が見えないと噛みつく", "リアルな存在感を求める", "瞬発的な行動力", "インスピレーション重視"],  
    desc: "熱い想いと強いエネルギーを持つタイプ。直接誘われる・認められることで本気スイッチが入ります。リーダーに本気（リアルな居場所・貢献する姿・想い）が見えないと噛みつく一面も。\n\n心理学的には「感情的覚醒」が行動の最大エンジン。超一流のパフォーマンスや天才の軌跡に触れることで、自分への「自己投影」とインスピレーションが生まれます。マグヌス・カールセンやメッシのような「極限のパフォーマンス動画」が脳のドーパミンを最大化し、次の行動への火付けになります。\n\n成長すると「最強♡」と言われるほどのポテンシャルがあります。",  
    growth: "サポートしてくれる優秀な組織・チームを整えてもらうことで、あなたの想いが最大限に爆発します♡\n\n「卓越したスキルと神業」コンテンツ（10〜60分）でインスピレーションをチャージする習慣が、あなたの行動エンジンをさらに強化します。驚嘆とインスピレーションが認知負荷を上げ、パフォーマンス向上への自己投影を促します。",  
    contentType: "🏆 卓越したスキルと神業型",  
    contentDesc: "超一流の圧倒的パフォーマンス・天才の軌跡・極限の挑戦動画。「自己投影」と「インスピレーション」でドーパミンをチャージ。10〜60分の中尺コンテンツで覚醒度を高め、次の行動への火付けになります。",  
    compat: [  
      { emoji: "🐢", name: "長距離" },  
      { emoji: "🐒", name: "第五" },  
    ]  
  },  
  daigo: {  
    animal: "🐒",  
    name: "第五タイプ",  
    tagline: "自分の能力と輝きで動くエース",  
    motto: "キラキラで頑張れる♡",  
    heroBg: "🐒",  
    heroColor: "#fff8f0",  
    traits: ["能力に誇りがある", "お礼・手土産は惜しい", "同タイプとはぶつかる", "キラキラが原動力", "知的好奇心旺盛", "自己評価が高い"],  
    desc: "自分の能力に価値があると信じる、高い自己評価を持つタイプ。キラキラした環境・認められる場で輝きます。第五同士は惹かれ合うけれど合わない、という不思議な関係性も。性質で人の善悪を決めず、自分を理解して戦略を立てることが大切。\n\n心理学的には「知的好奇心の充足」が自律神経を整える特徴的なパターンを持っています。雑学・科学・社会分析など「結果が確定している（不確実性のない）知識」への没頭が、相場リスクから離れた安全な知的探求となり、認知負荷を適度に保ちます。\n\nお礼・手土産を惜しむ傾向がある一方、自分が「貢献できる」と感じた瞬間には驚くほどのエネルギーを発揮します。",  
    growth: "己のために♡やる——「仲間を勝たせたい」ではなく、自分の魂が喜ぶ言葉を言い続けることが成長の鍵。\n\n「好奇心と科学」コンテンツ（5〜15分）で知的満足を得る習慣が、あなたの思考の切り替え力を高めます。リスクのない知識探求が認知バランスを保ち、本番の意思決定精度を上げます。",  
    contentType: "🔬 好奇心と科学・知的探求型",  
    contentDesc: "雑学・科学・社会分析・没入できる手作業系。相場リスクから離れた「結果の確定している知識」への没頭が安全な知的充電になります。5〜15分の適度な認知負荷で知的満足をチャージ。",  
    compat: [  
      { emoji: "🐯", name: "タイガー" },  
      { emoji: "🐢", name: "長距離" },  
    ]  
  }  
};  
  
// ══════════════════════════  
// ゲームロジック  
// ══════════════════════════  
let currentQ = 0;  
let scores = { long: 0, short: 0, negi: 0, tiger: 0, daigo: 0 };  
  
function show(id) {  
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));  
  document.getElementById(id).classList.add('active');  
}  
  
function startQuiz() {  
  currentQ = 0;  
  scores = { long: 0, short: 0, negi: 0, tiger: 0, daigo: 0 };  
  document.getElementById('quizChatArea').innerHTML = '';  
  show('screen-quiz');  
  appendAIBubble(questions[0].text, () => showChoices(0));  
}  
  
function appendAIBubble(text, callback) {  
  const area = document.getElementById('quizChatArea');  
  
  // タイピング  
  const typing = document.createElement('div');  
  typing.className = 'typing-row';  
  typing.innerHTML = `<div class="av">🤖</div><div class="typing-bubble"><div class="typing-dot"></div><div class="typing-dot"></div><div class="typing-dot"></div></div>`;  
  area.appendChild(typing);  
  area.scrollTop = area.scrollHeight;  
  
  setTimeout(() => {  
    typing.remove();  
    const row = document.createElement('div');  
    row.className = 'msg-row';  
    row.innerHTML = `<div class="av">🤖</div><div class="bubble ai-bubble">${text.replace(/\n/g,'<br>')}</div>`;  
    area.appendChild(row);  
    area.scrollTop = area.scrollHeight;  
    if (callback) callback();  
  }, 800);  
}  
  
function appendUserBubble(text) {  
  const area = document.getElementById('quizChatArea');  
  const row = document.createElement('div');  
  row.className = 'msg-row user-row';  
  row.innerHTML = `<div class="av user-av">👤</div><div class="bubble user-bubble">${text}</div>`;  
  area.appendChild(row);  
  area.scrollTop = area.scrollHeight;  
}  
  
function showChoices(qIndex) {  
  const q = questions[qIndex];  
  const bottom = document.getElementById('choicesBottom');  
  bottom.innerHTML = '';  
  document.getElementById('progressFill').style.width = ((qIndex / questions.length) * 100) + '%';  
  document.getElementById('progressLabel').textContent = `${qIndex + 1}/12`;  
  document.getElementById('quizStatusText').textContent = `質問 ${qIndex + 1} / 12`;  
  
  q.choices.forEach((c, i) => {  
    const btn = document.createElement('button');  
    btn.className = 'choice-btn';  
    btn.innerHTML = `<span class="choice-btn-emoji">${c.emoji}</span><span>${c.text}</span>`;  
    btn.style.animationDelay = `${i * 0.06}s`;  
    btn.onclick = () => {  
      bottom.querySelectorAll('.choice-btn').forEach(b => { b.style.pointerEvents='none'; b.style.opacity='0.5'; });  
      btn.classList.add('selected');  
      btn.style.opacity = '1';  
  
      for (const [k, v] of Object.entries(c.score)) scores[k] = (scores[k]||0) + v;  
      appendUserBubble(c.text);  
      bottom.innerHTML = '';  
  
      currentQ++;  
      if (currentQ < questions.length) {  
        appendAIBubble(questions[currentQ].text, () => showChoices(currentQ));  
      } else {  
        showLoading();  
      }  
    };  
    bottom.appendChild(btn);  
  });  
}  
  
function showLoading() {  
  show('screen-loading');  
  const emojis = ['🔍','💫','🌀','✨','🎯'];  
  let i = 0;  
  const iv = setInterval(() => { document.getElementById('loadingEmoji').textContent = emojis[i++ % emojis.length]; }, 300);  
  setTimeout(() => { clearInterval(iv); showResult(); }, 2000);  
}  
  
function showResult() {  
  const top = Object.entries(scores).sort((a,b)=>b[1]-a[1])[0][0];  
  const t = types[top];  
  
  const imgEl = document.getElementById('resultAnimal');  
  imgEl.innerHTML = '';  
  imgEl.className = 'result-main-animal';  
  imgEl.textContent = t.animal;  
  document.getElementById('resultHeaderIcon').textContent = t.animal;  
  document.getElementById('resultName').textContent = t.name;  
  document.getElementById('resultTagline').textContent = t.tagline;  
  document.getElementById('resultMotto').textContent = t.motto;  
  document.getElementById('resultHeaderIcon').textContent = t.animal;  
  
  const loopHTML = t.loop ? `  
    <div class="r-card loop-card">  
      <div class="r-card-title loop-card-title">⚠️ 注意したいパターン</div>  
      <p class="loop-intro">このループに入っていないか、チェックしてみましょう。</p>  
      <div class="loop-chain">  
        ${t.loop.map((item, i) => {  
          const isLast = i === t.loop.length - 1;  
          if (isLast) {  
            return `<div class="loop-step loop-step-last"><span class="loop-step-text">${item}</span></div>`;  
          }  
          return `  
            <div class="loop-step">  
              <span class="loop-step-num">${i+1}</span>  
              <span class="loop-step-text">${item}</span>  
            </div>  
            <span class="loop-step-arrow">↓</span>`;  
        }).join('')}  
      </div>  
    </div>` : '';  
  
  document.getElementById('resultCards').innerHTML = `  
    <div class="r-card">  
      <div class="r-card-title">特徴・性質</div>  
      <div class="trait-grid">${t.traits.map(tr=>`<span class="trait-chip">${tr}</span>`).join('')}</div>  
    </div>  
    <div class="r-card">  
      <div class="r-card-title">あなたについて</div>  
      <p class="desc-text">${t.desc.replace(/\n/g,'<br><br>')}</p>  
    </div>  
    ${loopHTML}  
    <div class="r-card content-type-card">  
      <div class="r-card-title">🧠 あなたの脳が求めるコンテンツタイプ</div>  
      <div class="content-type-label">${t.contentType}</div>  
      <p class="content-type-desc">${t.contentDesc}</p>  
      <p class="content-type-note">※ アルゴリズムは「好きなもの」ではなく、あなたの脳が今必要としているものを提示しています。</p>  
    </div>  
    <div class="r-card">  
      <div class="r-card-title">成長のポイント</div>  
      <p class="growth-text">${t.growth.replace(/\n/g,'<br><br>')}</p>  
    </div>  
    <div class="r-card">  
      <div class="r-card-title">相性の良いタイプ</div>  
      <div class="compat-row">  
        ${t.compat.map(c=>`<div class="compat-item"><div class="compat-emoji">${c.emoji}</div><div class="compat-name">${c.name}</div></div>`).join('')}  
      </div>  
    </div>  
  `;  
  
  show('screen-result');  
  window.scrollTo({ top: 0, behavior: 'smooth' });  
}  
  
function reset() {  
  show('screen-top');  
  window.scrollTo({ top: 0, behavior: 'smooth' });  
}  
</script>  
</body>  
</html>  
