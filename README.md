<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>你是哪類型父母？</title>
  <style>
    body {
      font-family: 'Noto Sans TC', sans-serif;
      background: url('https://i.imgur.com/RwIoMJ2.jpeg') center/cover no-repeat fixed;
      margin: 0;
      padding: 0;
      background-color: #fff8f5;
    }
    .container {
      max-width: 600px;
      margin: 40px auto;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 1.5rem;
      padding: 2rem;
      box-shadow: 0 8px 24px rgba(0,0,0,0.15);
    }
    h1 {
      font-size: 1.8rem;
      color: #d63384;
      margin-bottom: 1rem;
      text-align: center;
    }
    .question {
      font-size: 1.2rem;
      margin-bottom: 1rem;
      text-align: center;
    }
    .options button {
      display: block;
      width: 100%;
      margin: 0.4rem 0;
      padding: 0.6rem 1rem;
      font-size: 1rem;
      border-radius: 999px;
      border: 1px solid #ccc;
      background: #fff;
      transition: all 0.3s;
      cursor: pointer;
    }
    .options button:hover {
      background: #ffe0ec;
      transform: scale(1.02);
    }
    .result {
      font-size: 1.2rem;
      color: #444;
      margin-top: 1rem;
      text-align: center;
    }
    .restart, .share {
      margin-top: 1.5rem;
      text-align: center;
    }
    .restart button, .share button {
      background: #f783ac;
      color: white;
      border: none;
      padding: 0.6rem 1.2rem;
      border-radius: 999px;
      cursor: pointer;
      margin: 0.2rem;
    }
    .restart button:hover, .share button:hover {
      background: #ec2b8f;
    }
    @media (max-width: 600px) {
      .container {
        margin: 20px;
        padding: 1.2rem;
      }
      h1 { font-size: 1.5rem; }
      .question { font-size: 1rem; }
      .options button { font-size: 0.95rem; padding: 0.5rem; }
    }
  </style>
</head>
<body>
  <div class="container" id="quiz">
    <h1>💐 你是哪類型父母？</h1>
    <div class="question" id="questionText"></div>
    <div class="options" id="options"></div>
    <div class="result" id="result"></div>
    <div class="restart" id="restart"></div>
    <div class="share" id="share"></div>
  </div>

  <script>
    const allQuestions = [
      {
        question: "當遇到特價商品打的折扣很划算時，你會怎麼做？",
        options: [
          { text: "先看看是否是你要的再決定要不要買", type: "精打細算型" },
          { text: "有特價就是先搶一波", type: "活潑型" },
          { text: "看當時心情決定隨緣吧", type: "自由放養型" },
          { text: "雖然不是自己想要的，但家人有需求會考慮買", type: "操勞型" }
        ]
      },
      {
        question: "孩子問了你一個很難的問題，你會怎麼辦？",
        options: [
          { text: "上網查資料給他或者是親自解釋", type: "知識百科型" },
          { text: "一邊解釋一邊詢問孩子的狀況了解孩子的困難", type: "溫柔體貼型" },
          { text: "搞笑回答轉移注意力讓孩子忘記", type: "活潑型" },
          { text: "說『先吃飯再說』我不知道，你找其他人或許知道", type: "自由放養型" }
        ]
      },
      {
        question: "假日終於有空閒時，你最想做什麼？",
        options: [
          { text: "整理家務讓空間舒適", type: "操勞型" },
          { text: "規劃全家一起出遊", type: "活潑型" },
          { text: "獨自閱讀或聽音樂", type: "自由放養型" },
          { text: "買些家人愛吃的東西準備驚喜", type: "溫柔體貼型" },
          { text: "趕緊關心孩子的作業！", type: "領袖型" }
        ]
      }
    ];

    let currentQuestionIndex = 0;
    let userAnswers = [];

    function showQuestion() {
      const question = allQuestions[currentQuestionIndex];
      document.getElementById('questionText').textContent = question.question;
      const optionsDiv = document.getElementById('options');
      optionsDiv.innerHTML = '';
      question.options.forEach(option => {
        const button = document.createElement('button');
        button.textContent = option.text;
        button.onclick = () => {
          userAnswers.push(option.type);
          currentQuestionIndex++;
          if (currentQuestionIndex < allQuestions.length) {
            showQuestion();
          } else {
            showResult();
          }
        };
        optionsDiv.appendChild(button);
      });
    }

    function showResult() {
      const resultCount = {
        "精打細算型": 0,
        "活潑型": 0,
        "自由放養型": 0,
        "操勞型": 0,
        "知識百科型": 0,
        "溫柔體貼型": 0,
        "領袖型": 0,
        "無敵超人型": 0
      };
      
      userAnswers.forEach(answer => {
        resultCount[answer]++;
      });

      const maxResult = Object.keys(resultCount).reduce((a, b) => resultCount[a] > resultCount[b] ? a : b);
      document.getElementById('result').textContent = `你的父母類型是：${maxResult}`;

      const resultDesc = {
        "精打細算型": "你是節儉聰明的家長，善於理財，讓家庭生活井井有條。",
        "活潑型": "你是充滿活力的家長，總能帶給家人歡樂與驚喜。",
        "自由放養型": "你尊重孩子的選擇，給予他們成長的自由與空間。",
        "操勞型": "你總是默默付出，是家中的無名英雄。",
        "知識百科型": "你是智慧型家長，總能以知識引導孩子探索世界。",
        "溫柔體貼型": "你以愛心和耐心陪伴孩子，是他們的避風港。",
        "領袖型": "你掌握家庭的節奏，善於指引方向。",
        "無敵超人型": "你是萬能的超級家長，什麼事情都難不倒你。"
      };
      
      document.getElementById('result').textContent += ` ${resultDesc[maxResult]}`;

      // 添加再玩一次和分享按钮
      document.getElementById('restart').innerHTML = '<button onclick="restartQuiz()">再玩一次</button>';
      document.getElementById('share').innerHTML = '<button onclick="shareToFB()">分享至 Facebook</button>';
    }

    function restartQuiz() {
      currentQuestionIndex = 0;
      userAnswers = [];
      document.getElementById('result').textContent = '';
      document.getElementById('restart').innerHTML = '';
      document.getElementById('share').innerHTML = '';
      showQuestion();
    }

    function shareToFB() {
      const resultText = document.getElementById('result').textContent;
      const fbShareUrl = `https://www.facebook.com/shengchisteak/?message=${encodeURIComponent(resultText)}`;
      window.open(fbShareUrl, '_blank');
    }

    showQuestion();
  </script>
</body>
</html>
