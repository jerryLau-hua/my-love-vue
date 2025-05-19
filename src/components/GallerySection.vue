<template>
  <section id="gallery" class="content-section">
    <h2 class="section-title">我们的足迹 <span class="heart-icon">✈️</span></h2>
    <p class="gallery-intro">每一张照片，都是我们共同走过的路，珍藏的美好回忆。</p>
    <p class="gallery-tip">照片有点多，我就放几张代表性的唔🎐🎐🎐</p>

    <div class="travel-timeline" v-if="travelStops.length > 0">
      <div v-for="(stop, index) in travelStops" :key="stop.id"
           :class="['timeline-item', index % 2 === 0 ? 'timeline-item-left' : 'timeline-item-right']">
        <div class="timeline-point"></div>
        <div class="timeline-content">
          <div class="image-container" @click="openImageModal(stop.imageUrl)" title="点击放大查看">
            <img :src="stop.imageUrl" :alt="stop.location || 'Travel photo'" @error="imageLoadError"/>
          </div>
          <div class="info-container">
            <span class="date" v-if="stop.date">{{ stop.date }}</span>
            <h3 class="location">{{ stop.location || '一个美好的地方' }}</h3>
            <p class="description" v-if="stop.description">{{ stop.description }}</p>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="empty-gallery">
      <p>还没有添加旅行足迹哦，快来记录你们的甜蜜旅程吧！</p>
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
import { ref, onMounted, onBeforeUnmount } from 'vue'; // 引入 onMounted 和 onBeforeUnmount

// --- 【重要】在这里填充你的旅行足迹数据 ---
const travelStops = ref([
  {
    id: 1,
    imageUrl: '/img01_220910.png',
    date: '2022-09-10',
    location: '[第一次牵手·jinan]',
    description: ''
  },
  {
    id: 2,
    imageUrl: '/img01_220910.jpg',
    date: '2022-09-10',
    location: '[第一次电影·jinan]',
    description: ''
  },
  {
    id: 3,
    imageUrl: '/img_4.png',
    date: '2022-09-13',
    location: '[某一把游戏·jinan]',
    description: ''
  },
  {
    id: 4,
    imageUrl: '/img02_230430.png',
    date: '2023-04-30',
    location: '[另一次牵手·tai‘an]',
    description: ''
  },
  {
    id: 5,
    imageUrl: '/img03_230502.png',
    date: '2023-05-02',
    location: '[笔芯·tai‘an]',
    description: ''
  },
  {
    id: 6,
    imageUrl: '/image2.jpg',
    date: '2023-06-05',
    location: '[千佛山的晚霞·jinan]',
    description: ''
  },
  {
    id: 7,
    imageUrl: '/img_3.png',
    date: '2023-07-01',
    location: '[去吃烧烤啦·zibo]',
    description: ''
  },
  {
    id: 8,
    imageUrl: '/img_5.png',
    date: '2024-05-01',
    location: '[海边·qingdao]',
    description: ''
  },
  {
    id: 9,
    imageUrl: '/img_6.png',
    date: '2024-05-01',
    location: '[海边·qingdao]',
    description: ''
  },
  {
    id: 10,
    imageUrl: '/img_2.png',
    date: '2024-05-01',
    location: '[海边·qingdao]',
    description: ''
  },
  {
    id: 11,
    imageUrl: '/img.png',
    date: '2025-05-03',
    location: '[路边·jinan]',
    description: ''
  },
]);
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
  // 阻止为占位符或无效图片打开模态框 (根据你的图片命名规则调整)
  if (!imageUrl || imageUrl.toLowerCase().includes('placeholder')) {
    console.log("Placeholder or invalid image clicked, modal not opened.");
    return;
  }
  enlargedImageUrl.value = imageUrl;
  isModalOpen.value = true;
  document.body.style.overflow = 'hidden'; // 防止背景滚动
}

function closeImageModal() {
  isModalOpen.value = false;
  enlargedImageUrl.value = ''; // 清空图片URL
  document.body.style.overflow = ''; // 恢复背景滚动
}

// 处理Escape键关闭模态框
function handleKeydown(event) {
  if (event.key === 'Escape' && isModalOpen.value) {
    closeImageModal();
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeydown);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeydown);
  // 确保如果组件卸载时模态框是打开的，恢复滚动条
  if (isModalOpen.value) {
    document.body.style.overflow = '';
  }
});
// --- End Image Modal Logic ---
</script>

<style scoped>
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
  background-color: var(--secondary-color);
  top: 0;
  bottom: 0;
  left: 50%;
  margin-left: -2px;
  z-index: 1;
}

.timeline-item {
  padding: 10px 40px;
  position: relative;
  background-color: inherit; /* 或者 transparent */
  width: 50%;
  box-sizing: border-box;
  z-index: 2;
}

.timeline-point {
  content: '';
  position: absolute;
  width: 18px;
  height: 18px;
  background-color: white;
  border: 4px solid var(--primary-color);
  top: 35px;
  border-radius: 50%;
  z-index: 3;
}

.timeline-item-left { left: 0; }
.timeline-item-left .timeline-point { right: -9px; }
.timeline-item-left .timeline-content { text-align: right; padding-right: 25px; }
.timeline-item-left .timeline-content .image-container { float: right; margin-left: 15px; }

.timeline-item-right { left: 50%; }
.timeline-item-right .timeline-point { left: -9px; }
.timeline-item-right .timeline-content { text-align: left; padding-left: 25px; }
.timeline-item-right .timeline-content .image-container { float: left; margin-right: 15px; }

