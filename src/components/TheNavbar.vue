<template>
  <nav id="main-nav">
    <div class="nav-logo">To My Dearest <span class="heart-char">❤</span> {{ girlfriendName }}</div>
    <button class="menu-toggle" @click="toggleMenu" :aria-expanded="isMenuOpen.toString()">
      <span v-if="isMenuOpen">&times;</span> <span v-else>&#9776;</span> </button>
    <ul :class="{ active: isMenuOpen }" @click="closeMenuOnMobileLinkClick"> <li v-for="item in menuItems" :key="item.id">
      <a :href="'#' + item.id"
         :class="{ active: currentSection === item.id }"
         @click.prevent="navigate(item.id)">
        {{ item.icon }} {{ item.text }}
      </a>
    </li>
    </ul>
  </nav>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue';

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  girlfriendName: String,
  currentSection: String,
  menuItems: Array
});

const emit = defineEmits(['navigate']);

const isMenuOpen = ref(false);

function toggleMenu() {
  isMenuOpen.value = !isMenuOpen.value;
}

function navigate(sectionId) {
  emit('navigate', sectionId);
  // 在小屏幕上，点击导航链接后自动关闭汉堡菜单
  if (window.innerWidth <= 768 && isMenuOpen.value) {
    isMenuOpen.value = false;
  }
}

// 如果直接点击ul背景也想关闭菜单 (可选行为)
// function closeMenuOnMobileLinkClick(event) {
//   if (window.innerWidth <= 768 && isMenuOpen.value && event.target.tagName === 'A') {
//     isMenuOpen.value = false;
//   }
// }
</script>

<style scoped>
#main-nav {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: rgba(255, 255, 255, 0.95);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 1000;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  height: 70px; /* 默认导航栏高度 */
}

.nav-logo {
  font-size: 1.4em;
  font-weight: bold;
  color: var(--primary-color);
  padding: 0 15px; /* 给logo一点空间 */
}

.nav-logo .heart-char {
  color: var(--heart-color);
  animation: beatNavLogo 1.5s infinite ease-in-out;
  display: inline-block;
}

@keyframes beatNavLogo {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

#main-nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  align-items: center; /* 垂直居中列表项 */
  height: 100%; /* 确保ul和nav一样高，方便垂直居中a */
}

#main-nav ul li a {
  display: flex; /* 使用flex使图标和文字垂直居中 */
  align-items: center;
  height: 100%; /* 让a标签撑满li的高度 */
  padding: 0 20px; /* 左右内边距，上下由height和align-items控制 */
  text-decoration: none;
  color: var(--text-color);
  font-weight: 500;
  position: relative;
  transition: color 0.3s ease;
}

#main-nav ul li a:hover,
#main-nav ul li a.active {
  color: var(--primary-color);
}

#main-nav ul li a.active::after {
  content: '';
  position: absolute;
  bottom: 1px; /* 让下划线更靠下一点 */
  left: 50%;
  transform: translateX(-50%);
  width: 100%; /* 可以调整宽度 */
  max-width: 40px;
  height: 2px; /* 更细的下划线 */
  background-color: var(--secondary-color); /* 可以用浅一点的粉色 */
  border-radius: 1px;
}

.menu-toggle {
  display: none; /* 默认隐藏 */
  font-size: 24px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px 15px; /* 增大点击区域 */
}

/* 响应式 Navbar */
@media (max-width: 768px) {
  #main-nav {
    height: 60px; /* 小屏幕导航栏高度 */
  }
  .nav-logo {
    font-size: 1.2em;
    padding: 0 10px;
  }
  #main-nav ul {
    display: none; /* 在小屏幕上隐藏常规菜单 */
    flex-direction: column;
    position: absolute;
    top: 60px; /* 导航栏高度 */
    left: 0;
    width: 100%;
    background-color: rgba(255, 255, 255, 0.98);
    box-shadow: 0 5px 10px rgba(0,0,0,0.1);
    height: auto; /* 高度自适应 */
  }
  #main-nav ul.active { /* 点击汉堡菜单时显示 */
    display: flex;
  }
  #main-nav ul li {
    width: 100%; /* 列表项宽度占满 */
  }
  #main-nav ul li a {
    padding: 18px 20px; /* 调整内边距 */
    text-align: center;
    border-bottom: 1px solid var(--light-gray);
    width: 100%; /* 链接宽度占满 */
    justify-content: center; /* 使内容居中 */
    height: auto; /* 高度自适应 */
  }
  #main-nav ul li:last-child a {
    border-bottom: none;
  }
  #main-nav ul li a.active::after {
    width: 50px;
    bottom: 8px;
  }

  .menu-toggle {
    display: block; /* 在小屏幕上显示汉堡菜单按钮 */
  }
}
</style>