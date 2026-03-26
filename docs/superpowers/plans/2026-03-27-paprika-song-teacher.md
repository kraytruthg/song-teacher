# Paprika Song Teacher Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file interactive lyrics practice page for Paprika (Foorin team E) that lets a child tap lyrics to hear slow TTS pronunciation and play the original song per section.

**Architecture:** Single HTML file with embedded CSS and JS. Lyrics data stored as a JS array of sections. Web Speech API for per-line TTS, HTML5 Audio for per-section mp3 playback. No build tools, no dependencies.

**Tech Stack:** HTML5, CSS3 (gradients, flexbox), vanilla JavaScript, Web Speech API, HTML5 Audio

**Spec:** `docs/superpowers/specs/2026-03-27-paprika-song-teacher-design.md`

---

### Task 1: Project Setup and Audio Files

**Files:**
- Create: `audio/` directory
- Create: `audio/part1.mp3` through `audio/part8.mp3`

This task requires the user to provide the source mp3. The implementer will cut it using ffmpeg.

- [ ] **Step 1: Initialize git repo**

```bash
cd /Users/jerry/development/song_teacher
git init
```

- [ ] **Step 2: Create .gitignore**

Create `.gitignore`:
```
.superpowers/
.DS_Store
```

- [ ] **Step 3: Create audio directory**

```bash
mkdir -p audio
```

- [ ] **Step 4: Cut audio files with ffmpeg**

The user must provide the downloaded mp3 file (e.g., `paprika-full.mp3`). Then cut using these timestamps:

```bash
ffmpeg -i paprika-full.mp3 -ss 10 -to 30 -c copy audio/part1.mp3
ffmpeg -i paprika-full.mp3 -ss 30 -to 49 -c copy audio/part2.mp3
ffmpeg -i paprika-full.mp3 -ss 50 -to 70 -c copy audio/part3.mp3
ffmpeg -i paprika-full.mp3 -ss 70 -to 89 -c copy audio/part4.mp3
ffmpeg -i paprika-full.mp3 -ss 90 -to 109 -c copy audio/part5.mp3
ffmpeg -i paprika-full.mp3 -ss 109 -to 130 -c copy audio/part6.mp3
ffmpeg -i paprika-full.mp3 -ss 131 -to 170 -c copy audio/part7.mp3
ffmpeg -i paprika-full.mp3 -ss 170 -to 198 -c copy audio/part8.mp3
```

- [ ] **Step 5: Verify audio files exist**

```bash
ls -la audio/
```

Expected: 8 mp3 files, each a few hundred KB.

- [ ] **Step 6: Commit**

```bash
git add .gitignore audio/
git commit -m "chore: add project setup and audio files for Paprika"
```

---

### Task 2: HTML Structure and Lyrics Data

**Files:**
- Create: `index.html`

Build the HTML skeleton and embed all lyrics data as a JS array. No styling yet — just structure.

- [ ] **Step 1: Create index.html with HTML structure**

Create `index.html` with:
- `<!DOCTYPE html>` with `<meta charset="UTF-8">` and mobile viewport meta
- `<title>Paprika 歌詞練習</title>`
- Header section: song title "🌸 Paprika" and subtitle "Foorin team E"
- Section indicator: `<div id="section-indicator">第 1 段 / 共 8 段</div>`
- Lyrics container: `<div id="lyrics-container"></div>`
- Controls bar: play button + prev/next navigation buttons
- `<audio id="audio-player">` element (no src yet, set dynamically)

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Paprika 歌詞練習</title>
  <style>
    /* styles added in Task 3 */
  </style>
