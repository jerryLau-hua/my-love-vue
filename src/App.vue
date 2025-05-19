<template>
  <div id="app-container">
    <canvas id="particles-canvas" ref="particlesCanvasRef"></canvas>

    <TheNavbar
        :girlfriendName="girlfriendName"
        :currentSection="currentSection"
        :menuItems="menuItems"
        @navigate="handleNavigation"
    />

    <div class="content-wrapper">
      <HomeSection v-if="currentSection === 'home'" :girlfriendName="girlfriendName"/>
      <StorySection v-if="currentSection === 'story'"/>
      <GallerySection v-if="currentSection === 'gallery'"/>
      <ReasonsSection v-if="currentSection === 'reasons'"/>
      <WishSection v-if="currentSection === 'wish'" :girlfriendName="girlfriendName" :yourName="yourName"/>
    </div>

    <footer>
      <p><span class="heart-icon">❤</span> Made with love by <span class="highlight">{{ yourName }}</span> for his one
        and only <span class="highlight">{{ girlfriendName }}</span> <span class="heart-icon">❤</span></p>
      <p>&copy; 2025 - Happy 520!</p>
    </footer>
  </div>
</template>

<script setup>
import {onBeforeUnmount, onMounted, ref} from 'vue';
import TheNavbar from './components/TheNavbar.vue';
import HomeSection from './components/HomeSection.vue';
import StorySection from './components/StorySection.vue';
import GallerySection from './components/GallerySection.vue';
import ReasonsSection from './components/ReasonsSection.vue';
import WishSection from './components/WishSection.vue';

// --- 【重要】配置区 ---
const girlfriendName = ref("AAA"); // 在这里修改成你女朋友的名字
const yourName = ref("BBB");       // 在这里修改成你的名字
// --- 配置区结束 ---

const menuItems = ref([
  {id: 'home', text: '主页', icon: '❤️'},
  {id: 'story', text: '我们的故事', icon: '📜'},
  {id: 'gallery', text: '甜蜜相册', icon: '🖼️'},
  {id: 'reasons', text: '爱你的理由', icon: '💌'},
  {id: 'wish', text: '特别的期许', icon: '🌟'},
]);

const currentSection = ref(menuItems.value[0].id); // 默认显示主页

function handleNavigation(sectionId) {
  currentSection.value = sectionId;
}

// --- 粒子效果 ---
const particlesCanvasRef = ref(null);
let animationFrameId;
let particlesArray = []; // 在外部作用域定义，以便 resize 时可以重新初始化

class Particle {
  constructor(x, y, directionX, directionY, size, color, canvasContext, canvasElement) {
    this.x = x;
    this.y = y;
    this.directionX = directionX;
    this.directionY = directionY;
    this.size = size;
    this.color = color;
    this.ctx = canvasContext;
    this.canvas = canvasElement;
  }

  draw() {
    this.ctx.beginPath();
    this.ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2, false);
    this.ctx.fillStyle = this.color;
    this.ctx.fill();
  }

  update() {
    if (this.x + this.size > this.canvas.width || this.x - this.size < 0) {
      this.directionX = -this.directionX;
    }
    if (this.y + this.size > this.canvas.height || this.y - this.size < 0) {
      this.directionY = -this.directionY;
    }
    this.x += this.directionX;
    this.y += this.directionY;
    this.draw();
  }
}

function initParticles() {
  const canvas = particlesCanvasRef.value;
  if (!canvas) return;
  const ctx = canvas.getContext('2d');

  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  particlesArray = []; // 清空之前的粒子

  let numberOfParticles = (canvas.height * canvas.width) / 10000;
  if (numberOfParticles > 120) numberOfParticles = 120;
  if (numberOfParticles < 30) numberOfParticles = 30; // 保证最少粒子数

  for (let i = 0; i < numberOfParticles; i++) {
    let size = (Math.random() * 2.5) + 0.5;
    let x = (Math.random() * ((canvas.width - size * 2) - (size * 2)) + size * 2);
    let y = (Math.random() * ((canvas.height - size * 2) - (size * 2)) + size * 2);
    let directionX = (Math.random() * .3) - .15;
    let directionY = (Math.random() * .3) - .15;
    let color = `rgba(233, 30, 99, ${Math.random() * 0.3 + 0.2})`; // 调整透明度和颜色
    particlesArray.push(new Particle(x, y, directionX, directionY, size, color, ctx, canvas));
  }
}

function animateParticles() {
  const canvas = particlesCanvasRef.value;
  if (!canvas || !particlesArray.length) return; // 如果没有粒子数组则不执行
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  for (let i = 0; i < particlesArray.length; i++) {
    particlesArray[i].update();
  }
  animationFrameId = requestAnimationFrame(animateParticles);
}

let resizeTimeout;

function handleResize() {
  clearTimeout(resizeTimeout);
  resizeTimeout = setTimeout(() => {
    initParticles(); // 重新初始化粒子
    // 如果动画已停止，可以考虑在这里重新启动
    if (!animationFrameId && particlesArray.length > 0) {
      // animateParticles(); // 确保只在必要时启动，通常initParticles后animate会继续
    }
  }, 250); // 防抖
}


onMounted(() => {
  initParticles();
  if (particlesArray.length > 0) { // 只有当粒子成功初始化后才开始动画
    animateParticles();
  }
  window.addEventListener('resize', handleResize);
});

onBeforeUnmount(() => {
  cancelAnimationFrame(animationFrameId);
  window.removeEventListener('resize', handleResize);
});

</script>

<style>
/* App.vue specific styles or global overrides if needed,
   but most global styles should be in assets/main.css */
#app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh; /* Ensure footer is at bottom if content is short */
}

/* .content-wrapper is already styled globally in main.css to have flex-grow: 1 */
</style>