<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>単語帳 - 東大英語リーディング Session 10</title>
<style>
  :root {
    --bg: #E9EDF2; --ink: #1E2A40; --sub: #5B6B85; --faint: #98A6BC;
    --line: #C4CEDC; --navy: #2E4A8F; --star: #E0A62E; --danger: #A04040;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg); color: var(--ink); min-height: 100vh;
    font-family: 'Hiragino Sans', 'Hiragino Kaku Gothic ProN', 'Yu Gothic', Meiryo, sans-serif;
  }
  .wrap { max-width: 640px; margin: 0 auto; padding: 24px 16px 48px; }
  header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 22px; gap: 10px; }
  h1 { font-size: 22px; letter-spacing: 0.04em; }
  .count { font-size: 12px; color: var(--sub); margin-top: 4px; }
  .tabs { display: flex; gap: 3px; padding: 4px; border-radius: 999px; background: #D8DFE9; flex-wrap: wrap; }
  .tabs button {
    border: none; background: transparent; color: #3A4A66;
    padding: 7px 13px; border-radius: 999px; font-size: 13px; font-weight: 500;
    cursor: pointer; font-family: inherit;
  }
  .tabs button.active { background: var(--navy); color: #fff; }

  .toolbar { display: flex; align-items: center; justify-content: space-between; margin-bottom: 16px; }
  .mode { display: flex; gap: 4px; padding: 4px; border-radius: 10px; background: #D8DFE9; }
  .mode button {
    border: none; background: transparent; color: #3A4A66;
    padding: 5px 12px; border-radius: 8px; font-size: 14px; cursor: pointer; font-family: inherit;
  }
  .mode button.active { background: #fff; font-weight: 600; }
  .mode button.active.star-mode { color: #B8860B; }
  .btn {
    font-family: inherit; font-size: 14px; cursor: pointer;
    padding: 8px 14px; border-radius: 10px; border: 1px solid var(--line);
    background: #fff; color: #3A4A66; font-weight: 500;
  }
  .btn.primary { background: var(--navy); color: #fff; border-color: var(--navy); }
  .btn:disabled { background: var(--line); border-color: var(--line); color: #fff; cursor: default; }
  button:focus-visible { outline: 2px solid var(--navy); outline-offset: 2px; }

  /* カード */
  .card3d { perspective: 1200px; position: relative; height: 300px; }
  .ring-metal {
    position: absolute; top: -15px; left: 19px; width: 20px; height: 36px;
    border: 3px solid var(--faint); border-radius: 50%;
    border-bottom-color: transparent; pointer-events: none; z-index: 5;
  }
  .card-inner {
    position: relative; width: 100%; height: 100%;
    transition: transform 0.5s cubic-bezier(.2,.7,.3,1);
    transform-style: preserve-3d; cursor: pointer;
  }
  .card-inner.flipped { transform: rotateY(180deg); }
  .card-face {
    position: absolute; inset: 0; border-radius: 14px;
    backface-visibility: hidden; -webkit-backface-visibility: hidden;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    box-shadow: 0 8px 24px rgba(30,42,64,0.14), 0 1px 3px rgba(30,42,64,0.10);
  }
  .card-front { background: #fff; transform: rotateY(0deg); }
  .card-back { background: var(--navy); transform: rotateY(180deg); }
  .ring-hole {
    position: absolute; top: 14px; left: 18px; width: 22px; height: 22px;
    border-radius: 50%; background: var(--bg);
    box-shadow: inset 0 1px 3px rgba(30,42,64,0.35);
  }
  .face-label { font-size: 11px; letter-spacing: 0.25em; color: var(--faint); margin-bottom: 12px; }
  .card-back .face-label { color: #AAB9DC; }
  .word-en { font-family: Georgia, 'Times New Roman', serif; font-size: 40px; font-weight: 600; text-align: center; padding: 0 24px; }
  .word-en.marked {
    background: linear-gradient(transparent 55%, #FFE071 55%, #FFE071 92%, transparent 92%);
    padding: 0 10px; border-radius: 2px;
  }
  .word-ja { font-size: 30px; font-weight: 600; color: #fff; text-align: center; padding: 0 24px; }
  .word-en-small { font-family: Georgia, serif; font-size: 14px; color: #AAB9DC; margin-top: 12px; }
  .hint { font-size: 11px; color: var(--faint); margin-top: 24px; }
  .star-btn {
    position: absolute; top: 12px; right: 16px;
    border: none; background: transparent; font-size: 26px; line-height: 1;
    cursor: pointer; color: var(--line); transition: transform .15s;
    backface-visibility: hidden; -webkit-backface-visibility: hidden;
  }
  .star-btn:hover { transform: scale(1.15); }
  .star-btn.on { color: var(--star); }
  .card-back .star-btn { color: #5A73B0; }
  .card-back .star-btn.on { color: var(--star); }
  .card-mark {
    position: absolute; top: 14px; right: 52px;
    border: none; background: transparent; font-size: 20px; line-height: 1;
    cursor: pointer; opacity: .3; filter: grayscale(1); transition: transform .15s;
    backface-visibility: hidden; -webkit-backface-visibility: hidden;
  }
  .card-mark:hover { transform: scale(1.15); }
  .card-mark.on { opacity: 1; filter: none; }
  .card-speak {
    position: absolute; bottom: 12px; right: 14px; padding: 4px;
    border: none; background: transparent; line-height: 0; cursor: pointer;
    color: var(--line); transition: transform .15s, color .15s;
    backface-visibility: hidden; -webkit-backface-visibility: hidden;
  }
  .card-speak:hover { transform: scale(1.15); color: var(--navy); }
  .card-back .card-speak { color: #5A73B0; }
  .card-back .card-speak:hover { color: #fff; }

  .nav { display: flex; align-items: center; justify-content: space-between; margin-top: 20px; }
  .nav .pos { font-size: 14px; color: var(--sub); font-variant-numeric: tabular-nums; }
  .keys { text-align: center; font-size: 11px; color: var(--faint); margin-top: 16px; }
  .empty { background: #fff; border: 1px dashed var(--line); border-radius: 14px; padding: 40px 20px; text-align: center; }
  .empty p:first-child { font-weight: 600; margin-bottom: 6px; }
  .empty p:last-child { font-size: 14px; color: var(--sub); }

  /* 追加ボックス */
  .import-box { background: #fff; border: 1px solid var(--line); border-radius: 14px; padding: 16px; margin-bottom: 20px; }
  .import-box h2 { font-size: 16px; margin-bottom: 4px; }
  .import-box .note { font-size: 12px; color: var(--sub); margin-bottom: 12px; line-height: 1.6; }
  textarea {
    width: 100%; border-radius: 10px; border: 1px solid var(--line);
    background: #F4F6F9; padding: 12px; font-size: 14px; font-family: inherit;
    color: var(--ink); resize: vertical; min-height: 96px;
  }
  .import-box .btn { margin-top: 8px; }

  /* 単語帳リスト */
  .list-toolbar { display: flex; gap: 8px; align-items: center; margin-bottom: 10px; flex-wrap: wrap; }
  #list-search, .del-search {
    flex: 1; min-width: 130px; padding: 9px 13px; border-radius: 10px;
    border: 1px solid var(--line); background: #fff; color: var(--ink);
    font-family: inherit; font-size: 14px; -webkit-appearance: none;
  }
  #list-search:focus, .del-search:focus { outline: none; border-color: var(--navy); }
  .del-search { background: #F4F6F9; width: 100%; }
  .chips { display: flex; gap: 4px; padding: 4px; border-radius: 10px; background: #D8DFE9; flex-shrink: 0; }
  .chips button {
    border: none; background: transparent; color: #3A4A66; padding: 5px 10px;
    border-radius: 8px; font-size: 13px; cursor: pointer; font-family: inherit;
  }
  .chips button.sel { background: #fff; font-weight: 600; }
  .list-count { font-size: 12px; color: var(--sub); margin-bottom: 10px; }
  ul.word-list { list-style: none; display: flex; flex-direction: column; gap: 8px; }
  ul.word-list li {
    display: flex; align-items: center; gap: 8px;
    background: #fff; border: 1px solid #E1E7EF; border-radius: 12px; padding: 13px 8px 13px 15px;
  }
  ul.word-list li.is-star { border-color: #EBD9A6; background: #FFFCF4; }
  .li-main { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 4px; }
  li .li-en { font-family: Georgia, 'Times New Roman', serif; font-weight: 600; font-size: 18px; align-self: flex-start; letter-spacing: .01em; }
  li .li-en.marked { background: linear-gradient(transparent 42%, #FFE071 42%, #FFE071 94%, transparent 94%); padding: 0 3px; border-radius: 2px; }
  li .li-ja { font-size: 13px; color: var(--sub); line-height: 1.55; }
  .li-actions { display: flex; align-items: center; gap: 0; flex-shrink: 0; }
  .li-actions button { border: none; background: transparent; cursor: pointer; line-height: 1; padding: 8px 5px; border-radius: 8px; }
  li .li-star { font-size: 21px; color: #D5DCE6; }
  li .li-star.on { color: var(--star); }
  li .li-mark { font-size: 17px; opacity: .28; filter: grayscale(1); }
  li .li-mark.on { opacity: 1; filter: none; }
  li .li-speak { color: #B7C2D2; line-height: 0; }
  li .li-speak:hover { color: var(--navy); }

  /* 追加/編集タブの編集リスト */
  ul.edit-list { list-style: none; display: flex; flex-direction: column; gap: 6px; margin-top: 10px; }
  ul.edit-list li {
    display: flex; align-items: center; gap: 6px;
    background: #fff; border: 1px solid #E1E7EF; border-radius: 10px; padding: 8px 8px 8px 4px;
  }
  ul.edit-list li.dragging { opacity: .4; }
  ul.edit-list li.drop-above { box-shadow: 0 -2px 0 var(--navy); }
  ul.edit-list li.drop-below { box-shadow: 0 2px 0 var(--navy); }
  .drag-handle {
    cursor: grab; color: #B7C2D2; font-size: 18px; padding: 8px 5px; line-height: 1;
    touch-action: none; user-select: none; -webkit-user-select: none; flex-shrink: 0;
  }
  .drag-handle:active { cursor: grabbing; }
  .e-main { flex: 1; min-width: 0; }
  .e-en { font-family: Georgia, serif; font-weight: 600; font-size: 16px; }
  .e-ja { font-size: 12px; color: var(--sub); line-height: 1.5; }
  .e-actions { display: flex; align-items: center; gap: 0; flex-shrink: 0; }
  .e-actions button { border: none; background: transparent; cursor: pointer; padding: 7px 6px; line-height: 0; border-radius: 8px; }
  .e-move { display: flex; flex-direction: column; gap: 1px; }
  .e-move button {
    border: 1px solid var(--line); background: #F4F6F9; color: #3A4A66;
    width: 26px; height: 18px; border-radius: 5px; cursor: pointer; font-size: 10px; line-height: 1; padding: 0;
  }
  .e-move button:disabled { opacity: .3; cursor: default; }
  .e-edit { color: #7A8AA5; font-size: 15px; line-height: 1; }
  .e-del { color: var(--danger); font-size: 15px; line-height: 1; }

  /* 編集モーダル */
  .edit-overlay { position: fixed; inset: 0; background: rgba(30,42,64,.45); display: flex; align-items: center; justify-content: center; padding: 20px; z-index: 100; }
  .edit-card { background: #fff; border-radius: 16px; padding: 22px; width: 100%; max-width: 420px; box-shadow: 0 20px 50px rgba(0,0,0,.3); }
  .edit-card h2 { font-size: 17px; margin-bottom: 16px; }
  .edit-field { margin-bottom: 14px; }
  .edit-field label { display: block; font-size: 12px; color: var(--sub); margin-bottom: 5px; }
  .edit-field input {
    width: 100%; padding: 10px 13px; border-radius: 10px; border: 1px solid var(--line);
    background: #F4F6F9; font-size: 15px; font-family: inherit; color: var(--ink); -webkit-appearance: none;
  }
  .edit-field input:focus { outline: none; border-color: var(--navy); background: #fff; }
  .edit-field input.en-input { font-family: Georgia, serif; font-weight: 600; }
  .edit-actions { display: flex; gap: 8px; justify-content: flex-end; margin-top: 20px; }

  /* テスト */
  .test-setup, .test-quiz, .test-result { background: #fff; border: 1px solid var(--line); border-radius: 14px; padding: 20px; }
  .test-setup h2, .test-result h2 { font-size: 17px; margin-bottom: 14px; }
  .opt-group { margin-bottom: 16px; }
  .opt-label { font-size: 13px; color: var(--sub); margin-bottom: 8px; display: block; }
  .opt-row { display: flex; gap: 8px; flex-wrap: wrap; }
  .opt-row button {
    font-family: inherit; font-size: 14px; cursor: pointer;
    padding: 8px 16px; border-radius: 10px; border: 1px solid var(--line);
    background: #F4F6F9; color: #3A4A66;
  }
  .opt-row button.sel { background: var(--navy); border-color: var(--navy); color: #fff; font-weight: 600; }
  .opt-row button:disabled { opacity: .4; cursor: default; }
  .quiz-head { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 6px; }
  .quiz-no { font-size: 13px; color: var(--sub); font-variant-numeric: tabular-nums; }
  .quiz-score { font-size: 13px; color: var(--sub); font-variant-numeric: tabular-nums; }
  .quiz-bar { height: 6px; border-radius: 3px; background: #E3E8F0; overflow: hidden; margin-bottom: 20px; }
  .quiz-bar > div { height: 100%; background: var(--navy); border-radius: 3px; transition: width .3s; }
  .quiz-word { font-family: Georgia, 'Times New Roman', serif; font-size: 34px; font-weight: 600; text-align: center; margin: 10px 0 24px; }
  .speak-inline { border: none; background: transparent; cursor: pointer; vertical-align: middle; line-height: 0; padding: 4px; color: var(--navy); transition: transform .15s; }
  .speak-inline:hover { transform: scale(1.15); }
  .choices { display: flex; flex-direction: column; gap: 10px; }
  .choices button {
    font-family: inherit; font-size: 15px; text-align: left; cursor: pointer;
    padding: 13px 16px; border-radius: 10px; border: 1px solid var(--line);
    background: #F4F6F9; color: var(--ink); line-height: 1.5;
  }
  .choices button:not(:disabled):hover { border-color: var(--navy); }
  .choices button.correct { background: #E4F2E7; border-color: #4E9B60; color: #205C30; font-weight: 600; }
  .choices button.wrong { background: #F8E7E7; border-color: #C46A6A; color: #8A3535; }
  .choices button:disabled { cursor: default; }
  .quiz-next { margin-top: 18px; text-align: right; }
  .result-score { text-align: center; margin: 8px 0 20px; }
  .result-score .big { font-size: 46px; font-weight: 700; color: var(--navy); font-variant-numeric: tabular-nums; }
  .result-score .rate { font-size: 15px; color: var(--sub); margin-top: 4px; }
  .wrong-list { list-style: none; display: flex; flex-direction: column; gap: 6px; margin: 12px 0 18px; }
  .wrong-list li { display: flex; gap: 12px; align-items: baseline; background: #F8F4EE; border: 1px solid #E8DECE; border-radius: 8px; padding: 10px 14px; font-size: 14px; }
  .wrong-list .w-en { font-family: Georgia, serif; font-weight: 600; min-width: 7rem; }
  .wrong-list .w-ja { color: #3A4A66; }
  .result-actions { display: flex; gap: 8px; flex-wrap: wrap; }
  .reveal-box { text-align: center; margin: 8px 0 20px; }
  .reveal-ja { font-size: 26px; font-weight: 600; color: var(--navy); margin-bottom: 4px; }
  .selfcheck-btns { display: flex; gap: 10px; justify-content: center; }
  .selfcheck-btns button { font-family: inherit; font-size: 16px; font-weight: 600; cursor: pointer; padding: 14px 26px; border-radius: 12px; border: none; min-width: 130px; }
  .btn-know { background: #E4F2E7; color: #205C30; border: 1px solid #4E9B60; }
  .btn-know:hover { background: #D3EAD8; }
  .btn-dontknow { background: #F8E7E7; color: #8A3535; border: 1px solid #C46A6A; }
  .btn-dontknow:hover { background: #F2D8D8; }
  .celebrate { text-align: center; padding: 8px 0 4px; }
  .celebrate .emoji { font-size: 52px; line-height: 1; animation: pop .5s cubic-bezier(.2,1.6,.4,1); }
  .celebrate .headline { font-size: 22px; font-weight: 700; margin-top: 10px; }
  .celebrate .sub { font-size: 14px; color: var(--sub); margin-top: 6px; }
  @keyframes pop { 0% { transform: scale(.3); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }

  #toast {
    position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%);
    background: var(--ink); color: #fff; font-size: 14px; padding: 8px 18px; border-radius: 999px;
    box-shadow: 0 4px 12px rgba(0,0,0,.2); opacity: 0; transition: opacity .25s; pointer-events: none; z-index: 200;
  }
  #toast.show { opacity: 1; }
  @media (prefers-reduced-motion: reduce) { .card-inner { transition: none; } .celebrate .emoji { animation: none; } }
  @media (max-width: 560px) {
    header { flex-direction: column; align-items: stretch; gap: 12px; margin-bottom: 18px; }
    .tabs {
      flex-wrap: nowrap; overflow-x: auto; -webkit-overflow-scrolling: touch;
      justify-content: flex-start; scrollbar-width: none;
    }
    .tabs::-webkit-scrollbar { display: none; }
    .tabs button { flex: 1 0 auto; white-space: nowrap; text-align: center; }
  }
  @media (max-width: 480px) { .word-en { font-size: 32px; } .word-ja { font-size: 24px; } }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div>
      <h1>単語帳</h1>
      <p class="count" id="count"></p>
    </div>
    <nav class="tabs">
      <button id="tab-study" class="active">学習</button>
      <button id="tab-test">テスト</button>
      <button id="tab-list">単語帳</button>
      <button id="tab-add">追加/編集</button>
    </nav>
  </header>

  <section id="view-study">
    <div class="toolbar">
      <div class="mode">
        <button id="mode-all" class="active">すべて</button>
        <button id="mode-star" class="star-mode">★のみ</button>
        <button id="mode-mark">マーカー</button>
      </div>
      <button class="btn" id="shuffle-btn">シャッフル</button>
    </div>
    <div id="study-area"></div>
  </section>

  <section id="view-test" hidden><div id="test-area"></div></section>

  <section id="view-list" hidden>
    <div class="list-toolbar">
      <input id="list-search" type="search" placeholder="単語を検索…" autocomplete="off">
      <div class="chips" id="list-filter">
        <button data-lf="all" class="sel">すべて</button>
        <button data-lf="star">★</button>
        <button data-lf="mark">🖍</button>
      </div>
    </div>
    <p class="list-count" id="list-count"></p>
    <ul class="word-list" id="word-list"></ul>
  </section>

  <section id="view-add" hidden>
    <div class="import-box">
      <h2>単語をまとめて追加</h2>
      <p class="note">1行に1語。「apple,りんご」「apple りんご」「apple→りんご」の形式に対応。<br>Claudeに単語リストを送れば、この形式に整えてもらえます。</p>
      <textarea id="import-text" placeholder="apple,りんご&#10;run,走る"></textarea>
      <button class="btn primary" id="import-btn" disabled>追加する</button>
    </div>
    <div class="import-box">
      <h2>単語の編集・並べ替え</h2>
      <p class="note">✎で意味やスペルを編集、ハンドル(⠿)をドラッグ、または▲▼で並べ替え、🗑で削除できます。</p>
      <input id="del-search" type="search" placeholder="単語を検索" class="del-search">
      <ul class="edit-list" id="edit-list"></ul>
    </div>
  </section>
</div>
<div id="toast" role="status"></div>
<script>
const DEFAULT_WORDS = [
  { en: "discursive", ja: "論述的な、言説としての" },
  { en: "undergo", ja: "(変化などを)経験する、被る" },
  { en: "profound", ja: "深遠な、根本的な" },
  { en: "in the face of", ja: "~に直面して" },
  { en: "material", ja: "具体的な、物質的な" },
  { en: "evidence", ja: "証拠" },
  { en: "composition", ja: "構成、成り立ち" },
  { en: "extent", ja: "広がり、範囲" },
  { en: "undertake", ja: "引き受ける、着手する" },
  { en: "entail", ja: "含む、必然的に伴う" },
  { en: "radical", ja: "根本的な、徹底的な" },
  { en: "revision", ja: "修正、改訂" },
  { en: "theologically", ja: "神学的に" },
  { en: "dominate", ja: "支配する" },
  { en: "on the one hand", ja: "一方では(⇔ on the other hand)" },
  { en: "body of work", ja: "(画家・作家などの)作品全体" },
  { en: "sheer", ja: "まったくの、純然たる" },
  { en: "counterpart", ja: "対応するもの、対の片方" },
  { en: "be taken as", ja: "~とみなされる" },
  { en: "celebrated", ja: "有名な、名高い" },
  { en: "the latter", ja: "後者(⇔ the former 前者)" },
  { en: "possess", ja: "所有する、持つ" },
  { en: "human Nature", ja: "人間の性質" },
  { en: "trace", ja: "たどる、跡をたどって調べる" },
  { en: "comparative", ja: "相対的な、(歴史が)まだ浅い" },
  { en: "but", ja: "~にすぎない(only の意味の副詞)" },
  { en: "unroll", ja: "広げる、展開する" },
  { en: "at once", ja: "一気に、同時に" },
  { en: "refinement", ja: "洗練、洗練された文化" },
  { en: "state", ja: "状態、段階(present state=現在の状態)" },
  { en: "civility", ja: "文明、洗練された状態" },
  { en: "barbarism", ja: "野蛮、未開の状態" },
  { en: "savage", ja: "未開の、野蛮な" },
  { en: "perception", ja: "認識" },
  { en: "underestimate", ja: "過小評価する" },
  { en: "relatively", ja: "比較的、割合に" },
  { en: "geographical", ja: "地理的な" },
  { en: "exploration", ja: "探検、探査" },
  { en: "progressively", ja: "次第に、徐々に" },
  { en: "accord with", ja: "~と一致する、合致する" },
  { en: "such A as B", ja: "BのようなA" },
  { en: "consist of", ja: "~から成る" },
  { en: "bring ... to light", ja: "~を明るみに出す、明らかにする" },
  { en: "see", ja: "わかる、理解する、見て取れる" },
  { en: "on the other hand", ja: "他方では" },
  { en: "observation", ja: "観察、所見" },
  { en: "dreadful", ja: "恐ろしい、悲惨な" },
  { en: "align", ja: "一致させる、軌を一にする" },
  { en: "despotism", ja: "専制政治" },
  { en: "pertain", ja: "関係する、かかわる" },
  { en: "debate", ja: "議論、論争" },
  { en: "morality", ja: "道徳性、モラル" },
  { en: "acute", ja: "深刻な、切実な" },
  { en: "in turn", ja: "つまり、今度は、ひいては" },
  { en: "in the wake of", ja: "~の直後で、~を受けて" },
  { en: "pronounced", ja: "顕著な、はっきり表れた" },
  { en: "description", ja: "記述、描写" },
  { en: "tomb", ja: "墓、廟" },
  { en: "splendid", ja: "素晴らしい、壮麗な" },
  { en: "ruined", ja: "廃墟となった" },
  { en: "panoramic", ja: "全景の、パノラマの" },
  { en: "balcony", ja: "バルコニー、露台" },
  { en: "minaret", ja: "(イスラム寺院の)光塔、ミナレット" },
  { en: "confirm", ja: "裏づける、確証する" },
  { en: "impression", ja: "印象" },
  { en: "correspondingly", ja: "それに応じて" },
  { en: "overview", ja: "全体像、概観" },
  { en: "metonymic", ja: "換喩的な" },
  { en: "summit", ja: "頂上" },
  { en: "range over", ja: "~を見わたす、~に及ぶ" },
  { en: "circuit of country", ja: "田園地帯、一帯の土地" },
  { en: "not less than", ja: "~を下らない、少なくとも~" },
  { en: "whole", ja: "全体、すべて" },
  { en: "prodigious", ja: "巨大な、驚異的な" },
  { en: "ruins", ja: "廃墟、遺跡" },
  { en: "ancient", ja: "古代の" },
  { en: "grandeur", ja: "壮大さ、壮麗" },
  { en: "glittering", ja: "きらめく、輝く" },
  { en: "exhibit", ja: "示す、呈する" },
  { en: "melancholy", ja: "物悲しい、憂鬱な" },
  { en: "attend", ja: "~に伴う" },
  { en: "present", ja: "現在の(present state=現在の状態)/示す" },
  { en: "consequence", ja: "結果、帰結" },
  { en: "ambition", ja: "野心" },
  { en: "horror", ja: "惨劇、恐怖" },
  { en: "dissention", ja: "内紛、不和(= dissension)" },
  { en: "plenitude", ja: "全盛期、絶頂(in plenitude of power=全盛期に)" },
  { en: "exercise", ja: "(権力を)行使する" },
  { en: "for", ja: "というのも~だから(接続詞)" },
  { en: "industry", ja: "勤勉さ/産業" },
  { en: "climate", ja: "気候、風土" },
  { en: "degree", ja: "程度" },
  { en: "desolation", ja: "荒廃、荒れ果てた状態" },
  { en: "silence", ja: "静寂" },
  { en: "ravages", ja: "損害、惨禍" },
  { en: "insignificant", ja: "取るに足りない、無意味な" },
  { en: "resound", ja: "鳴り響く、広く知れ渡る" },
  { en: "site", ja: "場所、用地" },
  { en: "sight", ja: "眺め、光景" },
  { en: "liberality", ja: "寛大さ、気前のよさ" },
  { en: "humanity", ja: "人間性、人道" },
  { en: "praise", ja: "称賛" },
  { en: "founder", ja: "創設者" },
  { en: "associate", ja: "結びつける、関連づける" },
  { en: "corrupting", ja: "堕落させる、腐敗させる" },
  { en: "corrupting effects", ja: "弊害、堕落させる影響" },
  { en: "luxury", ja: "贅沢、奢侈" },
  { en: "vice", ja: "悪徳、不道徳" },
  { en: "reduce A to B", ja: "AをB(悪い状態)に陥らせる/隷属させる" },
  { en: "prince", ja: "君主、王侯" },
  { en: "poverty", ja: "貧困" },
  { en: "revenue", ja: "収入、歳入" },
  { en: "delegate", ja: "委任する、任せる" },
  { en: "artful", ja: "狡猾な、老獪な" },
  { en: "avaricious", ja: "強欲な" },
  { en: "designing", ja: "陰謀を企む、たくらみのある" },
  { en: "concerns", ja: "(国家の)職務、関心事" },
  { en: "virtually", ja: "事実上、ほとんど" },
  { en: "plunderer", ja: "略奪者" },
  { en: "subject", ja: "臣民、国民" },
  { en: "retain", ja: "保ち続ける、抱き続ける" },
  { en: "regard", ja: "尊敬、敬意" },
  { en: "distress", ja: "苦難、危急" },
  { en: "destitute", ja: "(~を)まったく欠いた、見捨てられた" },
  { en: "at large", ja: "全体としての、一般の" },
  { en: "prey", ja: "餌食、獲物" },
  { en: "governor", ja: "統治者、総督" },
  { en: "invader", ja: "侵略者" },
  { en: "adaptation", ja: "翻案、適応させたもの" },
  { en: "longstanding", ja: "長年にわたる" },
  { en: "argument", ja: "議論、主張" },
  { en: "derive", ja: "由来する、派生する" },
  { en: "ultimately", ja: "究極的には、もとをたどれば" },
  { en: "classical", ja: "古典の、古代ギリシア・ローマの" },
  { en: "source", ja: "源、典拠" },
  { en: "deplorable", ja: "嘆かわしい、由々しい" },
  { en: "manly", ja: "男らしい、雄々しい" },
  { en: "broadly put", ja: "大まかに言えば" },
  { en: "hold that", ja: "~と主張する、考える" },
  { en: "virtue", ja: "徳、美徳" },
  { en: "bear arms", ja: "武器を取る、武装する" },
  { en: "be subject to", ja: "~にさらされやすい、~を受けやすい" },
  { en: "corruption", ja: "腐敗、堕落" },
  { en: "external", ja: "外部の、外来の" },
  { en: "commerce", ja: "商業、通商" },
  { en: "to do with", ja: "~に関係する" },
  { en: "mostly", ja: "たいてい、主として" },
  { en: "consumption", ja: "消費" },
  { en: "indulgence", ja: "ふけること、おぼれること(恩恵)" },
  { en: "commodity", ja: "商品、物資" },
  { en: "enervate", ja: "衰弱させる、気力を奪う" },
  { en: "indolence", ja: "怠惰" },
  { en: "dissipation", ja: "遊びほうけること、遊興" },
  { en: "consequent", ja: "結果として生じる" },
  { en: "public spiritedness", ja: "公共心" },
  { en: "body politic", ja: "国家、政治的共同体" },
  { en: "corrupt", ja: "堕落させる、腐敗させる" },
  { en: "over-importation", ja: "過剰な輸入" },
  { en: "empowerment", ja: "権力の付与、地位の向上" },
  { en: "merchant", ja: "商人" },
  { en: "interests", ja: "利益、利害" },
  { en: "propensity", ja: "傾向、性向" },
  { en: "social ties", ja: "社会的絆" },
  { en: "internecine", ja: "相互破滅的な、共倒れの" },
  { en: "exposure", ja: "さらされること" },
  { en: "invasion", ja: "侵略" },
  { en: "maritime", ja: "海洋の、海運の" },
  { en: "notion", ja: "考え、観念" },
  { en: "ominous", ja: "不吉な" },
  { en: "moreover", ja: "さらに、その上" },
  { en: "theorist", ja: "理論家" },
  { en: "acquisition", ja: "獲得、取得" },
  { en: "explicitly", ja: "明示的に、はっきりと" },
  { en: "imperialism", ja: "帝国主義" },
  { en: "fleet", ja: "艦隊" },
  { en: "facility", ja: "能力、才能(facility for ~)/容易さ" },
  { en: "conveyance", ja: "輸送、運搬" },
  { en: "ruinous", ja: "破滅的な" },
  { en: "maxim", ja: "格言、原則" },
  { en: "prevail", ja: "普及する、広まる" },
  { en: "estimate", ja: "評価する、判断する" },
  { en: "consist", ja: "(~に)ある、存する" },
  { en: "servitude", ja: "隷属、屈従" },
  { en: "reduce", ja: "~の状態にする、減らす(reduce A to B)" },
  { en: "prevailing", ja: "支配的な、大勢を占める" },
  { en: "attitude", ja: "態度、姿勢" },
  { en: "among", ja: "~のあいだで、~の中で" },
  { en: "displace", ja: "置き換える、転移させる" },
  { en: "displace onto", ja: "~を…へ転移させる、移し替える" },
  { en: "via", ja: "~を通じて、~を経由して" },
  { en: "overt", ja: "あからさまな、公然の" },
  { en: "concern", ja: "関心、こだわり" },
  { en: "surrogate", ja: "代理として置き換える" },
  { en: "presence", ja: "存在、駐留" },
  { en: "lament", ja: "嘆き、哀歌" },
  { en: "complaint", ja: "不平、苦情" },
  { en: "over-run", ja: "蹂躙する、席巻する" },
  { en: "no matter if", ja: "たとえ~であろうとも" },
  { en: "siege", ja: "包囲攻撃" },
  { en: "despite", ja: "~にもかかわらず" },
  { en: "witness", ja: "目撃する" },
  { en: "belong to", ja: "~に属する、~のものである" },
  { en: "properly", ja: "適切に、本来は" },
  { en: "admonition", ja: "訓戒、警告" },
  { en: "benefits", ja: "恩恵、利益" },
  { en: "pitch against", ja: "~に対置する、対立させる" },
  { en: "Ozymandian", ja: "オジマンディアス的な(栄華の無常を示す)" },
  { en: "dialectically", ja: "弁証法的に" },
  { en: "frequent", ja: "頻繁な、たびたびの" },
  { en: "account", ja: "記述、説明" },
  { en: "dialectic", ja: "弁証法、対立する二項の緊張" },
  { en: "split", ja: "分裂、隔たり" },
  { en: "aesthetically", ja: "美的に、美学的に" },
  { en: "expressed", ja: "表現された" },
  { en: "repository", ja: "貯蔵庫、宝庫" },
  { en: "representation", ja: "描写、表象" },
  { en: "region", ja: "地域、地方" },
  { en: "A as well as B", ja: "BだけでなくAも" },
  { en: "aspect", ja: "側面" },
  { en: "instant", ja: "瞬間" },
  { en: "exhibition", ja: "展覧会" },
  { en: "amount", ja: "量、総量" },
  { en: "whether", ja: "~であれ(…であれ)、~かどうか" },
  { en: "illustration", ja: "挿絵、図版" },
  { en: "not to mention", ja: "~は言うまでもなく" },
  { en: "prints", ja: "版画" },
  { en: "sense", ja: "感覚、意味" },
  { en: "visible", ja: "目に見える" },
  { en: "perhaps", ja: "おそらく" },
  { en: "arresting", ja: "人目を引く、はっとさせる" },
  { en: "vivid", ja: "鮮烈な、鮮やかな" },
  { en: "propose", ja: "提示する、提案する" },
  { en: "suggest that", ja: "~を示唆する" },
  { en: "one … the other", ja: "一方…他方" },
  { en: "elision", ja: "省略、融合" },
  { en: "mankind", ja: "人類" },
  { en: "mapping", ja: "地図化、地図作成" },
  { en: "exact", ja: "正確な、寸分たがわぬ" },
  { en: "correlate", ja: "対応物、相関するもの" },
  { en: "necessarily", ja: "必然的に" },
  { en: "register", ja: "記録、(表現の)様式" },
  { en: "visual", ja: "視覚的な" },
  { en: "available", ja: "利用できる、~されうる" },
  { en: "ascribe", ja: "(~に)帰する、あるとみなす" },
  { en: "manner", ja: "方法、やり方(the manner in which=~のありさま)" },
];

const KEY = "vocab-todai-reading-s10";
const SPK = '<svg viewBox="0 0 24 24" width="22" height="22" aria-hidden="true"><polygon points="11 5 6.5 9 3 9 3 15 6.5 15 11 19" fill="currentColor"/><path d="M14.8 8.6a5 5 0 0 1 0 6.8" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"/><path d="M17.6 5.9a9 9 0 0 1 0 12.2" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"/></svg>';

let words = [];
let tab = "study";
let studyFilter = "all";      // all | star | mark
let order = [], pos = 0, flipped = false, studyDone = false;
let listFilter = "all", listQuery = "", delQuery = "";

// ── 保存・読み込み ──
function load() {
  let saved = null;
  try { const raw = localStorage.getItem(KEY); if (raw) saved = JSON.parse(raw); } catch (e) {}
  if (saved && Array.isArray(saved.words) && saved.words.length > 0) {
    words = saved.words;
    const known = new Set(words.map(w => w.en.toLowerCase()));
    DEFAULT_WORDS.forEach((d, i) => {
      if (!known.has(d.en.toLowerCase()))
        words.push({ id: "d" + Date.now() + "_" + i, en: d.en, ja: d.ja, starred: false, marked: false });
    });
  } else {
    words = DEFAULT_WORDS.map((d, i) => ({ id: "w" + i, en: d.en, ja: d.ja, starred: false, marked: false }));
  }
  words.forEach(w => { w.marked = !!w.marked; w.starred = !!w.starred; });
}
let saveTimer;
function save() {
  clearTimeout(saveTimer);
  saveTimer = setTimeout(() => {
    try { localStorage.setItem(KEY, JSON.stringify({ words })); } catch (e) {}
  }, 300);
}

function speak(text) {
  try {
    if (!("speechSynthesis" in window)) { toast("この端末では音声を再生できません"); return; }
    speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    u.lang = "en-US"; u.rate = 0.95;
    speechSynthesis.speak(u);
  } catch (e) {}
}

const $ = id => document.getElementById(id);
function esc(s) { return String(s).replace(/[&<>"']/g, c => ({ "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;" }[c])); }
function matchQuery(w, q) {
  q = (q || "").trim().toLowerCase();
  if (!q) return true;
  return w.en.toLowerCase().includes(q) || w.ja.includes(q);
}

// ── 学習 ──
function pool() {
  if (studyFilter === "star") return words.filter(w => w.starred);
  if (studyFilter === "mark") return words.filter(w => w.marked);
  return words.slice();
}
function rebuildOrder() {
  const ids = pool().map(w => w.id);
  const kept = order.filter(id => ids.includes(id));
  const added = ids.filter(id => !kept.includes(id));
  order = kept.concat(added);
  if (pos >= order.length) pos = 0;
}
function shuffleOrder() {
  for (let i = order.length - 1; i > 0; i--) { const j = Math.floor(Math.random()*(i+1)); [order[i],order[j]]=[order[j],order[i]]; }
  pos = 0; flipped = false;
}

function render() {
  $("count").textContent = "全 " + words.length + " ・ ★ " + words.filter(w=>w.starred).length + " ・ 🖍 " + words.filter(w=>w.marked).length;
  $("tab-study").classList.toggle("active", tab === "study");
  $("tab-test").classList.toggle("active", tab === "test");
  $("tab-list").classList.toggle("active", tab === "list");
  $("tab-add").classList.toggle("active", tab === "add");
  $("view-study").hidden = tab !== "study";
  $("view-test").hidden = tab !== "test";
  $("view-list").hidden = tab !== "list";
  $("view-add").hidden = tab !== "add";
  $("mode-all").classList.toggle("active", studyFilter === "all");
  $("mode-star").classList.toggle("active", studyFilter === "star");
  $("mode-mark").classList.toggle("active", studyFilter === "mark");
  if (tab === "study") renderStudy();
  else if (tab === "test") renderTest();
  else if (tab === "list") renderList();
  else renderAdd();
}

function renderStudy() {
  const area = $("study-area");
  rebuildOrder();
  if (order.length === 0) {
    area.innerHTML = '<div class="empty"><p>' +
      (studyFilter === "star" ? "★の付いた単語がありません" : studyFilter === "mark" ? "マーカーの付いた単語がありません" : "単語がありません") +
      "</p><p>" +
      (studyFilter === "star" ? "学習中にカード右上の ★ を押すと、ここに集まります。"
        : studyFilter === "mark" ? "「単語帳」タブで 🖍 を押すと、ここに集まります。"
        : "「追加/編集」タブから単語を追加してください。") + "</p></div>";
    return;
  }
  if (studyDone) {
    const starN = words.filter(x => x.starred).length;
    area.innerHTML = '<div class="test-result"><div class="celebrate">' +
        '<div class="emoji">🎉</div><div class="headline">よく頑張りました!</div>' +
        '<div class="sub">' + order.length + '語を一周しました' + (studyFilter==="star"?'(★のみ)':studyFilter==="mark"?'(マーカーのみ)':'') + '。</div></div>' +
        '<div class="result-actions" style="justify-content:center;margin-top:16px;">' +
          (studyFilter !== "star" && starN > 0 ? '<button class="btn primary" id="review-star">★付きの単語で始める</button>' : '') +
          '<button class="btn' + ((studyFilter==="star"||starN===0)?' primary':'') + '" id="again-shuffle">シャッフルしてもう一周</button>' +
          '<button class="btn" id="again-top">最初からもう一周</button>' +
          '<button class="btn" id="goto-test">テストに挑戦</button>' +
        '</div></div>';
    const rs = $("review-star");
    if (rs) rs.addEventListener("click", () => { studyFilter="star"; studyDone=false; pos=0; flipped=false; render(); });
    $("again-shuffle").addEventListener("click", () => { studyDone=false; shuffleOrder(); render(); });
    $("again-top").addEventListener("click", () => { studyDone=false; pos=0; flipped=false; render(); });
    $("goto-test").addEventListener("click", () => { studyDone=false; pos=0; tab="test"; test.phase="setup"; render(); });
    return;
  }
  const w = words.find(x => x.id === order[pos]);
  area.innerHTML =
    '<div class="card3d"><div class="ring-metal"></div>' +
      '<div class="card-inner' + (flipped?" flipped":"") + '" id="card" role="button" tabindex="0" aria-label="カードをめくる">' +
        '<div class="card-face card-front"><div class="ring-hole"></div>' +
          '<button class="star-btn' + (w.starred?" on":"") + '" data-star aria-label="★を切り替え">★</button>' +
          '<button class="card-mark' + (w.marked?" on":"") + '" data-mark aria-label="マーカーを切り替え">🖍</button>' +
          '<button class="card-speak" data-speak aria-label="発音を聞く">' + SPK + '</button>' +
          '<span class="face-label">ENGLISH</span><p class="word-en' + (w.marked?" marked":"") + '">' + esc(w.en) + '</p>' +
          '<span class="hint">タップ / スワイプでめくる</span></div>' +
        '<div class="card-face card-back"><div class="ring-hole"></div>' +
          '<button class="star-btn' + (w.starred?" on":"") + '" data-star aria-label="★を切り替え">★</button>' +
          '<button class="card-mark' + (w.marked?" on":"") + '" data-mark aria-label="マーカーを切り替え">🖍</button>' +
          '<button class="card-speak" data-speak aria-label="発音を聞く">' + SPK + '</button>' +
          '<span class="face-label">日本語</span><p class="word-ja">' + esc(w.ja) + '</p>' +
          '<p class="word-en-small">' + esc(w.en) + '</p></div>' +
      '</div></div>' +
    '<div class="nav"><button class="btn" id="prev-btn">← 前へ</button>' +
      '<span class="pos">' + (pos+1) + ' / ' + order.length + '</span>' +
      '<button class="btn primary" id="next-btn">次へ →</button></div>' +
    '<p class="keys">スワイプ / ←→ = 移動 ・ タップ / スペース = めくる ・ S = ★</p>';

  const card = $("card");
  card.addEventListener("click", () => { flipped = !flipped; card.classList.toggle("flipped", flipped); });
  card.addEventListener("keydown", e => { if (e.key === "Enter") { flipped = !flipped; card.classList.toggle("flipped", flipped); } });
  area.querySelectorAll("[data-star]").forEach(b => b.addEventListener("click", e => { e.stopPropagation(); toggleStar(w.id); }));
  area.querySelectorAll("[data-mark]").forEach(b => b.addEventListener("click", e => { e.stopPropagation(); toggleMark(w.id); }));
  area.querySelectorAll("[data-speak]").forEach(b => b.addEventListener("click", e => { e.stopPropagation(); speak(w.en); }));
  $("prev-btn").addEventListener("click", prev);
  $("next-btn").addEventListener("click", next);

  const wrap = area.querySelector(".card3d");
  let tsX = 0, tsY = 0;
  wrap.addEventListener("touchstart", e => { tsX = e.touches[0].clientX; tsY = e.touches[0].clientY; }, { passive: true });
  wrap.addEventListener("touchend", e => {
    const dx = e.changedTouches[0].clientX - tsX, dy = e.changedTouches[0].clientY - tsY;
    if (Math.abs(dx) > 50 && Math.abs(dx) > Math.abs(dy)*1.5) { e.preventDefault(); if (dx < 0) next(); else prev(); }
  }, { passive: false });
}

function next() {
  if (order.length === 0 || studyDone) return;
  flipped = false;
  if (pos + 1 >= order.length) { studyDone = true; render(); } else { pos++; render(); }
}
function prev() {
  if (order.length === 0) return;
  flipped = false;
  if (studyDone) { studyDone = false; render(); return; }
  pos = (pos - 1 + order.length) % order.length; render();
}

// ── 単語帳 ──
function renderList() {
  $("list-filter").querySelectorAll("button").forEach(b => b.classList.toggle("sel", b.dataset.lf === listFilter));
  let arr = words;
  if (listFilter === "star") arr = arr.filter(w => w.starred);
  if (listFilter === "mark") arr = arr.filter(w => w.marked);
  arr = arr.filter(w => matchQuery(w, listQuery));
  const ul = $("word-list");
  $("list-count").textContent = arr.length + " 語を表示中";
  if (arr.length === 0) {
    ul.innerHTML = '<li style="justify-content:center;color:var(--sub);font-size:13px;">該当する単語がありません</li>';
    return;
  }
  ul.innerHTML = arr.map(w =>
    '<li' + (w.starred?' class="is-star"':'') + '>' +
      '<div class="li-main"><span class="li-en' + (w.marked?" marked":"") + '">' + esc(w.en) + '</span>' +
        '<span class="li-ja">' + esc(w.ja) + '</span></div>' +
      '<div class="li-actions">' +
        '<button class="li-speak" data-speak="' + w.id + '" aria-label="発音を聞く">' + SPK + '</button>' +
        '<button class="li-mark' + (w.marked?" on":"") + '" data-mark="' + w.id + '" aria-label="マーカー">🖍</button>' +
        '<button class="li-star' + (w.starred?" on":"") + '" data-id="' + w.id + '" aria-label="★">★</button>' +
      '</div></li>'
  ).join("");
  ul.querySelectorAll(".li-star").forEach(b => b.addEventListener("click", () => toggleStar(b.dataset.id)));
  ul.querySelectorAll(".li-mark").forEach(b => b.addEventListener("click", () => toggleMark(b.dataset.mark)));
  ul.querySelectorAll(".li-speak").forEach(b => b.addEventListener("click", () => { const w = words.find(x=>x.id===b.dataset.speak); if (w) speak(w.en); }));
}

// ── 追加/編集 ──
function renderAdd() {
  const ul = $("edit-list");
  const filtering = (delQuery || "").trim().length > 0;
  const arr = words.filter(w => matchQuery(w, delQuery));
  if (arr.length === 0) {
    ul.innerHTML = '<li style="border:none;justify-content:center;color:var(--sub);font-size:13px;">該当する単語がありません</li>';
    return;
  }
  let html = "";
  if (filtering) html += '<li style="border:none;justify-content:center;color:var(--faint);font-size:11px;padding:2px;">検索中は並べ替えできません</li>';
  html += arr.map((w, idx) =>
    '<li draggable="' + (filtering?"false":"true") + '" data-id="' + w.id + '">' +
      (filtering ? '' : '<span class="drag-handle" data-handle aria-hidden="true">⠿</span>') +
      '<div class="e-move">' +
        '<button data-up="' + w.id + '"' + ((filtering||idx===0)?' disabled':'') + ' aria-label="上へ">▲</button>' +
        '<button data-down="' + w.id + '"' + ((filtering||idx===arr.length-1)?' disabled':'') + ' aria-label="下へ">▼</button>' +
      '</div>' +
      '<div class="e-main"><div class="e-en">' + esc(w.en) + '</div><div class="e-ja">' + esc(w.ja) + '</div></div>' +
      '<div class="e-actions">' +
        '<button class="e-edit" data-edit="' + w.id + '" aria-label="編集">✎</button>' +
        '<button class="e-del" data-del="' + w.id + '" aria-label="削除">🗑</button>' +
      '</div></li>'
  ).join("");
  ul.innerHTML = html;

  ul.querySelectorAll("[data-up]").forEach(b => b.addEventListener("click", () => moveWord(b.dataset.up, -1)));
  ul.querySelectorAll("[data-down]").forEach(b => b.addEventListener("click", () => moveWord(b.dataset.down, 1)));
  ul.querySelectorAll("[data-edit]").forEach(b => b.addEventListener("click", () => openEdit(b.dataset.edit)));
  ul.querySelectorAll("[data-del]").forEach(b => b.addEventListener("click", () => {
    const w = words.find(x => x.id === b.dataset.del);
    if (w && !confirm("「" + w.en + "」を削除しますか?")) return;
    words = words.filter(x => x.id !== b.dataset.del);
    order = order.filter(id => id !== b.dataset.del);
    save(); render();
    if (w) toast("「" + w.en + "」を削除しました");
  }));
  if (!filtering) setupDrag(ul);
}

function moveWord(id, dir) {
  const i = words.findIndex(w => w.id === id);
  if (i < 0) return;
  const j = i + dir;
  if (j < 0 || j >= words.length) return;
  [words[i], words[j]] = [words[j], words[i]];
  save(); renderAdd();
}

function reorderTo(dragId, targetId, below) {
  const from = words.findIndex(w => w.id === dragId);
  if (from < 0) return;
  const [moved] = words.splice(from, 1);
  let to = words.findIndex(w => w.id === targetId);
  if (to < 0) { words.splice(from, 0, moved); return; }
  words.splice(below ? to + 1 : to, 0, moved);
  save(); renderAdd();
}

function setupDrag(ul) {
  let dragId = null;
  ul.querySelectorAll("li[draggable='true']").forEach(li => {
    li.addEventListener("dragstart", e => { dragId = li.dataset.id; li.classList.add("dragging"); try { e.dataTransfer.effectAllowed = "move"; } catch (x) {} });
    li.addEventListener("dragend", () => { dragId = null; ul.querySelectorAll("li").forEach(x => x.classList.remove("dragging","drop-above","drop-below")); });
    li.addEventListener("dragover", e => {
      e.preventDefault();
      if (!dragId || li.dataset.id === dragId) return;
      const r = li.getBoundingClientRect(), below = (e.clientY - r.top) > r.height/2;
      ul.querySelectorAll("li").forEach(x => x.classList.remove("drop-above","drop-below"));
      li.classList.add(below ? "drop-below" : "drop-above");
    });
    li.addEventListener("drop", e => {
      e.preventDefault();
      if (!dragId || li.dataset.id === dragId) return;
      const r = li.getBoundingClientRect(), below = (e.clientY - r.top) > r.height/2;
      reorderTo(dragId, li.dataset.id, below);
    });
  });
  ul.querySelectorAll("[data-handle]").forEach(handle => {
    handle.addEventListener("touchstart", () => { const li = handle.closest("li"); dragId = li.dataset.id; li.classList.add("dragging"); }, { passive: true });
    handle.addEventListener("touchmove", e => {
      if (!dragId) return;
      e.preventDefault();
      const t = e.touches[0], target = document.elementFromPoint(t.clientX, t.clientY);
      const li = target && target.closest ? target.closest("li[data-id]") : null;
      ul.querySelectorAll("li").forEach(x => x.classList.remove("drop-above","drop-below"));
      if (li && li.dataset.id !== dragId) {
        const r = li.getBoundingClientRect(), below = (t.clientY - r.top) > r.height/2;
        li.classList.add(below ? "drop-below" : "drop-above");
      }
    }, { passive: false });
    handle.addEventListener("touchend", () => {
      if (!dragId) return;
      const marked = ul.querySelector(".drop-above, .drop-below");
      if (marked && marked.dataset.id !== dragId) reorderTo(dragId, marked.dataset.id, marked.classList.contains("drop-below"));
      else ul.querySelectorAll("li").forEach(x => x.classList.remove("dragging","drop-above","drop-below"));
      dragId = null;
    });
  });
}

function openEdit(id) {
  const w = words.find(x => x.id === id);
  if (!w) return;
  const ov = document.createElement("div");
  ov.className = "edit-overlay";
  ov.innerHTML = '<div class="edit-card" role="dialog" aria-modal="true"><h2>単語を編集</h2>' +
    '<div class="edit-field"><label>英語(スペル)</label><input class="en-input" id="edit-en"></div>' +
    '<div class="edit-field"><label>意味</label><input id="edit-ja"></div>' +
    '<div class="edit-actions"><button class="btn" id="edit-cancel">キャンセル</button><button class="btn primary" id="edit-save">保存</button></div></div>';
  document.body.appendChild(ov);
  const enI = $("edit-en"), jaI = $("edit-ja");
  enI.value = w.en; jaI.value = w.ja; enI.focus();
  const close = () => ov.remove();
  ov.addEventListener("click", e => { if (e.target === ov) close(); });
  $("edit-cancel").addEventListener("click", close);
  $("edit-save").addEventListener("click", () => {
    const en = enI.value.trim(), ja = jaI.value.trim();
    if (!en || !ja) { toast("英語と意味を入力してください"); return; }
    w.en = en; w.ja = ja; save(); close(); renderAdd(); toast("保存しました");
  });
}

// ── 操作 ──
function toggleStar(id) { const w = words.find(x=>x.id===id); if (w) { w.starred = !w.starred; save(); render(); } }
function toggleMark(id) { const w = words.find(x=>x.id===id); if (w) { w.marked = !w.marked; save(); render(); } }

let toastTimer;
function toast(msg) { const t = $("toast"); t.textContent = msg; t.classList.add("show"); clearTimeout(toastTimer); toastTimer = setTimeout(() => t.classList.remove("show"), 1800); }

function importWords() {
  const lines = $("import-text").value.split("\n").map(l=>l.trim()).filter(Boolean);
  let added = 0;
  for (const line of lines) {
    const m = line.match(/^([A-Za-z][A-Za-z' -]*?)\s*(?:[,、:：→\t]|\s{1,})\s*(.+)$/);
    if (m) {
      const en = m[1].trim(), ja = m[2].trim();
      if (en && ja && !words.some(w => w.en.toLowerCase() === en.toLowerCase())) {
        words.push({ id: "u" + Date.now() + "_" + added, en, ja, starred: false, marked: false }); added++;
      }
    }
  }
  if (added > 0) { $("import-text").value = ""; $("import-btn").disabled = true; save(); render(); toast(added + "語を追加しました"); }
  else toast("追加できる単語が見つかりませんでした");
}

// ── テスト ──
let test = { phase: "setup", scope: "all", count: 10, format: "choice", qs: [], i: 0, correctN: 0, wrongIds: [], answered: false, lastWrongIds: [] };
function testPool() {
  if (test.scope === "star") return words.filter(w => w.starred);
  if (test.scope === "mark") return words.filter(w => w.marked);
  if (test.scope === "wrong") return words.filter(w => test.lastWrongIds.includes(w.id));
  return words.slice();
}
function shuffleArr(a) { a = a.slice(); for (let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]]; } return a; }
function startTest(scopeOverride) {
  if (scopeOverride) test.scope = scopeOverride;
  const p = testPool();
  if (p.length === 0) { toast("対象の単語がありません"); return; }
  const n = test.count === "all" ? p.length : Math.min(test.count, p.length);
  test.qs = shuffleArr(p).slice(0, n).map(w => w.id);
  test.i = 0; test.correctN = 0; test.wrongIds = []; test.answered = false; test.phase = "quiz";
  renderTest();
}
function makeChoices(word) {
  const others = shuffleArr(words.filter(w => w.id !== word.id)).slice(0, 3).map(w => w.ja);
  return shuffleArr([word.ja].concat(others));
}
function finishTest() { test.lastWrongIds = test.wrongIds.slice(); test.phase = "result"; renderTest(); }

function renderTest() {
  const area = $("test-area");
  if (test.phase === "setup") {
    const starN = words.filter(w=>w.starred).length, markN = words.filter(w=>w.marked).length;
    const wrongN = words.filter(w => test.lastWrongIds.includes(w.id)).length;
    area.innerHTML = '<div class="test-setup"><h2>テスト設定</h2>' +
      '<div class="opt-group"><span class="opt-label">形式</span><div class="opt-row" id="format-row">' +
        '<button data-format="choice"' + (test.format==="choice"?' class="sel"':'') + '>4択クイズ</button>' +
        '<button data-format="self"' + (test.format==="self"?' class="sel"':'') + '>わかる・わからない</button></div></div>' +
      '<div class="opt-group"><span class="opt-label">出題範囲</span><div class="opt-row" id="scope-row">' +
        '<button data-scope="all"' + (test.scope==="all"?' class="sel"':'') + '>すべて(' + words.length + ')</button>' +
        '<button data-scope="star"' + (test.scope==="star"?' class="sel"':'') + (starN===0?' disabled':'') + '>★のみ(' + starN + ')</button>' +
        '<button data-scope="mark"' + (test.scope==="mark"?' class="sel"':'') + (markN===0?' disabled':'') + '>マーカー(' + markN + ')</button>' +
        (wrongN>0 ? '<button data-scope="wrong"' + (test.scope==="wrong"?' class="sel"':'') + '>前回の間違い(' + wrongN + ')</button>' : '') + '</div></div>' +
      '<div class="opt-group"><span class="opt-label">問題数</span><div class="opt-row" id="count-row">' +
        [10,20,30,50,100].map(n => '<button data-count="' + n + '"' + (test.count===n?' class="sel"':'') + '>' + n + '問</button>').join('') +
        '<button data-count="all"' + (test.count==="all"?' class="sel"':'') + '>全部</button></div></div>' +
      '<p style="font-size:12px;color:var(--sub);margin-bottom:16px;">' +
        (test.format==="choice" ? '英語を見て正しい意味を4択から選びます。出題と同時に音声が流れます。'
          : '「わかる / わからない」を選ぶと意味が表示されます。出題と同時に音声が流れます。') + '</p>' +
      '<button class="btn primary" id="start-test">テスト開始</button></div>';
    area.querySelectorAll("#format-row button").forEach(b => b.addEventListener("click", () => { test.format=b.dataset.format; renderTest(); }));
    area.querySelectorAll("#scope-row button").forEach(b => b.addEventListener("click", () => { if(!b.disabled){ test.scope=b.dataset.scope; renderTest(); } }));
    area.querySelectorAll("#count-row button").forEach(b => b.addEventListener("click", () => { test.count = b.dataset.count==="all"?"all":Number(b.dataset.count); renderTest(); }));
    $("start-test").addEventListener("click", () => startTest());
    return;
  }
  if (test.phase === "result") {
    const total = test.qs.length, rate = Math.round(test.correctN/total*100);
    const wrongWords = test.wrongIds.map(id => words.find(w=>w.id===id)).filter(Boolean);
    const starN = words.filter(w=>w.starred).length;
    let emoji, headline, sub;
    if (rate===100){emoji="🎉";headline="全問正解!よく頑張りました!";sub="この調子で次の範囲にも挑戦しよう。";}
    else if (rate>=80){emoji="✨";headline="よく頑張りました!";sub="あと少しで完璧。間違えた単語を復習しよう。";}
    else if (rate>=50){emoji="💪";headline="いい調子!";sub="間違えた単語だけでもう一周すると定着します。";}
    else {emoji="🌱";headline="ここからが伸びどころ!";sub="間違えた単語に★を付けて、カードで復習してからまた挑戦しよう。";}
    area.innerHTML = '<div class="test-result"><div class="celebrate">' +
        '<div class="emoji">' + emoji + '</div><div class="headline">' + headline + '</div><div class="sub">' + sub + '</div></div>' +
        '<div class="result-score"><div class="big">' + test.correctN + ' / ' + total + '</div><div class="rate">正答率 ' + rate + '%</div></div>' +
        (wrongWords.length>0
          ? '<p style="font-size:14px;font-weight:600;margin-bottom:4px;">間違えた単語(' + wrongWords.length + '語)</p><ul class="wrong-list">' +
            wrongWords.map(w => '<li><span class="w-en">' + esc(w.en) + '</span><span class="w-ja">' + esc(w.ja) + '</span></li>').join('') + '</ul>'
          : '') +
        '<div class="result-actions">' +
          (wrongWords.length>0 ? '<button class="btn primary" id="retry-wrong">間違いだけで再テスト</button>' : '') +
          (wrongWords.length>0 ? '<button class="btn" id="star-wrong">間違いに★を付ける</button>' : '') +
          (wrongWords.length===0 && starN>0 ? '<button class="btn primary" id="test-star">★付きの単語でテスト</button>' : '') +
          '<button class="btn" id="retry-same">もう一度</button><button class="btn" id="back-setup">設定に戻る</button></div></div>';
    const rw = $("retry-wrong"); if (rw) rw.addEventListener("click", () => { test.count="all"; startTest("wrong"); });
    const sw = $("star-wrong"); if (sw) sw.addEventListener("click", () => {
      let n = 0; test.lastWrongIds.forEach(id => { const w = words.find(x=>x.id===id); if (w && !w.starred){ w.starred=true; n++; } });
      save(); toast(n>0?n+"語に★を付けました":"すでに★が付いています"); renderTest();
      $("count").textContent = "全 " + words.length + " ・ ★ " + words.filter(w=>w.starred).length + " ・ 🖍 " + words.filter(w=>w.marked).length;
    });
    const ts = $("test-star"); if (ts) ts.addEventListener("click", () => { test.count="all"; startTest("star"); });
    $("retry-same").addEventListener("click", () => startTest());
    $("back-setup").addEventListener("click", () => { test.phase="setup"; renderTest(); });
    return;
  }
  const w = words.find(x => x.id === test.qs[test.i]);
  const head = '<div class="quiz-head"><span class="quiz-no">第 ' + (test.i+1) + ' 問 / ' + test.qs.length + '</span>' +
    '<span class="quiz-score">正解 ' + test.correctN + '</span></div>' +
    '<div class="quiz-bar"><div style="width:' + (test.i/test.qs.length*100) + '%"></div></div>' +
    '<p class="quiz-word">' + esc(w.en) + ' <button class="speak-inline" id="quiz-speak" aria-label="発音を聞く">' + SPK + '</button></p>';
  function goNext() { test.answered = false; if (test.i+1 < test.qs.length) { test.i++; renderTest(); } else finishTest(); }

  if (test.format === "choice") {
    const choices = makeChoices(w);
    area.innerHTML = '<div class="test-quiz">' + head + '<div class="choices">' +
      choices.map(c => '<button data-ja="' + esc(c) + '">' + esc(c) + '</button>').join('') + '</div>' +
      '<div class="quiz-next" hidden><button class="btn primary" id="next-q">次へ →</button></div></div>';
    $("quiz-speak").addEventListener("click", (e) => { e.stopPropagation(); speak(w.en); });
    speak(w.en);
    area.querySelectorAll(".choices button").forEach(btn => btn.addEventListener("click", () => {
      if (test.answered) return;
      test.answered = true;
      const ok = btn.dataset.ja === w.ja;
      if (ok) test.correctN++; else test.wrongIds.push(w.id);
      area.querySelectorAll(".choices button").forEach(b => { b.disabled = true; if (b.dataset.ja === w.ja) b.classList.add("correct"); else if (b === btn && !ok) b.classList.add("wrong"); });
      const nw = area.querySelector(".quiz-next"); nw.hidden = false;
      const nb = $("next-q"); nb.textContent = (test.i+1 < test.qs.length) ? "次へ →" : "結果を見る"; nb.focus();
      nb.addEventListener("click", goNext);
    }));
    return;
  }
  // わかる・わからない
  area.innerHTML = '<div class="test-quiz">' + head +
    '<div class="reveal-box" id="reveal-box"><p style="font-size:13px;color:var(--sub);margin-bottom:14px;">この単語の意味がわかりますか?</p>' +
      '<div class="selfcheck-btns"><button class="btn-know" id="know-btn">○ わかる</button><button class="btn-dontknow" id="dontknow-btn">✕ わからない</button></div></div></div>';
  $("quiz-speak").addEventListener("click", (e) => { e.stopPropagation(); speak(w.en); });
  speak(w.en);
  function showAnswer(knew) {
    if (knew) test.correctN++; else test.wrongIds.push(w.id);
    $("reveal-box").innerHTML = '<p class="reveal-ja">' + esc(w.ja) + '</p>' +
      '<p style="font-size:13px;font-weight:600;color:' + (knew?'#205C30':'#8A3535') + ';margin-bottom:16px;">' +
        (knew?'○ わかる':'✕ わからない → ここで覚えよう!') + '</p>' +
      '<button class="btn primary" id="next-q">' + (test.i+1 < test.qs.length ? '次へ →' : '結果を見る') + '</button>' +
      '<p style="font-size:11px;color:var(--faint);margin-top:10px;">画面のどこをタップしても進めます</p>';
    const box = area.querySelector(".test-quiz");
    const onTap = () => { box.removeEventListener("click", onTap); goNext(); };
    box.addEventListener("click", onTap);
    $("next-q").focus();
  }
  $("know-btn").addEventListener("click", (e) => { e.stopPropagation(); showAnswer(true); });
  $("dontknow-btn").addEventListener("click", (e) => { e.stopPropagation(); showAnswer(false); });
}

// ── イベント ──
$("tab-study").addEventListener("click", () => { tab = "study"; render(); });
$("tab-test").addEventListener("click", () => { tab = "test"; render(); });
$("tab-list").addEventListener("click", () => { tab = "list"; render(); });
$("tab-add").addEventListener("click", () => { tab = "add"; render(); });
$("mode-all").addEventListener("click", () => { studyFilter="all"; flipped=false; pos=0; studyDone=false; render(); });
$("mode-star").addEventListener("click", () => { studyFilter="star"; flipped=false; pos=0; studyDone=false; render(); });
$("mode-mark").addEventListener("click", () => { studyFilter="mark"; flipped=false; pos=0; studyDone=false; render(); });
$("shuffle-btn").addEventListener("click", () => { studyDone=false; rebuildOrder(); shuffleOrder(); render(); toast("シャッフルしました"); });
$("list-filter").querySelectorAll("button").forEach(b => b.addEventListener("click", () => { listFilter = b.dataset.lf; renderList(); }));
$("list-search").addEventListener("input", e => { listQuery = e.target.value; renderList(); });
$("del-search").addEventListener("input", e => { delQuery = e.target.value; renderAdd(); });
$("import-text").addEventListener("input", e => { $("import-btn").disabled = !e.target.value.trim(); });
$("import-btn").addEventListener("click", importWords);

document.addEventListener("keydown", e => {
  if (tab !== "study") return;
  if (e.target.tagName === "TEXTAREA" || e.target.tagName === "INPUT") return;
  if (e.code === "Space") { e.preventDefault(); if (studyDone) return; flipped = !flipped; const c = $("card"); if (c) c.classList.toggle("flipped", flipped); }
  else if (e.key === "ArrowRight") next();
  else if (e.key === "ArrowLeft") prev();
  else if (e.key.toLowerCase() === "s" && order.length > 0 && !studyDone) toggleStar(order[pos]);
});

load();
render();
</script>
</body>
</html>
