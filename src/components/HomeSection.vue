<template>
  <section id="home" class="content-section">
    <h2 class="section-title">亲爱的 <span class="highlight">{{ girlfriendName }}</span>，520快乐！</h2>
    <p>在这个特别的日子里，我的心里充满了对你的<span class="easter-egg-trigger-word" @click="onLoveWordClick" title="轻点三下，看看我的心声 ❤️">爱意</span>与感激。 <span class="heart-icon">💖</span></p>
    <p>从我们相遇的那一刻起，我的世界就因你而变得更加精彩。你的笑容，你的温柔，你的一切，都深深吸引着我。希望这个小小的页面，能带给你一丝丝的惊喜和温暖。</p>
    <p>愿我们的爱情，像永不凋谢的花朵，永远鲜艳芬芳。 <span class="heart-icon">🌹</span></p>

    <Transition name="fade-bounce">
      <div v-if="isLoveMessageVisible" class="secret-message love-message">
        <span class="heart-icon">💖</span> 你点亮了我的心！mua~ <span class="heart-icon">💖</span>
        <p style="font-size: 0.8em; margin-top: 5px;">(这个消息5秒后消失哦)</p>
      </div>
    </Transition>

    <Transition name="fade-scale">
      <div v-if="isSecretCodeMessageVisible" class="secret-message secret-code-message">
        <h2>🎉 秘密指令生效 🎉</h2>
        <p>你解锁了隐藏的爱心祝福！</p>
        <p>送你漫天小心心！<span class="animated-heart">💕</span><span class="animated-heart delay1">💕</span><span class="animated-heart delay2">💕</span></p>
        <p style="font-size: 0.8em; margin-top: 10px;">(7秒后自动关闭)</p>
      </div>
    </Transition>

    <div class="secret-spot" @click="onSecretSpotClick" title="一个闪亮的小发现..."></div>
    <Transition name="slide-right">
      <div v-if="isSecretSpotMessageVisible" class="secret-message spot-message special-surprise">
        <h4>✨ 秘密通道启动 ✨</h4>
        <p>恭喜你发现了给宝宝的专属福利！</p>
        <div class="coupon">
          <h5>💖 免做家务一天券 💖</h5>
          <p class="coupon-details">凭此券可指定任意一天，Jerry先生包揽所有家务！</p>
          <p class="coupon-expiry">有效期：永远有效</p>
        </div>
        <p style="font-size: 0.8em; margin-top: 10px;">(这个惊喜10秒后消失哦)</p>
      </div>
    </Transition>

    <div class="konami-code-hint" title="还记得那些经典的操作吗？也许有惊喜哦！">
      提示：<span class="key-symbol">↑</span><span class="key-symbol">↑</span><span class="key-symbol">↓</span><span class="key-symbol">↓</span>...？
    </div>

  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, defineProps } from 'vue';

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  girlfriendName: String
});

// --- 彩蛋1: "心有灵犀"点击 ---
const loveClickCount = ref(0);
const isLoveMessageVisible = ref(false);
let loveMessageTimeout = null;

function onLoveWordClick() {
  loveClickCount.value++;
  if (loveClickCount.value >= 3) {
    if (loveMessageTimeout) clearTimeout(loveMessageTimeout);
    isLoveMessageVisible.value = true;
    loveMessageTimeout = setTimeout(() => {
      isLoveMessageVisible.value = false;
      loveClickCount.value = 0;
    }, 5000);
  }
}

// --- 彩蛋2: "秘密指令" ---
const secretCodeSequence = ['arrowup', 'arrowup', 'arrowdown', 'arrowdown', 'arrowleft', 'arrowright','arrowleft', 'arrowright',
  'b', 'a', 'b', 'a'];
const currentSecretCodeIndex = ref(0);
const isSecretCodeMessageVisible = ref(false);
let secretCodeMessageTimeout = null;

function handleSecretCodeKey(event) {
  if (isSecretCodeMessageVisible.value) return;

  const requiredKey = secretCodeSequence[currentSecretCodeIndex.value];
  if (event.key.toLowerCase() === requiredKey) {
    currentSecretCodeIndex.value++;
    if (currentSecretCodeIndex.value === secretCodeSequence.length) {
      if (secretCodeMessageTimeout) clearTimeout(secretCodeMessageTimeout);
      isSecretCodeMessageVisible.value = true;
      currentSecretCodeIndex.value = 0;
      secretCodeMessageTimeout = setTimeout(() => {
        isSecretCodeMessageVisible.value = false;
      }, 7000);
    }
  } else {
    currentSecretCodeIndex.value = 0;
    // 如果希望严格匹配第一个键，可以取消注释下面这段
    // if (event.key.toLowerCase() === secretCodeSequence[0]) {
    //   currentSecretCodeIndex.value = 1;
    // } else {
    //   currentSecretCodeIndex.value = 0;
    // }
  }
}

// --- 彩蛋3: "神秘角落" ---
const isSecretSpotMessageVisible = ref(false);
let secretSpotMessageTimeout = null;

function onSecretSpotClick() {
  isSecretSpotMessageVisible.value = !isSecretSpotMessageVisible.value;
  if (secretSpotMessageTimeout) clearTimeout(secretSpotMessageTimeout);

  if (isSecretSpotMessageVisible.value) {
    secretSpotMessageTimeout = setTimeout(() => {
      isSecretSpotMessageVisible.value = false;
    }, 4000);
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleSecretCodeKey);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleSecretCodeKey);
  if (loveMessageTimeout) clearTimeout(loveMessageTimeout);
  if (secretCodeMessageTimeout) clearTimeout(secretCodeMessageTimeout);
  if (secretSpotMessageTimeout) clearTimeout(secretSpotMessageTimeout);
});
</script>

