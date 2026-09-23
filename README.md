# 黃金鵜鶘騎單車 🚲

一隻剪紙風的黃金鵜鶘，騎著紅色單車環遊世界（還有月球）。

**▶ 線上看：** https://crystal32378.github.io/golden-pelican/

**🎬 97 秒環遊影片：** https://crystal32378.github.io/golden-pelican/golden-pelican-tour.mp4

## 十三個場景

按畫面左下角的「場景」按鈕切換：

| 場景 | 看點 |
|---|---|
| 小島 | 夕陽、燈塔、跳躍的海豚、海鷗 |
| 操場 | 放學後的跑道、鵜鶘校旗、司令台 |
| 夜市 | 雞排、珍奶、臭豆腐、撈金魚，籃子裡還有一杯珍奶 |
| 圓環 | 台北式圓環、計程車與機車群、金色鵜鶘銅像噴水池 |
| 銀閣寺 | 銀沙灘、向月台、錦鯉、飄落的楓葉 |
| 登月 | 低重力彈跳、永遠不會消失的輪胎圓圈、地平線上的地球 |
| 極地 | 企鵝和北極熊第一次見面、極光、毛帽與圍巾 |
| 雨林 | 超慢的樹懶、大嘴鳥、王蓮、水豚、探險帽 |
| 雲海 | 日出雲海、客機凝結尾、火箭升空、V 字形鵜鶘朋友 |
| 凱旋門 | 十二條大道、沒有車道線的車流、艾菲爾鐵塔、貝雷帽與法棍 |
| 深海 | 座頭鯨、發光水母、海帶森林、鬼蝠魟、海龜、珊瑚礁上的寶箱、潛水面鏡與呼吸管 |
| 宇宙 | 騎在巨大行星的光環上、星雲、旋轉的太空站、彗星、打光束的飛碟 |
| 家 | 追著單車跑的黃金獵犬、三隻貓、拍手的小嬰兒、會走的時鐘、電視裡播著燈塔小島 |

拖曳可以轉視角、滾輪縮放；也可以直接用網址打開某個場景，例如 `#moon`、`#paris`。

## 由來

2026 年 9 月 Claude Opus 5.5 發表時，Addy Osmani 用一隻 Three.js 騎單車的鵜鶘慶祝。
我們從那裡出發，一站一站把這隻鵜鶘帶去了十個地方。

- 構想與導演：Crystal
- 建模與程式：Claude（Anthropic）
- 配樂：Crystal（`score.mp3`）

📐 **[工程設計筆記（Claude 寫的）](DESIGN.md)**：架構、兩段式 IK、萬用羽毛、程式畫的貼圖、錄影流程，以及抓到過的 bug。

整個作品是一個 `index.html`，用 [Three.js r128](https://threejs.org/) 手刻低多邊形模型，沒有任何外部 3D 素材。

---

*A low-poly golden pelican rides a bicycle around ten places — an island, a school field, a Taiwanese night market, a Taipei roundabout, Ginkaku-ji, the Moon, the poles, the Amazon, a sea of clouds and the Arc de Triomphe. Single-file Three.js, made by Crystal together with Claude.*
