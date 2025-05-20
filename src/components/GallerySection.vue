<template>
  <section id="gallery" class="content-section">
    <h2 class="section-title">猜猜我们在哪儿 <span class="heart-icon">🌍</span></h2>
    <p class="gallery-intro">每张照片背后都有一个故事，和一段独特的记忆。试试看，你还记得这些地方吗？</p>
    <p class="gallery-tip">提示：地点请输入拼音，例如 "beijing" 🎐</p>

    <div class="travel-timeline" v-if="interactiveTravelStops.length > 0">
      <div v-for="(stop, index) in interactiveTravelStops" :key="stop.id"
           :class="['timeline-item', index % 2 === 0 ? 'timeline-item-left' : 'timeline-item-right']">
        <div class="timeline-point"></div>
        <div class="timeline-content">
          <div class="image-container" @click="openImageModal(stop.imageUrl)" title="点击放大查看">
            <img :src="stop.imageUrl" :alt="stop.location || 'Travel photo'" @error="imageLoadError"/>
          </div>

          <div class="info-container">
            <div v-if="!stop.isGuessed" class="guess-area">
              <input
                  type="text"
                  v-model="stop.userInput"
                  :placeholder="`猜猜这是哪儿? (${stop.guessableLocation.length}个字母)`"
                  @keyup.enter="checkGuess(stop)"
                  class="guess-input"
              />
              <button @click="checkGuess(stop)" class="guess-button">猜!</button>
              <p v-if="stop.showIncorrect" class="feedback incorrect-feedback">
                哎呀，不对哦，再想想？🤔
              </p>
            </div>

            <div v-if="stop.isGuessed" class="revealed-info">
              <p v-if="stop.showReward" class="feedback correct-feedback">
                🎉 答对啦！太棒了！ 🎉
              </p>
              <span class="date">{{ stop.date }}</span>
              <h3 class="location">{{ stop.location }}</h3>
              <p class="description" v-if="stop.description">{{ stop.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="empty-gallery">
      <p>还没有添加旅行足迹哦，快来记录你们的甜蜜旅程吧！</p>
    </div>

    <div v-if="allGuessed" class="all-guessed-message">
      <h2>🎉 恭喜你！全部猜对啦！不愧是你！ 🏆</h2>
      <p>这些回忆，我们都一起珍藏！</p>
    </div>

    <div v-if="isModalOpen" class="image-modal-overlay" @click.self="closeImageModal">
      <div class="image-modal-content">
        <img :src="enlargedImageUrl" alt="放大的旅行照片" @click.stop />
        <button class="close-modal-button" @click="closeImageModal" title="关闭">&times;</button>
      </div>
    </div>

  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed } from 'vue';

// --- 【重要】在这里填充你的旅行足迹数据 ---
// ---  并为每个地点添加 'guessableLocation' (小写拼音，无空格) ---
const rawTravelStops = [
  {
    id: 1,
    imageUrl: '/img01_220910.png',
    date: '2022-09-10',
    location: '[第一次牵手·📍jinan]',
    description: '那天风也温柔，你也温柔。',
    guessableLocation: 'jinan'
  },
  {
    id: 2,
    imageUrl: '/img01_220910.jpg', // Assuming this is a different moment for a "first movie"
    date: '2022-09-10',
    location: '[第一次电影·📍jinan]',
    description: '电影情节忘了，但爆米花很甜。',
    guessableLocation: 'jinan'
  },
  {
    id: 3,
    imageUrl: '/img_4.png',
    date: '2022-09-13',
    location: '[某一把游戏·📍jinan]', // Assuming this is a distinct memory related to Jinan
    description: '峡谷重要，你更重要！',
    guessableLocation: 'jinan'
  },
  {
    id: 4,
    imageUrl: '/img02_230430.png',
    date: '2023-04-30',
    location: '[另一次牵手·📍tai‘an]',
    description: '泰山脚下，心意相连。',
    guessableLocation: 'taian' // Corrected to 'taian'
  },
  {
    id: 5,
    imageUrl: '/img03_230502.png',
    date: '2023-05-02',
    location: '[笔芯·📍tai‘an]',
    description: '爱你的形状❤️。',
    guessableLocation: 'taian' // Corrected to 'taian'
  },
  {
    id: 6,
    imageUrl: '/image2.jpg',
    date: '2023-06-05',
    location: '[千佛山的晚霞·📍jinan]',
    description: '晚霞和你一样，美得不像话。',
    guessableLocation: 'jinan'
  },
  {
    id: 7,
    imageUrl: '/img_3.png',
    date: '2023-07-01',
    location: '[去吃烧烤啦·📍zibo]',
    description: '滋滋作响的快乐！',
    guessableLocation: 'zibo'
  },
  {
    id: 8,
    imageUrl: '/img_5.png',
    date: '2024-05-01',
    location: '[海边·📍qingdao]',
    description: '海风捎来你的笑声。',
    guessableLocation: 'qingdao'
  },
  {
    id: 9,
    imageUrl: '/img_6.png',
    date: '2024-05-01',
    location: '[海边·📍qingdao]',
    description: '沙滩上的脚印，一步一步都是爱。',
    guessableLocation: 'qingdao'
  },
  {
    id: 10,
    imageUrl: '/img_2.png',
    date: '2024-05-01',
    location: '[海边·📍qingdao]',
    description: '浪花朵朵，不及你眼中星光闪烁。',
    guessableLocation: 'qingdao'
  },
  {
    id: 11,
    imageUrl: '/img.png',
    date: '2025-05-03', // This date is in the future, but let's assume it's a planned or past event for the game
    location: '[路边·📍jinan]',
    description: '随便走走也很开心。',
    guessableLocation: 'jinan'
  },
];

const interactiveTravelStops = ref(
    rawTravelStops.map(stop => ({
      ...stop,
      userInput: '',
      isGuessed: false,
      showReward: false,
      showIncorrect: false,
    }))
);
// --- 数据填充结束 ---

function imageLoadError(event) {
  console.warn("Timeline Image Load Error:", event.target.src);
  event.target.style.border = '1px solid var(--secondary-color)';
  event.target.alt = "图片加载失败";
}

// --- Image Modal Logic ---
const isModalOpen = ref(false);
const enlargedImageUrl = ref('');

function openImageModal(imageUrl) {
  if (!imageUrl || imageUrl.toLowerCase().includes('placeholder')) {
    console.log("Placeholder or invalid image clicked, modal not opened.");
    return;
  }
  enlargedImageUrl.value = imageUrl;
  isModalOpen.value = true;
  document.body.style.overflow = 'hidden';
}

function closeImageModal() {
  isModalOpen.value = false;
  enlargedImageUrl.value = '';
  document.body.style.overflow = '';
}

function handleKeydown(event) {
  if (event.key === 'Escape' && isModalOpen.value) {
    closeImageModal();
  }
}

// --- Game Logic ---
function checkGuess(stop) {
  if (stop.userInput.trim().toLowerCase() === stop.guessableLocation.toLowerCase()) {
    stop.isGuessed = true;
    stop.showReward = true;
    stop.showIncorrect = false; // Hide incorrect message if it was shown
    setTimeout(() => {
      stop.showReward = false;
    }, 3000); // Reward message disappears after 3 seconds
  } else {
    stop.showIncorrect = true;
    stop.userInput = ''; // Clear input on incorrect guess
    setTimeout(() => {
      stop.showIncorrect = false;
    }, 2000); // Incorrect message disappears
  }
}

const allGuessed = computed(() => {
  if (interactiveTravelStops.value.length === 0) return false;
  return interactiveTravelStops.value.every(stop => stop.isGuessed);
});


onMounted(() => {
  window.addEventListener('keydown', handleKeydown);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeydown);
  if (isModalOpen.value) {
    document.body.style.overflow = '';
  }
});
</script>