<style scoped>
.content-section {
  position: relative;
  padding-bottom: 70px; /* 为彩蛋提示和神秘角落留出更多空间 */
}

/* 彩蛋1提示优化 */
.easter-egg-trigger-word {
  cursor: help;
  font-weight: bold;
  color: var(--accent-color);
  user-select: none;
  border-bottom: 1px dashed var(--accent-color);
  transition: color 0.2s, border-bottom-color 0.2s, text-shadow 0.3s;
  animation: subtle-pulse 2.5s infinite ease-in-out; /* 添加呼吸动画 */
}
.easter-egg-trigger-word:hover {
  color: var(--primary-color);
  border-bottom-color: var(--primary-color);
  animation-play-state: paused; /* 悬停时暂停动画 */
  text-shadow: 0 0 5px var(--secondary-color);
}
@keyframes subtle-pulse {
  0%, 100% { text-shadow: 0 0 3px rgba(233, 30, 99, 0.2); } /* 使用 accent color 的浅色阴影 */
  50% { text-shadow: 0 0 8px rgba(233, 30, 99, 0.5); }
}


/* 彩蛋2提示：秘密指令提示 */
.konami-code-hint {
  position: absolute;
  bottom: 10px;
  left: 15px; /* 放在左下角 */
  font-size: 0.75em;
  color: #aaa; /* 非常浅的灰色，不显眼 */
  padding: 3px 6px;
  background-color: rgba(255, 255, 255, 0.7); /* 半透明背景，以防和页面背景冲突 */
  border-radius: 3px;
  user-select: none;
  cursor: help;
  transition: color 0.3s;
}
.konami-code-hint:hover {
  color: #666; /* 悬停时变深一点 */
}
.key-symbol {
  display: inline-block;
  padding: 1px 3px;
  border: 1px solid #ccc;
  border-radius: 2px;
  margin: 0 1px;
  background-color: #f0f0f0;
  color: #555;
  font-family: 'Courier New', Courier, monospace; /* 更像按键的字体 */
}


/* 彩蛋3提示优化 */
.secret-spot {
  position: absolute;
  bottom: 15px;
  right: 15px;
  width: 12px;
  height: 12px;
  background-color: rgba(255, 182, 193, 0.5); /* 初始更淡一点 */
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.3s, box-shadow 0.3s;
  z-index: 10;
  animation: gentle-shimmer 3s infinite ease-in-out 1s; /* 延迟1秒开始闪烁 */
}
.secret-spot:hover {
  background-color: rgba(255, 105, 180, 0.8);
  transform: scale(1.4);
  box-shadow: 0 0 10px rgba(255, 105, 180, 0.5);
  animation-play-state: paused;
}
@keyframes gentle-shimmer {
  0%, 100% { box-shadow: 0 0 5px rgba(255, 105, 180, 0.2); opacity: 0.7; }
  50% { box-shadow: 0 0 15px rgba(255, 105, 180, 0.6); opacity: 1; }
}


/* --- 彩蛋消息框样式 (基本不变，可以根据需要调整) --- */
.secret-message {
  position: fixed;
  padding: 20px 25px;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
  text-align: center;
  z-index: 1100;
  border-left: 5px solid var(--primary-color);
  max-width: 90%;
}
.love-message {
  bottom: 40px; left: 50%; transform: translateX(-50%); background-color: #fff0f5; color: var(--accent-color); font-size: 1.15em;
}
.love-message .heart-icon { font-size: 1.2em; vertical-align: middle; margin: 0 5px; }
.secret-code-message {
  top: 50%; left: 50%; transform: translate(-50%, -50%); background-color: #e3f2fd; color: #0d47a1; border-left-color: #1976d2;
}
.secret-code-message h2 { color: #0d47a1; margin-top: 0; font-size: 1.5em; }
.secret-code-message p { margin-bottom: 8px; }
.animated-heart { display: inline-block; animation: beat 0.7s infinite ease-in-out; font-size: 1.8em; color: var(--heart-color); margin: 0 3px; }
.animated-heart.delay1 { animation-delay: 0.2s; }
.animated-heart.delay2 { animation-delay: 0.4s; }
.spot-message {
  bottom: 50px; right: 40px; background-color: #fffacd; color: #8b4513; border-left-color: #daa520; font-size: 1em; padding: 15px 20px;
}

/* --- Vue Transitions (基本不变) --- */
.fade-bounce-enter-active { animation: fade-bounce-in 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55); }
.fade-bounce-leave-active { animation: fade-bounce-out 0.4s ease-in; }
@keyframes fade-bounce-in {
  0% { transform: translateX(-50%) scale(0.5); opacity: 0; bottom: 10px; }
  70% { transform: translateX(-50%) scale(1.05); opacity: 0.9; bottom: 45px; }
  100% { transform: translateX(-50%) scale(1); opacity: 1; bottom: 40px; }
}
@keyframes fade-bounce-out {
  0% { transform: translateX(-50%) scale(1); opacity: 1; bottom: 40px; }
  100% { transform: translateX(-50%) scale(0.7); opacity: 0; bottom: 10px; }
}
.fade-scale-enter-active, .fade-scale-leave-active { transition: opacity 0.4s ease, transform 0.4s ease; }
.fade-scale-enter-from, .fade-scale-leave-to { opacity: 0; transform: translate(-50%, -50%) scale(0.85); }
.slide-right-enter-active { animation: slide-from-right 0.4s ease-out; }
.slide-right-leave-active { animation: slide-to-right 0.3s ease-in; }
@keyframes slide-from-right { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }
@keyframes slide-to-right { from { transform: translateX(0); opacity: 1; } to { transform: translateX(100%); opacity: 0; } }
</style>