</head>
<body>
  <div id="app">
    <header>
      <h1>🌸 Paprika</h1>
      <p class="subtitle">Foorin team E</p>
    </header>

    <div id="section-indicator">第 1 段 / 共 8 段</div>

    <div id="lyrics-container"></div>

    <div id="controls">
      <button id="btn-prev" onclick="prevSection()">◀ 上一段</button>
      <button id="btn-play" onclick="togglePlay()">▶ 播放本段</button>
      <button id="btn-next" onclick="nextSection()">下一段 ▶</button>
    </div>
  </div>

  <audio id="audio-player"></audio>

  <script>
    // --- Lyrics Data ---
    const sections = [
      {
        title: "第一段",
        audio: "audio/part1.mp3",
        lines: [
          { en: "Twisting and turning, down this road we go", zh: "彎彎繞繞，沿著這條路往前走" },
          { en: "Running to the forest where we can play all day", zh: "跑進森林裡，在那兒玩上一整天" },
          { en: "The sun shines so brightly on our country town", zh: "陽光燦爛地照耀著我們的小鎮" },
          { en: "Someone's always calling out your name", zh: "總有人在呼喚你的名字" }
        ]
      },
      {
        title: "第二段",
        audio: "audio/part2.mp3",
        lines: [
          { en: "And when summer comes, see our shadows grow", zh: "當夏天來臨，看我們的影子越來越長" },
          { en: "Always know I will miss you so", zh: "你要知道，我會非常想念你" },
          { en: "Come on, look up, find the first star in the sky", zh: "來吧，抬頭看，找到天空中的第一顆星星" },
          { en: "I hope tomorrow will be sunny, too", zh: "希望明天也是晴朗的好天氣" }
        ]
      },
      {
        title: "第三段",
        audio: "audio/part3.mp3",
        lines: [
          { en: "Paprika, when our flowers start to bloom", zh: "Paprika，當我們的花朵綻放" },
          { en: "Put the seeds into your hands and throw them in the sky", zh: "把種子捧在手心，拋向天空吧" },
          { en: "Paprika, we can make our dreams come alive", zh: "Paprika，我們能讓夢想成真" },
          { en: "Rain or shine, we'll find a way to play again another day", zh: "無論晴天或雨天，我們總會找到機會再一起玩耍" }
        ]
      },
      {
        title: "第四段",
        audio: "audio/part4.mp3",
        lines: [
          { en: "It's raining and pouring, the moon's hiding away", zh: "大雨傾盆而下，月亮也躲了起來" },
          { en: "I think I can hear someone crying in the shade", zh: "我好像聽見有人在陰影中哭泣" },
          { en: "Don't worry I promise, there's no need to be afraid", zh: "別擔心，我向你保證，不需要害怕" },
          { en: "Someone's always calling out your name", zh: "總有人在呼喚你的名字" }
        ]
      },
      {
        title: "第五段",
        audio: "audio/part5.mp3",
        lines: [
          { en: "Come and count with me all the happy things", zh: "來吧，和我一起數數那些快樂的事" },
          { en: "So much joy you always bring", zh: "你總是帶來那麼多的歡樂" },
          { en: "Now it's time to go, I'll see you tomorrow", zh: "該出發了，明天見" },
          { en: "Memories will light the way back home", zh: "回憶將會照亮回家的路" }
        ]
      },
      {
        title: "第六段",
        audio: "audio/part6.mp3",
        lines: [
          { en: "Paprika, when our flowers start to bloom", zh: "Paprika，當我們的花朵綻放" },
          { en: "Put the seeds into your hands and throw them in the sky", zh: "把種子捧在手心，拋向天空吧" },
          { en: "Paprika, we can make our dreams come alive", zh: "Paprika，我們能讓夢想成真" },
          { en: "Rain or shine, we'll find a way to play again another day", zh: "無論晴天或雨天，我們總會找到機會再一起玩耍" }
        ]
      },
      {
        title: "第七段",
        audio: "audio/part7.mp3",
        lines: [
          { en: "I will run to you Through the forest where we played", zh: "我會朝你跑去，穿過我們曾經玩耍的森林" },
          { en: "Singing songs we made", zh: "唱著我們一起編的歌" },
          { en: "And I will fill both hands with flowers along the way", zh: "沿途摘滿雙手的花朵" },
          { en: "La-di la-di-da", zh: "啦滴 啦滴答" }
        ]
      },
      {
        title: "第八段",
        audio: "audio/part8.mp3",
        lines: [
          { en: "Paprika, when our flowers start to bloom", zh: "Paprika，當我們的花朵綻放" },
          { en: "Put the seeds into your hands and throw them in the sky", zh: "把種子捧在手心，拋向天空吧" },
          { en: "Paprika, we can make our dreams come alive", zh: "Paprika，我們能讓夢想成真" },
          { en: "Rain or shine, we'll find a way to play again another day", zh: "無論晴天或雨天，我們總會找到機會再一起玩耍" },
          { en: "Let's all come together now, point our fingers to the sky", zh: "大家聚在一起，把手指向天空" }
        ]
      }
    ];

    let currentSection = 0;
    const audioPlayer = document.getElementById('audio-player');
    const lyricsContainer = document.getElementById('lyrics-container');
    const sectionIndicator = document.getElementById('section-indicator');
    const btnPlay = document.getElementById('btn-play');
    const btnPrev = document.getElementById('btn-prev');
    const btnNext = document.getElementById('btn-next');

    // --- Render ---
    function renderSection() {
      const section = sections[currentSection];
      sectionIndicator.textContent = `${section.title} / 共八段`;

      lyricsContainer.innerHTML = '';
      section.lines.forEach((line, i) => {
        const card = document.createElement('div');
        card.className = 'lyric-card';
        card.onclick = () => speakLine(line.en, card);
        card.innerHTML = `
          <div class="lyric-en">${line.en}</div>
          <div class="lyric-zh">${line.zh}</div>
        `;
        lyricsContainer.appendChild(card);
      });

      btnPrev.disabled = currentSection === 0;
      btnNext.disabled = currentSection === sections.length - 1;

      // Stop any playing audio when switching sections
      audioPlayer.pause();
      audioPlayer.currentTime = 0;
      btnPlay.textContent = '▶ 播放本段';
    }

    // --- Navigation ---
    function prevSection() {
      if (currentSection > 0) { currentSection--; renderSection(); }
    }

    function nextSection() {
      if (currentSection < sections.length - 1) { currentSection++; renderSection(); }
    }

    // --- Audio Playback ---
    function togglePlay() {
      if (!audioPlayer.paused) {
        audioPlayer.pause();
        btnPlay.textContent = '▶ 播放本段';
      } else {
        audioPlayer.src = sections[currentSection].audio;
        audioPlayer.play();
        btnPlay.textContent = '⏸ 暫停';
      }
    }

    audioPlayer.addEventListener('ended', () => {
      btnPlay.textContent = '▶ 播放本段';
    });

    // --- TTS ---
    let selectedVoice = null;

    function loadVoices() {
      const voices = speechSynthesis.getVoices();
      selectedVoice = voices.find(v => v.lang.startsWith('en') && v.name.includes('Samantha'))
        || voices.find(v => v.lang.startsWith('en') && (v.name.includes('Female') || v.name.includes('female')))
        || voices.find(v => v.lang.startsWith('en-US'))
        || voices.find(v => v.lang.startsWith('en'))
        || null;
    }

    speechSynthesis.onvoiceschanged = loadVoices;
    loadVoices();

    function speakLine(text, cardEl) {
      speechSynthesis.cancel();

      // Remove highlight from all cards
      document.querySelectorAll('.lyric-card').forEach(c => c.classList.remove('speaking'));
      cardEl.classList.add('speaking');

      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = 'en-US';
      utterance.rate = 0.75;
      utterance.pitch = 1.1;
      if (selectedVoice) utterance.voice = selectedVoice;

      utterance.onend = () => {
        cardEl.classList.remove('speaking');
      };

      speechSynthesis.speak(utterance);
    }

    // --- Init ---
    renderSection();
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify structure renders**