.timeline-content {
  padding: 15px;
  background-color: white;
  position: relative;
  border-radius: 8px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
  overflow: hidden;
  transition: transform 0.3s ease;
}
.timeline-content:hover { transform: translateY(-5px); }

.image-container {
  border-radius: 6px;
  overflow: hidden;
  margin-bottom: 12px;
  max-width: 100%;
  cursor: pointer; /* 添加手型光标 */
  transition: transform 0.2s ease-in-out; /* 轻微交互效果 */
}
.image-container:hover {
  transform: scale(1.03); /* 鼠标悬停时轻微放大 */
}

.timeline-item-left .image-container, .timeline-item-right .image-container {
  width: 185px;
  /* height: auto; 已在img中设置 */
}

.timeline-content img {
  width: 100%;
  height: auto;
  display: block;
  object-fit: cover; /* 改回 cover 使图片填满185px宽度容器，高度会被裁剪以保持比例 */
  /* 如果希望图片完整但容器高度不一，请保持 initial 或 contain，并确保容器高度自适应 */
  background-color: var(--light-gray);
}

.info-container { /* No specific styles needed for now */ }

.timeline-content .date { display: block; font-size: 0.85em; color: var(--primary-color); font-weight: bold; margin-bottom: 5px; }
.timeline-content .location { font-size: 1.3em; color: var(--accent-color); margin-top: 0; margin-bottom: 8px; font-weight: 600; }
.timeline-content .description { font-size: 0.95em; line-height: 1.6; color: #444; margin-bottom: 0; }

.timeline-item::before {
  content: " "; height: 0; position: absolute; top: 38px; width: 0; z-index: 2; border: medium solid white;
}
.timeline-item-left::before { right: 30px; border-width: 10px 0 10px 10px; border-color: transparent transparent transparent white; }
.timeline-item-right::before { left: 30px; border-width: 10px 10px 10px 0; border-color: transparent white transparent transparent; }

.empty-gallery { text-align: center; padding: 40px 20px; color: #777; }
.empty-gallery p { font-size: 1.1em; }

/* --- Image Modal Styles --- */
.image-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.85);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1050;
  padding: 20px;
  box-sizing: border-box;
  animation: fadeInModal 0.3s ease-out;
}

@keyframes fadeInModal {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}

.image-modal-content {
  position: relative;
  background-color: #fff; /* 可选，如果图片有透明部分，可以看到白色背景板 */
  padding: 4px; /* 图片与白色背景板之间的轻微间距 */
  border-radius: 6px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.3);
  max-width: calc(100vw - 40px);    /* 考虑overlay的padding */
  max-height: calc(100vh - 40px);   /* 考虑overlay的padding */
  display: flex;
  justify-content: center;
  align-items: center;
}

.image-modal-content img {
  display: block;
  max-width: 100%;
  max-height: 100%; /* 确保图片不会超出白色背景板 */
  /* 如果上面 .image-modal-content 已有 max-width/height 限制，
     并且图片父容器就是 .image-modal-content，这里可能可以省略，
     但显式设置更保险 */
  object-fit: contain; /* 确保完整图片在容器内显示 */
  border-radius: 3px; /* 图片本身轻微圆角 */
}

.close-modal-button {
  position: absolute;
  top: -18px;  /* 调整到图片框外部一点 */
  right: -18px; /* 调整到图片框外部一点 */
  background-color: white;
  color: var(--primary-color);
  border: none; /* 改为无边框，依赖阴影 */
  border-radius: 50%;
  width: 36px;
  height: 36px;
  font-size: 28px; /* 增大 "×" 号 */
  line-height: 34px; /* 调整使 "×" 居中 */
  text-align: center;
  font-weight: 300; /* 使 "×" 细一点 */
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0,0,0,0.25);
  transition: transform 0.2s ease, background-color 0.2s ease;
  z-index: 10; /* 确保在图片内容之上 */
}
.close-modal-button:hover {
  transform: scale(1.1) rotate(90deg);
  background-color: #f1f1f1;
}
/* --- End Image Modal Styles --- */


@media screen and (max-width: 768px) {
  .travel-timeline::after { left: 20px; margin-left: 0; }
  .timeline-item { width: 100%; padding-left: 50px; padding-right: 15px; margin-bottom: 30px; }
  .timeline-item-left, .timeline-item-right { left: 0; }
  .timeline-item-left .timeline-content, .timeline-item-right .timeline-content { text-align: left; padding-left: 15px; padding-right: 15px; }
  .timeline-item-left .timeline-content .image-container,
  .timeline-item-right .timeline-content .image-container {
    float: none; width: 100%; margin-left: 0; margin-right: 0; margin-bottom: 10px;
  }
  /* 针对小屏幕图片高度，如果上面img已经是height:auto，这里可能不需要额外设置 */
  /* .timeline-item-left .timeline-content .image-container img, */
  /* .timeline-item-right .timeline-content .image-container img { } */
  .timeline-point { left: 11px; }
  .timeline-item-left::before, .timeline-item-right::before { left: 40px; border-width: 10px 10px 10px 0; border-color: transparent white transparent transparent; }

  .image-modal-content {
    max-width: calc(100vw - 20px);
    max-height: calc(100vh - 20px);
    padding: 2px;
  }
  .close-modal-button {
    top: -10px; /* 调整按钮位置更贴近图片 */
    right: -10px;
    width: 30px;
    height: 30px;
    font-size: 22px;
    line-height: 28px;
  }
}
</style>