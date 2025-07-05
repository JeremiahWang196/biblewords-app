<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>BibleWords - Learn English with Scripture</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(to right, #f6d365, #fda085);
      color: #333;
      margin: 0;
      padding: 2rem;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
    }
    .card {
      background: #fff;
      padding: 2rem;
      border-radius: 20px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.15);
      max-width: 500px;
      width: 100%;
      text-align: center;
      transition: 0.3s;
    }
    .word {
      font-size: 2.5rem;
      font-weight: 600;
      margin-bottom: 1rem;
    }
    .definition {
      font-size: 1.1rem;
      margin-bottom: 0.5rem;
      color: #666;
    }
    .verse {
      font-style: italic;
      font-size: 1rem;
      color: #444;
    }
    .remember-status {
      margin-top: 0.6rem;
      font-size: 0.9rem;
      color: #999;
    }
    .controls {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 0.5rem;
      margin-top: 1.5rem;
    }
    button {
      padding: 0.6rem 1.2rem;
      font-size: 1rem;
      border: none;
      border-radius: 20px;
      background: linear-gradient(to right, #89f7fe, #66a6ff);
      color: white;
      cursor: pointer;
      transition: 0.2s ease;
    }
    button:hover {
      background: linear-gradient(to right, #66a6ff, #89f7fe);
      transform: translateY(-1px);
    }
    .search-bar {
      margin-bottom: 1rem;
      width: 100%;
      max-width: 500px;
    }
    input[type="text"] {
      width: 100%;
      padding: 0.8rem 1rem;
      border: 1px solid #ccc;
      border-radius: 20px;
      font-size: 1rem;
      font-family: 'Poppins', sans-serif;
    }
  </style>
</head>
<body>

  <div class="search-bar">
    <input type="text" id="search" placeholder="🔍 Search word..." />
  </div>

  <div class="card">
    <div class="word" id="word">Loading...</div>
    <div class="definition" id="definition"></div>
    <div class="verse" id="verse"></div>
    <div class="remember-status" id="remember-status"></div>
    <div class="controls">
      <button onclick="prevWord()">⬅️ Previous</button>
      <button onclick="toggleRemember()">✅ I remembered this</button>
      <button onclick="toggleFilter()">🔁 Review only unremembered</button>
      <button onclick="nextWord()">Next ➡️</button>
    </div>
  </div>

  <script>
    const words = [
      { word: "Grace", definition: "God's grace, undeserved favor", verse: "For it is by grace you have been saved... — Ephesians 2:8" },
      { word: "Faith", definition: "Trust in God", verse: "Now faith is confidence in what we hope for... — Hebrews 11:1" },
      { word: "Hope", definition: "Hope for the future", verse: "May the God of hope fill you with all joy... — Romans 15:13" },
      { word: "Love", definition: "God is love", verse: "God is love. Whoever lives in love lives in God. — 1 John 4:16" },
      { word: "Wisdom", definition: "Reverence for God is the beginning of wisdom", verse: "The fear of the Lord is the beginning of wisdom. — Proverbs 9:10" },
      { word: "Truth", definition: "Jesus is the truth", verse: "I am the way and the truth and the life. — John 14:6" },
      { word: "Peace", definition: "Inner peace from God", verse: "And the peace of God will guard your hearts. — Philippians 4:7" },
      { word: "Light", definition: "Light to the world", verse: "You are the light of the world. — Matthew 5:14" },
      { word: "Joy", definition: "Joy from the Lord", verse: "The joy of the Lord is your strength. — Nehemiah 8:10" },
      { word: "Forgiveness", definition: "God's forgiveness", verse: "Forgive as the Lord forgave you. — Colossians 3:13" }
    ];

    let index = 0;
    let remembered = JSON.parse(localStorage.getItem("rememberedWords") || "{}");
    let filterMode = false;

    function render() {
      const word = words[index];
      document.getElementById("word").innerText = word.word;
      document.getElementById("definition").innerText = word.definition;
      document.getElementById("verse").innerText = word.verse;
      document.getElementById("remember-status").innerText = remembered[word.word] ? "✅ Remembered" : "❓ Not remembered";
    }

    function nextWord() {
      do {
        index = (index + 1) % words.length;
      } while (filterMode && remembered[words[index].word]);
      render();
    }

    function prevWord() {
      do {
        index = (index - 1 + words.length) % words.length;
      } while (filterMode && remembered[words[index].word]);
      render();
    }

    function toggleRemember() {
      const current = words[index].word;
      remembered[current] = !remembered[current];
      localStorage.setItem("rememberedWords", JSON.stringify(remembered));
      render();
    }

    function toggleFilter() {
      filterMode = !filterMode;
      alert(filterMode ? "✅ Now reviewing only unremembered words." : "🔁 Showing all words.");
      if (filterMode && remembered[words[index].word]) nextWord();
      render();
    }

    document.getElementById("search").addEventListener("input", function (e) {
      const value = e.target.value.toLowerCase();
      const matchIndex = words.findIndex(w => w.word.toLowerCase().includes(value));
      if (matchIndex !== -1) {
        index = matchIndex;
        render();
      }
    });

    render();
  </script>
</body>
</html>
