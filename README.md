<!DOCTYPE html>
<html lang="zh-HK">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <meta name="apple-mobile-web-app-title" content="粵音英聯">
  <meta name="theme-color" content="#F2F2F7">
  <title>粵音英聯 | 廣東話延伸記憶字典</title>
  
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/@phosphor-icons/web"></script>
  
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            iosBlue: '#007AFF',
            iosBlueLight: '#E8F2FF',
            iosGreen: '#34C759',
            iosOrange: '#FF9500',
            iosOrangeLight: '#FFF4E5',
            iosRed: '#FF3B30',
            iosPurple: '#AF52DE',
            iosBg: '#F2F2F7',
            iosCard: '#FFFFFF',
            iosMuted: '#8E8E93',
          },
          fontFamily: {
            sans: ['-apple-system', 'BlinkMacSystemFont', '"SF Pro Text"', '"SF Pro Display"', '"PingFang HK"', '"Noto Sans HK"', 'sans-serif'],
          }
        }
      }
    }
  </script>
  
  <style>
    body {
      -webkit-tap-highlight-color: transparent;
      user-select: none;
      touch-action: manipulation;
    }
    input, textarea {
      user-select: text;
    }
    .safe-top {
      padding-top: max(env(safe-area-inset-top), 14px);
    }
    .safe-bottom {
      padding-bottom: max(env(safe-area-inset-bottom), 18px);
    }
    .hide-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .hide-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }
    .ios-glass {
      background: rgba(255, 255, 255, 0.88);
      backdrop-filter: blur(24px);
      -webkit-backdrop-filter: blur(24px);
    }
    .ios-card-shadow {
      box-shadow: 0 4px 20px -2px rgba(0, 0, 0, 0.05), 0 2px 6px -1px rgba(0, 0, 0, 0.02);
    }
    @keyframes pulse-subtle {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.02); }
    }
    .anim-pulse {
      animation: pulse-subtle 2s infinite ease-in-out;
    }
  </style>
</head>