```bash
open index.html
```

Expected: Page shows with header, section indicator, 4 lyric lines (unstyled), and 3 buttons.

- [ ] **Step 3: Verify TTS works**

Click any lyric line. Expected: English text is read aloud in a slow female voice.

- [ ] **Step 4: Verify section navigation works**

Click "下一段 ▶". Expected: lyrics change to section 2, indicator updates.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add HTML structure with lyrics data, TTS, and navigation"
```

---

### Task 3: Macaron Pastel Styling

**Files:**
- Modify: `index.html` — replace the `<style>` block

Apply the rainbow pastel macaron visual design: gradient background, card styling with colored left borders, large readable fonts, mobile-optimized layout.

- [ ] **Step 1: Add CSS styles**

Replace the `<style>` comment in `index.html` with the full CSS. Key design decisions:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  -webkit-tap-highlight-color: transparent;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Noto Sans TC", sans-serif;
  background: linear-gradient(135deg, #e1f5fe, #e8f5e9, #fff9c4, #fce4ec);
  min-height: 100vh;
  padding: 16px;
  padding-bottom: 100px;
}

#app {
  max-width: 480px;
  margin: 0 auto;
}

header {
  text-align: center;
  margin-bottom: 16px;
}

header h1 {
  font-size: 28px;
  font-weight: 800;
  color: #e91e63;
}

header .subtitle {
  font-size: 14px;
  color: #9c27b0;
  margin-top: 2px;
}

#section-indicator {
  text-align: center;
  margin-bottom: 16px;
}

#section-indicator {
  display: inline-block;
  background: rgba(255, 255, 255, 0.7);
  color: #6a1b9a;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  width: 100%;
  text-align: center;
}

.lyric-card {
  background: rgba(255, 255, 255, 0.92);
  border-radius: 12px;
  padding: 14px 14px 14px 18px;
  margin-bottom: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  cursor: pointer;
  transition: transform 0.15s, box-shadow 0.15s;
  border-left: 4px solid #ccc;
  -webkit-user-select: none;
  user-select: none;
}

.lyric-card:active {
  transform: scale(0.98);
}

.lyric-card.speaking {
  background: rgba(255, 255, 255, 1);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.12);
  transform: scale(1.01);
}

/* Rainbow left-border colors, cycling through pastel palette */
.lyric-card:nth-child(8n+1) { border-left-color: #f48fb1; }
.lyric-card:nth-child(8n+2) { border-left-color: #81d4fa; }
.lyric-card:nth-child(8n+3) { border-left-color: #a5d6a7; }
.lyric-card:nth-child(8n+4) { border-left-color: #ffe082; }
.lyric-card:nth-child(8n+5) { border-left-color: #ce93d8; }
.lyric-card:nth-child(8n+6) { border-left-color: #80cbc4; }
.lyric-card:nth-child(8n+7) { border-left-color: #ffab91; }
.lyric-card:nth-child(8n+8) { border-left-color: #90caf9; }

.lyric-en {
  font-size: 18px;
  font-weight: 600;
  color: #333;
  line-height: 1.5;
  margin-bottom: 4px;
}

.lyric-zh {
  font-size: 14px;
  color: #999;
  line-height: 1.4;
}

#controls {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  gap: 8px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.08);
  justify-content: center;
  max-width: 480px;
  margin: 0 auto;
}

#controls button {
  flex: 1;
  padding: 12px 8px;
  border-radius: 24px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  border: none;
  transition: opacity 0.2s;
}

#controls button:disabled {
  opacity: 0.35;
  cursor: default;
}

#btn-prev, #btn-next {
  background: white;
  color: #e91e63;
  border: 2px solid #f48fb1;
}

#btn-play {
  background: linear-gradient(135deg, #4dd0e1, #81c784);
  color: white;
}
```

