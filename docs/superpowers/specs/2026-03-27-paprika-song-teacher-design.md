# Paprika 歌詞互動練習平台 — 設計規格

## 概述

為 6-8 歲英文初學者製作的互動歌詞練習頁面，幫助孩子準備班級英文歌唱比賽。核心目標是讓孩子能「唸出來」，而非深度理解文法。中文翻譯輔助理解歌詞大意。

## 技術方案

單一 HTML 檔案，內嵌 CSS 和 JavaScript，零依賴。瀏覽器直接開啟即可使用，針對手機和平板優化。

- **語音合成**：Web Speech API，女聲，rate 0.7-0.8，穩定說故事節奏
- **原曲播放**：HTML5 Audio，預切好的 mp3 檔案（每段一個）
- **部署**：靜態檔案，無需伺服器

## 介面設計

### 版面配置

- **卡片式歌詞** — 每句英文歌詞是一張獨立的白色圓角卡片
- 英文歌詞為主文字（大、粗體、好辨識）
- 中文翻譯在下方（較小、灰色）
- 點擊整張卡片觸發慢速朗讀
- **段落導航** — 頂部顯示「第 X 段 / 共八段」，底部有「上一段」「下一段」按鈕
- **播放本段** — 每段有播放原曲按鈕

### 視覺風格

彩虹粉彩馬卡龍風格：

- 背景：淺藍 → 薄荷綠 → 鵝黃 → 粉紅 多色漸層
- 歌詞卡片：白色底，圓角 12px，左側有彩色邊條（每句不同顏色）
- 按鈕：圓角膠囊形，漸層色
- 字型：系統預設 sans-serif，大字體（英文 16-18px，中文 13-14px）
- 整體色調柔和，適合兒童

### 互動設計

1. **點擊歌詞卡片** → Web Speech API 女聲慢速朗讀該句英文
2. **點擊「播放本段」** → 播放該段的原曲 mp3
3. **點擊「上一段/下一段」** → 切換段落，更新歌詞卡片
4. 正在朗讀的卡片有視覺高亮提示
5. 正在播放原曲時按鈕變為暫停狀態

## 歌詞與翻譯

### 第一段
| 英文 | 中文 |
|------|------|
| Twisting and turning, down this road we go | 彎彎繞繞，沿著這條路往前走 |
| Running to the forest where we can play all day | 跑進森林裡，在那兒玩上一整天 |
| The sun shines so brightly on our country town | 陽光燦爛地照耀著我們的小鎮 |
| Someone's always calling out your name | 總有人在呼喚你的名字 |

### 第二段
| 英文 | 中文 |
|------|------|
| And when summer comes, see our shadows grow | 當夏天來臨，看我們的影子越來越長 |
| Always know I will miss you so | 你要知道，我會非常想念你 |
| Come on, look up, find the first star in the sky | 來吧，抬頭看，找到天空中的第一顆星星 |
| I hope tomorrow will be sunny, too | 希望明天也是晴朗的好天氣 |

### 第三段
| 英文 | 中文 |
|------|------|
| Paprika, when our flowers start to bloom | Paprika，當我們的花朵綻放 |
| Put the seeds into your hands and throw them in the sky | 把種子捧在手心，拋向天空吧 |
| Paprika, we can make our dreams come alive | Paprika，我們能讓夢想成真 |
| Rain or shine, we'll find a way to play again another day | 無論晴天或雨天，我們總會找到機會再一起玩耍 |

### 第四段
| 英文 | 中文 |
|------|------|
| It's raining and pouring, the moon's hiding away | 大雨傾盆而下，月亮也躲了起來 |
| I think I can hear someone crying in the shade | 我好像聽見有人在陰影中哭泣 |
| Don't worry I promise, there's no need to be afraid | 別擔心，我向你保證，不需要害怕 |
| Someone's always calling out your name | 總有人在呼喚你的名字 |

### 第五段
| 英文 | 中文 |
|------|------|
| Come and count with me all the happy things | 來吧，和我一起數數那些快樂的事 |
| So much joy you always bring | 你總是帶來那麼多的歡樂 |
| Now it's time to go, I'll see you tomorrow | 該出發了，明天見 |
| Memories will light the way back home | 回憶將會照亮回家的路 |

### 第六段
（同第三段）
| 英文 | 中文 |
|------|------|
| Paprika, when our flowers start to bloom | Paprika，當我們的花朵綻放 |
| Put the seeds into your hands and throw them in the sky | 把種子捧在手心，拋向天空吧 |
| Paprika, we can make our dreams come alive | Paprika，我們能讓夢想成真 |
| Rain or shine, we'll find a way to play again another day | 無論晴天或雨天，我們總會找到機會再一起玩耍 |

### 第七段
| 英文 | 中文 |
|------|------|
| I will run to you Through the forest where we played | 我會朝你跑去，穿過我們曾經玩耍的森林 |
| Singing songs we made | 唱著我們一起編的歌 |
| And I will fill both hands with flowers along the way | 沿途摘滿雙手的花朵 |
| La-di la-di-da | 啦滴 啦滴答 |

### 第八段
| 英文 | 中文 |
|------|------|
| Paprika, when our flowers start to bloom | Paprika，當我們的花朵綻放 |
| Put the seeds into your hands and throw them in the sky | 把種子捧在手心，拋向天空吧 |
| Paprika, we can make our dreams come alive | Paprika，我們能讓夢想成真 |
| Rain or shine, we'll find a way to play again another day | 無論晴天或雨天，我們總會找到機會再一起玩耍 |
| Let's all come together now, point our fingers to the sky | 大家聚在一起，把手指向天空 |

## 音檔規格

從 YouTube 原曲切割 8 個 mp3 檔案：

| 檔案 | 時間範圍 |
|------|----------|
| part1.mp3 | 0:10 - 0:30 |
| part2.mp3 | 0:30 - 0:49 |
| part3.mp3 | 0:50 - 1:10 |
| part4.mp3 | 1:10 - 1:29 |
| part5.mp3 | 1:30 - 1:49 |
| part6.mp3 | 1:49 - 2:10 |
| part7.mp3 | 2:11 - 2:50 |
| part8.mp3 | 2:50 - 3:18 |

音檔放在 `audio/` 目錄下。

## 檔案結構

```
song_teacher/
├── index.html          # 主頁面（所有邏輯內嵌）
├── audio/
│   ├── part1.mp3
│   ├── part2.mp3
│   ├── ...
│   └── part8.mp3
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-03-27-paprika-song-teacher-design.md
```

## Web Speech API 設定

```javascript
const utterance = new SpeechSynthesisUtterance(text);
utterance.lang = 'en-US';
utterance.rate = 0.75;       // 慢速，穩定節奏
utterance.pitch = 1.1;       // 稍高，女聲
// 選擇女性聲音
const voices = speechSynthesis.getVoices();
const femaleVoice = voices.find(v => v.name.includes('Female') || v.name.includes('Samantha'));
if (femaleVoice) utterance.voice = femaleVoice;
```

## 不在範圍內

- 進度追蹤 / 熟練度標記
- 注音或拼音標註
- 使用者帳號 / 登入
- 逐句原曲播放（只做整段）
- 後端伺服器