<body class="bg-iosBg text-[#1C1C1E] font-sans antialiased min-h-screen flex flex-col justify-between selection:bg-iosBlue/20">

  <header class="sticky top-0 z-40 ios-glass border-b border-gray-200/70 safe-top">
    <div class="max-w-md mx-auto px-4 py-2.5 flex items-center justify-between">
      <div class="flex items-center space-x-2.5">
        <div class="w-8 h-8 rounded-xl bg-gradient-to-tr from-iosBlue via-blue-500 to-indigo-500 flex items-center justify-center text-white shadow-sm">
          <i class="ph-bold ph-translate text-base"></i>
        </div>
        <div>
          <div class="flex items-center space-x-1.5">
            <h1 class="text-base font-bold text-gray-900 tracking-tight leading-none">粵音英聯</h1>
            <span class="text-[10px] px-1.5 py-0.5 bg-blue-100/70 text-iosBlue rounded font-semibold tracking-wide">AI • 廣東話</span>
          </div>
          <p class="text-[11px] text-gray-500 font-medium">母語諧音 • 時事例句 • 延伸記憶法</p>
        </div>
      </div>
      
      <div class="flex items-center space-x-1.5">
        <button id="ai-status-indicator" class="px-2 py-1 rounded-full bg-emerald-50 text-emerald-700 border border-emerald-200 text-[11px] font-semibold flex items-center space-x-1 shadow-2xs">
          <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-ping"></span>
          <span>隨查隨解</span>
        </button>
        <button id="add-word-btn" title="手動收錄新詞" class="w-8 h-8 rounded-full bg-gray-100 hover:bg-gray-200 text-gray-700 flex items-center justify-center transition active:scale-90 shadow-2xs">
          <i class="ph-bold ph-plus text-base"></i>
        </button>
      </div>
    </div>
  </header>

  <main class="flex-1 max-w-md w-full mx-auto pb-24 overflow-y-auto px-4 pt-3 hide-scrollbar">

    <!-- TAB 1: 查字典與記憶卡片 (Dictionary Tab) -->
    <section id="tab-dictionary" class="space-y-4">
      <!-- Search Bar -->
      <div class="space-y-2">
        <div class="relative flex items-center">
          <i class="ph-bold ph-magnifying-glass absolute left-3.5 text-gray-400 text-base"></i>
          <input 
            id="search-input" 
            type="search" 
            placeholder="搜尋單字、片語（例：reluctant 或 call it a day）" 
            class="w-full pl-10 pr-24 py-2.5 bg-white text-sm rounded-xl border border-gray-200/80 shadow-sm focus:outline-none focus:ring-2 focus:ring-iosBlue/40 focus:border-iosBlue transition placeholder:text-gray-400"
            autocomplete="off"
            autocorrect="off"
            autocapitalize="none"
          >
          <button id="search-action-btn" class="absolute right-1.5 px-3 py-1.5 bg-iosBlue text-white text-xs font-semibold rounded-lg hover:bg-blue-600 active:scale-95 transition flex items-center space-x-1 shadow-sm">
            <span>查詢</span>
            <i class="ph-bold ph-sparkle text-[11px]"></i>
          </button>
        </div>

        <!-- Quick Chips Bar for instant testing -->
        <div class="flex items-center space-x-1.5 overflow-x-auto py-1 hide-scrollbar text-xs">
          <span class="text-gray-400 font-medium text-[11px] whitespace-nowrap"><i class="ph-bold ph-fire text-amber-500 mr-0.5"></i>推薦試查：</span>
          <button class="chip-btn px-2.5 py-1 bg-white border border-gray-200 rounded-full text-gray-700 whitespace-nowrap active:bg-gray-100 font-medium shadow-2xs">reluctant</button>
          <button class="chip-btn px-2.5 py-1 bg-white border border-gray-200 rounded-full text-gray-700 whitespace-nowrap active:bg-gray-100 font-medium shadow-2xs">call it a day</button>
          <button class="chip-btn px-2.5 py-1 bg-white border border-gray-200 rounded-full text-gray-700 whitespace-nowrap active:bg-gray-100 font-medium shadow-2xs">meticulous</button>
          <button class="chip-btn px-2.5 py-1 bg-white border border-gray-200 rounded-full text-gray-700 whitespace-nowrap active:bg-gray-100 font-medium shadow-2xs">bite the bullet</button>
          <button class="chip-btn px-2.5 py-1 bg-white border border-gray-200 rounded-full text-gray-700 whitespace-nowrap active:bg-gray-100 font-medium shadow-2xs">serendipity</button>
        </div>
      </div>

      <div id="multi-word-breakdown-panel" class="hidden bg-white rounded-2xl p-4 border border-blue-100/90 ios-card-shadow space-y-2.5 transition-all">
        <div class="flex items-center justify-between border-b border-gray-100 pb-2">
          <div class="flex items-center space-x-1.5 text-iosBlue text-xs font-bold">
            <i class="ph-bold ph-list-checks text-sm"></i>
            <span>多詞分別解析與最相關中文意思</span>
          </div>
          <span id="multi-word-count" class="text-[10px] bg-blue-50 text-iosBlue px-2 py-0.5 rounded-full font-semibold border border-blue-100">共 0 個單字</span>
        </div>
        <p class="text-[11px] text-gray-500 leading-tight">已為你獨立列出各英文字的最精準相關意思，點擊單字可直接展開其廣東話諧音卡：</p>
        <div id="multi-word-items-list" class="space-y-2 pt-1">
          <!-- Dynamically populated with individual word meanings -->
        </div>
      </div>

      <!-- Loading State Indicator -->
      <div id="loading-state" class="hidden bg-white rounded-2xl p-8 border border-gray-100 text-center space-y-3 ios-card-shadow">
        <div class="w-10 h-10 border-3 border-iosBlue border-t-transparent rounded-full animate-spin mx-auto"></div>
        <p class="text-sm font-semibold text-gray-800" id="loading-text">正在分析詞義及生成廣東話諧音口訣...</p>
        <p class="text-xs text-gray-400">正在調用 Gemini 模型構建延伸語義網絡與時事新聞例句</p>
      </div>

      <article id="vocab-card" class="bg-white rounded-2xl p-5 border border-gray-100 ios-card-shadow space-y-4 transition-all">
        <!-- Word Header -->
        <div class="flex items-start justify-between">
          <div class="space-y-1">
            <div class="flex items-center space-x-2">
              <h2 id="card-word" class="text-2xl sm:text-3xl font-black tracking-tight text-gray-950">reluctant</h2>
              <span id="card-pos" class="text-[11px] font-semibold px-2 py-0.5 bg-blue-50 text-iosBlue rounded-md border border-blue-100">adj.</span>
            </div>
            <div class="flex items-center space-x-2">
              <span id="card-phonetic" class="text-sm font-mono text-gray-500">/rɪˈlʌktənt/</span>
              <button id="tts-word-btn" title="聽發音" class="w-7 h-7 rounded-full bg-blue-50 text-iosBlue hover:bg-iosBlue hover:text-white flex items-center justify-center transition active:scale-90 shadow-2xs">
                <i class="ph-bold ph-speaker-simple-high text-xs"></i>
              </button>
            </div>
          </div>
          <button id="bookmark-btn" title="加入生字簿" class="w-10 h-10 rounded-full bg-gray-50 text-gray-400 hover:text-amber-500 hover:bg-amber-50 flex items-center justify-center transition active:scale-90">
            <i class="ph-fill ph-bookmark-simple text-xl"></i>
          </button>
        </div>

        <!-- Chinese Meaning & Usage -->
        <div class="bg-blue-50/70 rounded-xl p-3.5 border border-blue-100 space-y-1.5">
          <div class="flex items-center space-x-1.5 text-iosBlue text-xs font-bold uppercase tracking-wider">
            <i class="ph-bold ph-book-open-text"></i>
            <span>中文意思及用法</span>
          </div>
          <p id="card-meaning" class="text-lg font-extrabold text-gray-900 leading-snug">不情願的、勉強的</p>
          <p id="card-usage" class="text-xs text-gray-700 leading-relaxed font-normal">
            形容主觀上好抗拒做某事，但迫於情勢最後都要做。最典型固定搭配為「reluctant to do something」。
          </p>
        </div>

        <!-- 組成單字分別解析專區 (Individual Word Breakdown for Phrases & Terms) -->
        <div id="card-breakdown-box" class="hidden bg-slate-50/90 rounded-xl p-3.5 border border-slate-200/90 space-y-2">
          <div class="flex items-center justify-between">
            <div class="flex items-center space-x-1.5 text-slate-800 text-xs font-bold uppercase tracking-wider">
              <i class="ph-bold ph-brackets-curly text-iosBlue"></i>
              <span>各英文字分別意思（最相關對照）</span>
            </div>
            <span class="text-[10px] text-slate-500 bg-white px-1.5 py-0.5 rounded border border-slate-200 font-medium">單字拆解</span>
          </div>
          <div id="card-breakdown-container" class="space-y-1.5">
            <!-- Dynamically populated breakdown items -->
          </div>
        </div>

        <!-- Cantonese Homophone Mnemonic -->
        <div class="bg-amber-50/90 rounded-xl p-3.5 border border-amber-200/90 space-y-2.5 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <div class="flex items-center space-x-1.5 text-iosOrange text-xs font-bold uppercase tracking-wider">
              <i class="ph-bold ph-lightbulb"></i>
              <span>廣東話諧音記憶法</span>
            </div>
            <span class="text-[10px] text-amber-700 bg-amber-100/80 px-1.5 py-0.5 rounded font-semibold">母語聯想</span>
          </div>

          <div class="flex items-baseline space-x-2">
            <span class="text-xs text-gray-500 whitespace-nowrap">口訣諧音：</span>
            <span id="card-cantonese-pun" class="text-sm font-black text-amber-950 bg-amber-200/80 px-2 py-0.5 rounded tracking-wide shadow-2xs">「驢力吞」/ 李歷緊</span>
          </div>

          <p id="card-cantonese-story" class="text-xs text-amber-950 leading-relaxed font-medium">
            想像一隻驢仔（re-luc-）要好用「力吞」（-tant）一嚿極苦嘅藥，佢成塊面皺晒，非常<strong>唔情願、好勉強</strong>！
          </p>

          <!-- Visual Cue -->
          <div class="pt-2 border-t border-amber-200/60 flex items-start space-x-2 text-xs text-amber-900">
            <div class="w-5 h-5 rounded-md bg-amber-200/80 flex items-center justify-center flex-shrink-0 mt-0.5">
              <i class="ph-bold ph-eye text-amber-800 text-xs"></i>
            </div>
            <p id="card-visual-cue" class="leading-relaxed">
              <strong>圖像記憶：</strong>一隻四腳抓緊地面、死都唔肯前進嘅驢仔，頭頂冒出「我唔想做」嘅汗滴與抗拒表情。
            </p>
          </div>
        </div>

        <!-- Real-world News Sentences with Audio Playback -->
        <div class="space-y-2">
          <div class="flex items-center justify-between">
            <span class="text-xs font-bold text-gray-600 uppercase tracking-wider flex items-center space-x-1">
              <i class="ph-bold ph-newspaper text-iosBlue text-sm"></i>
              <span>最新時事與新聞例句（點擊聽讀）</span>
            </span>
            <span class="text-[10px] text-gray-400 font-medium">外媒權威語境</span>
          </div>
          <div id="card-examples-container" class="space-y-2">
            <!-- Dynamically populated examples -->
          </div>
        </div>

        <!-- Semantic Network: Synonyms & Antonyms (延伸記憶法核心) -->
        <div class="space-y-2 pt-1 border-t border-gray-100">
          <div class="flex items-center justify-between">
            <span class="text-xs font-bold text-gray-600 uppercase tracking-wider flex items-center space-x-1">
              <i class="ph-bold ph-tree-structure text-iosBlue"></i>
              <span>延伸詞彙網絡（點擊即查）</span>
            </span>
            <span class="text-[10px] text-gray-400">建立語義連結</span>
          </div>

          <div class="space-y-2">
            <!-- Synonyms -->
            <div class="flex items-start space-x-2">
              <span class="text-[11px] font-bold px-2 py-0.5 bg-emerald-50 text-emerald-700 rounded-md border border-emerald-200 whitespace-nowrap mt-0.5">同義</span>
              <div id="card-synonyms" class="flex flex-wrap gap-1.5 flex-1">
                <!-- Dynamically populated -->
              </div>
            </div>

            <!-- Antonyms -->
            <div class="flex items-start space-x-2">
              <span class="text-[11px] font-bold px-2 py-0.5 bg-rose-50 text-rose-700 rounded-md border border-rose-200 whitespace-nowrap mt-0.5">反義</span>
              <div id="card-antonyms" class="flex flex-wrap gap-1.5 flex-1">
                <!-- Dynamically populated -->
              </div>
            </div>
          </div>
        </div>
      </article>
    </section>

    <!-- TAB 2: 延伸記憶法複習視圖 (Associative Memory Quiz) -->
    <section id="tab-review" class="hidden space-y-4">
      <!-- Quiz Progress & Streak Header -->
      <div class="bg-white rounded-2xl p-4 border border-gray-100 ios-card-shadow flex items-center justify-between">
        <div class="space-y-0.5">
          <span class="text-[10px] font-bold uppercase tracking-wider text-iosPurple">延伸對比複習法</span>
          <h3 class="text-sm font-extrabold text-gray-900">透過同義 / 反義辨析詞義</h3>
        </div>
        <div class="flex items-center space-x-2">
          <div class="flex items-center space-x-1 bg-amber-50 text-amber-600 px-2 py-1 rounded-full text-xs font-bold border border-amber-200">
            <i class="ph-fill ph-fire text-amber-500"></i>
            <span id="quiz-streak">0 連勝</span>
          </div>
          <span id="quiz-score-badge" class="px-2.5 py-1 bg-blue-50 text-iosBlue text-xs font-bold rounded-full border border-blue-100">
            得分: 0
          </span>
        </div>
      </div>

      <!-- Quiz Interactive Card -->
      <div class="bg-white rounded-2xl p-5 border border-gray-100 ios-card-shadow space-y-5">
        <div class="text-center space-y-1.5">
          <span id="quiz-relation-type" class="inline-block px-3 py-1 bg-rose-50 text-rose-700 text-xs font-extrabold rounded-full border border-rose-200">
            請選出該詞的「反義詞 (Antonym)」
          </span>
          <h2 id="quiz-target-word" class="text-3xl font-black text-gray-900 tracking-tight pt-1">reluctant</h2>
          <p id="quiz-word-hint" class="text-xs text-gray-400 font-medium">音標: /rɪˈlʌktənt/ • 粵音口訣: 驢力吞</p>
        </div>

        <p class="text-xs text-gray-500 text-center font-medium">在下列選項中，邊一個詞語與目標詞的意思形成相反對比？</p>

        <!-- Options Container -->
        <div id="quiz-options-container" class="space-y-2.5">
          <!-- Dynamically populated options -->
        </div>

        <!-- Explanation Feedback Box -->
        <div id="quiz-feedback-box" class="hidden rounded-xl p-4 transition-all space-y-2">
          <div class="flex items-center space-x-2 font-bold text-sm" id="feedback-title"></div>
          <p id="feedback-detail" class="text-xs leading-relaxed"></p>
          <div class="pt-2">
            <button id="quiz-next-btn" class="w-full py-2.5 bg-iosBlue text-white text-xs font-bold rounded-xl shadow-sm hover:bg-blue-600 active:scale-95 transition flex items-center justify-center space-x-1">
              <span>下一題延伸複習</span>
              <i class="ph-bold ph-arrow-right text-xs"></i>
            </button>
          </div>
        </div>
      </div>

      <!-- Brain Concept Educational Box -->
      <div class="bg-indigo-50/70 rounded-xl p-3.5 border border-indigo-100 flex items-start space-x-2.5">
        <div class="w-7 h-7 rounded-lg bg-indigo-100 text-indigo-600 flex items-center justify-center flex-shrink-0">
          <i class="ph-bold ph-brain text-base"></i>
        </div>
        <div class="text-xs text-indigo-950 leading-relaxed">
          <strong class="font-bold text-indigo-900">為什麼要用延伸記憶法？</strong>
          <p class="mt-0.5 text-indigo-800/90">孤立死記單字極易遺忘。透過辨識「同義連結」與「反義對照」，能在腦海編織語義網絡，記憶留存率提高 3 倍以上！</p>
        </div>
      </div>
    </section>

    <!-- TAB 3: 生字庫清單 (Wordbook) -->
    <section id="tab-wordbook" class="hidden space-y-3">
      <div class="flex items-center justify-between px-1">
        <div>
          <span class="text-xs font-bold text-gray-500 uppercase tracking-wider">我的生字庫</span>
          <p class="text-[11px] text-gray-400">點擊卡片可即時查看口訣與例句</p>
        </div>
        <span id="wordbook-count" class="text-xs font-bold px-2 py-0.5 bg-blue-50 text-iosBlue rounded-full">共 0 個</span>
      </div>

      <!-- Quick Filter -->
      <div class="flex items-center space-x-1.5 text-xs pb-1">
        <button id="filter-all-btn" class="px-3 py-1 bg-iosBlue text-white font-semibold rounded-lg shadow-2xs">全部詞彙</button>
        <button id="filter-fav-btn" class="px-3 py-1 bg-white border border-gray-200 text-gray-600 font-medium rounded-lg active:bg-gray-50">精選星標 ⭐</button>
      </div>

      <div id="wordbook-list" class="space-y-2">
        <!-- Dynamically generated list -->
      </div>
    </section>
  </main>

  <!-- Modal: 手動加入新詞 (Add Word Modal) -->
  <div id="add-modal" class="fixed inset-0 z-50 bg-black/40 backdrop-blur-xs hidden flex items-center justify-center p-4">
    <div class="bg-white w-full max-w-sm rounded-2xl p-5 shadow-2xl space-y-3.5 border border-gray-100">
      <div class="flex items-center justify-between border-b pb-2.5">
        <div class="flex items-center space-x-2">
          <i class="ph-bold ph-plus-circle text-iosBlue text-lg"></i>
          <h3 class="font-bold text-sm text-gray-900">收錄新單字 / 片語</h3>
        </div>
        <button id="close-modal-btn" class="w-6 h-6 rounded-full bg-gray-100 text-gray-500 hover:text-gray-800 flex items-center justify-center">
          <i class="ph-bold ph-x text-xs"></i>
        </button>
      </div>

      <div class="space-y-2.5 text-xs max-h-[70vh] overflow-y-auto hide-scrollbar">
        <div>
          <label class="block font-semibold text-gray-700 mb-1">英文單字 / 片語 *</label>
          <input id="input-new-word" type="text" placeholder="例：serendipity 或 call it a day" class="w-full px-3 py-2 border rounded-xl focus:outline-none focus:ring-1 focus:ring-iosBlue">
        </div>
        <div>
          <label class="block font-semibold text-gray-700 mb-1">中文意思 *</label>
          <input id="input-new-meaning" type="text" placeholder="例：不期而遇的美好緣分" class="w-full px-3 py-2 border rounded-xl focus:outline-none focus:ring-1 focus:ring-iosBlue">
        </div>
        <div>
          <label class="block font-semibold text-gray-700 mb-1">廣東話諧音口訣</label>
          <input id="input-new-pun" type="text" placeholder="例：「死忍得皮」" class="w-full px-3 py-2 border rounded-xl focus:outline-none focus:ring-1 focus:ring-iosBlue">
        </div>
        <div>
          <label class="block font-semibold text-gray-700 mb-1">同義詞（用逗號隔開）</label>
          <input id="input-new-syn" type="text" placeholder="例：fluke, chance, coincidence" class="w-full px-3 py-2 border rounded-xl focus:outline-none focus:ring-1 focus:ring-iosBlue">
        </div>
        <div>
          <label class="block font-semibold text-gray-700 mb-1">反義詞（用逗號隔開）</label>
          <input id="input-new-ant" type="text" placeholder="例：misfortune, bad luck" class="w-full px-3 py-2 border rounded-xl focus:outline-none focus:ring-1 focus:ring-iosBlue">
        </div>
      </div>

      <div class="pt-2 flex space-x-2">
        <button id="cancel-add-btn" class="w-1/2 py-2 bg-gray-100 text-gray-700 rounded-xl font-semibold text-xs active:bg-gray-200">取消</button>
        <button id="confirm-add-btn" class="w-1/2 py-2 bg-iosBlue text-white rounded-xl font-semibold text-xs active:bg-blue-600 shadow-2xs">儲存詞彙</button>
      </div>
    </div>
  </div>

  <!-- Toast Notification Message Box -->
  <div id="toast" class="fixed top-14 left-1/2 -translate-x-1/2 z-50 bg-gray-900/90 text-white text-xs font-semibold px-4 py-2 rounded-full shadow-lg transition-all duration-300 opacity-0 pointer-events-none transform -translate-y-3">
    提示訊息
  </div>

  <!-- Bottom iOS Style Tab Bar -->
  <nav class="fixed bottom-0 left-0 right-0 z-40 ios-glass border-t border-gray-200/80 safe-bottom">
    <div class="max-w-md mx-auto grid grid-cols-3 h-14">
      <button id="tab-btn-dictionary" class="nav-tab-btn flex flex-col items-center justify-center text-iosBlue transition active:scale-95" data-tab="tab-dictionary">
        <i class="ph-bold ph-magnifying-glass text-xl"></i>
        <span class="text-[10px] font-semibold mt-0.5">查單字</span>
      </button>

      <button id="tab-btn-review" class="nav-tab-btn flex flex-col items-center justify-center text-gray-400 transition active:scale-95" data-tab="tab-review">
        <i class="ph-bold ph-brain text-xl"></i>
        <span class="text-[10px] font-semibold mt-0.5">延伸複習</span>
      </button>

      <button id="tab-btn-wordbook" class="nav-tab-btn flex flex-col items-center justify-center text-gray-400 transition active:scale-95" data-tab="tab-wordbook">
        <i class="ph-bold ph-notebook text-xl"></i>
        <span class="text-[10px] font-semibold mt-0.5">生字庫</span>
      </button>
    </div>
  </nav>

  <script>
    // 預設精選詞庫（針對廣東話母語學習者設計，包含最新新聞語境例句及拆解）
    const defaultVocabulary = [
      {
        id: "v1",
        word: "reluctant",
        phonetic: "/rɪˈlʌktənt/",
        pos: "adj.",
        chineseMeaning: "不情願的、勉強的",
        chineseUsage: "形容主觀上極度抗拒做某事，但迫於形勢最後都要做。最常見搭配為「reluctant to do something」。",
        breakdown: [
          { word: "reluctant", pos: "adj.", meaning: "不情願的、抱持猶豫態度的", explanation: "源自拉丁文 reluctari（反抗、搏鬥），形容心理上的抗拒與躊躇。" }
        ],
        examples: [
          {
            tag: "財經時事",
            en: "Central banks remain reluctant to cut interest rates aggressively amid lingering inflation risks.",
            zh: "在通脹反彈風險持續的背景下，各國央行對於大刀闊斧減息仍顯得相當猶豫和不情願。"
          },
          {
            tag: "科技監管",
            en: "AI companies are reluctant to disclose their proprietary dataset sources to international regulators.",
            zh: "各大人工智能企業很不情願向各國監管部門公開其專有的模型訓練數據來源。"
          }
        ],
        synonyms: ["unwilling", "hesitant", "loath", "averse"],
        antonyms: ["eager", "willing", "enthusiastic", "keen"],
        cantoneseMnemonic: {
          pun: "「驢力吞」/ 李歷緊",
          story: "想像一隻驢仔（re-luc-）要好用「力吞」（-tant）一嚿極苦嘅藥，佢成塊面皺晒，非常「唔情願、好勉強」。",
          visualCue: "一隻四隻腳死撐住地面、死都唔肯行嘅驢仔，頭頂冒冷汗兼滿臉抗拒。"
        },
        favorite: true
      },
      {
        id: "v2",
        word: "call it a day",
        phonetic: "/kɔːl ɪt ə deɪ/",
        pos: "phrase",
        chineseMeaning: "收工、今天到此為止、結束生涯",
        chineseUsage: "常用於經過長時間工作或談判後提議停工休息；新聞中亦極常用於體壇名將退役或政客卸任宣布「告一段落」。",
        breakdown: [
          { word: "call", pos: "v.", meaning: "正式宣布、裁定、界定為", explanation: "在此作口頭宣告或裁決，相當於「認定/將之界定為」。" },
          { word: "it", pos: "pron.", meaning: "這件事情、手頭上的工作", explanation: "代名詞，指代當天一直在進行的事務或任務。" },
          { word: "a day", pos: "n. phr.", meaning: "一天的工作量、完整的一日", explanation: "指一整天應有的工作配額已達成，故可畫上句號。" }
        ],
        examples: [
          {
            tag: "體壇焦點",
            en: "After 26 historic years on the basketball broadcast desk, the NBA legend announced it was finally time to call it a day.",
            zh: "在籃球轉播台度過了輝煌的 26 年之後，這位 NBA 傳奇球星正式宣佈是時候功成身退、結束幕前生涯了。"
          },
          {
            tag: "國際外交",
            en: "After fourteen hours of tense negotiations yielded little breakthrough, weary delegates decided to call it a day.",
            zh: "經過長達十四個小時毫無重大突破的緊繃談判後，筋疲力竭的外交代表決定今天到此為止、暫停會談。"
          }
        ],
        synonyms: ["wrap up", "conclude", "finish up", "pack it in"],
        antonyms: ["carry on", "continue", "keep going", "persevere"],
        cantoneseMnemonic: {
          pun: "「Call 佢個 Day」",
          story: "打個電話（Call）通知老細，話今日呢一日（Day）已經完咗，即係大嗌「今日收工啦」！",
          visualCue: "辦公室鬧鐘一響到 6 點，所有人即刻用力啪埋手提電腦彈起身歡呼。"
        },
        favorite: true
      },
      {
        id: "v3",
        word: "meticulous",
        phonetic: "/məˈtɪkjələs/",
        pos: "adj.",
        chineseMeaning: "一絲不苟的、極其細心的",
        chineseUsage: "褒義詞，形容做事極其注重微小細節，對準確度與安全性要求極高。常搭「meticulous planning/research/inspection」。",
        breakdown: [
          { word: "meticulous", pos: "adj.", meaning: "極細緻的、謹小慎微的", explanation: "源自拉丁文 metus（恐懼），原本形容因害怕出錯而極端嚴謹，後引申為專注每處細節。" }
        ],
        examples: [
          {
            tag: "航空安全",
            en: "Air safety investigators conducted a meticulous forensic inspection of the engine sensors following the emergency diversion.",
            zh: "客機緊急迫降後，航空安全事故調查員對發動機傳感器進行了極其嚴密、一絲不苟的法證級檢驗。"
          },
          {
            tag: "晶片科技",
            en: "Manufacturing advanced 2-nanometer semiconductors requires meticulous cleanroom standards and environmental controls.",
            zh: "製造頂尖的 2 奈米半導體晶片需要達到極致苛刻、一絲不苟的無塵室標準與環境管控。"
          }
        ],
        synonyms: ["thorough", "scrupulous", "painstaking", "diligent"],
        antonyms: ["careless", "sloppy", "negligent", "slapdash"],
        cantoneseMnemonic: {
          pun: "「咪剔快囉」/ 微剔緊",
          story: "做校對唔好「咪剔快囉」，要逐行逐隻字慢慢「微剔」，因為佢為人做事真係「一絲不苟、極之細心」。",
          visualCue: "一個戴住放大鏡嘅偵探，連地毯上一粒微塵都要用鑷子小心夾起記錄。"
        },
        favorite: false
      },
      {
        id: "v4",
        word: "bite the bullet",
        phonetic: "/baɪt ðə ˈbʊlɪt/",
        pos: "phrase",
        chineseMeaning: "咬緊牙關硬著頭皮面對、忍痛下定決心",
        chineseUsage: "形容面對無可避免且令人痛苦的情勢時，不再拖延，毅然決然去承擔後果。",
        breakdown: [
          { word: "bite", pos: "v.", meaning: "用力咬緊", explanation: "動作動詞，指用力咬住某物以承受劇烈疼痛。" },
          { word: "the", pos: "art.", meaning: "那枚特定的", explanation: "定冠詞。" },
          { word: "bullet", pos: "n.", meaning: "子彈、彈頭", explanation: "源自古代戰場未有麻醉藥時，讓受傷士兵咬住鉛彈以耐受手術劇痛。" }
        ],
        examples: [
          {
            tag: "科技重組",
            en: "Facing rising compute costs, the startup had to bite the bullet and initiate a company-wide restructuring.",
            zh: "面對日益激增的算力成本，該初創公司不得不咬緊牙關，痛下決心展開全公司的架構重組。"
          },
          {
            tag: "財政預算",
            en: "Ministers were forced to bite the bullet and raise taxes ahead of the upcoming fiscal deficit deadline.",
            zh: "在財政赤字截止期限逼近前，內閣部長們被迫咬牙承受民意壓力提高稅率。"
          }
        ],
        synonyms: ["face the music", "swallow the bitter pill", "brace oneself", "tough it out"],
        antonyms: ["chicken out", "evade", "procrastinate", "shrink back"],
        cantoneseMnemonic: {
          pun: "「拜託個波歷」/ 咬住粒 Bullet",
          story: "打仗傷咗無麻醉藥，唯有用力「Bite」住粒子彈（Bullet），「咬緊牙關硬住頭皮」頂過去！",
          visualCue: "一個硬漢額頭流冷汗，嘴裡實牙實齒咬住一枚金屬子彈，眼神堅毅。"
        },
        favorite: true
      },
      {
        id: "v5",
        word: "serendipity",
        phonetic: "/ˌserənˈdɪpəti/",
        pos: "n.",
        chineseMeaning: "機緣巧合、意外尋獲美好事物之運氣",
        chineseUsage: "新聞常客，常用於醫學重大發現、天文物理學驚人成果或考古突破中「意外撞正的幸運驚喜」。",
        breakdown: [
          { word: "serendipity", pos: "n.", meaning: "意外發現珍寶的幸運運氣", explanation: "源自波斯童話《錫蘭三王子》(The Three Princes of Serendip)，他們總是在路上意外發現美好事物。" }
        ],
        examples: [
          {
            tag: "醫療前沿",
            en: "Cancer researchers noted that discovering the synthetic antibody was pure serendipity during an unrelated cardiovascular trial.",
            zh: "癌症研究團隊指出，在一次無關的心血管臨床試驗中偶然發現該合成抗體，完全是一場純粹的機緣巧合與意外之喜。"
          },
          {
            tag: "天文探測",
            en: "Astronomers credited sheer serendipity after capturing the rare flash of a distant supernova while calibrating deep-space sensors.",
            zh: "天文學家在校準深空傳感器時偶然捕捉到了遙遠超新星的罕見爆發閃光，紛紛感嘆這是一場不可多得的幸運巧遇。"
          }
        ],
        synonyms: ["chance", "fluke", "coincidence", "providence"],
        antonyms: ["misfortune", "adversity", "bad luck"],
        cantoneseMnemonic: {
          pun: "「死忍得皮」",
          story: "「死忍」痛苦之後，終於得到厚「皮」回報，原來係「意想不到嘅美好驚喜同好運」！",
          visualCue: "一個喺街行緊被舊書絆倒嘅人，低頭一睇發現地下竟然夾住一張頭獎彩票。"
        },
        favorite: true
      }
    ];

    // 全局狀態管理
    let vocabList = [...defaultVocabulary];
    let currentWordItem = vocabList[0];
    let quizScore = 0;
    let quizStreak = 0;
    let currentQuizData = null;
    let activeFilter = 'all';

    // 簡易純程式合成音效（無外部音訊依賴）
    function playTapAudio(isSuccess = true) {
      try {
        const AudioCtx = window.AudioContext || window.webkitAudioContext;
        if (!AudioCtx) return;
        const ctx = new AudioCtx();
        const osc = ctx.createOscillator();
        const gain = ctx.createGain();
        osc.connect(gain);
        gain.connect(ctx.destination);
        
        if (isSuccess) {
          osc.frequency.setValueAtTime(587.33, ctx.currentTime); // D5
          osc.frequency.exponentialRampToValueAtTime(880, ctx.currentTime + 0.12); // A5
          gain.gain.setValueAtTime(0.12, ctx.currentTime);
          gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.14);
          osc.start();
          osc.stop(ctx.currentTime + 0.15);
        } else {
          osc.frequency.setValueAtTime(260, ctx.currentTime);
          osc.frequency.exponentialRampToValueAtTime(180, ctx.currentTime + 0.18);
          gain.gain.setValueAtTime(0.15, ctx.currentTime);
          gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.2);
          osc.start();
          osc.stop(ctx.currentTime + 0.22);
        }
      } catch (e) {
        // AudioContext disabled or ignored
      }
    }

    // 呼叫 Gemini 3 Flash Preview 進行深度新聞時事與多詞分析
    async function fetchGeminiWordAnalysis(query) {
      const apiKey = ""; // Canvas 自動注入運行
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

      const systemPrompt = `你是一個精通廣東話（香港粵語）的專業英語教學與新聞英語專家。
用戶會提供一個或多個英文單字或片語（例如 "reluctant, meticulous" 或 "bite the bullet"）。
請嚴格以純 JSON 格式回傳，必須包含：
1. "word": 查詢的主單字或片語
2. "phonetic": 國際音標
3. "pos": 詞性（如 adj., v., phrase）
4. "chineseMeaning": 繁體中文最準確核心意思
5. "chineseUsage": 深入淺出的繁體中文語境用法（一至兩句）
6. "breakdown": 陣列格式。重要要求：若查詢包含多個英文字、多個詞彙或片語，必須將每個英文字分別列出，並給予在該組合/語境下最相關的繁體中文意思：
   - "word": 單個英文字
   - "pos": 該單字的詞性（如 v., n., adj., prep.）
   - "meaning": 最貼合、最相關的繁體中文意思（例如 "call" -> "宣佈、認定為"；"bullet" -> "子彈；引申為咬緊硬挺之代價"）
   - "explanation": 簡述它在該語境或字面上的具體作用
7. "examples": 一個包含 2 個物件的陣列。每個例句取材自真實時事新聞（路透社、BBC、彭博風格）：
   - "tag": 領域分類（如 "國際時事", "財經焦點", "科技前沿", "體育焦點"）
   - "en": 高階自然的英文新聞例句
   - "zh": 繁體中文準確流暢翻譯
8. "synonyms": 3-4 個常見同義詞陣列
9. "antonyms": 3-4 個常見反義詞陣列
10. "cantoneseMnemonic": 包含：
   - "pun": 幽默生動的廣東話諧音口訣
   - "story": 用廣東話（粵語口語）解釋諧音如何連繫字義
   - "visualCue": 圖像化記憶聯想畫面描繪
請只回傳 JSON 字串，不要包含任何 markdown 標籤或額外文字。`;

      const payload = {
        contents: [
          { role: "user", parts: [{ text: `請為英文詞彙「${query}」生成字典卡片。若包含多個單字，請特別在 breakdown 欄位分別列出每一個英文字的最相關中文意思。` }] }
        ],
        systemInstruction: {
          parts: [{ text: systemPrompt }]
        },
        generationConfig: {
          responseMimeType: "application/json"
        }
      };

      const response = await fetch(apiUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      if (!response.ok) {
        throw new Error(`API 請求失敗: ${response.status}`);
      }

      const result = await response.json();
      const rawText = result.candidates?.[0]?.content?.parts?.[0]?.text;
      if (!rawText) throw new Error("未能取得回傳內容");
      
      const parsed = JSON.parse(rawText);
      return {
        id: "v_" + Date.now(),
        word: parsed.word || query,
        phonetic: parsed.phonetic || `/${query}/`,
        pos: parsed.pos || 'expression',
        chineseMeaning: parsed.chineseMeaning || '相關含義',
        chineseUsage: parsed.chineseUsage || '常用於地道英文表達中。',
        breakdown: parsed.breakdown || [],
        examples: parsed.examples && parsed.examples.length ? parsed.examples : [
          { tag: "時事報導", en: `International analysts noted the increasing relevance of ${query} in today's developments.`, zh: `國際分析人士指出，在當前的時態發展中此現象愈發受到關注。` },
          { tag: "財經觀察", en: `Markets reacted promptly after central figures commented on ${query}.`, zh: `在核心官員對相關情況發表評論後，市場迅速作出了反應。` }
        ],
        synonyms: parsed.synonyms && parsed.synonyms.length ? parsed.synonyms : ["similar term"],
        antonyms: parsed.antonyms && parsed.antonyms.length ? parsed.antonyms : ["opposite term"],
        cantoneseMnemonic: {
          pun: parsed.cantoneseMnemonic?.pun || `粵音：「${query.slice(0, 3)}」`,
          story: parsed.cantoneseMnemonic?.story || `利用母語諧音發音，生動記憶 ${query}！`,
          visualCue: parsed.cantoneseMnemonic?.visualCue || `腦海中想像一個誇張有趣的場景。`
        },
        favorite: false
      };
    }

    // 語音朗讀 (Text-to-Speech)
    function speakText(text) {
      if (!('speechSynthesis' in window)) {
        showToast("你的裝置瀏覽器未啟用語音合成發音");
        return;
      }
      window.speechSynthesis.cancel();
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = 'en-US';
      utterance.rate = 0.88;
      utterance.pitch = 1.0;
      window.speechSynthesis.speak(utterance);
      showToast(`正在朗讀: "${text.slice(0, 18)}..."`);
    }

    // 彈出 Toast 訊息
    function showToast(msg) {
      const toast = document.getElementById('toast');
      toast.innerText = msg;
      toast.classList.remove('opacity-0', '-translate-y-3', 'pointer-events-none');
      toast.classList.add('opacity-100', 'translate-y-0');
      clearTimeout(window.__toastTimeout);
      window.__toastTimeout = setTimeout(() => {
        toast.classList.remove('opacity-100', 'translate-y-0');
        toast.classList.add('opacity-0', '-translate-y-3', 'pointer-events-none');
      }, 2200);
    }

    // 渲染查字典詳細卡片
    function renderVocabCard(item) {
      if (!item) return;
      currentWordItem = item;

      document.getElementById('card-word').textContent = item.word;
      document.getElementById('card-pos').textContent = item.pos || 'phrase';
      document.getElementById('card-phonetic').textContent = item.phonetic || '';
      document.getElementById('card-meaning').textContent = item.chineseMeaning;
      document.getElementById('card-usage').textContent = item.chineseUsage;
      document.getElementById('card-cantonese-pun').textContent = item.cantoneseMnemonic.pun;
      document.getElementById('card-cantonese-story').innerHTML = item.cantoneseMnemonic.story;
      document.getElementById('card-visual-cue').innerHTML = `<strong>圖像記憶：</strong>${item.cantoneseMnemonic.visualCue}`;

      // 渲染單字分別解析 (Breakdown)
      const breakdownBox = document.getElementById('card-breakdown-box');
      const breakdownContainer = document.getElementById('card-breakdown-container');
      
      if (item.breakdown && item.breakdown.length > 0) {
        breakdownBox.classList.remove('hidden');
        breakdownContainer.innerHTML = '';
        item.breakdown.forEach(sub => {
          const row = document.createElement('div');
          row.className = 'bg-white rounded-lg p-2.5 border border-slate-200/70 flex items-start justify-between space-x-2 shadow-2xs';
          row.innerHTML = `
            <div class="space-y-0.5 flex-1">
              <div class="flex items-center space-x-1.5">
                <span class="font-extrabold text-xs text-gray-900">${sub.word}</span>
                <span class="text-[10px] font-bold px-1 py-0.2 rounded bg-slate-100 text-slate-600 border border-slate-200">${sub.pos || '詞'}</span>
                <span class="text-xs font-bold text-iosBlue">${sub.meaning}</span>
              </div>
              ${sub.explanation ? `<p class="text-[11px] text-gray-500 leading-tight">${sub.explanation}</p>` : ''}
            </div>
            <button class="quick-subword-btn text-[11px] font-semibold text-iosBlue hover:underline px-1.5 py-1 bg-blue-50 rounded flex items-center space-x-0.5 flex-shrink-0" data-word="${sub.word}">
              <span>查此字</span>
              <i class="ph-bold ph-caret-right text-[10px]"></i>
            </button>
          `;
          breakdownContainer.appendChild(row);
        });

        // 綁定點擊查各別單字
        breakdownContainer.querySelectorAll('.quick-subword-btn').forEach(btn => {
          btn.onclick = (e) => {
            e.stopPropagation();
            const w = btn.getAttribute('data-word');
            quickSearchWord(w);
          };
        });
      } else {
        breakdownBox.classList.add('hidden');
      }

      // 星標狀態
      const bookmarkBtn = document.getElementById('bookmark-btn');
      if (item.favorite) {
        bookmarkBtn.className = "w-10 h-10 rounded-full bg-amber-50 text-amber-500 flex items-center justify-center transition active:scale-90";
      } else {
        bookmarkBtn.className = "w-10 h-10 rounded-full bg-gray-50 text-gray-400 hover:text-amber-500 hover:bg-amber-50 flex items-center justify-center transition active:scale-90";
      }

      // 朗讀單字事件
      document.getElementById('tts-word-btn').onclick = () => speakText(item.word);

      // 渲染雙例句（帶新聞標籤與語音朗讀）
      const examplesContainer = document.getElementById('card-examples-container');
      examplesContainer.innerHTML = '';
      item.examples.forEach((ex) => {
        const tagText = ex.tag || '新聞實例';
        const exEl = document.createElement('div');
        exEl.className = 'bg-gray-50/90 rounded-xl p-3 border border-gray-100 flex items-start justify-between group hover:bg-blue-50/40 transition';
        exEl.innerHTML = `
          <div class="space-y-1 pr-2">
            <div class="flex items-center space-x-1.5">
              <span class="text-[10px] font-bold px-1.5 py-0.5 rounded bg-blue-100/90 text-iosBlue tracking-wide flex items-center space-x-0.5">
                <i class="ph-bold ph-globe-hemisphere-east text-[9px]"></i>
                <span>${tagText}</span>
              </span>
            </div>
            <p class="text-xs font-semibold text-gray-900 leading-snug tracking-tight">${ex.en}</p>
            <p class="text-[11px] text-gray-500 leading-relaxed">${ex.zh}</p>
          </div>
          <button class="tts-sentence-btn w-6 h-6 rounded-full bg-white text-gray-400 hover:text-iosBlue flex items-center justify-center shadow-xs flex-shrink-0 mt-0.5" data-en="${encodeURIComponent(ex.en)}">
            <i class="ph-bold ph-speaker-simple-high text-xs"></i>
          </button>
        `;
        examplesContainer.appendChild(exEl);
      });

      // 綁定例句朗讀按鈕
      document.querySelectorAll('.tts-sentence-btn').forEach(btn => {
        btn.onclick = (e) => {
          e.stopPropagation();
          const sentence = decodeURIComponent(btn.getAttribute('data-en'));
          speakText(sentence);
        };
      });

      // 渲染同義詞（點擊即查）
      const synContainer = document.getElementById('card-synonyms');
      synContainer.innerHTML = '';
      item.synonyms.forEach(syn => {
        const badge = document.createElement('button');
        badge.className = 'text-xs px-2.5 py-1 bg-emerald-50 text-emerald-800 rounded-lg hover:bg-emerald-100 transition active:scale-95 font-medium flex items-center space-x-1 border border-emerald-200/70 shadow-2xs';
        badge.innerHTML = `<span>${syn}</span><i class="ph ph-arrow-up-right text-[10px]"></i>`;
        badge.onclick = () => quickSearchWord(syn);
        synContainer.appendChild(badge);
      });

      // 渲染反義詞（點擊即查）
      const antContainer = document.getElementById('card-antonyms');
      antContainer.innerHTML = '';
      item.antonyms.forEach(ant => {
        const badge = document.createElement('button');
        badge.className = 'text-xs px-2.5 py-1 bg-rose-50 text-rose-800 rounded-lg hover:bg-rose-100 transition active:scale-95 font-medium flex items-center space-x-1 border border-rose-200/70 shadow-2xs';
        badge.innerHTML = `<span>${ant}</span><i class="ph ph-arrow-up-right text-[10px]"></i>`;
        badge.onclick = () => quickSearchWord(ant);
        antContainer.appendChild(badge);
      });
    }

    // 處理多詞分別解析對照表
    function renderMultiWordPanel(tokens) {
      const panel = document.getElementById('multi-word-breakdown-panel');
      const listEl = document.getElementById('multi-word-items-list');
      const countEl = document.getElementById('multi-word-count');

      if (!tokens || tokens.length <= 1) {
        panel.classList.add('hidden');
        return;
      }

      panel.classList.remove('hidden');
      countEl.textContent = `共 ${tokens.length} 個單字`;
      listEl.innerHTML = '';

      tokens.forEach((tk, idx) => {
        const matched = vocabList.find(v => v.word.toLowerCase() === tk.toLowerCase());
        const meaning = matched ? matched.chineseMeaning : `點擊即時解析「${tk}」的最相關語義`;
        const pos = matched ? matched.pos : '單字';
        const pun = matched ? matched.cantoneseMnemonic.pun : '自動諧音聯想';

        const row = document.createElement('div');
        row.className = 'bg-blue-50/50 hover:bg-blue-50 border border-blue-100/80 rounded-xl p-3 flex items-center justify-between cursor-pointer transition active:scale-[0.99] group shadow-2xs';
        row.innerHTML = `
          <div class="space-y-1 pr-2">
            <div class="flex items-center space-x-2">
              <span class="w-5 h-5 rounded-full bg-iosBlue text-white text-[10px] font-black flex items-center justify-center">${idx + 1}</span>
              <span class="font-black text-sm text-gray-900 group-hover:text-iosBlue transition">${tk}</span>
              <span class="text-[10px] font-semibold px-1.5 py-0.5 rounded bg-white text-gray-600 border border-gray-200">${pos}</span>
            </div>
            <p class="text-xs font-extrabold text-gray-800 leading-snug">
              <span class="text-gray-400 font-normal">最相關意思：</span>${meaning}
            </p>
            <p class="text-[11px] text-amber-700 font-medium">
              <i class="ph-bold ph-lightbulb mr-0.5 text-amber-500"></i>${pun}
            </p>
          </div>
          <button class="px-2.5 py-1.5 bg-white group-hover:bg-iosBlue group-hover:text-white text-iosBlue border border-blue-200 rounded-lg text-xs font-bold flex items-center space-x-1 shadow-2xs flex-shrink-0 transition">
            <span>看卡片</span>
            <i class="ph-bold ph-arrow-right text-[10px]"></i>
          </button>
        `;
        row.onclick = () => {
          quickSearchWord(tk);
        };
        listEl.appendChild(row);
      });
    }

    // 執行搜尋（支援多詞拆解）
    async function searchVocabulary(query) {
      const q = query.trim();
      if (!q) return;

      // 拆分單字（支援逗號、空格、分號等分隔）
      const tokens = q.split(/[,，;\s]+/).map(t => t.trim()).filter(Boolean);

      // 如果輸入了多個字，顯示多詞對照表
      if (tokens.length > 1) {
        renderMultiWordPanel(tokens);
      } else {
        document.getElementById('multi-word-breakdown-panel').classList.add('hidden');
      }

      // 首先尋找本地是否有完全符合的單字或片語
      const exactMatch = vocabList.find(item => item.word.toLowerCase() === q.toLowerCase());
      if (exactMatch) {
        renderVocabCard(exactMatch);
        showToast(`已載入「${exactMatch.word}」`);
        return;
      }

      // 未收錄詞彙，調用 Gemini AI 進行深度多詞拆解與分析
      const loadingState = document.getElementById('loading-state');
      const vocabCard = document.getElementById('vocab-card');
      loadingState.classList.remove('hidden');
      vocabCard.classList.add('opacity-40', 'pointer-events-none');

      try {
        const aiVocab = await fetchGeminiWordAnalysis(q);
        vocabList.unshift(aiVocab);
        renderVocabCard(aiVocab);
        renderWordbookList();
        
        // 若 AI 回傳了拆解項，同步更新多詞面板
        if (aiVocab.breakdown && aiVocab.breakdown.length > 1) {
          const breakdownTokens = aiVocab.breakdown.map(b => b.word);
          renderMultiWordPanel(breakdownTokens);
        }

        showToast(`已分別解析並生成「${aiVocab.word}」卡片！`);
      } catch (err) {
        console.error("Gemini 查詢失敗，切換至備用在地拆解:", err);
        // 本地多詞拆解回退方案
        const localBreakdown = tokens.map(t => ({
          word: t,
          pos: "單字",
          meaning: `「${t}」於當前語境之核心含義`,
          explanation: "輸入詞組之構成單元"
        }));

        const fallbackVocab = {
          id: 'v_' + Date.now(),
          word: q,
          phonetic: `/${q.toLowerCase()}/`,
          pos: tokens.length > 1 ? "phrase/words" : "word",
          chineseMeaning: `關於「${q}」之相關核心意思`,
          chineseUsage: `於地道英語會話或書面語境中常用於精準表達。`,
          breakdown: localBreakdown,
          examples: [
            { tag: "新聞語境", en: `International reports highlighted how ${q} played a key role in recent events.`, zh: `國際報導重點闡述了該詞彙在近期時態中的核心意義。` },
            { tag: "深度分析", en: `Analysts thoroughly evaluated the strategic impact of ${q}.`, zh: `分析師深入評估了相關概念所產生的深遠影響。` }
          ],
          synonyms: ["relevant expression", "contextual term"],
          antonyms: ["contrast concept"],
          cantoneseMnemonic: {
            pun: `粵音諧音：「${tokens[0].slice(0, 3)}...」`,
            story: `利用母語諧音，將 ${q} 嘅個別字音同整體意思緊扣！`,
            visualCue: `腦海中將這幾個字具象化為一個生動互動的畫面。`
          },
          favorite: false
        };
        vocabList.unshift(fallbackVocab);
        renderVocabCard(fallbackVocab);
        renderWordbookList();
        showToast(`已為你建立「${q}」多詞解析卡！`);
      } finally {
        loadingState.classList.add('hidden');
        vocabCard.classList.remove('opacity-40', 'pointer-events-none');
      }
    }

    function quickSearchWord(word) {
      document.getElementById('search-input').value = word;
      searchVocabulary(word);
      switchTab('tab-dictionary');
    }

    // 延伸記憶複習測驗生成器
    function generateQuizQuestion() {
      if (vocabList.length === 0) return;

      // 隨機選一個字
      const targetItem = vocabList[Math.floor(Math.random() * vocabList.length)];
      // 隨機決定考反義詞或同義詞（若無反義詞則考同義詞）
      const canTestAntonym = targetItem.antonyms && targetItem.antonyms.length > 0;
      const isAntonymTest = canTestAntonym ? (Math.random() > 0.45) : false;

      const relationTypeStr = isAntonymTest ? "反義詞 (Antonym)" : "同義詞 (Synonym)";
      const targetPool = isAntonymTest ? targetItem.antonyms : targetItem.synonyms;
      const oppositePool = isAntonymTest ? targetItem.synonyms : targetItem.antonyms;

      const correctAnswer = targetPool[Math.floor(Math.random() * targetPool.length)];

      // 構建 4 個選項（1 正確 + 1 對比干擾 + 2 其他字的詞）
      let options = [correctAnswer];
      if (oppositePool && oppositePool.length > 0) {
        options.push(oppositePool[0]);
      }

      // 從其他單字抓取混淆詞
      vocabList.forEach(other => {
        if (other.word !== targetItem.word && options.length < 4) {
          const cand = isAntonymTest ? other.antonyms[0] : other.synonyms[0];
          if (cand && !options.includes(cand)) {
            options.push(cand);
          }
        }
      });

      // 洗牌
      options.sort(() => Math.random() - 0.5);

      currentQuizData = {
        targetWord: targetItem.word,
        phonetic: targetItem.phonetic,
        pun: targetItem.cantoneseMnemonic.pun,
        meaning: targetItem.chineseMeaning,
        isAntonymTest: isAntonymTest,
        correctAnswer: correctAnswer,
        options: options,
        synonyms: targetItem.synonyms,
        antonyms: targetItem.antonyms
      };

      // 渲染題目標籤
      const typeBadge = document.getElementById('quiz-relation-type');
      typeBadge.textContent = `請選出該詞的「${relationTypeStr}」`;
      typeBadge.className = isAntonymTest
        ? 'inline-block px-3 py-1 bg-rose-50 text-rose-700 text-xs font-extrabold rounded-full border border-rose-200'
        : 'inline-block px-3 py-1 bg-emerald-50 text-emerald-700 text-xs font-extrabold rounded-full border border-emerald-200';

      document.getElementById('quiz-target-word').textContent = targetItem.word;
      document.getElementById('quiz-word-hint').textContent = `音標: ${targetItem.phonetic} • 粵音口訣: ${targetItem.cantoneseMnemonic.pun}`;

      const optionsContainer = document.getElementById('quiz-options-container');
      optionsContainer.innerHTML = '';

      options.forEach(opt => {
        const btn = document.createElement('button');
        btn.className = 'w-full p-3.5 bg-white border border-gray-200/90 rounded-xl text-left font-bold text-sm text-gray-800 hover:border-iosBlue hover:bg-blue-50/30 transition flex items-center justify-between active:scale-[0.99] shadow-2xs';
        btn.innerHTML = `<span>${opt}</span><i class="ph ph-circle text-gray-300 text-lg option-icon"></i>`;
        btn.onclick = () => selectQuizOption(opt);
        optionsContainer.appendChild(btn);
      });

      document.getElementById('quiz-feedback-box').classList.add('hidden');
    }

    // 檢查作答結果
    function selectQuizOption(selectedOpt) {
      if (!currentQuizData) return;

      const isCorrect = selectedOpt === currentQuizData.correctAnswer;
      const feedbackBox = document.getElementById('quiz-feedback-box');
      const feedbackTitle = document.getElementById('feedback-title');
      const feedbackDetail = document.getElementById('feedback-detail');

      // 鎖定所有選項按鈕並標示顏色
      const allButtons = document.querySelectorAll('#quiz-options-container button');
      allButtons.forEach(b => {
        b.disabled = true;
        const text = b.querySelector('span').textContent;
        const icon = b.querySelector('.option-icon');

        if (text === currentQuizData.correctAnswer) {
          b.className = 'w-full p-3.5 bg-emerald-50 border-2 border-emerald-500 rounded-xl text-left font-bold text-sm text-emerald-950 flex items-center justify-between shadow-2xs';
          icon.className = 'ph-fill ph-check-circle text-emerald-600 text-xl';
        } else if (text === selectedOpt && !isCorrect) {
          b.className = 'w-full p-3.5 bg-rose-50 border-2 border-rose-500 rounded-xl text-left font-bold text-sm text-rose-950 flex items-center justify-between shadow-2xs';
          icon.className = 'ph-fill ph-x-circle text-rose-600 text-xl';
        }
      });

      playTapAudio(isCorrect);

      if (isCorrect) {
        quizScore += 10;
        quizStreak += 1;
        document.getElementById('quiz-score-badge').textContent = `得分: ${quizScore}`;
        document.getElementById('quiz-streak').textContent = `${quizStreak} 連勝`;
        feedbackBox.className = 'rounded-xl p-4 bg-emerald-50 border border-emerald-200 text-emerald-900 transition-all';
        feedbackTitle.className = 'flex items-center space-x-2 font-bold text-sm text-emerald-800';
        feedbackTitle.innerHTML = `<i class="ph-bold ph-check-circle text-lg text-emerald-600"></i><span>答啱咗！好嘢！🎉（延伸連結建立成功）</span>`;
      } else {
        quizStreak = 0;
        document.getElementById('quiz-streak').textContent = `0 連勝`;
        feedbackBox.className = 'rounded-xl p-4 bg-rose-50 border border-rose-200 text-rose-900 transition-all';
        feedbackTitle.className = 'flex items-center space-x-2 font-bold text-sm text-rose-800';
        feedbackTitle.innerHTML = `<i class="ph-bold ph-warning-circle text-lg text-rose-600"></i><span>再諗諗，睇下對照網絡！💪</span>`;
      }

      feedbackDetail.innerHTML = `
        <strong>「${currentQuizData.targetWord}」</strong>的意思為「${currentQuizData.meaning}」。<br>
        • 正確${currentQuizData.isAntonymTest ? '反義詞' : '同義詞'}是 <strong>${currentQuizData.correctAnswer}</strong>。<br>
        • 同義詞彙網：<span class="text-emerald-700 font-medium">${currentQuizData.synonyms.join(', ')}</span><br>
        • 反義對照網：<span class="text-rose-700 font-medium">${currentQuizData.antonyms.join(', ')}</span>
      `;

      feedbackBox.classList.remove('hidden');
    }

    // 渲染生字簿列表
    function renderWordbookList() {
      const listEl = document.getElementById('wordbook-list');
      const countEl = document.getElementById('wordbook-count');
      listEl.innerHTML = '';

      let filtered = vocabList;
      if (activeFilter === 'fav') {
        filtered = vocabList.filter(item => item.favorite);
      }

      countEl.textContent = `共 ${filtered.length} 個`;

      if (filtered.length === 0) {
        listEl.innerHTML = `
          <div class="bg-white rounded-xl p-8 border border-gray-100 text-center space-y-2">
            <i class="ph-bold ph-star text-3xl text-gray-300"></i>
            <p class="text-xs font-medium text-gray-400">目前沒有符合篩選的詞彙</p>
          </div>
        `;
        return;
      }

      filtered.forEach(item => {
        const card = document.createElement('div');
        card.className = 'bg-white rounded-xl p-3.5 border border-gray-100 ios-card-shadow flex items-center justify-between hover:border-iosBlue transition cursor-pointer active:bg-gray-50';
        card.innerHTML = `
          <div class="space-y-0.5 pr-2">
            <div class="flex items-center space-x-2">
              <span class="font-bold text-sm text-gray-900">${item.word}</span>
              <span class="text-[10px] text-gray-400 font-mono">${item.phonetic}</span>
              ${item.favorite ? '<i class="ph-fill ph-star text-amber-400 text-xs"></i>' : ''}
            </div>
            <p class="text-xs text-gray-600 font-medium">${item.chineseMeaning}</p>
            <p class="text-[11px] text-iosOrange font-semibold">口訣：${item.cantoneseMnemonic.pun}</p>
          </div>
          <div class="flex items-center space-x-2">
            <button class="w-8 h-8 rounded-full bg-gray-50 hover:bg-amber-50 text-gray-400 hover:text-amber-500 flex items-center justify-center toggle-fav-btn" data-id="${item.id}">
              <i class="ph-fill ph-star ${item.favorite ? 'text-amber-400' : 'text-gray-300'} text-sm"></i>
            </button>
            <i class="ph-bold ph-caret-right text-gray-300 text-sm"></i>
          </div>
        `;

        // 點擊卡片跳轉查詞
        card.onclick = (e) => {
          if (e.target.closest('.toggle-fav-btn')) return;
          renderVocabCard(item);
          switchTab('tab-dictionary');
        };

        // 星標按鈕
        const favBtn = card.querySelector('.toggle-fav-btn');
        favBtn.onclick = (e) => {
          e.stopPropagation();
          item.favorite = !item.favorite;
          renderWordbookList();
          if (currentWordItem.id === item.id) {
            renderVocabCard(item);
          }
          showToast(item.favorite ? `已將「${item.word}」加入精選星標 ⭐` : `已取消星標`);
        };

        listEl.appendChild(card);
      });
    }

    // 分頁切換
    function switchTab(tabId) {
      document.getElementById('tab-dictionary').classList.add('hidden');
      document.getElementById('tab-review').classList.add('hidden');
      document.getElementById('tab-wordbook').classList.add('hidden');

      document.getElementById(tabId).classList.remove('hidden');

      // 更新 Tab 按鈕外觀
      document.querySelectorAll('.nav-tab-btn').forEach(btn => {
        if (btn.getAttribute('data-tab') === tabId) {
          btn.classList.remove('text-gray-400');
          btn.classList.add('text-iosBlue');
        } else {
          btn.classList.remove('text-iosBlue');
          btn.classList.add('text-gray-400');
        }
      });

      if (tabId === 'tab-review') {
        generateQuizQuestion();
      }
    }

    // 初始化事件監聽
    document.addEventListener('DOMContentLoaded', () => {
      renderVocabCard(vocabList[0]);
      renderWordbookList();

      // 底欄 Tab 按鈕切換
      document.querySelectorAll('.nav-tab-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          switchTab(btn.getAttribute('data-tab'));
        });
      });

      // 搜尋觸發
      document.getElementById('search-action-btn').addEventListener('click', () => {
        searchVocabulary(document.getElementById('search-input').value);
      });

      document.getElementById('search-input').addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
          searchVocabulary(e.target.value);
        }
      });

      // 推薦標籤晶片
      document.querySelectorAll('.chip-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          quickSearchWord(btn.textContent.trim());
        });
      });

      // 收藏切換
      const bookmarkBtn = document.getElementById('bookmark-btn');
      bookmarkBtn.addEventListener('click', () => {
        currentWordItem.favorite = !currentWordItem.favorite;
        renderVocabCard(currentWordItem);
        renderWordbookList();
        showToast(currentWordItem.favorite ? "已加入精選生字簿 ⭐" : "已從精選移除");
      });

      // 複習下一題
      document.getElementById('quiz-next-btn').addEventListener('click', () => {
        generateQuizQuestion();
      });

      // 生字庫篩選按鈕
      document.getElementById('filter-all-btn').addEventListener('click', (e) => {
        activeFilter = 'all';
        e.target.className = "px-3 py-1 bg-iosBlue text-white font-semibold rounded-lg shadow-2xs";
        document.getElementById('filter-fav-btn').className = "px-3 py-1 bg-white border border-gray-200 text-gray-600 font-medium rounded-lg active:bg-gray-50";
        renderWordbookList();
      });

      document.getElementById('filter-fav-btn').addEventListener('click', (e) => {
        activeFilter = 'fav';
        e.target.className = "px-3 py-1 bg-iosBlue text-white font-semibold rounded-lg shadow-2xs";
        document.getElementById('filter-all-btn').className = "px-3 py-1 bg-white border border-gray-200 text-gray-600 font-medium rounded-lg active:bg-gray-50";
        renderWordbookList();
      });

      // 新增單字彈窗
      const modal = document.getElementById('add-modal');
      document.getElementById('add-word-btn').addEventListener('click', () => {
        modal.classList.remove('hidden');
      });
      document.getElementById('close-modal-btn').addEventListener('click', () => {
        modal.classList.add('hidden');
      });
      document.getElementById('cancel-add-btn').addEventListener('click', () => {
        modal.classList.add('hidden');
      });

      // 確認儲存自定義詞彙
      document.getElementById('confirm-add-btn').addEventListener('click', () => {
        const word = document.getElementById('input-new-word').value.trim();
        const meaning = document.getElementById('input-new-meaning').value.trim();
        const pun = document.getElementById('input-new-pun').value.trim();
        const synStr = document.getElementById('input-new-syn').value.trim();
        const antStr = document.getElementById('input-new-ant').value.trim();

        if (!word || !meaning) {
          showToast("請至少填寫單字及中文意思！");
          return;
        }

        const newVocab = {
          id: 'v_' + Date.now(),
          word: word,
          phonetic: `/${word.toLowerCase()}/`,
          pos: "expression",
          chineseMeaning: meaning,
          chineseUsage: "使用者自定義收錄詞彙。",
          breakdown: [
            { word: word, pos: "詞", meaning: meaning, explanation: "使用者手動收錄" }
          ],
          examples: [
            { tag: "學習例句", en: `Remember to practice using ${word} in context.`, zh: `記得在語境中多加練習使用這個詞彙。` },
            { tag: "語境運用", en: `He grasped the meaning of ${word} immediately.`, zh: `他立刻掌握了這個詞彙的意思。` }
          ],
          synonyms: synStr ? synStr.split(',').map(s => s.trim()).filter(Boolean) : ["related term"],
          antonyms: antStr ? antStr.split(',').map(s => s.trim()).filter(Boolean) : ["contrary term"],
          cantoneseMnemonic: {
            pun: pun || `「${word.slice(0, 3)}」口訣`,
            story: `利用母語諧音，將 ${word} 牢牢印在記憶深處！`,
            visualCue: `在腦海中構思屬於這個詞彙的生動畫面。`
          },
          favorite: true
        };

        vocabList.unshift(newVocab);
        renderWordbookList();
        renderVocabCard(newVocab);
        modal.classList.add('hidden');
        showToast(`已成功加入「${word}」！`);

        document.getElementById('input-new-word').value = '';
        document.getElementById('input-new-meaning').value = '';
        document.getElementById('input-new-pun').value = '';
        document.getElementById('input-new-syn').value = '';
        document.getElementById('input-new-ant').value = '';
      });
    });
  </script>
</body>
</html>
