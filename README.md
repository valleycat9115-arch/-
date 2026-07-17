<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>単語帳 - 東大英語リーディング Session 10</title>
<style>
  :root {
    --bg: #E9EDF2;
    --ink: #1E2A40;
    --sub: #5B6B85;
    --faint: #98A6BC;
    --line: #C4CEDC;
    --navy: #2E4A8F;
    --star: #E0A62E;
    --danger: #A04040;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--ink);
    font-family: 'Hiragino Sans', 'Hiragino Kaku Gothic ProN', 'Yu Gothic', Meiryo, sans-serif;
    min-height: 100vh;
  }
  .wrap { max-width: 640px; margin: 0 auto; padding: 24px 16px 48px; }

  header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 24px; }
  h1 { font-size: 24px; letter-spacing: 0.05em; }
  .count { font-size: 12px; color: var(--sub); margin-top: 4px; }
  .tabs { display: flex; gap: 4px; padding: 4px; border-radius: 999px; background: #D8DFE9; }
  .tabs button {
    border: none; background: transparent; color: #3A4A66;
    padding: 7px 16px; border-radius: 999px; font-size: 14px; font-weight: 500;
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

  /* ---- カード ---- */
  .card3d { perspective: 1200px; position: relative; height: 300px; }
  .ring-metal {
    position: absolute; top: -12px; left: 16px; width: 26px; height: 34px;
    border: 3px solid var(--faint); border-radius: 50%;
    border-bottom-color: transparent; transform: rotate(20deg);
    pointer-events: none; z-index: 5;
  }
  .card-inner {
    position: relative; width: 100%; height: 100%;
    transition: transform 0.5s cubic-bezier(.2,.7,.3,1);
    transform-style: preserve-3d; cursor: pointer;
  }
  .card-inner.flipped { transform: rotateX(180deg); }
  .card-face {
    position: absolute; inset: 0; border-radius: 14px;
    backface-visibility: hidden; -webkit-backface-visibility: hidden;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    box-shadow: 0 8px 24px rgba(30,42,64,0.14), 0 1px 3px rgba(30,42,64,0.10);
  }
  .card-front { background: #fff; }
  .card-back { background: var(--navy); transform: rotateX(180deg); }
  .ring-hole {
    position: absolute; top: 14px; left: 18px; width: 22px; height: 22px;
    border-radius: 50%; background: var(--bg);
    box-shadow: inset 0 1px 3px rgba(30,42,64,0.35);
  }
  .face-label { font-size: 11px; letter-spacing: 0.25em; color: var(--faint); margin-bottom: 12px; }
  .card-back .face-label { color: #AAB9DC; }
  .word-en {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 40px; font-weight: 600; text-align: center; padding: 0 24px;
  }
  .word-ja { font-size: 30px; font-weight: 600; color: #fff; text-align: center; padding: 0 24px; }
  .word-en-small { font-family: Georgia, serif; font-size: 14px; color: #AAB9DC; margin-top: 12px; }
  .hint { font-size: 11px; color: var(--faint); margin-top: 24px; }
  .star-btn {
    position: absolute; top: 12px; right: 16px; z-index: 10;
    border: none; background: transparent; font-size: 26px; line-height: 1;
    cursor: pointer; color: var(--line); transition: transform .15s;
  }
  .star-btn:hover { transform: scale(1.15); }
  .star-btn.on { color: var(--star); }
  .card-back .star-btn { color: #5A73B0; }
  .card-back .star-btn.on { color: var(--star); }

  .nav { display: flex; align-items: center; justify-content: space-between; margin-top: 20px; }
  .nav .pos { font-size: 14px; color: var(--sub); font-variant-numeric: tabular-nums; }
  .keys { text-align: center; font-size: 11px; color: var(--faint); margin-top: 16px; }

  .empty {
    background: #fff; border: 1px dashed var(--line); border-radius: 14px;
    padding: 40px 20px; text-align: center;
  }
  .empty p:first-child { font-weight: 600; margin-bottom: 6px; }
  .empty p:last-child { font-size: 14px; color: var(--sub); }

  /* ---- 一覧 ---- */
  .import-box {
    background: #fff; border: 1px solid var(--line); border-radius: 14px;
    padding: 16px; margin-bottom: 20px;
  }
  .import-box h2 { font-size: 16px; margin-bottom: 4px; }
  .import-box .note { font-size: 12px; color: var(--sub); margin-bottom: 12px; line-height: 1.6; }
  textarea {
    width: 100%; border-radius: 10px; border: 1px solid var(--line);
    background: #F4F6F9; padding: 12px; font-size: 14px; font-family: inherit;
    color: var(--ink); resize: vertical; min-height: 96px;
  }
  .import-box .btn { margin-top: 8px; }

  ul.word-list { list-style: none; display: flex; flex-direction: column; gap: 8px; }
  ul.word-list li {
    display: flex; align-items: center; gap: 12px;
    background: #fff; border: 1px solid #DDE3EC; border-radius: 10px;
    padding: 12px 16px;
  }
  li .li-star {
    border: none; background: transparent; font-size: 20px; line-height: 1;
    cursor: pointer; color: var(--line);
  }
  li .li-star.on { color: var(--star); }
  li .li-en { font-family: Georgia, serif; font-weight: 600; min-width: 7.5rem; }
  li .li-ja { font-size: 14px; color: #3A4A66; flex: 1; }
  li .li-del {
    border: none; background: transparent; color: var(--danger);
    font-size: 12px; cursor: pointer; padding: 4px 6px; font-family: inherit;
  }

  #toast {
    position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%);
    background: var(--ink); color: #fff; font-size: 14px;
    padding: 8px 18px; border-radius: 999px;
    box-shadow: 0 4px 12px rgba(0,0,0,.2);
    opacity: 0; transition: opacity .25s; pointer-events: none;
  }
  #toast.show { opacity: 1; }

  @media (prefers-reduced-motion: reduce) {
    .card-inner { transition: none; }
  }
  @media (max-width: 480px) {
    .word-en { font-size: 32px; }
    .word-ja { font-size: 24px; }
    li .li-en { min-width: 5.5rem; }
  }
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
      <button id="tab-list">単語一覧</button>
    </nav>
  </header>

  <!-- 学習画面 -->
  <section id="view-study">
    <div class="toolbar">
      <div class="mode">
        <button id="mode-all" class="active">すべて</button>
        <button id="mode-star" class="star-mode">★のみ</button>
      </div>
      <button class="btn" id="shuffle-btn">シャッフル</button>
    </div>
    <div id="study-area"></div>
  </section>

  <!-- 一覧画面 -->
  <section id="view-list" hidden>
    <div class="import-box">
      <h2>単語をまとめて追加</h2>
      <p class="note">
        1行に1語。「apple,りんご」「apple りんご」「apple→りんご」の形式に対応。<br>
        Claudeに単語リストを送れば、この形式に整えてもらえます。
      </p>
      <textarea id="import-text" placeholder="apple,りんご&#10;run,走る"></textarea>
      <button class="btn primary" id="import-btn" disabled>追加する</button>
    </div>
    <ul class="word-list" id="word-list"></ul>
  </section>
</div>
<div id="toast" role="status"></div>

<script>
// ── デフォルト単語(Claudeに単語リストを送ると、ここに追加できます)──
const DEFAULT_WORDS = [
  { en: "discursive", ja: "論説的な、言説としての" },
  { en: "undergo", ja: "(変化などを)経験する、被る" },
  { en: "profound", ja: "深遠な、根本的な" },
  { en: "in the face of", ja: "~に直面して" },
  { en: "evidence", ja: "証拠" },
  { en: "composition", ja: "構成、成り立ち" },
  { en: "extent", ja: "広がり、範囲" },
  { en: "undertake", ja: "引き受ける、着手する" },
  { en: "entail", ja: "必然的に伴う" },
  { en: "radical", ja: "根本的な、徹底的な" },
  { en: "revision", ja: "修正、改訂" },
  { en: "theologically", ja: "神学的に" },
  { en: "dominate", ja: "支配する" },
  { en: "sheer", ja: "まったくの、純然たる" },
  { en: "counterpart", ja: "対応するもの、対の片方" },
  { en: "possess", ja: "所有する、持つ" },
  { en: "trace", ja: "たどる、跡をたどって調べる" },
  { en: "comparative", ja: "比較的な、(歴史が)まだ浅い" },
  { en: "unroll", ja: "広げる、展開する" },
  { en: "refinement", ja: "洗練、洗練された文化" },
  { en: "barbarism", ja: "野蛮、未開の状態" },
  { en: "perception", ja: "認識、捉え方" },
  { en: "underestimate", ja: "過小評価する" },
  { en: "relatively", ja: "比較的、割合に" },
  { en: "geographical", ja: "地理的な" },
  { en: "exploration", ja: "探検、探査" },
  { en: "progressively", ja: "次第に、徐々に" },
  { en: "on the other hand", ja: "他方では" },
  { en: "observation", ja: "観察、所見" },
  { en: "dreadful", ja: "恐ろしい、悲惨な" },
  { en: "align", ja: "一致させる、軌を一にする" },
  { en: "despotism", ja: "専制政治" },
  { en: "pertain", ja: "関係する、かかわる" },
  { en: "debate", ja: "議論、論争" },
  { en: "morality", ja: "道徳性、モラル" },
  { en: "acute", ja: "深刻な、切実な" },
  { en: "pronounced", ja: "顕著な、はっきり表れた" },
  { en: "description", ja: "記述、描写" },
  { en: "tomb", ja: "墓、廟" },
  { en: "splendid", ja: "壮麗な、見事な" },
  { en: "ruined", ja: "廃墟となった" },
  { en: "panoramic", ja: "全景の、パノラマの" },
  { en: "balcony", ja: "バルコニー、露台" },
  { en: "minaret", ja: "(イスラム寺院の)光塔、ミナレット" },
  { en: "confirm", ja: "裏づける、確証する" },
  { en: "impression", ja: "印象" },
  { en: "correspondingly", ja: "それに応じて" },
  { en: "overview", ja: "概観、全体の眺め" },
  { en: "metonymic", ja: "換喩的な" },
  { en: "summit", ja: "頂上" },
  { en: "prodigious", ja: "巨大な、驚異的な" },
  { en: "ruins", ja: "廃墟、遺跡" },
  { en: "ancient", ja: "古代の" },
  { en: "grandeur", ja: "壮大さ、壮麗" },
  { en: "glittering", ja: "きらめく、輝く" },
  { en: "exhibit", ja: "示す、呈する" },
  { en: "melancholy", ja: "物悲しい、憂鬱な" },
  { en: "consequence", ja: "結果、帰結" },
  { en: "ambition", ja: "野心" },
  { en: "horror", ja: "惨劇、恐怖" },
  { en: "dissention", ja: "内紛、不和(= dissension)" },
  { en: "plenitude", ja: "充満、絶頂" },
  { en: "exercise", ja: "(権力を)行使する" },
  { en: "climate", ja: "気候、風土" },
  { en: "degree", ja: "程度" },
  { en: "desolation", ja: "荒廃、荒れ果てた状態" },
  { en: "silence", ja: "静寂" },
  { en: "ravages", ja: "惨禍、破壊の爪痕" },
  { en: "insignificant", ja: "取るに足りない、無意味な" },
  { en: "resound", ja: "鳴り響く、広く知れ渡る" },
  { en: "founder", ja: "創設者" },
  { en: "associate", ja: "結びつける、関連づける" },
  { en: "corrupting", ja: "堕落させる、腐敗させる" },
  { en: "luxury", ja: "贅沢、奢侈" },
  { en: "prince", ja: "君主、王侯" },
  { en: "poverty", ja: "貧困" },
  { en: "revenue", ja: "収入、歳入" },
  { en: "delegate", ja: "委任する、任せる" },
  { en: "artful", ja: "狡猾な、老獪な" },
  { en: "avaricious", ja: "強欲な" },
  { en: "concerns", ja: "(国家の)職務、関心事" },
  { en: "virtually", ja: "事実上、ほとんど" },
  { en: "plunderer", ja: "略奪者" },
  { en: "retain", ja: "保ち続ける、抱き続ける" },
  { en: "regard", ja: "尊敬、敬意" },
  { en: "distress", ja: "苦難、危急" },
  { en: "destitute", ja: "(~を)まったく欠いた、見捨てられた" },
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
  { en: "virtue", ja: "徳、美徳" },
  { en: "bear arms", ja: "武器を取る、武装する" },
  { en: "corruption", ja: "腐敗、堕落" },
  { en: "external", ja: "外部の、外来の" },
  { en: "commerce", ja: "商業、通商" },
  { en: "consumption", ja: "消費" },
  { en: "indulgence", ja: "耽溺、ふけること" },
  { en: "commodity", ja: "商品、物資" },
  { en: "enervate", ja: "衰弱させる、気力を奪う" },
  { en: "indolence", ja: "怠惰" },
  { en: "dissipation", ja: "放蕩、遊興" },
  { en: "consequent", ja: "結果として生じる" },
  { en: "public spiritedness", ja: "公共心" },
  { en: "body politic", ja: "国家、政治的共同体" },
  { en: "corrupt", ja: "堕落させる、腐敗させる" },
  { en: "over-importation", ja: "過剰な輸入" },
  { en: "empowerment", ja: "権力の付与、地位の向上" },
  { en: "merchant", ja: "商人" },
  { en: "interests", ja: "利益、利害" },
  { en: "propensity", ja: "傾向、性向" },
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
  { en: "facility", ja: "容易さ、手軽さ" },
  { en: "conveyance", ja: "輸送、運搬" },
  { en: "ruinous", ja: "破滅的な" },
  { en: "maxim", ja: "格言、原則" },
  { en: "prevail", ja: "広まる、はびこる" },
  { en: "estimate", ja: "評価する、判断する" },
  { en: "consist", ja: "(~に)ある、存する" },
  { en: "servitude", ja: "隷属、屈従" },
  { en: "prevailing", ja: "支配的な、大勢を占める" },
  { en: "attitude", ja: "態度、姿勢" },
  { en: "displace", ja: "置き換える、転移させる" },
  { en: "overt", ja: "あからさまな、公然の" },
  { en: "concern", ja: "関心、こだわり" },
  { en: "surrogate", ja: "代理として置き換える" },
  { en: "presence", ja: "存在、駐留" },
  { en: "lament", ja: "嘆き、哀歌" },
  { en: "complaint", ja: "不平、苦情" },
  { en: "over-run", ja: "蹂躙する、席巻する" },
  { en: "no matter if", ja: "たとえ~であろうと" },
  { en: "siege", ja: "包囲攻撃" },
  { en: "admonition", ja: "訓戒、警告" },
  { en: "dialectically", ja: "弁証法的に" },
  { en: "frequent", ja: "頻繁な、たびたびの" },
  { en: "account", ja: "記述、説明" },
  { en: "dialectic", ja: "弁証法、対立する二項の緊張" },
  { en: "split", ja: "分裂、隔たり" },
  { en: "aesthetically", ja: "美的に、美学的に" },
  { en: "expressed", ja: "表現された" },
  { en: "repository", ja: "貯蔵庫、宝庫" },
  { en: "region", ja: "地域、地方" },
  { en: "illustration", ja: "挿絵、図版" },
  { en: "visible", ja: "目に見える" },
  { en: "perhaps", ja: "おそらく" },
  { en: "arresting", ja: "人目を引く、はっとさせる" },
  { en: "elision", ja: "省略、融合" },
  { en: "mankind", ja: "人類" },
  { en: "mapping", ja: "地図化、地図作成" },
  { en: "exact", ja: "正確な、寸分たがわぬ" },
  { en: "correlate", ja: "対応物、相関するもの" },
  { en: "necessarily", ja: "必然的に" },
  { en: "register", ja: "(表現の)様式、位相" },
  { en: "ascribe", ja: "(~に)帰する、あるとみなす" },
  { en: "manner", ja: "方法、やり方" },
];

const KEY = "vocab-todai-reading-s10";
let words = [];        // {id, en, ja, starred}
let tab = "study";
let starOnly = false;
let order = [];
let pos = 0;
let flipped = false;

// ── 保存・読み込み(ブラウザに自動保存)──
function load() {
  let saved = null;
  try {
    const raw = localStorage.getItem(KEY);
    if (raw) saved = JSON.parse(raw);
  } catch (e) { /* プライベートモード等で使えない場合はそのまま */ }

  if (saved && Array.isArray(saved.words) && saved.words.length > 0) {
    words = saved.words;
    // デフォルトに新しい単語が増えていたらマージ
    const known = new Set(words.map(w => w.en.toLowerCase()));
    DEFAULT_WORDS.forEach((d, i) => {
      if (!known.has(d.en.toLowerCase())) {
        words.push({ id: "d" + Date.now() + "_" + i, en: d.en, ja: d.ja, starred: false });
      }
    });
  } else {
    words = DEFAULT_WORDS.map((d, i) => ({ id: "w" + i, en: d.en, ja: d.ja, starred: false }));
  }
}
let saveTimer;
function save() {
  clearTimeout(saveTimer);
  saveTimer = setTimeout(() => {
    try { localStorage.setItem(KEY, JSON.stringify({ words })); } catch (e) {}
  }, 300);
}

// ── 学習順序 ──
function pool() { return starOnly ? words.filter(w => w.starred) : words; }
function rebuildOrder() {
  const ids = pool().map(w => w.id);
  const kept = order.filter(id => ids.includes(id));
  const added = ids.filter(id => !kept.includes(id));
  order = kept.concat(added);
  if (pos >= order.length) pos = 0;
}
function shuffleOrder() {
  for (let i = order.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [order[i], order[j]] = [order[j], order[i]];
  }
  pos = 0; flipped = false;
}

// ── 描画 ──
const $ = id => document.getElementById(id);
function esc(s) {
  return s.replace(/[&<>"']/g, c => ({ "&":"&amp;", "<":"&lt;", ">":"&gt;", '"':"&quot;", "'":"&#39;" }[c]));
}

function render() {
  $("count").textContent = "全 " + words.length + " 語 ・ ★ " + words.filter(w => w.starred).length + " 語";
  $("tab-study").classList.toggle("active", tab === "study");
  $("tab-list").classList.toggle("active", tab === "list");
  $("view-study").hidden = tab !== "study";
  $("view-list").hidden = tab !== "list";
  $("mode-all").classList.toggle("active", !starOnly);
  $("mode-star").classList.toggle("active", starOnly);
  if (tab === "study") renderStudy(); else renderList();
}

function renderStudy() {
  const area = $("study-area");
  rebuildOrder();
  if (order.length === 0) {
    area.innerHTML =
      '<div class="empty"><p>' +
      (starOnly ? "★の付いた単語がありません" : "単語がありません") +
      "</p><p>" +
      (starOnly
        ? "学習中にカード右上の ★ を押すと、ここに集まります。"
        : "「単語一覧」タブから単語を追加してください。") +
      "</p></div>";
    return;
  }
  const w = words.find(x => x.id === order[pos]);
  area.innerHTML =
    '<div class="card3d">' +
      '<div class="ring-metal"></div>' +
      '<div class="card-inner' + (flipped ? " flipped" : "") + '" id="card" role="button" tabindex="0" aria-label="カードをめくる">' +
        '<div class="card-face card-front">' +
          '<div class="ring-hole"></div>' +
          '<button class="star-btn' + (w.starred ? " on" : "") + '" data-star aria-label="★を切り替え">★</button>' +
          '<span class="face-label">ENGLISH</span>' +
          '<p class="word-en">' + esc(w.en) + "</p>" +
          '<span class="hint">タップでめくる</span>' +
        "</div>" +
        '<div class="card-face card-back">' +
          '<div class="ring-hole"></div>' +
          '<button class="star-btn' + (w.starred ? " on" : "") + '" data-star aria-label="★を切り替え">★</button>' +
          '<span class="face-label">日本語</span>' +
          '<p class="word-ja">' + esc(w.ja) + "</p>" +
          '<p class="word-en-small">' + esc(w.en) + "</p>" +
        "</div>" +
      "</div>" +
    "</div>" +
    '<div class="nav">' +
      '<button class="btn" id="prev-btn">← 前へ</button>' +
      '<span class="pos">' + (pos + 1) + " / " + order.length + "</span>" +
      '<button class="btn primary" id="next-btn">次へ →</button>' +
    "</div>" +
    '<p class="keys">スペース = めくる ・ ←→ = 移動 ・ S = ★</p>';

  const card = $("card");
  card.addEventListener("click", () => { flipped = !flipped; card.classList.toggle("flipped", flipped); });
  card.addEventListener("keydown", e => {
    if (e.key === "Enter") { flipped = !flipped; card.classList.toggle("flipped", flipped); }
  });
  area.querySelectorAll("[data-star]").forEach(btn =>
    btn.addEventListener("click", e => { e.stopPropagation(); toggleStar(w.id); })
  );
  $("prev-btn").addEventListener("click", prev);
  $("next-btn").addEventListener("click", next);
}

function renderList() {
  const ul = $("word-list");
  ul.innerHTML = words.map(w =>
    "<li>" +
      '<button class="li-star' + (w.starred ? " on" : "") + '" data-id="' + w.id + '" aria-label="★を切り替え">★</button>' +
      '<span class="li-en">' + esc(w.en) + "</span>" +
      '<span class="li-ja">' + esc(w.ja) + "</span>" +
      '<button class="li-del" data-del="' + w.id + '" aria-label="' + esc(w.en) + ' を削除">削除</button>' +
    "</li>"
  ).join("");
  ul.querySelectorAll(".li-star").forEach(b =>
    b.addEventListener("click", () => toggleStar(b.dataset.id))
  );
  ul.querySelectorAll(".li-del").forEach(b =>
    b.addEventListener("click", () => {
      words = words.filter(w => w.id !== b.dataset.del);
      order = order.filter(id => id !== b.dataset.del);
      save(); render();
    })
  );
}

// ── 操作 ──
function toggleStar(id) {
  const w = words.find(x => x.id === id);
  if (w) { w.starred = !w.starred; save(); render(); }
}
function next() {
  if (order.length === 0) return;
  flipped = false; pos = (pos + 1) % order.length; render();
}
function prev() {
  if (order.length === 0) return;
  flipped = false; pos = (pos - 1 + order.length) % order.length; render();
}

let toastTimer;
function toast(msg) {
  const t = $("toast");
  t.textContent = msg; t.classList.add("show");
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove("show"), 1800);
}

// ── 一括追加 ──
function importWords() {
  const lines = $("import-text").value.split("\n").map(l => l.trim()).filter(Boolean);
  let added = 0;
  for (const line of lines) {
    const m = line.match(/^([A-Za-z][A-Za-z' -]*?)\s*(?:[,、:：→\t]|\s{1,})\s*(.+)$/);
    if (m) {
      const en = m[1].trim(), ja = m[2].trim();
      if (en && ja && !words.some(w => w.en.toLowerCase() === en.toLowerCase())) {
        words.push({ id: "u" + Date.now() + "_" + added, en, ja, starred: false });
        added++;
      }
    }
  }
  if (added > 0) {
    $("import-text").value = "";
    $("import-btn").disabled = true;
    save(); render();
    toast(added + "語を追加しました");
  } else {
    toast("追加できる単語が見つかりませんでした");
  }
}

// ── イベント ──
$("tab-study").addEventListener("click", () => { tab = "study"; render(); });
$("tab-list").addEventListener("click", () => { tab = "list"; render(); });
$("mode-all").addEventListener("click", () => { starOnly = false; flipped = false; pos = 0; render(); });
$("mode-star").addEventListener("click", () => { starOnly = true; flipped = false; pos = 0; render(); });
$("shuffle-btn").addEventListener("click", () => { rebuildOrder(); shuffleOrder(); render(); toast("シャッフルしました"); });
$("import-text").addEventListener("input", e => { $("import-btn").disabled = !e.target.value.trim(); });
$("import-btn").addEventListener("click", importWords);

document.addEventListener("keydown", e => {
  if (tab !== "study") return;
  if (e.target.tagName === "TEXTAREA" || e.target.tagName === "INPUT") return;
  if (e.code === "Space") {
    e.preventDefault();
    flipped = !flipped;
    const c = $("card"); if (c) c.classList.toggle("flipped", flipped);
  } else if (e.key === "ArrowRight") next();
  else if (e.key === "ArrowLeft") prev();
  else if (e.key.toLowerCase() === "s" && order.length > 0) toggleStar(order[pos]);
});

load();
render();
</script>
</body>
</html>