- [ ] **Step 2: Open in browser and verify visual design**

```bash
open index.html
```

Expected: Rainbow pastel background, white cards with colored left borders, fixed bottom controls, readable large fonts.

- [ ] **Step 3: Test on mobile viewport**

Open browser DevTools → toggle device toolbar → select iPhone SE or similar. Verify:
- Cards are full width and easy to tap
- Bottom controls are visible and not overlapping content
- Text is readable without zooming

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add macaron pastel rainbow styling for mobile"
```

---

### Task 4: Audio Playback Integration

**Files:**
- Modify: `index.html` — verify audio playback works with actual mp3 files

- [ ] **Step 1: Verify audio files are in place**

```bash
ls -la audio/
```

Expected: part1.mp3 through part8.mp3 exist.

- [ ] **Step 2: Test audio playback in browser**

Open `index.html`, click "▶ 播放本段". Expected: part1.mp3 plays. Button text changes to "⏸ 暫停".

- [ ] **Step 3: Test pause/resume**

Click "⏸ 暫停". Expected: audio pauses, button changes back to "▶ 播放本段".

- [ ] **Step 4: Test section switch stops audio**

Play audio, then click "下一段 ▶". Expected: audio stops, button resets.

- [ ] **Step 5: Test audio ends naturally**

Play a short section and let it finish. Expected: button resets to "▶ 播放本段" automatically.

- [ ] **Step 6: Commit (if any fixes were needed)**

```bash
git add index.html
git commit -m "fix: audio playback adjustments"
```

---

### Task 5: Final Polish and Testing

**Files:**
- Modify: `index.html` — any final adjustments

- [ ] **Step 1: End-to-end test on mobile device**

Serve the page locally and open on a phone/tablet:
```bash
python3 -m http.server 8000
```
Open `http://<your-ip>:8000` on phone. Test:
- Tap each lyric card → TTS reads slowly in female voice
- Play/pause button works
- Navigation between all 8 sections works
- Visual design looks correct

- [ ] **Step 2: Test TTS voice quality**

Verify on the target device (iPhone/iPad or Android tablet):
- Voice is female
- Speed is slow and steady (not accelerating on long sentences)
- Each card highlights while speaking

- [ ] **Step 3: Fix any issues found**

Apply fixes as needed.

- [ ] **Step 4: Final commit**

```bash
git add index.html
git commit -m "feat: complete Paprika song teacher interactive page"
```