<style scoped>
/* Existing styles from your original component should largely work */
/* Add new styles or modify existing ones as needed */

.gallery-intro, .gallery-tip {
  text-align: center;
  margin-bottom: 15px;
  color: #555;
}

.gallery-tip {
  font-size: 0.9em;
  color: #777;
  margin-bottom: 40px;
}

.travel-timeline {
  position: relative;
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px 0;
}

.travel-timeline::after {
  content: '';
  position: absolute;
  width: 4px;
  background-color: var(--secondary-color, #ff8a80); /* Added fallback */
  top: 0;
  bottom: 0;
  left: 50%;
  margin-left: -2px;
  z-index: 1;
}

.timeline-item {
  padding: 10px 40px;
  position: relative;
  background-color: inherit;
  width: 50%;
  box-sizing: border-box;
  z-index: 2;
  margin-bottom: 20px; /* Add some space between items */
}

.timeline-point {
  content: '';
  position: absolute;
  width: 18px;
  height: 18px;
  background-color: white;
  border: 4px solid var(--primary-color, #ff80ab); /* Added fallback */
  top: 35px;
  border-radius: 50%;
  z-index: 3;
}

.timeline-item-left { left: 0; }
.timeline-item-left .timeline-point { right: -9px; }
.timeline-item-left .timeline-content { text-align: right; padding-right: 25px; }
/* For guess area, we might want consistent text alignment */
.timeline-item-left .guess-area { text-align: left; } /* Adjust if needed */


.timeline-item-right { left: 50%; }
.timeline-item-right .timeline-point { left: -9px; }
.timeline-item-right .timeline-content { text-align: left; padding-left: 25px; }

.timeline-content {
  padding: 20px; /* Increased padding for more space */
  background-color: white;
  position: relative;
  border-radius: 8px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1); /* Slightly enhanced shadow */
  overflow: visible; /* Allow feedback messages to peek out if absolutely positioned */
  transition: transform 0.3s ease;
}
.timeline-content:hover {
  transform: translateY(-5px) scale(1.01); /* Enhanced hover */
}

.image-container {
  border-radius: 6px;
  overflow: hidden;
  margin-bottom: 15px; /* Spacing between image and guess/info area */
  max-width: 100%;
  cursor: pointer;
  transition: transform 0.2s ease-in-out;
  border: 1px solid #eee; /* Subtle border for image */
}
.image-container:hover {
  transform: scale(1.03);
}

.timeline-item-left .image-container, .timeline-item-right .image-container {
  /* Adjusted to be more flexible, or set a fixed height if desired */
  width: auto; /* Let it be natural or control via max-width on timeline-content */
  max-height: 250px; /* Example max height */
  display: flex; /* For centering image if it's smaller than container */
  justify-content: center;
  align-items: center;
}

.timeline-content img {
  max-width: 100%; /* Ensure image is responsive within its container */
  max-height: 250px; /* Match container's max height */
  height: auto;
  display: block;
  object-fit: cover; /* Or 'contain' if you want to see the whole image */
  background-color: var(--light-gray, #f0f0f0); /* Added fallback */
}

.info-container {
  margin-top: 10px;
}

.guess-area {
  display: flex;
  flex-direction: column;
  align-items: stretch; /* Make input and button full width of their area */
  gap: 10px; /* Space between input and button */
}

.guess-input {
  padding: 10px 12px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1em;
  transition: border-color 0.3s ease;
}
.guess-input:focus {
  border-color: var(--primary-color, #ff80ab);
  outline: none;
  box-shadow: 0 0 5px rgba(255, 128, 171, 0.5);
}

.guess-button {
  padding: 10px 15px;
  background-color: var(--primary-color, #ff80ab);
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1em;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}
.guess-button:hover {
  background-color: var(--accent-color, #ff4081); /* Added fallback for accent */
  transform: translateY(-2px);
}

.feedback {
  padding: 8px 12px;
  border-radius: 4px;
  margin-top: 10px;
  text-align: center;
  font-weight: bold;
  animation: fadeIn 0.5s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

.correct-feedback {
  background-color: #e6ffed; /* Light green */
  color: #2f855a; /* Dark green */
  border: 1px solid #9ae6b4; /* Green border */
}

.incorrect-feedback {
  background-color: #fff5f5; /* Light red */
  color: #c53030; /* Dark red */
  border: 1px solid #fc8181; /* Red border */
}

.revealed-info {
  animation: fadeIn 0.5s ease;
}

.timeline-content .date {
  display: block;
  font-size: 0.85em;
  color: var(--primary-color, #ff80ab);
  font-weight: bold;
  margin-bottom: 5px;
}
.timeline-content .location {
  font-size: 1.3em;
  color: var(--accent-color, #ff4081);
  margin-top: 0;
  margin-bottom: 8px;
  font-weight: 600;
}
.timeline-content .description {
  font-size: 0.95em;
  line-height: 1.6;
  color: #444;
  margin-bottom: 0;
}

.timeline-item::before {
  content: " "; height: 0; position: absolute; top: 38px; width: 0; z-index: 2; border: medium solid white;
}
.timeline-item-left::before { right: 30px; border-width: 10px 0 10px 10px; border-color: transparent transparent transparent white; }
.timeline-item-right::before { left: 30px; border-width: 10px 10px 10px 0; border-color: transparent white transparent transparent; }

.empty-gallery { text-align: center; padding: 40px 20px; color: #777; }
.empty-gallery p { font-size: 1.1em; }

.all-guessed-message {
  text-align: center;
  padding: 50px 20px;
  margin-top: 30px;
  background-color: #e6ffed;
  border-radius: 10px;
  color: #2f855a;
  border: 2px dashed var(--primary-color, #ff80ab);
}
.all-guessed-message h2 {
  color: var(--accent-color, #ff4081);
  margin-bottom: 15px;
}

/* --- Image Modal Styles --- */
.image-modal-overlay {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background-color: rgba(0, 0, 0, 0.85);
  display: flex; justify-content: center; align-items: center;
  z-index: 1050; padding: 20px; box-sizing: border-box;
  animation: fadeInModal 0.3s ease-out;
}

@keyframes fadeInModal {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}

.image-modal-content {
  position: relative; background-color: #fff; padding: 4px;
  border-radius: 6px; box-shadow: 0 10px 30px rgba(0,0,0,0.3);
  max-width: calc(100vw - 40px); max-height: calc(100vh - 40px);
  display: flex; justify-content: center; align-items: center;
}

.image-modal-content img {
  display: block; max-width: 100%; max-height: 100%;
  object-fit: contain; border-radius: 3px;
}

.close-modal-button {
  position: absolute; top: -18px; right: -18px;
  background-color: white; color: var(--primary-color, #ff80ab);
  border: none; border-radius: 50%;
  width: 36px; height: 36px; font-size: 28px; line-height: 34px;
  text-align: center; font-weight: 300; cursor: pointer;
  box-shadow: 0 2px 8px rgba(0,0,0,0.25);
  transition: transform 0.2s ease, background-color 0.2s ease;
  z-index: 10;
}
.close-modal-button:hover {
  transform: scale(1.1) rotate(90deg); background-color: #f1f1f1;
}
/* --- End Image Modal Styles --- */


@media screen and (max-width: 768px) {
  .travel-timeline::after { left: 20px; margin-left: 0; }
  .timeline-item { width: 100%; padding-left: 50px; padding-right: 15px; margin-bottom: 30px; }
  .timeline-item-left, .timeline-item-right { left: 0; }
  .timeline-item-left .timeline-content, .timeline-item-right .timeline-content {
    text-align: left; padding-left: 15px; padding-right: 15px;
  }
  .timeline-item-left .guess-area { text-align: left; } /* Ensure consistency */

  .timeline-item-left .timeline-content .image-container,
  .timeline-item-right .timeline-content .image-container {
    float: none; width: 100%; margin-left: 0; margin-right: 0; margin-bottom: 15px;
    max-height: 200px; /* Adjust for smaller screens */
  }
  .timeline-content img {
    max-height: 200px; /* Adjust for smaller screens */
  }

  .timeline-point { left: 11px; }
  .timeline-item-left::before, .timeline-item-right::before {
    left: 40px; border-width: 10px 10px 10px 0; border-color: transparent white transparent transparent;
  }

  .image-modal-content {
    max-width: calc(100vw - 20px); max-height: calc(100vh - 20px); padding: 2px;
  }
  .close-modal-button {
    top: -10px; right: -10px; width: 30px; height: 30px; font-size: 22px; line-height: 28px;
  }

  .all-guessed-message { padding: 30px 15px; }
  .all-guessed-message h2 { font-size: 1.5em; }
}
</style>