<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>唐詩繪卷：長安尋跡錄（三模式修練版）</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&family=Noto+Serif+TC:wght@500;700;900&display=swap" rel="stylesheet" />
  <style>
    :root {
      --bg-paper: #F5F2EB;
      --bg-card: #FAF8F3;
      --border-sand: #D6CFC2;
      --ink-main: #2F353B;
      --ink-sub: #5C646C;
      --morandi-blue: #5B7083;
      --morandi-blue-soft: #EBF0F5;
      --morandi-sage: #648272;
      --morandi-sage-soft: #EBF2EE;
      --morandi-red: #A85854;
      --morandi-red-soft: #F7ECEB;
      --morandi-ochre: #B5894C;
      --morandi-ochre-soft: #F9F3E6;
    }

    body {
      font-family: 'Noto Sans TC', sans-serif;
      background-color: var(--bg-paper);
      color: var(--ink-main);
      background-image: radial-gradient(#DED8CA 0.75px, transparent 0.75px);
      background-size: 20px 20px;
      user-select: none;
    }

    .font-serif-tc {
      font-family: 'Noto Serif TC', serif;
    }

    .scroll-panel {
      background-color: var(--bg-card);
      border: 1.5px solid var(--border-sand);
      box-shadow: 0 8px 24px rgba(47, 53, 59, 0.06);
    }

    .seal-badge {
      border: 1.5px solid var(--morandi-red);
      color: var(--morandi-red);
      background-color: var(--morandi-red-soft);
      font-family: 'Noto Serif TC', serif;
      font-weight: 700;
      letter-spacing: 0.05em;
    }

    .seal-badge-sage {
      border: 1.5px solid var(--morandi-sage);
      color: var(--morandi-sage);
      background-color: var(--morandi-sage-soft);
      font-family: 'Noto Serif TC', serif;
      font-weight: 700;
    }

    .word-chip {
      background-color: #FFFFFF;
      border: 1.5px solid #C8C0B2;
      color: var(--ink-main);
      box-shadow: 0 2px 6px rgba(47, 53, 59, 0.06);
      transition: all 0.18s ease;
      cursor: pointer;
    }
    .word-chip:hover:not(.chip-used) {
      border-color: var(--morandi-blue);
      transform: translateY(-2px);
      box-shadow: 0 4px 10px rgba(91, 112, 131, 0.15);
    }
    .word-chip.chip-selected {
      border-color: var(--morandi-blue);
      background-color: var(--morandi-blue-soft);
      box-shadow: 0 0 0 3px rgba(91, 112, 131, 0.22);
      transform: translateY(-2px);
    }
    .word-chip.chip-used {
      opacity: 0.32;
      pointer-events: none;
      background-color: #EAE5DC;
      box-shadow: none;
    }

    .blank-slot {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      min-width: 5.2rem;
      height: 2.75rem;
      padding: 0 0.75rem;
      margin: 0.15rem 0.25rem;
      border: 2px dashed #AFA696;
      background-color: #F2EFE7;
      border-radius: 0.5rem;
      vertical-align: middle;
      font-weight: 700;
      color: var(--morandi-blue);
      cursor: pointer;
      transition: all 0.2s ease;
    }
    .blank-slot:hover:not(.slot-locked),
    .blank-slot.drag-over {
      border-color: var(--morandi-blue);
      background-color: var(--morandi-blue-soft);
    }
    .blank-slot.slot-locked {
      border: 2px solid var(--morandi-sage);
      background-color: var(--morandi-sage-soft);
      color: #2E4A3B;
      cursor: default;
    }
    .blank-slot.slot-error {
      border-color: var(--morandi-red);
      background-color: var(--morandi-red-soft);
      animation: shake 0.32s ease-in-out;
    }

    .line-slot {
      min-height: 3.5rem;
      border: 2px dashed #BDB4A4;
      background-color: #F3EFE6;
      border-radius: 0.75rem;
      transition: all 0.2s ease;
      cursor: pointer;
    }
    .line-slot:hover:not(.line-locked),
    .line-slot.drag-over {
      border-color: var(--morandi-blue);
      background-color: var(--morandi-blue-soft);
    }
    .line-slot.line-filled {
      border-style: solid;
      border-color: #9FB0C0;
      background-color: #FFFFFF;
    }
    .line-slot.line-locked {
      border-style: solid;
      border-color: var(--morandi-sage);
      background-color: var(--morandi-sage-soft);
      cursor: default;
    }
    .line-slot.line-error {
      border-style: solid;
      border-color: var(--morandi-red);
      background-color: var(--morandi-red-soft);
      animation: shake 0.35s ease-in-out;
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25% { transform: translateX(-6px); }
      75% { transform: translateX(6px); }
    }
    @keyframes popIn {
      0% { transform: scale(0.94); opacity: 0.5; }
      100% { transform: scale(1); opacity: 1; }
    }
    .animate-pop {
      animation: popIn 0.25s cubic-bezier(0.16, 1, 0.3, 1);
    }
  </style>
</head>
<body class="min-h-screen flex flex-col">

  <!-- 頂部導覽列 -->
  <header class="w-full bg-[#FAF8F3]/95 border-b border-[#D6CFC2] sticky top-0 z-40 backdrop-blur-sm">
    <div class="max-w-5xl mx-auto px-4 py-3 flex flex-wrap items-center justify-between gap-3">
      <button onclick="goLobby()" class="flex items-center gap-2.5 text-left group">
        <span class="seal-badge px-2 py-0.5 rounded text-xs">唐詩修練</span>
        <span class="font-serif-tc text-xl md:text-2xl font-black tracking-wide text-[#2F353B] group-hover:text-[#5B7083] transition">
          唐詩繪卷：長安尋跡錄
        </span>
      </button>

      <div class="flex items-center gap-2 md:gap-3">
        <div class="flex items-center bg-[#F5F2EB] border border-[#D6CFC2] rounded-lg px-2.5 py-1">
          <span class="text-xs text-[#5C646C] mr-1.5 whitespace-nowrap">修練者：</span>
          <input
            id="student-info-input"
            type="text"
            placeholder="班級座號姓名"
            onchange="saveStudentInfo(this.value)"
            class="bg-transparent text-sm font-medium text-[#2F353B] w-28 md:w-36 focus:outline-none"
          />
        </div>

        <button
          id="btn-mute"
          onclick="toggleMute()"
          title="切換音效"
          class="px-3 py-1.5 rounded-lg border border-[#D6CFC2] bg-white hover:bg-[#F5F2EB] text-sm font-medium transition"
        >
          🔊 音效開
        </button>

        <button
          id="btn-home"
          onclick="goLobby()"
          class="hidden px-3 py-1.5 rounded-lg bg-[#5B7083] text-white hover:bg-[#4A5D6E] text-sm font-medium transition"
        >
          返回大廳
        </button>
      </div>
    </div>
  </header>

  <!-- 主內容容器 -->
  <main class="flex-grow max-w-5xl w-full mx-auto px-4 py-6">

    <!-- ==================== 畫面 1：修練大廳 (LOBBY) ==================== -->
    <section id="view-lobby" class="space-y-6 animate-pop">
      <div class="scroll-panel rounded-2xl p-5 md:p-6">
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 border-b border-[#E5DFD3] pb-4 mb-4">
          <div>
            <h2 class="font-serif-tc text-xl font-bold text-[#2F353B]">個人背誦修練進度總覽</h2>
            <p class="text-sm text-[#5C646C] mt-0.5">支援 Chromebook 鍵盤快捷操作與平板觸控，練習後可一鍵複製戰報繳交作業。</p>
          </div>
          <div class="flex flex-wrap gap-2">
            <button
              onclick="openReportModal()"
              class="px-4 py-2 rounded-xl bg-[#648272] hover:bg-[#526D5F] text-white text-sm font-bold shadow-sm transition flex items-center gap-1.5"
            >
              <span>📋 檢視與複製學習戰報</span>
            </button>
            <button
              onclick="confirmResetProgress()"
              class="px-3 py-2 rounded-xl border border-[#D6CFC2] hover:bg-[#F7ECEB] hover:text-[#A85854] text-[#5C646C] text-xs font-medium transition"
            >
              重置紀錄
            </button>
          </div>
        </div>

        <div class="grid grid-cols-2 md:grid-cols-4 gap-3 text-center">
          <div class="bg-[#F5F2EB] rounded-xl p-3 border border-[#E2DBD0]">
            <div class="text-xs text-[#5C646C]">模式一・詩句重組</div>
            <div id="stat-m1" class="font-serif-tc text-2xl font-black text-[#5B7083] mt-1">0 / 8 首</div>
          </div>
          <div class="bg-[#F5F2EB] rounded-xl p-3 border border-[#E2DBD0]">
            <div class="text-xs text-[#5C646C]">模式二・字詞填空</div>
            <div id="stat-m2" class="font-serif-tc text-2xl font-black text-[#648272] mt-1">0 / 8 首</div>
          </div>
          <div class="bg-[#F5F2EB] rounded-xl p-3 border border-[#E2DBD0]">
            <div class="text-xs text-[#5C646C]">飛花令・最高積分</div>
            <div id="stat-m3-score" class="font-serif-tc text-2xl font-black text-[#A85854] mt-1">0 分</div>
          </div>
          <div class="bg-[#F5F2EB] rounded-xl p-3 border border-[#E2DBD0]">
            <div class="text-xs text-[#5C646C]">飛花令・最高段位</div>
            <div id="stat-m3-rank" class="font-serif-tc text-lg font-bold text-[#B5894C] mt-1.5">尚未挑戰</div>
          </div>
        </div>
      </div>

      <!-- 三大挑戰模式入口 -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div class="scroll-panel rounded-2xl p-5 flex flex-col justify-between border-t-4 border-t-[#5B7083]">
          <div>
            <div class="flex items-center justify-between mb-2">
              <span class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-[#EBF0F5] text-[#5B7083]">初學建構期</span>
              <span class="text-xs text-[#5C646C]">脈絡與押韻</span>
            </div>
            <h3 class="font-serif-tc text-xl font-black text-[#2F353B] mb-2">模式一：詩句乾坤大挪移</h3>
            <p class="text-sm text-[#5C646C] leading-relaxed mb-4">
              將打亂的四句唐詩重新排列組合。依據起承轉合語意與二、四句押韻線索，建立全詩結構記憶。
            </p>
          </div>
          <button
            onclick="openPoemSelector(1)"
            class="w-full py-2.5 rounded-xl bg-[#5B7083] hover:bg-[#495B6B] text-white font-bold text-sm transition"
          >
            進入模式一（選擇詩卷）
          </button>
        </div>

        <div class="scroll-panel rounded-2xl p-5 flex flex-col justify-between border-t-4 border-t-[#648272]">
          <div>
            <div class="flex items-center justify-between mb-2">
              <span class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-[#EBF2EE] text-[#648272]">字詞精熟期</span>
              <span class="text-xs text-[#5C646C]">杜絕錯別字</span>
            </div>
            <h3 class="font-serif-tc text-xl font-black text-[#2F353B] mb-2">模式二：字詞尋跡拼圖</h3>
            <p class="text-sm text-[#5C646C] leading-relaxed mb-4">
              從混有「形音義誘答字」的字庫中，拖曳或點選正確關鍵詞填入空格。答錯觸發三階漸進鷹架提示。
            </p>
          </div>
          <button
            onclick="openPoemSelector(2)"
            class="w-full py-2.5 rounded-xl bg-[#648272] hover:bg-[#526B5D] text-white font-bold text-sm transition"
          >
            進入模式二（選擇詩卷）
          </button>
        </div>

        <div class="scroll-panel rounded-2xl p-5 flex flex-col justify-between border-t-4 border-t-[#A85854]">
          <div>
            <div class="flex items-center justify-between mb-2">
              <span class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-[#F7ECEB] text-[#A85854]">課後驗收期</span>
              <span class="text-xs text-[#5C646C]">60 秒個人衝榜</span>
            </div>
            <h3 class="font-serif-tc text-xl font-black text-[#2F353B] mb-2">模式三：大唐飛花令</h3>
            <p class="text-sm text-[#5C646C] leading-relaxed mb-4">
              60 秒個人限時積分賽！隨機輪播「飛花令點將、上下句接龍、辨錯字」，支援鍵盤 1～4 快速連擊衝高分。
            </p>
          </div>
          <button
            onclick="startMode3Intro()"
            class="w-full py-2.5 rounded-xl bg-[#A85854] hover:bg-[#8F4743] text-white font-bold text-sm transition"
          >
            開始 60 秒個人積分賽
          </button>
        </div>
      </div>

      <!-- 八首唐詩收錄一覽與印章牆 -->
      <div class="scroll-panel rounded-2xl p-5 md:p-6">
        <div class="flex flex-wrap items-center justify-between gap-2 mb-4">
          <h3 class="font-serif-tc text-lg font-bold text-[#2F353B]">八首必背唐詩修練印章牆</h3>
          <span class="text-xs text-[#5C646C]">可直接點擊各詩下方的「詩句重組」或「字詞填空」開始練習</span>
        </div>
        <div id="lobby-poem-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-3"></div>
      </div>
    </section>

    <!-- ==================== 畫面 2：詩卷選擇器 (MODE 1 & 2) ==================== -->
    <section id="view-selector" class="hidden space-y-5 animate-pop">
      <div class="scroll-panel rounded-2xl p-6">
        <div class="flex flex-wrap items-center justify-between gap-3 border-b border-[#E5DFD3] pb-4 mb-5">
          <div>
            <span id="selector-badge" class="px-2.5 py-0.5 rounded text-xs font-bold"></span>
            <h2 id="selector-title" class="font-serif-tc text-2xl font-black text-[#2F353B] mt-1"></h2>
            <p id="selector-desc" class="text-sm text-[#5C646C] mt-0.5"></p>
          </div>
          <button onclick="goLobby()" class="px-4 py-2 rounded-xl border border-[#D6CFC2] bg-white hover:bg-[#F5F2EB] text-sm font-medium">
            返回大廳
          </button>
        </div>
        <div id="selector-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4"></div>
      </div>
    </section>

    <!-- ==================== 畫面 3：模式一・詩句乾坤大挪移 ==================== -->
    <section id="view-mode1" class="hidden space-y-5 animate-pop">
      <div class="scroll-panel rounded-2xl p-5 md:p-8">
        <div class="flex flex-wrap items-center justify-between gap-3 border-b border-[#E5DFD3] pb-4 mb-6">
          <div class="flex items-center gap-3">
            <span class="px-2.5 py-1 rounded-lg bg-[#EBF0F5] text-[#5B7083] text-xs font-bold">模式一・詩句重組</span>
            <h2 id="m1-poem-title" class="font-serif-tc text-2xl md:text-3xl font-black text-[#2F353B]"></h2>
            <span id="m1-poem-author" class="seal-badge px-2.5 py-0.5 rounded text-sm"></span>
          </div>
          <div class="flex items-center gap-2">
            <button onclick="openPoemSelector(1)" class="px-3 py-1.5 rounded-lg border border-[#D6CFC2] bg-white text-xs font-medium hover:bg-[#F5F2EB]">
              切換詩卷
            </button>
            <button onclick="initMode1(currentPoemId)" class="px-3 py-1.5 rounded-lg border border-[#D6CFC2] bg-white text-xs font-medium hover:bg-[#F5F2EB]">
              重新洗牌
            </button>
          </div>
        </div>

        <p class="text-xs md:text-sm text-[#5C646C] mb-4 bg-[#F5F2EB] px-3.5 py-2 rounded-lg border border-[#E5DFD3]">
          💡 操作提示：可將下方詩句卡<strong>拖曳</strong>至上方詩槽，或<strong>先點擊詩句卡、再點擊目標詩槽</strong>快速放入（點擊已放入的詩句可退回）。
        </p>

        <div id="m1-slots" class="space-y-3 max-w-2xl mx-auto mb-6"></div>

        <div id="m1-scaffold-box" class="hidden max-w-2xl mx-auto mb-6 p-4 rounded-xl bg-[#F9F3E6] border border-[#DFC9A6] text-sm text-[#5E4624]">
          <div class="font-bold mb-1">📜 結構與韻腳點撥：</div>
          <div id="m1-scaffold-text"></div>
        </div>

        <div class="max-w-2xl mx-auto bg-[#F5F2EB] p-4 rounded-xl border border-[#D6CFC2]">
          <div class="text-xs font-bold text-[#5C646C] mb-2.5">待排列詩句卡：</div>
          <div id="m1-pool" class="grid grid-cols-1 sm:grid-cols-2 gap-2.5 min-h-[72px]"></div>
        </div>

        <div class="mt-6 flex justify-center gap-3">
          <button
            id="m1-btn-check"
            onclick="checkMode1Order()"
            class="px-8 py-3 rounded-xl bg-[#5B7083] hover:bg-[#495B6B] text-white font-bold text-base shadow-sm transition"
          >
            印證詩卷順序
          </button>
        </div>

        <div id="m1-clear-panel" class="hidden mt-6 max-w-2xl mx-auto p-5 rounded-2xl bg-[#EBF2EE] border-2 border-[#648272] text-center animate-pop">
          <div class="seal-badge-sage inline-block px-3 py-1 rounded text-sm mb-2">詩句順序完全正確・詩句已通！</div>
          <p id="m1-translation" class="text-sm md:text-base text-[#2F353B] leading-relaxed mb-4"></p>
          <div class="flex flex-wrap justify-center gap-3">
            <button onclick="initMode2(currentPoemId)" class="px-5 py-2.5 rounded-xl bg-[#648272] hover:bg-[#526B5D] text-white text-sm font-bold transition">
              挑戰本詩「字詞填空」→
            </button>
            <button onclick="nextPoemInMode(1)" class="px-5 py-2.5 rounded-xl bg-[#5B7083] hover:bg-[#495B6B] text-white text-sm font-bold transition">
              下一首詩卷 →
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== 畫面 4：模式二・字詞尋跡拼圖 ==================== -->
    <section id="view-mode2" class="hidden space-y-5 animate-pop">
      <div class="scroll-panel rounded-2xl p-5 md:p-8">
        <div class="flex flex-wrap items-center justify-between gap-3 border-b border-[#E5DFD3] pb-4 mb-6">
          <div class="flex items-center gap-3">
            <span class="px-2.5 py-1 rounded-lg bg-[#EBF2EE] text-[#648272] text-xs font-bold">模式二・字詞填空</span>
            <h2 id="m2-poem-title" class="font-serif-tc text-2xl md:text-3xl font-black text-[#2F353B]"></h2>
            <span id="m2-poem-author" class="seal-badge px-2.5 py-0.5 rounded text-sm"></span>
          </div>
          <div class="flex items-center gap-2">
            <span id="m2-progress-pill" class="px-3 py-1 rounded-full bg-[#F5F2EB] border border-[#D6CFC2] text-xs font-bold text-[#5C646C]">
              已填 0 / 4
            </span>
            <button onclick="openPoemSelector(2)" class="px-3 py-1.5 rounded-lg border border-[#D6CFC2] bg-white text-xs font-medium hover:bg-[#F5F2EB]">
              切換詩卷
            </button>
            <button onclick="initMode2(currentPoemId)" class="px-3 py-1.5 rounded-lg border border-[#D6CFC2] bg-white text-xs font-medium hover:bg-[#F5F2EB]">
              重新挑戰
            </button>
          </div>
        </div>

        <p class="text-xs md:text-sm text-[#5C646C] mb-5 bg-[#F5F2EB] px-3.5 py-2 rounded-lg border border-[#E5DFD3]">
          💡 操作提示：字庫中混有<strong>同音／形近誘答詞</strong>！可將字卡<strong>拖曳</strong>進空格，或<strong>先點擊字卡、再點擊目標空格</strong>填入。
        </p>

        <div class="max-w-2xl mx-auto bg-[#FAF8F3] border-2 border-[#D6CFC2] rounded-2xl p-6 md:p-8 mb-6 shadow-inner">
          <div id="m2-poem-body" class="font-serif-tc text-2xl md:text-3xl font-bold leading-[2.6] text-center tracking-wider text-[#2F353B]"></div>
        </div>

        <div id="m2-scaffold-box" class="hidden max-w-2xl mx-auto mb-6 p-4 rounded-xl bg-[#F7ECEB] border border-[#DFB8B5] animate-pop">
          <div class="flex items-center justify-between mb-1">
            <span id="m2-scaffold-level" class="text-xs font-bold px-2 py-0.5 rounded bg-[#A85854] text-white">第一階鷹架提示</span>
            <button onclick="document.getElementById('m2-scaffold-box').classList.add('hidden')" class="text-xs text-[#5C646C] hover:underline">收合提示</button>
          </div>
          <p id="m2-scaffold-text" class="text-sm md:text-base text-[#2F353B] font-medium mt-1.5 leading-relaxed"></p>
        </div>

        <div class="max-w-2xl mx-auto bg-[#F5F2EB] p-4 md:p-5 rounded-xl border border-[#D6CFC2]">
          <div class="text-xs font-bold text-[#5C646C] mb-3">備選字詞卡（內含誘答干擾項，請謹慎辨析）：</div>
          <div id="m2-word-bank" class="flex flex-wrap justify-center gap-2.5"></div>
        </div>

        <div id="m2-clear-panel" class="hidden mt-6 max-w-2xl mx-auto p-5 rounded-2xl bg-[#EBF2EE] border-2 border-[#648272] text-center animate-pop">
          <div class="seal-badge-sage inline-block px-3 py-1 rounded text-sm mb-2">字詞精準歸位・字詞已精！</div>
          <p id="m2-translation" class="text-sm md:text-base text-[#2F353B] leading-relaxed mb-4"></p>
          <div class="flex flex-wrap justify-center gap-3">
            <button onclick="nextPoemInMode(2)" class="px-6 py-2.5 rounded-xl bg-[#648272] hover:bg-[#526B5D] text-white text-sm font-bold transition">
              挑戰下一首詩卷 →
            </button>
            <button onclick="startMode3Intro()" class="px-5 py-2.5 rounded-xl bg-[#A85854] hover:bg-[#8F4743] text-white text-sm font-bold transition">
              前往大唐飛花令驗收
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== 畫面 5：模式三・大唐飛花令（60秒個人積分賽） ==================== -->
    <section id="view-mode3" class="hidden space-y-5 animate-pop">
      <div id="m3-intro" class="scroll-panel rounded-2xl p-6 md:p-8 max-w-2xl mx-auto text-center">
        <span class="seal-badge px-3 py-1 rounded text-sm">個人限時積分挑戰</span>
        <h2 class="font-serif-tc text-3xl font-black text-[#2F353B] mt-3 mb-2">大唐飛花令・60 秒衝榜賽</h2>
        <p class="text-sm text-[#5C646C] mb-6">涵蓋本單元全部八首唐詩，測試你的詩句反射速度與錯別字辨識力！</p>

        <div class="grid grid-cols-3 gap-3 bg-[#F5F2EB] p-4 rounded-xl border border-[#D6CFC2] mb-6">
          <div>
            <div class="text-xs text-[#5C646C]">個人最高積分</div>
            <div id="m3-best-score" class="font-serif-tc text-2xl font-black text-[#A85854] mt-1">0</div>
          </div>
          <div>
            <div class="text-xs text-[#5C646C]">最高連擊 Combo</div>
            <div id="m3-best-combo" class="font-serif-tc text-2xl font-black text-[#B5894C] mt-1">0</div>
          </div>
          <div>
            <div class="text-xs text-[#5C646C]">累計挑戰次數</div>
            <div id="m3-play-count" class="font-serif-tc text-2xl font-black text-[#5B7083] mt-1">0</div>
          </div>
        </div>

        <div class="text-left bg-[#FAF8F3] border border-[#E5DFD3] rounded-xl p-4 text-sm text-[#2F353B] space-y-2 mb-6">
          <div class="font-bold text-[#5B7083]">📜 競賽計分與快捷操作說明：</div>
          <ul class="list-disc list-inside space-y-1 text-[#5C646C]">
            <li><strong>三大題型隨機出題</strong>：「飛花令字點將」、「上下句閃電接龍」、「火眼金睛辨錯字」。</li>
            <li><strong>連擊倍率獎勵</strong>：3 連擊起得分 <strong>×1.5 倍</strong>；5 連擊起得分 <strong>×2.0 倍</strong>（答對不增加秒數）。</li>
            <li><strong>答錯扣時與微鷹架</strong>：每答錯一次<strong>扣 3 秒</strong>且連擊歸零，並<strong>凍結計時 2 秒</strong>顯示紅字點撥解析。</li>
            <li><strong>Chromebook 快捷鍵</strong>：可直接按鍵盤數字鍵 <kbd class="px-1.5 py-0.5 bg-white border rounded text-xs font-bold">1</kbd> <kbd class="px-1.5 py-0.5 bg-white border rounded text-xs font-bold">2</kbd> <kbd class="px-1.5 py-0.5 bg-white border rounded text-xs font-bold">3</kbd> <kbd class="px-1.5 py-0.5 bg-white border rounded text-xs font-bold">4</kbd> 快速作答。</li>
          </ul>
        </div>

        <div class="flex justify-center gap-3">
          <button onclick="goLobby()" class="px-5 py-3 rounded-xl border border-[#D6CFC2] bg-white hover:bg-[#F5F2EB] text-sm font-bold">
            返回大廳
          </button>
          <button onclick="startMode3Game()" class="px-8 py-3 rounded-xl bg-[#A85854] hover:bg-[#8F4743] text-white font-bold text-base shadow-md transition">
            敲響銅鑼・開始計時！
          </button>
        </div>
      </div>

      <div id="m3-arena" class="hidden scroll-panel rounded-2xl p-5 md:p-8 max-w-3xl mx-auto relative overflow-hidden">
        <div class="flex flex-wrap items-center justify-between gap-3 mb-3">
          <div class="flex items-center gap-2">
            <span class="text-xs font-bold text-[#5C646C]">剩餘時間</span>
            <span id="m3-timer-text" class="font-serif-tc text-2xl font-black text-[#2F353B]">60s</span>
            <span id="m3-time-delta" class="text-xs font-bold opacity-0 transition-opacity"></span>
          </div>

          <div id="m3-combo-badge" class="px-3 py-1 rounded-full bg-[#F5F2EB] border border-[#D6CFC2] text-xs font-bold text-[#5C646C]">
            連擊 0（×1.0）
          </div>

          <div class="flex items-center gap-2">
            <span class="text-xs font-bold text-[#5C646C]">本局積分</span>
            <span id="m3-score-text" class="font-serif-tc text-2xl font-black text-[#A85854]">0</span>
          </div>
        </div>

        <div class="w-full h-2.5 bg-[#E6E0D4] rounded-full overflow-hidden mb-6">
          <div id="m3-timer-bar" class="h-full bg-[#648272] transition-all duration-200" style="width: 100%"></div>
        </div>

        <div class="text-center mb-6">
          <span id="m3-q-type" class="inline-block px-3 py-1 rounded-full bg-[#EBF0F5] text-[#5B7083] text-xs font-bold mb-3"></span>
          <div id="m3-q-prompt" class="font-serif-tc text-2xl md:text-3xl font-black text-[#2F353B] leading-snug"></div>
          <div id="m3-q-sub" class="text-xs md:text-sm text-[#5C646C] mt-1.5"></div>
        </div>

        <div id="m3-options" class="grid grid-cols-1 md:grid-cols-2 gap-3"></div>

        <div id="m3-freeze-overlay" class="hidden mt-5 p-4 rounded-xl bg-[#F7ECEB] border-2 border-[#A85854] text-center animate-pop">
          <div class="text-xs font-bold text-[#A85854] mb-1">⏸ 計時凍結 2 秒・紅字點撥（扣除 3 秒）</div>
          <div id="m3-freeze-msg" class="text-sm md:text-base font-bold text-[#2F353B]"></div>
        </div>
      </div>

      <div id="m3-result" class="hidden scroll-panel rounded-2xl p-6 md:p-8 max-w-2xl mx-auto text-center animate-pop">
        <div id="m3-new-record-badge" class="hidden inline-block px-3 py-1 rounded-full bg-[#F9F3E6] border border-[#B5894C] text-[#B5894C] text-xs font-bold mb-2">
          🎉 刷新個人歷史最高紀錄！
        </div>
        <h2 class="font-serif-tc text-3xl font-black text-[#2F353B]">飛花令・個人結算戰報</h2>
        <div id="m3-final-rank" class="seal-badge inline-block px-4 py-1 rounded text-lg mt-3 mb-5"></div>

        <div class="grid grid-cols-3 gap-3 bg-[#F5F2EB] p-4 rounded-xl border border-[#D6CFC2] mb-6">
          <div>
            <div class="text-xs text-[#5C646C]">本局總積分</div>
            <div id="m3-final-score" class="font-serif-tc text-3xl font-black text-[#A85854] mt-1">0</div>
          </div>
          <div>
            <div class="text-xs text-[#5C646C]">最高連擊數</div>
            <div id="m3-final-combo" class="font-serif-tc text-3xl font-black text-[#B5894C] mt-1">0</div>
          </div>
          <div>
            <div class="text-xs text-[#5C646C]">答對題數</div>
            <div id="m3-final-correct" class="font-serif-tc text-3xl font-black text-[#648272] mt-1">0</div>
          </div>
        </div>

        <div class="text-left bg-[#FAF8F3] border border-[#D6CFC2] rounded-xl p-4 mb-6">
          <div class="text-sm font-bold text-[#2F353B] mb-2">📌 本局易錯字與詩句復盤：</div>
          <div id="m3-mistake-list" class="space-y-2 text-sm text-[#5C646C] max-h-48 overflow-y-auto"></div>
        </div>

        <div class="flex flex-wrap justify-center gap-3">
          <button onclick="startMode3Game()" class="px-6 py-2.5 rounded-xl bg-[#A85854] hover:bg-[#8F4743] text-white font-bold text-sm transition">
            再戰一局刷新紀錄
          </button>
          <button onclick="openReportModal()" class="px-5 py-2.5 rounded-xl bg-[#648272] hover:bg-[#526B5D] text-white font-bold text-sm transition">
            📋 複製學習戰報
          </button>
          <button onclick="goLobby()" class="px-5 py-2.5 rounded-xl border border-[#D6CFC2] bg-white hover:bg-[#F5F2EB] text-sm font-bold">
            返回大廳
          </button>
        </div>
      </div>
    </section>
  </main>

  <!-- ==================== 戰報匯出 Modal ==================== -->
  <div id="report-modal" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
    <div class="scroll-panel rounded-2xl max-w-lg w-full p-6 animate-pop">
      <div class="flex items-center justify-between border-b border-[#E5DFD3] pb-3 mb-4">
        <h3 class="font-serif-tc text-xl font-black text-[#2F353B]">📋 唐詩背誦修練學習戰報</h3>
        <button onclick="closeReportModal()" class="text-[#5C646C] hover:text-[#2F353B] text-lg font-bold">✕</button>
      </div>
      <p class="text-xs text-[#5C646C] mb-2">點擊下方按鈕即可複製完整戰報文字，直接貼上 Google Classroom 作業區繳交：</p>
      <textarea
        id="report-textarea"
        readonly
        rows="11"
        class="w-full p-3 rounded-xl bg-[#F5F2EB] border border-[#D6CFC2] text-xs md:text-sm font-mono text-[#2F353B] focus:outline-none mb-4"
      ></textarea>
      <div class="flex items-center justify-between gap-3">
        <span id="copy-toast" class="text-xs font-bold text-[#648272] opacity-0 transition-opacity">✅ 已成功複製到剪貼簿！</span>
        <div class="flex gap-2">
          <button onclick="closeReportModal()" class="px-4 py-2 rounded-xl border border-[#D6CFC2] bg-white text-sm font-medium">關閉</button>
          <button onclick="copyReportText()" class="px-5 py-2 rounded-xl bg-[#648272] hover:bg-[#526B5D] text-white text-sm font-bold transition">
            一鍵複製戰報
          </button>
        </div>
      </div>
    </div>
  </div>

  <script>
    // ==================== 1. 八首唐詩核心題庫資料庫 ====================
    const POEMS_DB = [
      {
        id: 1,
        title: "登鸛雀樓",
        author: "王之渙",
        lines: [
          "白日依山盡，",
          "黃河入海流。",
          "欲窮千里目，",
          "更上一層樓。"
        ],
        rhymeHint: "第二句「流」與第四句「樓」押韻；前兩句先寫登樓所見壯闊山河，後兩句抒發「欲窮千里目，更上一層樓」的進取哲理。",
        keywords: ["山", "河", "樓"],
        template: "[b0]依山盡，[b1]入海流。[b2]千里目，[b3]一層樓。",
        blanks: [
          { word: "白日", distractor: "百日", hint1: "太陽（白日），與下句「黃河」色彩相對。", hint2: "注意詩題「鸛（ㄍㄨㄢˋ）」非「觀/鶴」；首句為「白日」，非「百日」。" },
          { word: "黃河", distractor: "長江", hint1: "中國北方大河，鸛雀樓位於山西黃河畔。", hint2: "王之渙登鸛雀樓俯瞰的是「黃河」入海流，非長江。" },
          { word: "欲窮", distractor: "欲求", hint1: "想要看盡（窮：極盡，ㄑㄩㄥˊ）。", hint2: "「欲窮千里目」是想要窮盡千里遠景，不是「欲求」。" },
          { word: "更上", distractor: "往上", hint1: "再登上一層。", hint2: "「更上一層樓」強調更進一步，非口語「往上」。" }
        ],
        typoVariants: [
          { correct: "白日依山盡，黃河入海流。", wrong: "白日依山盡，長江入海流。", note: "登鸛雀樓所見為「黃河」入海流。" },
          { correct: "欲窮千里目，更上一層樓。", wrong: "欲求千里目，更上一層樓。", note: "「欲窮」意為想要看盡，非「欲求」。" }
        ],
        translation: "夕陽依傍著山巒漸漸沉落，黃河滔滔向大海奔流。想要看盡千里之外的遠景，就要再登上一層樓。"
      },
      {
        id: 2,
        title: "黃鶴樓送孟浩然之廣陵",
        author: "李白",
        lines: [
          "故人西辭黃鶴樓，",
          "煙花三月下揚州。",
          "孤帆遠影碧山盡，",
          "唯見長江天際流。"
        ],
        rhymeHint: "第一、二、四句「樓、州、流」押平聲韻；視角由近處黃鶴樓告別，一路推向遠方長江水天相接之處。",
        keywords: ["樓", "山", "江", "春天"],
        template: "故人[b0]黃鶴樓，[b1]三月下揚州。孤帆遠影[b2]盡，[b3]長江天際流。",
        blanks: [
          { word: "西辭", distractor: "東去", hint1: "向西告別友人（黃鶴樓位於揚州西邊）。", hint2: "孟浩然向東前往揚州，故兩人在西邊的黃鶴樓「西辭」。" },
          { word: "煙花", distractor: "煙火", hint1: "形容春日煙霧迷濛、繁花似錦的景致。", hint2: "唐代沒有現代「煙火」喔！這裡指春天繁花盛開的美景。" },
          { word: "碧山", distractor: "綠水", hint1: "青翠的山巒。", hint2: "船帆遠去消失在「碧山」盡頭，與下句「長江」相對。" },
          { word: "唯見", distractor: "唯建", hint1: "只看見。", hint2: "「唯見」的「見」是看見之意，常誤寫為同音字「唯建」或「唯現」。" }
        ],
        typoVariants: [
          { correct: "故人西辭黃鶴樓，煙花三月下揚州。", wrong: "故人西辭黃鶴樓，煙火三月下揚州。", note: "「煙花」指春日繁花似錦，非現代「煙火」。" },
          { correct: "孤帆遠影碧山盡，唯見長江天際流。", wrong: "孤帆遠影碧山盡，唯建長江天際流。", note: "「唯見」意為只看見，不可寫成「唯建」。" }
        ],
        translation: "老朋友向西辭別了黃鶴樓，在繁花似錦的三月順流東下揚州。孤船的帆影漸漸消失在碧山盡頭，只看見長江水向天際奔流。"
      },
      {
        id: 3,
        title: "楓橋夜泊",
        author: "張繼",
        lines: [
          "月落烏啼霜滿天，",
          "江楓漁火對愁眠。",
          "姑蘇城外寒山寺，",
          "夜半鐘聲到客船。"
        ],
        rhymeHint: "第一、二、四句「天、眠、船」押平聲韻；前兩句寫夜泊所見（月落、江楓、漁火），後兩句寫夜半所聞（寒山寺鐘聲）。",
        keywords: ["江", "夜", "山"],
        template: "月落[b0]霜滿天，江楓[b1]對[b2]。[b3]城外寒山寺，夜半鐘聲到客船。",
        blanks: [
          { word: "烏啼", distractor: "鳥鳴", hint1: "烏鴉啼叫聲。", hint2: "深秋寒夜中是「烏啼」，渲染孤寂淒清氛圍，非一般「鳥鳴」。" },
          { word: "漁火", distractor: "漁船", hint1: "江面漁船上的點點燈火。", hint2: "與江邊楓樹相映的是「漁火」，烘托旅人「愁眠」。" },
          { word: "愁眠", distractor: "安眠", hint1: "帶著羈旅愁緒難以入眠。", hint2: "詩人客居他鄉心懷憂愁，故為「對愁眠」，非「安眠」。" },
          { word: "姑蘇", distractor: "估蘇", hint1: "蘇州的古稱（ㄍㄨ ㄙㄨ）。", hint2: "「姑蘇」的「姑」為女字旁，常誤寫為人字旁的「估蘇」。" }
        ],
        typoVariants: [
          { correct: "姑蘇城外寒山寺，夜半鐘聲到客船。", wrong: "估蘇城外寒山寺，夜半鐘聲到客船。", note: "「姑蘇」的「姑」為女字旁，非「估蘇」。" },
          { correct: "月落烏啼霜滿天，江楓漁火對愁眠。", wrong: "月落鳥鳴霜滿天，江楓漁火對愁眠。", note: "首句為「烏啼」，非「鳥鳴」。" }
        ],
        translation: "月亮西落伴著烏鴉啼叫，秋霜佈滿漫天；江邊楓樹與漁火伴我帶著愁緒入眠。蘇州城外的寒山寺，半夜鐘聲傳到了客船。"
      },
      {
        id: 4,
        title: "新嫁娘",
        author: "王建",
        lines: [
          "三日入廚下，",
          "洗手作羹湯。",
          "未諳姑食性，",
          "先遣小姑嘗。"
        ],
        rhymeHint: "第二句「湯」與第四句「嘗」押韻；依「入廚 → 作羹湯 → 思量婆婆口味 → 先請小姑試味」的心理與動作順序展開。",
        keywords: ["人"],
        template: "[b0]入廚下，洗手作[b1]。[b2]姑食性，[b3]小姑嘗。",
        blanks: [
          { word: "三日", distractor: "五日", hint1: "古代婚俗，新娘過門後第三天開始下廚。", hint2: "依古禮新婚「三日」入廚下，非五日。" },
          { word: "羹湯", distractor: "更湯", hint1: "肉汁濃湯（ㄍㄥ）。", hint2: "「羹」字下方為「美」變形（羔＋美），常誤寫為「更湯」。" },
          { word: "未諳", distractor: "不熟", hint1: "還不熟悉、不了解（ㄢ）。", hint2: "「諳」為言字旁，音 ㄢ，意為熟悉、精通。" },
          { word: "先遣", distractor: "先請", hint1: "先派遣、請託（ㄑㄧㄢˇ）。", hint2: "「先遣小姑嘗」展現新嫁娘聰慧謹慎的心思。" }
        ],
        typoVariants: [
          { correct: "三日入廚下，洗手作羹湯。", wrong: "三日入廚下，洗手作更湯。", note: "「羹湯」的「羹」不可寫成「更」。" },
          { correct: "未諳姑食性，先遣小姑嘗。", wrong: "未暗姑食性，先遣小姑嘗。", note: "「未諳」意為不熟悉，為言字旁「諳」。" }
        ],
        translation: "新婚第三天走進廚房，洗手親自烹調羹湯。因為還不熟悉婆婆的飲食口味，先請小姑品嘗看看。"
      },
      {
        id: 5,
        title: "問劉十九",
        author: "白居易",
        lines: [
          "綠螘新醅酒，",
          "紅泥小火爐。",
          "晚來天欲雪，",
          "能飲一杯無？"
        ],
        rhymeHint: "第二句「爐」與第四句「無」押韻；前兩句描繪室內暖酒的綠與紅，第三句轉寫室外天色欲雪，第四句發出熱情邀約。",
        keywords: ["晚", "酒"],
        template: "[b0][b1]，[b2]小火爐。晚來天欲雪，[b3]一杯無？",
        blanks: [
          { word: "綠螘", distractor: "綠螞蟻", hint1: "新釀未濾酒面上浮起的綠色泡沫（ㄧˇ）。", hint2: "「綠螘」是酒面浮沫的美稱，不是昆蟲「綠螞蟻」喔！" },
          { word: "新醅酒", distractor: "新釀酒", hint1: "剛剛釀好、尚未過濾的米酒（ㄆㄟ）。", hint2: "「醅」為酉字旁，音 ㄆㄟ，指未濾的酒。" },
          { word: "紅泥", distractor: "黃泥", hint1: "紅泥砌成的小火爐，與「綠螘」形成暖色對比。", hint2: "詩人以「綠螘」配「紅泥」，色彩鮮明溫暖。" },
          { word: "能飲", distractor: "敢飲", hint1: "能夠來喝一杯嗎（「無」相當於疑問助詞「嗎」）。", hint2: "「能飲一杯無」語氣親切溫馨，非挑釁的「敢飲」。" }
        ],
        typoVariants: [
          { correct: "綠螘新醅酒，紅泥小火爐。", wrong: "綠螞蟻新醅酒，紅泥小火爐。", note: "「綠螘」指酒面浮沫，非「綠螞蟻」。" },
          { correct: "晚來天欲雪，能飲一杯無？", wrong: "晚來天欲雪，敢飲一杯無？", note: "末句為溫馨邀約「能飲一杯無？」。" }
        ],
        translation: "新釀的酒上浮著綠色泡沫，紅泥砌成的小火爐正暖。傍晚時分天色將要下雪，能來共飲一杯酒嗎？"
      },
      {
        id: 6,
        title: "登樂遊原",
        author: "李商隱",
        lines: [
          "向晚意不適，",
          "驅車登古原。",
          "夕陽無限好，",
          "只是近黃昏。"
        ],
        rhymeHint: "第二句「原」與第四句「昏」古韻相押；前兩句交代因心情鬱悶而駕車登高，後兩句由讚歎夕陽美景急轉為感傷時光易逝。",
        keywords: ["晚", "原"],
        template: "[b0][b1]，[b2]登古原。夕陽無限好，只是近[b3]。",
        blanks: [
          { word: "向晚", distractor: "傍晚", hint1: "將近傍晚時分。", hint2: "唐詩原文用字為「向晚」，意為接近傍晚。" },
          { word: "意不適", distractor: "意不快", hint1: "心情感到不舒暢、鬱悶。", hint2: "「向晚意不適」交代了詩人登高散心的原因。" },
          { word: "驅車", distractor: "搭車", hint1: "駕著馬車。", hint2: "古代駕馬車稱為「驅車」，非現代「搭車」。" },
          { word: "黃昏", distractor: "殘照", hint1: "太陽落山後天色漸暗的時刻。", hint2: "「夕陽無限好，只是近黃昏」為千古名句。" }
        ],
        typoVariants: [
          { correct: "向晚意不適，驅車登古原。", wrong: "傍晚意不適，驅車登古原。", note: "首句原文為「向晚意不適」。" },
          { correct: "夕陽無限好，只是近黃昏。", wrong: "夕陽無限好，只是近殘照。", note: "末句為「只是近黃昏」，與「原」押韻。" }
        ],
        translation: "將近傍晚時心情感到不舒暢，便駕車登上古老的樂遊原。夕陽的景色真是無限美好，可惜已經接近黃昏時刻。"
      },
      {
        id: 7,
        title: "尋隱者不遇",
        author: "賈島",
        lines: [
          "松下問童子，",
          "言師採藥去；",
          "只在此山中，",
          "雲深不知處。"
        ],
        rhymeHint: "第二句「去」與第四句「處」押韻；全詩採用「問童子 → 答採藥去 → 再答在此山中 → 三答雲深不知處」的對話層次。",
        keywords: ["山"],
        template: "[b0]問童子，[b1]採藥去；只在[b2]，[b3]不知處。",
        blanks: [
          { word: "松下", distractor: "樹下", hint1: "高潔的松樹之下。", hint2: "松樹象徵隱者清高品格，原文為「松下」，非泛稱「樹下」。" },
          { word: "言師", distractor: "言去", hint1: "童子回答說師父……", hint2: "「言師採藥去」省略了主語童子，意為（童子）說師父採藥去了。" },
          { word: "此山中", distractor: "這山中", hint1: "就在這座山裡面。", hint2: "文言指示代詞用「此」，即「只在此山中」。" },
          { word: "雲深", distractor: "霧大", hint1: "山中雲霧繚繞深厚。", hint2: "「雲深不知處」既寫實景，也暗喻隱者行蹤超脫塵俗。" }
        ],
        typoVariants: [
          { correct: "松下問童子，言師採藥去；", wrong: "樹下問童子，言師採藥去；", note: "首句為「松下問童子」，松象徵隱者高潔。" },
          { correct: "只在此山中，雲深不知處。", wrong: "只在此山中，霧大不知處。", note: "末句為「雲深不知處」，非口語「霧大」。" }
        ],
        translation: "在松樹下詢問隱者的童子，童子說師父上山採藥去了；只知道就在這座山中，但雲霧繚繞太深，不知道確切去處。"
      },
      {
        id: 8,
        title: "渡漢江",
        author: "李頻",
        lines: [
          "嶺外音書絕，",
          "經冬復歷春。",
          "近鄉情更怯，",
          "不敢問來人。"
        ],
        rhymeHint: "第二句「春」與第四句「人」押平聲韻；前兩句追述客居嶺外音訊斷絕之苦，後兩句刻畫返鄉近在咫尺卻膽怯的心理轉折。",
        keywords: ["江", "春天", "人"],
        template: "[b0][b1]，經冬復[b2]。近鄉[b3]，不敢問來人。",
        blanks: [
          { word: "嶺外", distractor: "窗外", hint1: "五嶺以南的偏遠地區（廣東、廣西一帶）。", hint2: "「嶺外」指五嶺之外，古代交通不便，故「音書絕」。" },
          { word: "音書絕", distractor: "音訊斷", hint1: "家書與音訊完全斷絕。", hint2: "古詩原文為「嶺外音書絕」，音書即書信消息。" },
          { word: "歷春", distractor: "迎春", hint1: "度過冬天又經歷了春天。", hint2: "「經冬復歷春」以「經」對「歷」，強調離家時間漫長。" },
          { word: "情更怯", distractor: "情更怕", hint1: "心情反而更加膽怯（ㄑㄩㄝˋ）。", hint2: "「近鄉情更怯」精準寫出怕聽聞家鄉變故的矛盾心理。" }
        ],
        typoVariants: [
          { correct: "嶺外音書絕，經冬復歷春。", wrong: "窗外音書絕，經冬復歷春。", note: "首句為「嶺外音書絕」，嶺外指五嶺以南。" },
          { correct: "近鄉情更怯，不敢問來人。", wrong: "近鄉情更怕，不敢問來人。", note: "第三句為「近鄉情更怯」，非口語「情更怕」。" }
        ],
        translation: "客居五嶺之外時與家鄉書信斷絕，度過了寒冬又經歷了春天。如今越靠近家鄉心中越發膽怯，反而不敢向迎面走來的人打聽家鄉消息。"
      }
    ];

    // ==================== 2. 狀態管理與 LocalStorage ====================
    const STORAGE_KEY = 'tang_poetry_scroll_v1';
    let appState = {
      studentInfo: '',
      mode1Cleared: {},
      mode2Cleared: {},
      poemMistakes: {},
      feihuaHighScore: 0,
      feihuaMaxCombo: 0,
      feihuaPlays: 0,
      wrongLog: []
    };

    function loadState() {
      try {
        const saved = localStorage.getItem(STORAGE_KEY);
        if (saved) {
          appState = { ...appState, ...JSON.parse(saved) };
        }
      } catch (e) {
        console.warn('讀取本機進度失敗', e);
      }
      document.getElementById('student-info-input').value = appState.studentInfo || '';
    }

    function saveState() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(appState));
      } catch (e) {
        console.warn('儲存本機進度失敗', e);
      }
    }

    function saveStudentInfo(val) {
      appState.studentInfo = val.trim();
      saveState();
    }

    function logWrongItem(text) {
      if (!appState.wrongLog.includes(text)) {
        appState.wrongLog.unshift(text);
        if (appState.wrongLog.length > 20) appState.wrongLog.pop();
        saveState();
      }
    }

    function confirmResetProgress() {
      if (confirm('確定要清除所有通關印章與飛花令最高分紀錄嗎？')) {
        appState = {
          studentInfo: appState.studentInfo,
          mode1Cleared: {},
          mode2Cleared: {},
          poemMistakes: {},
          feihuaHighScore: 0,
          feihuaMaxCombo: 0,
          feihuaPlays: 0,
          wrongLog: []
        };
        saveState();
        renderLobby();
      }
    }

    // ==================== 3. Web Audio API 合成古風音效 ====================
    let audioCtx = null;
    let isMuted = false;

    function ensureAudio() {
      if (!audioCtx) {
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        if (AudioContext) audioCtx = new AudioContext();
      }
      if (audioCtx && audioCtx.state === 'suspended') {
        audioCtx.resume();
      }
    }

    function playTone(freq, type = 'sine', duration = 0.15, vol = 0.08) {
      if (isMuted) return;
      ensureAudio();
      if (!audioCtx) return;
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = type;
      osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
      gain.gain.setValueAtTime(vol, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.005, audioCtx.currentTime + duration);
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      osc.stop(audioCtx.currentTime + duration);
    }

    const sfx = {
      click: () => playTone(520, 'sine', 0.08, 0.05),
      place: () => playTone(660, 'triangle', 0.1, 0.06),
      correct: () => {
        playTone(784, 'sine', 0.14, 0.08);
        setTimeout(() => playTone(1046.5, 'sine', 0.22, 0.08), 80);
      },
      wrong: () => {
        playTone(220, 'sawtooth', 0.18, 0.07);
        setTimeout(() => playTone(185, 'sawtooth', 0.22, 0.07), 100);
      },
      clear: () => {
        [523.25, 659.25, 783.99, 1046.5].forEach((f, idx) => {
          setTimeout(() => playTone(f, 'triangle', 0.35, 0.08), idx * 110);
        });
      }
    };

    function toggleMute() {
      isMuted = !isMuted;
      const btn = document.getElementById('btn-mute');
      btn.textContent = isMuted ? '🔇 靜音中' : '🔊 音效開';
    }

    function shuffleArray(arr) {
      const copy = [...arr];
      for (let i = copy.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [copy[i], copy[j]] = [copy[j], copy[i]];
      }
      return copy;
    }

    // ==================== 4. 畫面切換與大廳渲染 ====================
    function switchView(viewId) {
      stopMode3Timer();
      ['view-lobby', 'view-selector', 'view-mode1', 'view-mode2', 'view-mode3'].forEach(id => {
        document.getElementById(id).classList.add('hidden');
      });
      document.getElementById(viewId).classList.remove('hidden');

      const btnHome = document.getElementById('btn-home');
      if (viewId === 'view-lobby') {
        btnHome.classList.add('hidden');
      } else {
        btnHome.classList.remove('hidden');
      }
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    function goLobby() {
      sfx.click();
      switchView('view-lobby');
      renderLobby();
    }

    function getFeihuaRank(score) {
      if (score >= 1800) return '大唐詩仙';
      if (score >= 1100) return '翰林學士';
      if (score >= 500) return '國子監生';
      if (score > 0) return '詩壇新秀';
      return '尚未挑戰';
    }

    function renderLobby() {
      const m1Count = Object.keys(appState.mode1Cleared).filter(k => appState.mode1Cleared[k]).length;
      const m2Count = Object.keys(appState.mode2Cleared).filter(k => appState.mode2Cleared[k]).length;

      document.getElementById('stat-m1').textContent = `${m1Count} / 8 首`;
      document.getElementById('stat-m2').textContent = `${m2Count} / 8 首`;
      document.getElementById('stat-m3-score').textContent = `${appState.feihuaHighScore} 分`;
      document.getElementById('stat-m3-rank').textContent =
        appState.feihuaPlays > 0 ? getFeihuaRank(appState.feihuaHighScore) : '尚未挑戰';

      const grid = document.getElementById('lobby-poem-grid');
      grid.innerHTML = '';

      POEMS_DB.forEach((poem, idx) => {
        const m1Done = !!appState.mode1Cleared[poem.id];
        const m2Done = !!appState.mode2Cleared[poem.id];
        const mistakes = appState.poemMistakes[poem.id] || 0;

        const card = document.createElement('div');
        card.className = 'bg-[#FAF8F3] border border-[#D6CFC2] rounded-xl p-3.5 flex flex-col justify-between hover:border-[#5B7083] transition';
        card.innerHTML = `
          <div>
            <div class="flex items-center justify-between gap-1 mb-1">
              <span class="text-xs font-bold text-[#5C646C]">卷${idx + 1}・${poem.author}</span>
              <div class="flex items-center gap-1">
                <span class="px-1.5 py-0.5 rounded text-[11px] font-bold ${m1Done ? 'bg-[#EBF0F5] text-[#5B7083] border border-[#5B7083]' : 'bg-[#EFECE6] text-[#9A9387]'}">句通</span>
                <span class="px-1.5 py-0.5 rounded text-[11px] font-bold ${m2Done ? 'bg-[#EBF2EE] text-[#648272] border border-[#648272]' : 'bg-[#EFECE6] text-[#9A9387]'}">字精</span>
              </div>
            </div>
            <div class="font-serif-tc text-base font-black text-[#2F353B] mb-2">${poem.title}</div>
          </div>
          <div>
            <div class="text-[11px] text-[#5C646C] mb-2">累計失誤：${mistakes} 次</div>
            <div class="grid grid-cols-2 gap-1.5">
              <button onclick="initMode1(${poem.id})" class="py-1.5 rounded-lg bg-[#EBF0F5] hover:bg-[#5B7083] text-[#5B7083] hover:text-white text-xs font-bold transition">
                詩句重組
              </button>
              <button onclick="initMode2(${poem.id})" class="py-1.5 rounded-lg bg-[#EBF2EE] hover:bg-[#648272] text-[#648272] hover:text-white text-xs font-bold transition">
                字詞填空
              </button>
            </div>
          </div>
        `;
        grid.appendChild(card);
      });
    }

    function openPoemSelector(modeNum) {
      sfx.click();
      switchView('view-selector');

      const badge = document.getElementById('selector-badge');
      const title = document.getElementById('selector-title');
      const desc = document.getElementById('selector-desc');

      if (modeNum === 1) {
        badge.className = 'px-2.5 py-0.5 rounded text-xs font-bold bg-[#EBF0F5] text-[#5B7083]';
        badge.textContent = '模式一・初學建構期';
        title.textContent = '詩句乾坤大挪移 — 選擇修練詩卷';
        desc.textContent = '請點選任一首唐詩，將打亂的四句詩卡排回正確順序。';
      } else {
        badge.className = 'px-2.5 py-0.5 rounded text-xs font-bold bg-[#EBF2EE] text-[#648272]';
        badge.textContent = '模式二・字詞精熟期';
        title.textContent = '字詞尋跡拼圖 — 選擇修練詩卷';
        desc.textContent = '請點選任一首唐詩，辨析形音義誘答詞並完成關鍵字填空。';
      }

      const grid = document.getElementById('selector-grid');
      grid.innerHTML = '';

      POEMS_DB.forEach((poem, idx) => {
        const cleared = modeNum === 1 ? !!appState.mode1Cleared[poem.id] : !!appState.mode2Cleared[poem.id];
        const btn = document.createElement('button');
        btn.className = `text-left p-4 rounded-xl border-2 transition transform hover:-translate-y-0.5 ${
          cleared
            ? 'bg-[#EBF2EE]/70 border-[#648272]'
            : 'bg-white border-[#D6CFC2] hover:border-[#5B7083]'
        }`;
        btn.innerHTML = `
          <div class="flex items-center justify-between mb-1.5">
            <span class="text-xs font-bold text-[#5C646C]">卷 ${idx + 1}・${poem.author}</span>
            ${cleared ? '<span class="seal-badge-sage px-2 py-0.5 rounded text-[11px]">已通關</span>' : '<span class="text-[11px] text-[#8E877B]">待修練</span>'}
          </div>
          <div class="font-serif-tc text-lg font-black text-[#2F353B]">${poem.title}</div>
          <div class="text-xs text-[#5C646C] mt-1 truncate">${poem.lines[0]}${poem.lines[1]}</div>
        `;
        btn.onclick = () => (modeNum === 1 ? initMode1(poem.id) : initMode2(poem.id));
        grid.appendChild(btn);
      });
    }

    // ==================== 5. 模式一：詩句乾坤大挪移 ====================
    let currentPoemId = 1;
    let m1Slots = [null, null, null, null];
    let m1Locked = [false, false, false, false];
    let m1PoolOrder = [0, 1, 2, 3];
    let m1SelectedLineIdx = null;
    let m1DraggedLineIdx = null;

    function initMode1(poemId) {
      sfx.click();
      currentPoemId = poemId;
      m1Slots = [null, null, null, null];
      m1Locked = [false, false, false, false];
      m1SelectedLineIdx = null;
      m1DraggedLineIdx = null;

      do {
        m1PoolOrder = shuffleArray([0, 1, 2, 3]);
      } while (m1PoolOrder.every((v, i) => v === i));

      const poem = POEMS_DB.find(p => p.id === poemId);
      document.getElementById('m1-poem-title').textContent = poem.title;
      document.getElementById('m1-poem-author').textContent = poem.author;
      document.getElementById('m1-scaffold-box').classList.add('hidden');
      document.getElementById('m1-clear-panel').classList.add('hidden');
      document.getElementById('m1-btn-check').classList.remove('hidden');

      switchView('view-mode1');
      renderMode1();
    }

    function renderMode1() {
      const poem = POEMS_DB.find(p => p.id === currentPoemId);
      const slotLabels = ['第一句（起）', '第二句（承・韻）', '第三句（轉）', '第四句（合・韻）'];
      const slotsContainer = document.getElementById('m1-slots');
      slotsContainer.innerHTML = '';

      for (let slotIdx = 0; slotIdx < 4; slotIdx++) {
        const lineIdx = m1Slots[slotIdx];
        const locked = m1Locked[slotIdx];

        const slotEl = document.createElement('div');
        slotEl.className = `line-slot px-4 py-3 flex items-center justify-between gap-3 ${
          locked ? 'line-locked' : lineIdx !== null ? 'line-filled' : ''
        }`;
        slotEl.dataset.slotIdx = slotIdx;

        if (lineIdx !== null) {
          slotEl.innerHTML = `
            <div class="flex items-center gap-3">
              <span class="text-xs font-bold px-2 py-0.5 rounded ${locked ? 'bg-[#648272] text-white' : 'bg-[#EBF0F5] text-[#5B7083]'}">${slotLabels[slotIdx]}</span>
              <span class="font-serif-tc text-xl md:text-2xl font-bold text-[#2F353B]">${poem.lines[lineIdx]}</span>
            </div>
            <span class="text-xs font-bold ${locked ? 'text-[#648272]' : 'text-[#8E877B]'}">${locked ? '✓ 已鎖定' : '點擊退回'}</span>
          `;
        } else {
          slotEl.innerHTML = `
            <div class="flex items-center gap-3">
              <span class="text-xs font-bold px-2 py-0.5 rounded bg-[#E6E0D4] text-[#5C646C]">${slotLabels[slotIdx]}</span>
              <span class="text-sm text-[#8E877B]">點擊或拖曳詩句至此處</span>
            </div>
          `;
        }

        slotEl.onclick = () => {
          if (locked) return;
          if (m1SelectedLineIdx !== null) {
            placeLineInSlot(slotIdx, m1SelectedLineIdx);
          } else if (lineIdx !== null) {
            sfx.click();
            m1Slots[slotIdx] = null;
            renderMode1();
          }
        };

        slotEl.ondragover = e => {
          e.preventDefault();
          if (!locked) slotEl.classList.add('drag-over');
        };
        slotEl.ondragleave = () => slotEl.classList.remove('drag-over');
        slotEl.ondrop = e => {
          e.preventDefault();
          slotEl.classList.remove('drag-over');
          if (!locked && m1DraggedLineIdx !== null) {
            placeLineInSlot(slotIdx, m1DraggedLineIdx);
            m1DraggedLineIdx = null;
          }
        };

        slotsContainer.appendChild(slotEl);
      }

      const poolContainer = document.getElementById('m1-pool');
      poolContainer.innerHTML = '';

      m1PoolOrder.forEach(lineIdx => {
        const isUsed = m1Slots.includes(lineIdx);
        const isSelected = m1SelectedLineIdx === lineIdx;

        const chip = document.createElement('div');
        chip.className = `word-chip px-4 py-3 rounded-xl font-serif-tc text-lg md:text-xl font-bold text-center ${
          isUsed ? 'chip-used' : ''
        } ${isSelected ? 'chip-selected' : ''}`;
        chip.textContent = poem.lines[lineIdx];
        chip.draggable = !isUsed;

        chip.onclick = () => {
          if (isUsed) return;
          sfx.click();
          m1SelectedLineIdx = m1SelectedLineIdx === lineIdx ? null : lineIdx;
          renderMode1();
        };

        chip.ondragstart = () => {
          if (isUsed) return;
          m1DraggedLineIdx = lineIdx;
          m1SelectedLineIdx = null;
          sfx.click();
        };
        chip.ondragend = () => {
          m1DraggedLineIdx = null;
        };

        poolContainer.appendChild(chip);
      });
    }

    function placeLineInSlot(slotIdx, lineIdx) {
      if (m1Locked[slotIdx]) return;
      for (let i = 0; i < 4; i++) {
        if (m1Slots[i] === lineIdx && !m1Locked[i]) {
          m1Slots[i] = null;
        }
      }
      sfx.place();
      m1Slots[slotIdx] = lineIdx;
      m1SelectedLineIdx = null;
      renderMode1();
    }

    function checkMode1Order() {
      const poem = POEMS_DB.find(p => p.id === currentPoemId);
      if (m1Slots.includes(null)) {
        sfx.wrong();
        const box = document.getElementById('m1-scaffold-box');
        document.getElementById('m1-scaffold-text').textContent = '請先將四張詩句卡都排入上方詩槽中，再點擊「印證詩卷順序」！';
        box.classList.remove('hidden');
        return;
      }

      let allCorrect = true;
      const wrongSlotIndices = [];

      for (let i = 0; i < 4; i++) {
        if (m1Slots[i] === i) {
          m1Locked[i] = true;
        } else {
          allCorrect = false;
          wrongSlotIndices.push(i);
        }
      }

      if (allCorrect) {
        sfx.clear();
        appState.mode1Cleared[poem.id] = true;
        saveState();
        renderMode1();
        document.getElementById('m1-scaffold-box').classList.add('hidden');
        document.getElementById('m1-btn-check').classList.add('hidden');
        document.getElementById('m1-translation').textContent = `【全詩語譯】${poem.translation}`;
        document.getElementById('m1-clear-panel').classList.remove('hidden');
      } else {
        sfx.wrong();
        appState.poemMistakes[poem.id] = (appState.poemMistakes[poem.id] || 0) + 1;
        saveState();
        renderMode1();

        const slotEls = document.getElementById('m1-slots').children;
        wrongSlotIndices.forEach(idx => {
          if (slotEls[idx]) slotEls[idx].classList.add('line-error');
        });

        document.getElementById('m1-scaffold-text').textContent =
          `綠色框代表位置正確已鎖定；紅色框順序有誤，已為你退回待選區。提示：${poem.rhymeHint}`;
        document.getElementById('m1-scaffold-box').classList.remove('hidden');

        setTimeout(() => {
          wrongSlotIndices.forEach(idx => {
            m1Slots[idx] = null;
          });
          renderMode1();
        }, 550);
      }
    }

    // ==================== 6. 模式二：字詞尋跡拼圖 ====================
    let m2Filled = [false, false, false, false];
    let m2MistakeCount = 0;
    let m2SelectedChipId = null;
    let m2DraggedChipId = null;
    let m2Chips = [];

    function initMode2(poemId) {
      sfx.click();
      currentPoemId = poemId;
      m2Filled = [false, false, false, false];
      m2MistakeCount = 0;
      m2SelectedChipId = null;
      m2DraggedChipId = null;

      const poem = POEMS_DB.find(p => p.id === poemId);
      const rawChips = [];
      poem.blanks.forEach((b, idx) => {
        rawChips.push({ id: `c_${idx}`, text: b.word, used: false });
        rawChips.push({ id: `d_${idx}`, text: b.distractor, used: false });
      });
      m2Chips = shuffleArray(rawChips);

      document.getElementById('m2-poem-title').textContent = poem.title;
      document.getElementById('m2-poem-author').textContent = poem.author;
      document.getElementById('m2-scaffold-box').classList.add('hidden');
      document.getElementById('m2-clear-panel').classList.add('hidden');

      switchView('view-mode2');
      renderMode2();
    }

    function renderMode2() {
      const poem = POEMS_DB.find(p => p.id === currentPoemId);
      const filledCount = m2Filled.filter(Boolean).length;
      document.getElementById('m2-progress-pill').textContent = `已填 ${filledCount} / 4`;

      const bodyEl = document.getElementById('m2-poem-body');
      bodyEl.innerHTML = '';

      const tokens = poem.template.split(/(\[b\d+\])/g);
      tokens.forEach(token => {
        if (token.startsWith('[b') && token.endsWith(']')) {
          const bIdx = parseInt(token.slice(2, -1), 10);
          const slot = document.createElement('span');
          slot.className = `blank-slot ${m2Filled[bIdx] ? 'slot-locked' : ''}`;
          slot.dataset.blankIdx = bIdx;
          slot.textContent = m2Filled[bIdx] ? poem.blanks[bIdx].word : `第 ${bIdx + 1} 格`;

          if (!m2Filled[bIdx]) {
            slot.onclick = () => {
              if (m2SelectedChipId) {
                attemptMode2Place(bIdx, m2SelectedChipId);
              } else {
                sfx.click();
              }
            };
            slot.ondragover = e => {
              e.preventDefault();
              slot.classList.add('drag-over');
            };
            slot.ondragleave = () => slot.classList.remove('drag-over');
            slot.ondrop = e => {
              e.preventDefault();
              slot.classList.remove('drag-over');
              if (m2DraggedChipId) {
                attemptMode2Place(bIdx, m2DraggedChipId);
                m2DraggedChipId = null;
              }
            };
          }
          bodyEl.appendChild(slot);
        } else if (token) {
          const parts = token.split(/(。|；)/g);
          parts.forEach((p, pIdx) => {
            if (!p) return;
            const span = document.createElement('span');
            span.textContent = p;
            bodyEl.appendChild(span);
            if ((p === '。' || p === '；') && pIdx < parts.length - 1) {
              bodyEl.appendChild(document.createElement('br'));
            }
          });
        }
      });

      const bankEl = document.getElementById('m2-word-bank');
      bankEl.innerHTML = '';

      m2Chips.forEach(chip => {
        const isSelected = m2SelectedChipId === chip.id;
        const btn = document.createElement('button');
        btn.className = `word-chip px-4 py-2.5 rounded-xl font-serif-tc text-xl font-bold ${
          chip.used ? 'chip-used' : ''
        } ${isSelected ? 'chip-selected' : ''}`;
        btn.textContent = chip.text;
        btn.draggable = !chip.used;

        btn.onclick = () => {
          if (chip.used) return;
          sfx.click();
          m2SelectedChipId = m2SelectedChipId === chip.id ? null : chip.id;
          renderMode2();
        };

        btn.ondragstart = () => {
          if (chip.used) return;
          m2DraggedChipId = chip.id;
          m2SelectedChipId = null;
          sfx.click();
        };
        btn.ondragend = () => {
          m2DraggedChipId = null;
        };

        bankEl.appendChild(btn);
      });
    }

    function attemptMode2Place(blankIdx, chipId) {
      if (m2Filled[blankIdx]) return;
      const poem = POEMS_DB.find(p => p.id === currentPoemId);
      const chip = m2Chips.find(c => c.id === chipId);
      const targetBlank = poem.blanks[blankIdx];

      if (!chip || chip.used) return;

      if (chip.text === targetBlank.word) {
        sfx.correct();
        m2Filled[blankIdx] = true;
        chip.used = true;
        m2SelectedChipId = null;
        renderMode2();

        if (m2Filled.every(Boolean)) {
          sfx.clear();
          appState.mode2Cleared[poem.id] = true;
          saveState();
          document.getElementById('m2-scaffold-box').classList.add('hidden');
          document.getElementById('m2-translation').textContent = `【全詩語譯】${poem.translation}`;
          document.getElementById('m2-clear-panel').classList.remove('hidden');
        }
      } else {
        sfx.wrong();
        m2MistakeCount++;
        appState.poemMistakes[poem.id] = (appState.poemMistakes[poem.id] || 0) + 1;
        logWrongItem(`《${poem.title}》填空誤選「${chip.text}」（正解：${targetBlank.word}）`);
        m2SelectedChipId = null;
        renderMode2();

        const slotEl = document.querySelector(`.blank-slot[data-blank-idx="${blankIdx}"]`);
        if (slotEl) {
          slotEl.classList.add('slot-error');
          setTimeout(() => slotEl.classList.remove('slot-error'), 400);
        }

        const scaffoldBox = document.getElementById('m2-scaffold-box');
        const levelBadge = document.getElementById('m2-scaffold-level');
        const scaffoldText = document.getElementById('m2-scaffold-text');

        if (m2MistakeCount === 1) {
          levelBadge.textContent = '第一階鷹架・詞義線索';
          scaffoldText.textContent = `第 ${blankIdx + 1} 格提示：${targetBlank.hint1}`;
        } else if (m2MistakeCount === 2) {
          levelBadge.textContent = '第二階鷹架・迷思辨析';
          scaffoldText.textContent = `第 ${blankIdx + 1} 格辨析：${targetBlank.hint2}`;
        } else {
          levelBadge.textContent = '第三階鷹架・全詩語譯輔助';
          scaffoldText.textContent = `請對照語譯推敲第 ${blankIdx + 1} 格用字：「${poem.translation}」`;
        }
        scaffoldBox.classList.remove('hidden');
      }
    }

    function nextPoemInMode(modeNum) {
      const nextId = currentPoemId >= POEMS_DB.length ? 1 : currentPoemId + 1;
      if (modeNum === 1) initMode1(nextId);
      else initMode2(nextId);
    }

    // ==================== 7. 模式三：大唐飛花令（60秒個人積分賽） ====================
    const FEIHUA_DURATION = 60;
    let m3Active = false;
    let m3Frozen = false;
    let m3TimeLeft = FEIHUA_DURATION;
    let m3Score = 0;
    let m3Combo = 0;
    let m3MaxCombo = 0;
    let m3CorrectCount = 0;
    let m3SessionMistakes = [];
    let m3TimerInterval = null;
    let m3FreezeTimeout = null;
    let m3CurrentQ = null;
    let m3LastPrompt = '';

    function stopMode3Timer() {
      m3Active = false;
      m3Frozen = false;
      if (m3TimerInterval) {
        clearInterval(m3TimerInterval);
        m3TimerInterval = null;
      }
      if (m3FreezeTimeout) {
        clearTimeout(m3FreezeTimeout);
        m3FreezeTimeout = null;
      }
    }

    function startMode3Intro() {
      sfx.click();
      switchView('view-mode3');
      document.getElementById('m3-intro').classList.remove('hidden');
      document.getElementById('m3-arena').classList.add('hidden');
      document.getElementById('m3-result').classList.add('hidden');

      document.getElementById('m3-best-score').textContent = appState.feihuaHighScore;
      document.getElementById('m3-best-combo').textContent = appState.feihuaMaxCombo;
      document.getElementById('m3-play-count').textContent = appState.feihuaPlays;
    }

    function startMode3Game() {
      sfx.clear();
      stopMode3Timer();

      m3Active = true;
      m3Frozen = false;
      m3TimeLeft = FEIHUA_DURATION;
      m3Score = 0;
      m3Combo = 0;
      m3MaxCombo = 0;
      m3CorrectCount = 0;
      m3SessionMistakes = [];
      m3LastPrompt = '';

      document.getElementById('m3-intro').classList.add('hidden');
      document.getElementById('m3-result').classList.add('hidden');
      document.getElementById('m3-arena').classList.remove('hidden');
      document.getElementById('m3-freeze-overlay').classList.add('hidden');

      updateMode3HUD();
      nextMode3Question();

      m3TimerInterval = setInterval(() => {
        if (!m3Active || m3Frozen) return;
        m3TimeLeft--;
        updateMode3HUD();
        if (m3TimeLeft <= 0) {
          endMode3Game();
        }
      }, 1000);
    }

    function updateMode3HUD() {
      document.getElementById('m3-timer-text').textContent = `${m3TimeLeft}s`;
      document.getElementById('m3-score-text').textContent = m3Score;

      const pct = Math.max(0, Math.min(100, (m3TimeLeft / FEIHUA_DURATION) * 100));
      const bar = document.getElementById('m3-timer-bar');
      bar.style.width = `${pct}%`;
      bar.className = `h-full transition-all duration-200 ${
        m3TimeLeft <= 12 ? 'bg-[#A85854]' : m3TimeLeft <= 25 ? 'bg-[#B5894C]' : 'bg-[#648272]'
      }`;

      const badge = document.getElementById('m3-combo-badge');
      if (m3Combo >= 5) {
        badge.className = 'px-3 py-1 rounded-full bg-[#F9F3E6] border border-[#B5894C] text-xs font-bold text-[#B5894C]';
        badge.textContent = `🔥 詩仙附體 ${m3Combo} 連擊（×2.0）`;
      } else if (m3Combo >= 3) {
        badge.className = 'px-3 py-1 rounded-full bg-[#EBF0F5] border border-[#5B7083] text-xs font-bold text-[#5B7083]';
        badge.textContent = `✨ 文思泉湧 ${m3Combo} 連擊（×1.5）`;
      } else {
        badge.className = 'px-3 py-1 rounded-full bg-[#F5F2EB] border border-[#D6CFC2] text-xs font-bold text-[#5C646C]';
        badge.textContent = `連擊 ${m3Combo}（×1.0）`;
      }
    }

    function showTimeDelta(text) {
      const el = document.getElementById('m3-time-delta');
      el.textContent = text;
      el.className = 'text-xs font-bold transition-opacity text-[#A85854]';
      el.style.opacity = '1';
      setTimeout(() => {
        el.style.opacity = '0';
      }, 900);
    }

    function generateFeihuaQuestion() {
      let qObj = null;
      let guard = 0;

      while ((!qObj || qObj.prompt === m3LastPrompt) && guard < 12) {
        guard++;
        const qType = Math.floor(Math.random() * 3);

        if (qType === 0) {
          // 題型一：飛花令字點將（含「春天」主題與指定混淆詩句）
          const kwPool = ['春天', '山', '江', '樓', '晚', '人'];
          const kw = kwPool[Math.floor(Math.random() * kwPool.length)];

          if (kw === '春天') {
            const springCorrectPool = [
              { line: "煙花三月下揚州。", poem: POEMS_DB[1] },
              { line: "經冬復歷春。", poem: POEMS_DB[7] }
            ];
            const chosen = springCorrectPool[Math.floor(Math.random() * springCorrectPool.length)];
            // 指定秋夜混淆詩句：「江楓漁火對愁眠」、「月落烏啼霜滿天」
            const fixedConfusers = ["江楓漁火對愁眠。", "月落烏啼霜滿天。"];
            const extraNonSpring = shuffleArray([
              "晚來天欲雪，",
              "白日依山盡，",
              "夕陽無限好，",
              "松下問童子，"
            ])[0];

            const options = shuffleArray([chosen.line, ...fixedConfusers, extraNonSpring]);
            qObj = {
              typeLabel: '題型一・飛花令季節點將',
              prompt: '飛花令：【春天】',
              sub: '請選出描寫「春天」景致或時節的詩句（留意秋夜混淆項）',
              options,
              correctIndex: options.indexOf(chosen.line),
              explanation: `正解：「${chosen.line}」（出自${chosen.poem.author}《${chosen.poem.title}》）。註：「江楓漁火對愁眠」、「月落烏啼霜滿天」描寫的是秋夜景色。`
            };
          } else {
            const matching = [];
            const nonMatching = [];

            POEMS_DB.forEach(p => {
              p.lines.forEach(line => {
                if (line.includes(kw)) matching.push({ line, poem: p });
                else nonMatching.push(line);
              });
            });

            const chosen = matching[Math.floor(Math.random() * matching.length)];
            const distractors = shuffleArray(nonMatching).slice(0, 3);
            const options = shuffleArray([chosen.line, ...distractors]);

            qObj = {
              typeLabel: '題型一・飛花令字點將',
              prompt: `飛花令：【${kw}】`,
              sub: `請選出出自本單元八首唐詩，且含有「${kw}」字的詩句`,
              options,
              correctIndex: options.indexOf(chosen.line),
              explanation: `正解：「${chosen.line}」（出自${chosen.poem.author}《${chosen.poem.title}》）`
            };
          }
        } else if (qType === 1) {
          // 題型二：上下句閃電接龍
          const poem = POEMS_DB[Math.floor(Math.random() * POEMS_DB.length)];
          const lineIdx = Math.floor(Math.random() * 3);
          const promptLine = poem.lines[lineIdx];
          const nextLine = poem.lines[lineIdx + 1];

          const otherLines = [];
          POEMS_DB.forEach(p => {
            p.lines.forEach(l => {
              if (l !== nextLine && l !== promptLine) otherLines.push(l);
            });
          });

          const distractors = shuffleArray(otherLines).slice(0, 3);
          const options = shuffleArray([nextLine, ...distractors]);

          qObj = {
            typeLabel: '題型二・上下句閃電接龍',
            prompt: `「${promptLine}」`,
            sub: `請選出緊接在後的下一句詩文（${poem.author}《${poem.title}》）`,
            options,
            correctIndex: options.indexOf(nextLine),
            explanation: `正解：「${promptLine}${nextLine}」（出自《${poem.title}》）`
          };
        } else {
          // 題型三：火眼金睛辨錯字
          const poem = POEMS_DB[Math.floor(Math.random() * POEMS_DB.length)];
          const variant = poem.typoVariants[Math.floor(Math.random() * poem.typoVariants.length)];
          const extraTypos = buildExtraTypos(variant.correct, variant.wrong);
          const options = shuffleArray([variant.correct, variant.wrong, ...extraTypos]);

          qObj = {
            typeLabel: '題型三・火眼金睛辨錯字',
            prompt: '請選出字形完全正確無誤的一聯詩句',
            sub: `出自 ${poem.author}《${poem.title}》`,
            options,
            correctIndex: options.indexOf(variant.correct),
            explanation: `${variant.note} 正確寫法：「${variant.correct}」`
          };
        }
      }

      m3LastPrompt = qObj.prompt;
      return qObj;
    }

    function buildExtraTypos(correctStr, existingWrong) {
      const swapRules = [
        ['白日', '百日'], ['依山', '依仙'], ['千里', '千裡'], ['一層', '一曾'],
        ['黃鶴樓', '黃鵠樓'], ['揚州', '楊州'], ['孤帆', '孤船'], ['天際', '天際邊'],
        ['霜滿天', '雙滿天'], ['愁眠', '愁眼'], ['寒山寺', '韓山寺'], ['客船', '客傳'],
        ['廚下', '櫥下'], ['洗手', '先手'], ['食性', '食姓'], ['小姑', '小姑娘'],
        ['小火爐', '小火盧'], ['天欲雪', '天欲雨'], ['一杯', '一柸'], ['新醅', '新陪'],
        ['古原', '古園'], ['夕陽', '西陽'], ['無限', '無現'], ['只是', '祇是'],
        ['童子', '瞳子'], ['採藥', '采藥'], ['此山', '此仙'], ['不知處', '不知去'],
        ['經冬', '驚冬'], ['歷春', '勵春'], ['近鄉', '進鄉'], ['不敢', '不感']
      ];

      const results = [];
      for (const [from, to] of swapRules) {
        if (correctStr.includes(from)) {
          const candidate = correctStr.replace(from, to);
          if (candidate !== correctStr && candidate !== existingWrong && !results.includes(candidate)) {
            results.push(candidate);
            if (results.length === 2) break;
          }
        }
      }

      if (results.length < 2) {
        for (const p of POEMS_DB) {
          const couplet = p.lines[0] + p.lines[1];
          if (couplet !== correctStr && couplet !== existingWrong && !results.includes(couplet)) {
            results.push(couplet);
            if (results.length === 2) break;
          }
        }
      }
      return results.slice(0, 2);
    }

    function nextMode3Question() {
      if (!m3Active) return;
      m3CurrentQ = generateFeihuaQuestion();

      document.getElementById('m3-q-type').textContent = m3CurrentQ.typeLabel;
      document.getElementById('m3-q-prompt').textContent = m3CurrentQ.prompt;
      document.getElementById('m3-q-sub').textContent = m3CurrentQ.sub;

      const optContainer = document.getElementById('m3-options');
      optContainer.innerHTML = '';

      m3CurrentQ.options.forEach((optText, idx) => {
        const btn = document.createElement('button');
        btn.className = 'p-4 rounded-xl bg-white border-2 border-[#D6CFC2] hover:border-[#5B7083] hover:bg-[#EBF0F5]/50 text-left flex items-center gap-3 transition shadow-sm';
        btn.innerHTML = `
          <span class="w-7 h-7 rounded-lg bg-[#F5F2EB] border border-[#D6CFC2] flex items-center justify-center text-xs font-black text-[#5C646C] shrink-0">${idx + 1}</span>
          <span class="font-serif-tc text-lg md:text-xl font-bold text-[#2F353B]">${optText}</span>
        `;
        btn.onclick = () => answerMode3(idx);
        optContainer.appendChild(btn);
      });
    }

    function answerMode3(optIdx) {
      if (!m3Active || m3Frozen || !m3CurrentQ) return;

      const optBtns = document.getElementById('m3-options').children;
      const isCorrect = optIdx === m3CurrentQ.correctIndex;

      if (isCorrect) {
        sfx.correct();
        m3Combo++;
        if (m3Combo > m3MaxCombo) m3MaxCombo = m3Combo;
        m3CorrectCount++;

        // 依連擊倍率加分，按對不增加秒數
        const mult = m3Combo >= 5 ? 2.0 : m3Combo >= 3 ? 1.5 : 1.0;
        m3Score += Math.round(100 * mult);

        if (optBtns[optIdx]) {
          optBtns[optIdx].classList.remove('border-[#D6CFC2]', 'bg-white');
          optBtns[optIdx].classList.add('border-[#648272]', 'bg-[#EBF2EE]');
        }

        updateMode3HUD();
        m3Frozen = true;
        setTimeout(() => {
          m3Frozen = false;
          if (m3Active) nextMode3Question();
        }, 180);
      } else {
        sfx.wrong();
        m3Combo = 0;
        // 按錯每一次扣 3 秒
        m3TimeLeft = Math.max(0, m3TimeLeft - 3);
        showTimeDelta('-3s');

        if (optBtns[optIdx]) {
          optBtns[optIdx].classList.remove('border-[#D6CFC2]', 'bg-white');
          optBtns[optIdx].classList.add('border-[#A85854]', 'bg-[#F7ECEB]');
        }
        if (optBtns[m3CurrentQ.correctIndex]) {
          optBtns[m3CurrentQ.correctIndex].classList.remove('border-[#D6CFC2]', 'bg-white');
          optBtns[m3CurrentQ.correctIndex].classList.add('border-[#648272]', 'bg-[#EBF2EE]');
        }

        m3SessionMistakes.push(m3CurrentQ.explanation);
        logWrongItem(m3CurrentQ.explanation);
        updateMode3HUD();

        m3Frozen = true;
        document.getElementById('m3-freeze-msg').textContent = m3CurrentQ.explanation;
        document.getElementById('m3-freeze-overlay').classList.remove('hidden');

        m3FreezeTimeout = setTimeout(() => {
          m3Frozen = false;
          document.getElementById('m3-freeze-overlay').classList.add('hidden');
          if (m3TimeLeft <= 0) {
            endMode3Game();
          } else if (m3Active) {
            nextMode3Question();
          }
        }, 2000);
      }
    }

    function endMode3Game() {
      stopMode3Timer();
      sfx.clear();

      const isNewRecord = m3Score > appState.feihuaHighScore && m3Score > 0;
      if (m3Score > appState.feihuaHighScore) appState.feihuaHighScore = m3Score;
      if (m3MaxCombo > appState.feihuaMaxCombo) appState.feihuaMaxCombo = m3MaxCombo;
      appState.feihuaPlays++;
      saveState();

      document.getElementById('m3-arena').classList.add('hidden');
      document.getElementById('m3-result').classList.remove('hidden');

      const recordBadge = document.getElementById('m3-new-record-badge');
      if (isNewRecord) recordBadge.classList.remove('hidden');
      else recordBadge.classList.add('hidden');

      document.getElementById('m3-final-rank').textContent = `獲得稱號：${getFeihuaRank(m3Score)}`;
      document.getElementById('m3-final-score').textContent = m3Score;
      document.getElementById('m3-final-combo').textContent = m3MaxCombo;
      document.getElementById('m3-final-correct').textContent = `${m3CorrectCount} 題`;

      const mistakeListEl = document.getElementById('m3-mistake-list');
      if (m3SessionMistakes.length === 0) {
        mistakeListEl.innerHTML = '<div class="text-[#648272] font-bold">🎉 本局零失誤全對，對八首唐詩掌握度極佳！</div>';
      } else {
        const uniqueMistakes = [...new Set(m3SessionMistakes)];
        mistakeListEl.innerHTML = uniqueMistakes
          .map(m => `<div class="p-2 rounded-lg bg-[#F7ECEB]/70 border border-[#E5C9C7] text-[#2F353B]">• ${m}</div>`)
          .join('');
      }
    }

    // Chromebook 鍵盤 1~4 快捷作答監聽
    document.addEventListener('keydown', e => {
      if (document.activeElement && document.activeElement.tagName === 'INPUT') return;
      const arenaVisible = !document.getElementById('m3-arena').classList.contains('hidden');
      if (arenaVisible && m3Active && !m3Frozen && ['1', '2', '3', '4'].includes(e.key)) {
        e.preventDefault();
        answerMode3(parseInt(e.key, 10) - 1);
      }
    });

    // ==================== 8. 學習戰報匯出 Modal ====================
    function openReportModal() {
      sfx.click();
      const student = appState.studentInfo || '（未填寫班級座號姓名）';
      const now = new Date();
      const dateStr = `${now.getFullYear()}/${String(now.getMonth() + 1).padStart(2, '0')}/${String(now.getDate()).padStart(2, '0')} ${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}`;

      const m1List = POEMS_DB.filter(p => appState.mode1Cleared[p.id]).map(p => p.title);
      const m2List = POEMS_DB.filter(p => appState.mode2Cleared[p.id]).map(p => p.title);
      const recentWrongs = appState.wrongLog.slice(0, 5);

      const reportLines = [
        '【唐詩繪卷：長安尋跡錄 — 個人背誦修練戰報】',
        `修練者：${student}`,
        `產出時間：${dateStr}`,
        '----------------------------------------',
        `1. 模式一（詩句重組）通關：${m1List.length} / 8 首`,
        `   已通關詩作：${m1List.length > 0 ? m1List.join('、') : '尚未完成'}`,
        `2. 模式二（字詞填空）通關：${m2List.length} / 8 首`,
        `   已通關詩作：${m2List.length > 0 ? m2List.join('、') : '尚未完成'}`,
        `3. 模式三（大唐飛花令・60秒賽）個人紀錄：`,
        `   最高積分：${appState.feihuaHighScore} 分｜最高連擊：${appState.feihuaMaxCombo} Combo｜段位：${getFeihuaRank(appState.feihuaHighScore)}（累計挑戰 ${appState.feihuaPlays} 次）`,
        '----------------------------------------',
        '📌 近期易錯字復盤提示：',
        ...(recentWrongs.length > 0 ? recentWrongs.map(w => `• ${w}`) : ['• 目前無錯題紀錄，表現優異！'])
      ];

      document.getElementById('report-textarea').value = reportLines.join('\n');
      document.getElementById('copy-toast').style.opacity = '0';
      document.getElementById('report-modal').classList.remove('hidden');
    }

    function closeReportModal() {
      document.getElementById('report-modal').classList.add('hidden');
    }

    function copyReportText() {
      const textarea = document.getElementById('report-textarea');
      textarea.select();
      textarea.setSelectionRange(0, 99999);

      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(textarea.value).then(() => {
          sfx.correct();
          document.getElementById('copy-toast').style.opacity = '1';
        }).catch(() => {
          document.execCommand('copy');
          sfx.correct();
          document.getElementById('copy-toast').style.opacity = '1';
        });
      } else {
        document.execCommand('copy');
        sfx.correct();
        document.getElementById('copy-toast').style.opacity = '1';
      }
    }

    // 初始化啟動
    loadState();
    renderLobby();
  </script>
</body>
</html>
