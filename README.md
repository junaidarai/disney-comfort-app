<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ディズニー快適度アプリ</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family:
        -apple-system, BlinkMacSystemFont, "Hiragino Kaku Gothic ProN",
        "Yu Gothic", "Meiryo", sans-serif;
      background: linear-gradient(180deg, #eef7ff 0%, #ffffff 45%, #f8fbff 100%);
      color: #222;
    }

    button,
    input,
    select {
      font: inherit;
    }

    .app {
      min-height: 100vh;
    }

    .header {
      background: rgba(255,255,255,.92);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid #e8edf4;
      position: sticky;
      top: 0;
      z-index: 20;
    }

    .header-inner {
      max-width: 1050px;
      margin: auto;
      padding: 15px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 15px;
    }

    .logo {
      font-size: 20px;
      font-weight: 800;
    }

    .badge {
      background: #eef4ff;
      color: #3366cc;
      padding: 7px 11px;
      border-radius: 999px;
      font-size: 12px;
      font-weight: 700;
    }

    .container {
      max-width: 1050px;
      margin: auto;
      padding: 35px 20px 60px;
    }

    .screen {
      display: none;
    }

    .screen.active {
      display: block;
      animation: fadeIn .35s ease;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: translateY(8px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .hero {
      text-align: center;
      padding: 45px 10px 30px;
    }

    .hero-icon {
      font-size: 55px;
      margin-bottom: 10px;
    }

    .hero h1 {
      font-size: clamp(28px, 5vw, 48px);
      margin: 0 0 15px;
      line-height: 1.25;
    }

    .hero p {
      color: #5d6878;
      font-size: 16px;
      line-height: 1.8;
      max-width: 680px;
      margin: auto;
    }

    .main-card {
      background: rgba(255,255,255,.96);
      border: 1px solid #e8edf4;
      border-radius: 24px;
      box-shadow: 0 14px 45px rgba(34, 68, 110, .09);
      padding: 28px;
    }

    .question-number {
      font-size: 13px;
      font-weight: 700;
      color: #5878b7;
      margin-bottom: 8px;
    }

    .progress {
      height: 8px;
      background: #e9eef5;
      border-radius: 999px;
      overflow: hidden;
      margin-bottom: 28px;
    }

    .progress-bar {
      height: 100%;
      background: linear-gradient(90deg, #5d88ff, #78c8ff);
      border-radius: 999px;
      transition: width .3s ease;
    }

    .question-title {
      font-size: clamp(24px, 4vw, 34px);
      margin: 0 0 10px;
    }

    .question-description {
      color: #6a7481;
      margin-bottom: 24px;
      line-height: 1.7;
    }

    .choices {
      display: grid;
      gap: 12px;
    }

    .choice {
      border: 2px solid #e3e8f0;
      background: white;
      border-radius: 16px;
      padding: 17px 18px;
      cursor: pointer;
      transition: .18s ease;
      text-align: left;
    }

    .choice:hover {
      border-color: #83a8ff;
      transform: translateY(-1px);
    }

    .choice.selected {
      border-color: #4f7ff7;
      background: #f3f7ff;
    }

    .choice-title {
      font-weight: 800;
      margin-bottom: 4px;
    }

    .choice-description {
      color: #707b88;
      font-size: 13px;
    }

    .input-group {
      margin-bottom: 18px;
    }

    .input-group label {
      display: block;
      font-weight: 700;
      margin-bottom: 8px;
    }

    input[type="text"],
    input[type="number"],
    select {
      width: 100%;
      border: 2px solid #e2e7ef;
      border-radius: 14px;
      padding: 14px;
      outline: none;
      background: white;
    }

    input:focus,
    select:focus {
      border-color: #6088f7;
    }

    .date-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
    }

    .date-item {
      position: relative;
    }

    .date-item input {
      display: none;
    }

    .date-label {
      display: block;
      padding: 16px 10px;
      border: 2px solid #e1e7ef;
      border-radius: 15px;
      text-align: center;
      cursor: pointer;
      background: #fff;
      transition: .18s;
    }

    .date-item input:checked + .date-label {
      border-color: #527ef1;
      background: #f1f6ff;
      color: #315fca;
      font-weight: 800;
    }

    .range-row {
      display: flex;
      align-items: center;
      gap: 15px;
    }

    input[type="range"] {
      width: 100%;
      accent-color: #557ff2;
    }

    .range-value {
      min-width: 60px;
      text-align: center;
      font-weight: 800;
      background: #f2f5f9;
      border-radius: 12px;
      padding: 10px;
    }

    .button-row {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      margin-top: 30px;
    }

    .btn {
      border: none;
      border-radius: 14px;
      padding: 14px 20px;
      font-weight: 800;
      cursor: pointer;
      transition: .18s ease;
    }

    .btn-primary {
      background: #4d7cf5;
      color: white;
      box-shadow: 0 8px 22px rgba(77,124,245,.22);
    }

    .btn-primary:hover {
      transform: translateY(-1px);
    }

    .btn-secondary {
      background: #edf1f6;
      color: #33404e;
    }

    .btn-danger {
      background: #fff0f0;
      color: #bd4747;
    }

    .center {
      text-align: center;
    }

    .loading {
      padding: 50px 20px;
      text-align: center;
    }

    .spinner {
      width: 55px;
      height: 55px;
      border-radius: 50%;
      border: 5px solid #e8eef7;
      border-top-color: #5b83f6;
      animation: spin 1s linear infinite;
      margin: 0 auto 20px;
    }

    @keyframes spin {
      to {
        transform: rotate(360deg);
      }
    }

    .result-header {
      text-align: center;
      margin-bottom: 28px;
    }

    .result-header h2 {
      font-size: clamp(28px, 5vw, 42px);
      margin: 10px 0;
    }

    .score-circle {
      width: 130px;
      height: 130px;
      border-radius: 50%;
      border: 10px solid #72a0ff;
      display: grid;
      place-items: center;
      margin: 20px auto;
      background: white;
    }

    .score-number {
      font-size: 42px;
      font-weight: 900;
      line-height: 1;
    }

    .score-label {
      font-size: 12px;
      color: #6d7885;
    }

    .recommend-card {
      background: linear-gradient(135deg, #f3f7ff, #ffffff);
      border: 1px solid #dfe8fb;
      border-radius: 20px;
      padding: 22px;
      margin-bottom: 20px;
    }

    .recommend-main {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 12px;
      margin-top: 18px;
    }

    .info-box {
      background: white;
      border: 1px solid #e6ebf2;
      border-radius: 15px;
      padding: 15px;
    }

    .info-label {
      font-size: 12px;
      color: #77818d;
      margin-bottom: 5px;
    }

    .info-value {
      font-size: 17px;
      font-weight: 800;
    }

    .section-card {
      background: white;
      border: 1px solid #e7ebf0;
      border-radius: 20px;
      padding: 22px;
      margin-bottom: 18px;
    }

    .section-title {
      margin: 0 0 17px;
      font-size: 21px;
    }

    .score-row {
      margin: 13px 0;
    }

    .score-row-top {
      display: flex;
      justify-content: space-between;
      margin-bottom: 7px;
      font-size: 14px;
    }

    .score-track {
      height: 10px;
      background: #eef1f5;
      border-radius: 999px;
      overflow: hidden;
    }

    .score-fill {
      height: 100%;
      background: linear-gradient(90deg, #648cf6, #82c7ff);
    }

    .ranking-item {
      display: grid;
      grid-template-columns: 45px 1fr auto;
      align-items: center;
      gap: 12px;
      padding: 15px 0;
      border-bottom: 1px solid #edf0f4;
    }

    .ranking-item:last-child {
      border-bottom: none;
    }

    .rank {
      font-size: 20px;
      font-weight: 900;
      text-align: center;
    }

    .rank-date {
      font-weight: 800;
    }

    .rank-detail {
      color: #727d8a;
      font-size: 13px;
      margin-top: 4px;
    }

    .rank-score {
      font-size: 20px;
      font-weight: 900;
    }

    .status-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
    }

    .status {
      padding: 13px;
      border-radius: 13px;
      background: #f7f9fb;
      border: 1px solid #e8edf1;
    }

    .status-name {
      font-size: 13px;
      font-weight: 700;
    }

    .status-value {
      font-size: 12px;
      margin-top: 4px;
      color: #61707f;
    }

    .note {
      font-size: 12px;
      color: #788390;
      line-height: 1.7;
    }

    @media (max-width: 700px) {
      .container {
        padding: 20px 12px 40px;
      }

      .main-card {
        padding: 20px;
      }

      .recommend-main,
      .status-grid {
        grid-template-columns: 1fr;
      }

      .date-grid {
        grid-template-columns: repeat(2, 1fr);
      }

      .button-row {
        flex-direction: column-reverse;
      }

      .btn {
        width: 100%;
      }
    }
  </style>
</head>

<body>
<div class="app">

  <header class="header">
    <div class="header-inner">
      <div class="logo">🏰 ディズニー快適度アプリ</div>
      <div class="badge">個人最適化</div>
    </div>
  </header>

  <main class="container">

    <!-- START -->
    <section id="startScreen" class="screen active">
      <div class="hero">
        <div class="hero-icon">🏰</div>
        <h1>あなたにぴったりの<br>ディズニー旅行を見つけよう</h1>
        <p>
          混雑だけではなく、天気・待ち時間・予算・交通・ホテル・
          好きなアトラクションなどを総合的に考えて、
          あなたに合った旅行プランを提案します。
        </p>
      </div>

      <div class="main-card center">
        <button class="btn btn-primary" onclick="startApp()">
          旅行を診断する
        </button>
        <p class="note" style="margin-top:15px;">
          ※現在は動作確認用のデータを含むプロトタイプです。
        </p>
      </div>
    </section>


    <!-- QUESTION -->
    <section id="questionScreen" class="screen">
      <div class="main-card">

        <div class="question-number" id="questionNumber"></div>

        <div class="progress">
          <div id="progressBar" class="progress-bar"></div>
        </div>

        <h2 id="questionTitle" class="question-title"></h2>
        <div id="questionDescription" class="question-description"></div>

        <div id="questionContent"></div>

        <div class="button-row">
          <button class="btn btn-secondary" onclick="previousQuestion()">
            ← 戻る
          </button>
          <button id="nextButton" class="btn btn-primary" onclick="nextQuestion()">
            次へ →
          </button>
        </div>

      </div>
    </section>


    <!-- LOADING -->
    <section id="loadingScreen" class="screen">
      <div class="main-card loading">
        <div class="spinner"></div>
        <h2>あなたに合った旅行を分析中…</h2>
        <p id="loadingText">
          入力された条件から最適な旅行を計算しています。
        </p>
      </div>
    </section>


    <!-- RESULT -->
    <section id="resultScreen" class="screen">

      <div class="result-header">
        <div class="badge">あなたへのおすすめ</div>
        <h2 id="resultTitle"></h2>

        <div class="score-circle">
          <div>
            <div id="totalScore" class="score-number"></div>
            <div class="score-label">総合スコア</div>
          </div>
        </div>
      </div>

      <div class="recommend-card">
        <h3 style="margin:0;">✨ 最適な旅行</h3>

        <div class="recommend-main">

          <div class="info-box">
            <div class="info-label">おすすめ日</div>
            <div id="resultDate" class="info-value"></div>
          </div>

          <div class="info-box">
            <div class="info-label">おすすめパーク</div>
            <div id="resultPark" class="info-value"></div>
          </div>

          <div class="info-box">
            <div class="info-label">予想総額</div>
            <div id="resultBudget" class="info-value"></div>
          </div>

        </div>
      </div>


      <div class="section-card">
        <h3 class="section-title">💡 なぜこの日がおすすめ？</h3>
        <div id="reasonText" style="line-height:1.9;"></div>
      </div>


      <div class="section-card">
        <h3 class="section-title">📊 スコアの内訳</h3>
        <div id="scoreBreakdown"></div>
      </div>


      <div class="section-card">
        <h3 class="section-title">🏆 候補日のランキング</h3>
        <div id="ranking"></div>
      </div>


      <div class="section-card">
        <h3 class="section-title">🌤️ 天気・混雑</h3>

        <div class="recommend-main">

          <div class="info-box">
            <div class="info-label">天気</div>
            <div id="weatherResult" class="info-value"></div>
          </div>

          <div class="info-box">
            <div class="info-label">最高気温</div>
            <div id="temperatureResult" class="info-value"></div>
          </div>

          <div class="info-box">
            <div class="info-label">混雑予測</div>
            <div id="crowdResult" class="info-value"></div>
          </div>

        </div>

        <p class="note" style="margin-top:15px;">
          ※混雑は現在のプロトタイプでは参考データです。
          実際のデータソースへ接続した場合は取得状況を表示します。
        </p>
      </div>


      <div class="section-card">
        <h3 class="section-title">🚃 交通</h3>

        <div class="recommend-main">

          <div class="info-box">
            <div class="info-label">おすすめ移動</div>
            <div id="transportResult" class="info-value"></div>
          </div>

          <div class="info-box">
            <div class="info-label">所要時間</div>
            <div class="info-value">約3時間</div>
          </div>

          <div class="info-box">
            <div class="info-label">交通費</div>
            <div class="info-value">約15,000円</div>
          </div>

        </div>

        <p class="note" style="margin-top:15px;">
          ※交通情報は現在プロトタイプ用の参考値です。
        </p>
      </div>


      <div class="section-card">
        <h3 class="section-title">🏨 ホテル</h3>

        <div class="info-box">
          <div class="info-label">おすすめホテル</div>
          <div class="info-value">パーク周辺ホテル</div>
          <p class="note">
            実際のホテルAPI接続時には、価格・空室・評価・アクセスを
            ユーザー条件に合わせて比較します。
          </p>
        </div>
      </div>


      <div class="section-card">
        <h3 class="section-title">🗓️ 当日のおすすめプラン</h3>

        <div style="line-height:2;">
          <strong>08:00</strong> パーク周辺到着<br>
          <strong>08:30</strong> 入園<br>
          <strong>09:00</strong> 人気アトラクション<br>
          <strong>11:00</strong> お気に入りアトラクション<br>
          <strong>12:30</strong> 昼食<br>
          <strong>14:00</strong> ショー・パレード<br>
          <strong>16:00</strong> アトラクション<br>
          <strong>18:00</strong> 夕食<br>
          <strong>20:00</strong> 帰宅開始
        </div>

        <p class="note">
          ※実際の営業時間、運営状況、ショー時間等を取得できた場合は、
          そのデータを優先してプランを作成します。
        </p>
      </div>


      <div class="section-card">
        <h3 class="section-title">🔌 データ接続状況</h3>

        <div class="status-grid">

          <div class="status">
            <div class="status-name">🌤️ 天気</div>
            <div class="status-value">プロトタイプ</div>
          </div>

          <div class="status">
            <div class="status-name">🚃 交通</div>
            <div class="status-value">プロトタイプ</div>
          </div>

          <div class="status">
            <div class="status-name">🏨 ホテル</div>
            <div class="status-value">API接続準備中</div>
          </div>

          <div class="status">
            <div class="status-name">🎢 混雑</div>
            <div class="status-value">プロトタイプ</div>
          </div>

        </div>

        <p class="note" style="margin-top:15px;">
          実際に接続できていないデータを「接続済み」と表示しない設計にしています。
        </p>
      </div>


      <div class="center">
        <button class="btn btn-primary" onclick="restartApp()">
          条件を変更して再診断
        </button>

        <button class="btn btn-danger" onclick="resetApp()" style="margin-left:8px;">
          最初から
        </button>
      </div>

    </section>

  </main>
</div>


<script>

  const questions = [
    {
      key: "park",
      title: "どのパークに行きたい？",
      description: "複数日旅行ならランドとシーの両方も選べます。",
      type: "choices",
      options: [
        {
          value: "land",
          title: "🏰 東京ディズニーランド",
          description: "ランドを中心に旅行を計画"
        },
        {
          value: "sea",
          title: "🌊 東京ディズニーシー",
          description: "シーを中心に旅行を計画"
        },
        {
          value: "both",
          title: "🏰🌊 両方",
          description: "ランドとシーの両方を候補にする"
        }
      ]
    },
    {
      key: "station",
      title: "出発駅を教えてください",
      description: "自宅住所ではなく、最寄り駅などを入力してください。",
      type: "text",
      placeholder: "例：高の原駅"
    },
    {
      key: "dates",
      title: "行ける日を選んでください",
      description: "複数の日を選択すると、その中からおすすめ日をランキングします。",
      type: "dates"
    },
    {
      key: "crowd",
      title: "混雑はどれくらい気になりますか？",
      description: "あなたの好みに合わせて混雑の重みを変えます。",
      type: "choices",
      options: [
        {
          value: "high",
          title: "😣 かなり気になる",
          description: "できるだけ空いている日がいい"
        },
        {
          value: "medium",
          title: "🙂 普通",
          description: "ある程度混んでいても大丈夫"
        },
        {
          value: "low",
          title: "😎 あまり気にしない",
          description: "混雑はあまり重視しない"
        }
      ]
    },
    {
      key: "wait",
      title: "待ち時間は何分までなら大丈夫？",
      description: "人気アトラクションの待ち時間評価に使います。",
      type: "range",
      min: 30,
      max: 120,
      step: 15,
      unit: "分"
    },
    {
      key: "weather",
      title: "天気はどれくらい気になりますか？",
      description: "雨や暑さをどれくらい重視するか設定します。",
      type: "choices",
      options: [
        {
          value: "high",
          title: "☀️ かなり気になる",
          description: "できるだけ快適な天気の日がいい"
        },
        {
          value: "medium",
          title: "⛅ 少し気になる",
          description: "多少の雨や暑さなら大丈夫"
        },
        {
          value: "low",
          title: "🌧️ あまり気にしない",
          description: "天気はそれほど重要ではない"
        }
      ]
    },
    {
      key: "budget",
      title: "旅行全体の予算は？",
      description: "交通・ホテル・食事などを含めたおおよその予算です。",
      type: "number",
      placeholder: "例：50000"
    },
    {
      key: "transport",
      title: "移動では何を重視しますか？",
      description: "最適な交通ルートを選ぶときの重みになります。",
      type: "choices",
      options: [
        {
          value: "time",
          title: "⚡ 移動時間を短くしたい",
          description: "多少高くても早い方がいい"
        },
        {
          value: "cheap",
          title: "💰 安さを重視",
          description: "多少時間がかかっても安い方がいい"
        },
        {
          value: "easy",
          title: "🚃 乗り換えを減らしたい",
          description: "できるだけ楽に移動したい"
        }
      ]
    },
    {
      key: "hotel",
      title: "宿泊はどうしますか？",
      description: "旅行の日数や予算にも反映されます。",
      type: "choices",
      options: [
        {
          value: "day",
          title: "🏠 日帰り",
          description: "ホテルには泊まらない"
        },
        {
          value: "one",
          title: "🏨 1泊",
          description: "パーク周辺などに1泊"
        },
        {
          value: "two",
          title: "🏨🏨 2泊以上",
          description: "ゆとりを持った旅行"
        }
      ]
    },
    {
      key: "favorite",
      title: "好きなアトラクションを選んでください",
      description: "パークとの相性を評価するために使います。",
      type: "favorite"
    }
  ];

  let currentQuestion = 0;

  let answers = {
    park: null,
    station: "",
    dates: [],
    crowd: null,
    wait: 60,
    weather: null,
    budget: 50000,
    transport: null,
    hotel: null,
    favorite: []
  };


  function startApp() {
    showScreen("questionScreen");
    currentQuestion = 0;
    renderQuestion();
  }


  function showScreen(id) {
    document.querySelectorAll(".screen").forEach(el => {
      el.classList.remove("active");
    });

    document.getElementById(id).classList.add("active");
    window.scrollTo({top:0, behavior:"smooth"});
  }


  function renderQuestion() {

    const q = questions[currentQuestion];

    document.getElementById("questionNumber").textContent =
      `${currentQuestion + 1} / ${questions.length}`;

    document.getElementById("progressBar").style.width =
      `${((currentQuestion + 1) / questions.length) * 100}%`;

    document.getElementById("questionTitle").textContent = q.title;
    document.getElementById("questionDescription").textContent = q.description;

    const content = document.getElementById("questionContent");

    if (q.type === "choices") {
      renderChoices(q, content);
    }

    if (q.type === "text") {
      content.innerHTML = `
        <div class="input-group">
          <label>出発駅</label>
          <input
            id="textInput"
            type="text"
            placeholder="${q.placeholder}"
            value="${answers[q.key] || ""}"
          >
        </div>
      `;
    }

    if (q.type === "number") {
      content.innerHTML = `
        <div class="input-group">
          <label>予算（円）</label>
          <input
            id="numberInput"
            type="number"
            min="0"
            step="1000"
            placeholder="${q.placeholder}"
            value="${answers[q.key] || ""}"
          >
        </div>
      `;
    }

    if (q.type === "range") {
      content.innerHTML = `
        <div class="range-row">
          <input
            id="rangeInput"
            type="range"
            min="${q.min}"
            max="${q.max}"
            step="${q.step}"
            value="${answers[q.key]}"
            oninput="updateRangeValue(this.value)"
          >
          <div class="range-value">
            <span id="rangeValue">${answers[q.key]}</span>${q.unit}
          </div>
        </div>
      `;
    }

    if (q.type === "dates") {
      renderDates(content);
    }

    if (q.type === "favorite") {
      renderFavorites(content);
    }

    document.getElementById("nextButton").textContent =
      currentQuestion === questions.length - 1
        ? "診断する ✨"
        : "次へ →";
  }


  function renderChoices(q, content) {

    content.innerHTML = `
      <div class="choices">
        ${q.options.map(option => `
          <div
            class="choice ${
              answers[q.key] === option.value ? "selected" : ""
            }"
            onclick="selectChoice('${q.key}', '${option.value}')"
          >
            <div class="choice-title">${option.title}</div>
            <div class="choice-description">${option.description}</div>
          </div>
        `).join("")}
      </div>
    `;
  }


  function selectChoice(key, value) {
    answers[key] = value;
    renderQuestion();
  }


  function renderDates(content) {

    const dateList = generateDates();

    content.innerHTML = `
      <div class="date-grid">
        ${dateList.map(date => {
          const checked = answers.dates.includes(date);
          return `
            <div class="date-item">
              <input
                type="checkbox"
                id="date-${date}"
                ${checked ? "checked" : ""}
                onchange="toggleDate('${date}', this.checked)"
              >
              <label for="date-${date}" class="date-label">
                ${formatDate(date)}
              </label>
            </div>
          `;
        }).join("")}
      </div>
    `;
  }


  function generateDates() {

    const dates = [];
    const today = new Date();

    for (let i = 1; i <= 12; i++) {
      const d = new Date(today);
      d.setDate(today.getDate() + i);

      const yyyy = d.getFullYear();
      const mm = String(d.getMonth() + 1).padStart(2, "0");
      const dd = String(d.getDate()).padStart(2, "0");

      dates.push(`${yyyy}-${mm}-${dd}`);
    }

    return dates;
  }


  function formatDate(dateString) {
    const d = new Date(dateString + "T00:00:00");
    return `${d.getMonth()+1}/${d.getDate()} (${["日","月","火","水","木","金","土"][d.getDay()]})`;
  }


  function toggleDate(date, checked) {

    if (checked) {
      if (!answers.dates.includes(date)) {
        answers.dates.push(date);
      }
    } else {
      answers.dates =
        answers.dates.filter(d => d !== date);
    }
  }


  function renderFavorites(content) {

    const favorites = [
      {
        value:"beauty",
        title:"美女と野獣",
        park:"land"
      },
      {
        value:"soarin",
        title:"ソアリン",
        park:"sea"
      },
      {
        value:"frozen",
        title:"アナとエルサのフローズンジャーニー",
        park:"sea"
      },
      {
        value:"toy",
        title:"トイ・ストーリー・マニア！",
        park:"sea"
      },
      {
        value:"splash",
        title:"スプラッシュ・マウンテン",
        park:"land"
      },
      {
        value:"bigThunder",
        title:"ビッグサンダー・マウンテン",
        park:"land"
      }
    ];

    content.innerHTML = `
      <div class="choices">
        ${favorites.map(f => `
          <div
            class="choice ${
              answers.favorite.includes(f.value) ? "selected" : ""
            }"
            onclick="toggleFavorite('${f.value}')"
          >
            <div class="choice-title">${f.title}</div>
            <div class="choice-description">
              ${f.park === "land" ? "ランド" : "シー"}
            </div>
          </div>
        `).join("")}
      </div>
    `;
  }


  function toggleFavorite(value) {

    if (answers.favorite.includes(value)) {
      answers.favorite =
        answers.favorite.filter(v => v !== value);
    } else {
      answers.favorite.push(value);
    }

    renderQuestion();
  }


  function updateRangeValue(value) {
    document.getElementById("rangeValue").textContent = value;
  }


  function validateCurrentQuestion() {

    const q = questions[currentQuestion];

    if (q.type === "choices" && !answers[q.key]) {
      alert("選択してください。");
      return false;
    }

    if (q.type === "text") {
      const value = document.getElementById("textInput").value.trim();

      if (!value) {
        alert("出発駅を入力してください。");
        return false;
      }

      answers.station = value;
    }

    if (q.type === "number") {
      const value = Number(
        document.getElementById("numberInput").value
      );

      if (!value || value <= 0) {
        alert("予算を入力してください。");
        return false;
      }

      answers.budget = value;
    }

    if (q.type === "range") {
      answers.wait =
        Number(document.getElementById("rangeInput").value);
    }

    if (q.type === "dates" && answers.dates.length === 0) {
      alert("候補日を1日以上選択してください。");
      return false;
    }

    return true;
  }


  function nextQuestion() {

    if (!validateCurrentQuestion()) {
      return;
    }

    if (currentQuestion === questions.length - 1) {
      runDiagnosis();
      return;
    }

    currentQuestion++;
    renderQuestion();
  }


  function previousQuestion() {

    if (currentQuestion === 0) {
      showScreen("startScreen");
      return;
    }

    currentQuestion--;
    renderQuestion();
  }


  function runDiagnosis() {

    showScreen("loadingScreen");

    const loadingTexts = [
      "候補日を比較しています…",
      "混雑への耐性を分析しています…",
      "天気の影響を評価しています…",
      "予算と交通条件を確認しています…",
      "好きなアトラクションとの相性を計算しています…"
    ];

    let i = 0;

    const interval = setInterval(() => {
      document.getElementById("loadingText").textContent =
        loadingTexts[i % loadingTexts.length];
      i++;
    }, 550);

    setTimeout(() => {
      clearInterval(interval);

      const results = calculateResults();

      renderResults(results);

      showScreen("resultScreen");

    }, 2800);
  }


  function calculateResults() {

    const results = answers.dates.map((date, index) => {

      const weekday = new Date(date + "T00:00:00").getDay();

      let score = 70;

      // 曜日による参考スコア
      if (weekday === 2 || weekday === 3 || weekday === 4) {
        score += 10;
      }

      if (weekday === 0 || weekday === 6) {
        score -= 5;
      }

      // 混雑への重み
      if (answers.crowd === "high") {
        score += weekday === 0 || weekday === 6 ? -8 : 10;
      }

      if (answers.crowd === "low") {
        score += 4;
      }

      // 天気への重み
      if (answers.weather === "high") {
        score += weekday === 2 || weekday === 3 ? 8 : 3;
      }

      if (answers.weather === "low") {
        score += 4;
      }

      // 待ち時間
      if (answers.wait <= 45) {
        score -= 4;
      } else if (answers.wait >= 90) {
        score += 5;
      }

      // 予算
      if (answers.budget < 30000) {
        score -= 7;
      } else if (answers.budget >= 60000) {
        score += 5;
      }

      // パーク
      const park = selectParkForDate(index);

      // 好きなアトラクション
      let favoriteMatch = 0;

      answers.favorite.forEach(f => {

        if (
          (f === "beauty" || f === "splash" || f === "bigThunder")
          && park === "land"
        ) {
          favoriteMatch += 5;
        }

        if (
          (f === "soarin" || f === "frozen" || f === "toy")
          && park === "sea"
        ) {
          favoriteMatch += 5;
        }

      });

      score += favoriteMatch;

      score = Math.max(0, Math.min(100, score));

      return {
        date,
        score,
        park
      };
    });

    results.sort((a,b) => b.score - a.score);

    return results;
  }


  function selectParkForDate(index) {

    if (answers.park === "land") return "land";
    if (answers.park === "sea") return "sea";

    if (answers.favorite.length > 0) {

      const landFavorites =
        ["beauty","splash","bigThunder"];

      const seaFavorites =
        ["soarin","frozen","toy"];

      const landCount =
        answers.favorite.filter(v => landFavorites.includes(v)).length;

      const seaCount =
        answers.favorite.filter(v => seaFavorites.includes(v)).length;

      return seaCount > landCount ? "sea" : "land";
    }

    return index % 2 === 0 ? "land" : "sea";
  }


  function renderResults(results) {

    const best = results[0];

    document.getElementById("resultTitle").textContent =
      `${formatDate(best.date)}がおすすめです！`;

    document.getElementById("totalScore").textContent =
      best.score;

    document.getElementById("resultDate").textContent =
      formatDate(best.date);

    document.getElementById("resultPark").textContent =
      best.park === "land"
        ? "東京ディズニーランド"
        : "東京ディズニーシー";

    document.getElementById("resultBudget").textContent =
      `約${Math.round(answers.budget * 0.72).toLocaleString()}円`;

    document.getElementById("reasonText").innerHTML = `
      あなたは
      <strong>${crowdText()}</strong>
      ため、混雑条件をスコアに反映しました。<br>
      また、天気については
      <strong>${weatherText()}</strong>
      という設定で評価しています。<br>
      さらに、待ち時間は
      <strong>${answers.wait}分</strong>
      まで許容するとして計算しました。<br>
      好きなアトラクションとパークの相性も考慮し、
      候補日の中から総合的に最も適した日を選んでいます。
    `;

    renderBreakdown(best);
    renderRanking(results);

    document.getElementById("weatherResult").textContent =
      "晴れ〜くもり（参考値）";

    document.getElementById("temperatureResult").textContent =
      "約22℃";

    document.getElementById("crowdResult").textContent =
      best.score >= 88
        ? "比較的少なめ"
        : best.score >= 78
          ? "普通"
          : "やや多め";

    document.getElementById("transportResult").textContent =
      transportText();
  }


  function crowdText() {

    if (answers.crowd === "high") {
      return "混雑をかなり気にする";
    }

    if (answers.crowd === "low") {
      return "混雑をあまり気にしない";
    }

    return "混雑を普通程度に気にする";
  }


  function weatherText() {

    if (answers.weather === "high") {
      return "天気をかなり重視する";
    }

    if (answers.weather === "low") {
      return "天気をあまり重視しない";
    }

    return "天気をある程度重視する";
  }


  function transportText() {

    if (answers.transport === "time") {
      return "時間優先ルート";
    }

    if (answers.transport === "cheap") {
      return "料金優先ルート";
    }

    return "乗り換え最小ルート";
  }


  function renderBreakdown(best) {

    const breakdownContainer = document.getElementById("scoreBreakdown");

    const items = [
      { name: "混雑スコア", score: Math.min(100, best.score + 5) },
      { name: "天気スコア", score: Math.min(100, best.score - 2) },
      { name: "待ち時間適合度", score: Math.min(100, best.score + 3) },
      { name: "アトラクション相性", score: Math.min(100, best.score + 8) }
    ];

    breakdownContainer.innerHTML = items.map(item => `
      <div class="score-row">
        <div class="score-row-top">
          <span>${item.name}</span>
          <strong>${item.score}点</strong>
        </div>
        <div class="score-track">
          <div class="score-fill" style="width: ${item.score}%;"></div>
        </div>
      </div>
    `).join("");
  }


  function renderRanking(results) {

    const rankingContainer = document.getElementById("ranking");

    rankingContainer.innerHTML = results.map((res, idx) => `
      <div class="ranking-item">
        <div class="rank">${idx + 1}</div>
        <div>
          <div class="rank-date">${formatDate(res.date)}</div>
          <div class="rank-detail">
            ${res.park === "land" ? "東京ディズニーランド" : "東京ディズニーシー"}
          </div>
        </div>
        <div class="rank-score">${res.score}点</div>
      </div>
    `).join("");
  }


  function restartApp() {
    startApp();
  }


  function resetApp() {

    answers = {
      park: null,
      station: "",
      dates: [],
      crowd: null,
      wait: 60,
      weather: null,
      budget: 50000,
      transport: null,
      hotel: null,
      favorite: []
    };

    showScreen("startScreen");
  }

</script>
</body>
</html>
