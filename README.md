# project
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Emoji Lookup</title>
  <link href="https://fonts.googleapis.com/css2?family=Righteous&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet" />
  <style>
    :root {
      --c1: #ff6b6b;
      --c2: #ffd93d;
      --c3: #6bcb77;
      --c4: #4d96ff;
      --c5: #c77dff;
      --bg: #0f0e17;
      --surface: #1a1828;
      --surface2: #241f38;
      --text: #fffffe;
      --muted: #a7a3c2;
      --radius: 18px;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'DM Sans', sans-serif;
      background: var(--bg);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      padding: 3rem 1rem;
      overflow-x: hidden;
    }

    body::before, body::after {
      content: '';
      position: fixed;
      border-radius: 50%;
      filter: blur(90px);
      opacity: 0.18;
      pointer-events: none;
      animation: drift 10s ease-in-out infinite alternate;
    }
    body::before {
      width: 500px; height: 500px;
      background: var(--c5);
      top: -120px; left: -120px;
    }
    body::after {
      width: 400px; height: 400px;
      background: var(--c4);
      bottom: -100px; right: -100px;
      animation-delay: -5s;
    }

    @keyframes drift {
      from { transform: translate(0, 0) scale(1); }
      to   { transform: translate(40px, 40px) scale(1.1); }
    }

    .card {
      background: var(--surface);
      border-radius: 28px;
      border: 1px solid rgba(255,255,255,0.07);
      padding: 2.5rem 2rem;
      width: 100%;
      max-width: 500px;
      position: relative;
      z-index: 1;
    }

    .card::before {
      content: '';
      display: block;
      height: 5px;
      border-radius: 99px;
      background: linear-gradient(90deg, var(--c1), var(--c2), var(--c3), var(--c4), var(--c5));
      margin-bottom: 2rem;
    }

    h1 {
      font-family: 'Righteous', sans-serif;
      font-size: 28px;
      font-weight: 400;
      color: var(--text);
      text-align: center;
      letter-spacing: 0.5px;
      margin-bottom: 0.4rem;
    }

    .subtitle {
      text-align: center;
      font-size: 13.5px;
      color: var(--muted);
      margin-bottom: 1.75rem;
    }

    .subtitle code {
      background: var(--surface2);
      color: var(--c2);
      padding: 2px 7px;
      border-radius: 6px;
      font-size: 12px;
    }

    .input-row {
      display: flex;
      gap: 10px;
      margin-bottom: 1.5rem;
    }

    input {
      flex: 1;
      padding: 12px 16px;
      font-size: 15px;
      font-family: 'DM Sans', sans-serif;
      background: var(--surface2);
      border: 1.5px solid rgba(255,255,255,0.08);
      border-radius: 14px;
      color: var(--text);
      outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
    }

    input::placeholder { color: var(--muted); }

    input:focus {
      border-color: var(--c4);
      box-shadow: 0 0 0 3px rgba(77,150,255,0.18);
    }

    button.show-btn {
      padding: 12px 22px;
      font-family: 'Righteous', sans-serif;
      font-size: 15px;
      border: none;
      border-radius: 14px;
      background: linear-gradient(135deg, var(--c4), var(--c5));
      color: #fff;
      cursor: pointer;
      transition: transform 0.15s, opacity 0.15s;
      white-space: nowrap;
    }

    button.show-btn:hover  { opacity: 0.9; }
    button.show-btn:active { transform: scale(0.96); }

    .result-box {
      min-height: 180px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background: var(--surface2);
      border: 1.5px solid rgba(255,255,255,0.06);
      border-radius: var(--radius);
      padding: 2rem 1.5rem;
      margin-bottom: 1.25rem;
      transition: border-color 0.3s;
      position: relative;
      overflow: hidden;
    }

    .result-box.has-emoji {
      border-color: rgba(255,255,255,0.15);
    }

    .result-box.has-emoji::after {
      content: '';
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at 50% 60%, rgba(77,150,255,0.08), transparent 70%);
      pointer-events: none;
    }

    .result-emoji {
      font-size: 100px;
      line-height: 1;
      display: block;
      margin-bottom: 14px;
      animation: pop 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
    }

    @keyframes pop {
      from { transform: scale(0.4); opacity: 0; }
      to   { transform: scale(1);   opacity: 1; }
    }

    .result-name {
      font-family: 'Righteous', sans-serif;
      font-size: 18px;
      letter-spacing: 0.5px;
      background: linear-gradient(90deg, var(--c1), var(--c2), var(--c3));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .result-empty {
      font-size: 13px;
      color: var(--muted);
    }

    .suggestions-label {
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 1.2px;
      color: var(--muted);
      margin-bottom: 10px;
    }

    .suggestions {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .suggestions button {
      font-family: 'DM Sans', sans-serif;
      font-size: 13px;
      padding: 5px 13px;
      background: var(--surface2);
      border: 1px solid rgba(255,255,255,0.1);
      border-radius: 99px;
      color: var(--text);
      cursor: pointer;
      transition: background 0.15s, border-color 0.15s, transform 0.1s;
    }

    .suggestions button:hover {
      background: rgba(255,255,255,0.08);
      border-color: rgba(255,255,255,0.22);
    }

    .suggestions button:active { transform: scale(0.95); }
  </style>
</head>
<body>
  <div class="card">
    <h1>✨ Emoji Lookup</h1>
    <p class="subtitle">
      Type a name like <code>rocket</code>, <code>pizza</code>, or <code>heart</code>
    </p>

    <div class="input-row">
      <input type="text" id="emojiInput" placeholder="Enter emoji name…" />
      <button class="show-btn" id="showBtn">Show ↗</button>
    </div>

    <div class="result-box" id="result">
      <span class="result-empty">Your emoji will appear here 🎯</span>
    </div>

    <p class="suggestions-label">Quick picks</p>
    <div class="suggestions" id="suggestions"></div>
  </div>

  <script>
    const emojiMap = {
      smile: "😊", happy: "😊", grin: "😁", laugh: "😂", lol: "😂", joy: "😂",
      wink: "😉", cool: "😎", sunglasses: "😎", heart: "❤️", love: "❤️",
      fire: "🔥", star: "⭐", sparkles: "✨", thumbsup: "👍", thumbs_up: "👍",
      thumbsdown: "👎", clap: "👏", wave: "👋", pray: "🙏", muscle: "💪",
      rocket: "🚀", pizza: "🍕", burger: "🍔", taco: "🌮", sushi: "🍣",
      donut: "🍩", cake: "🎂", coffee: "☕", beer: "🍺", wine: "🍷",
      cat: "🐱", dog: "🐶", unicorn: "🦄", dragon: "🐉", fox: "🦊",
      panda: "🐼", bear: "🐻", rabbit: "🐰", monkey: "🐵", penguin: "🐧",
      sun: "☀️", moon: "🌙", rainbow: "🌈", cloud: "☁️", snowflake: "❄️",
      lightning: "⚡", umbrella: "☂️", ocean: "🌊", tree: "🌳", flower: "🌸",
      rose: "🌹", cactus: "🌵", mushroom: "🍄", gem: "💎", trophy: "🏆",
      crown: "👑", sword: "⚔️", shield: "🛡️", magic: "🪄", ghost: "👻",
      skull: "💀", alien: "👽", robot: "🤖", ninja: "🥷", wizard: "🧙",
      party: "🎉", balloon: "🎈", gift: "🎁", music: "🎵", guitar: "🎸",
      piano: "🎹", drum: "🥁", headphones: "🎧", microphone: "🎤", camera: "📷",
      phone: "📱", laptop: "💻", book: "📖", pencil: "✏️", globe: "🌍",
      map: "🗺️", compass: "🧭", flag: "🏳️", house: "🏠", castle: "🏰",
      car: "🚗", train: "🚂", plane: "✈️", boat: "⛵", bicycle: "🚲",
      money: "💰", diamond: "💎", key: "🔑", lock: "🔒", magnifier: "🔍",
      eye: "👁️", brain: "🧠", bone: "🦴", seed: "🌱", sunrise: "🌅",
      comet: "☄️", hourglass: "⏳", bell: "🔔", bomb: "💣",
      stethoscope: "🩺", test: "🧪", dna: "🧬",
      sad: "😢", cry: "😭", angry: "😠", surprised: "😲", confused: "😕",
      thinking: "🤔", nerd: "🤓", sick: "🤒", sleeping: "😴", dizzy: "😵",
      ok: "👌", peace: "✌️", recycle: "♻️", warning: "⚠️",
      check: "✅", cross_mark: "❌", question: "❓", exclamation: "❗",
      hot: "🌶️", ice: "🧊", tornado: "🌪️", explosion: "💥"
    };

    const quickPicks = ["rocket", "pizza", "heart", "fire", "unicorn", "cat", "party", "sparkles"];

    const input = document.getElementById("emojiInput");
    const resultDiv = document.getElementById("result");
    const suggestionsDiv = document.getElementById("suggestions");

    function showEmoji(name) {
      const key  = name.trim().toLowerCase().replace(/\s+/g, "_");
      const alt  = name.trim().toLowerCase().replace(/\s+/g, "");
      const emoji = emojiMap[key] || emojiMap[alt];

      if (emoji) {
        resultDiv.classList.add("has-emoji");
        resultDiv.innerHTML = `
          <span class="result-emoji">${emoji}</span>
          <span class="result-name">${name.trim()}</span>
        `;
      } else {
        resultDiv.classList.remove("has-emoji");
        resultDiv.innerHTML = `
          <span class="result-emoji" style="font-size:52px;">🤷</span>
          <span class="result-name" style="font-size:15px; background:none; -webkit-text-fill-color:#a7a3c2;">No emoji for "${name.trim()}"</span>
          <span class="result-empty" style="margin-top:6px;">Try: smile, fire, rocket, cat…</span>
        `;
      }
    }

    document.getElementById("showBtn").addEventListener("click", () => {
      if (input.value.trim()) showEmoji(input.value);
    });

    input.addEventListener("keydown", e => {
      if (e.key === "Enter" && input.value.trim()) showEmoji(input.value);
    });

    quickPicks.forEach(name => {
      const btn = document.createElement("button");
      btn.textContent = (emojiMap[name] || "") + "  " + name;
      btn.addEventListener("click", () => {
        input.value = name;
        showEmoji(name);
      });
      suggestionsDiv.appendChild(btn);
    });
  </script>
</body>
</html>
