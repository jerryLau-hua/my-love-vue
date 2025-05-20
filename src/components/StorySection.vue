<template>
  <section id="story" class="content-section">
    <h2 class="section-title">我们的故事 <span class="heart-icon">📜</span></h2>
    <p class="intro-text">回忆我们一起走过的点点滴滴，每一刻都那么珍贵...</p>
    <p class="intro-text">我就不一一列举了哟，页面太小了，放不下咯🤪🤪</p>

    <div class="story-item">
      <h4>初遇的那一天 ✨</h4>
      <p>单枪匹马，勇闯济南的萍姐，提溜着两个大箱子就来了，哈哈哈 </p>
    </div>
    <div class="story-item">
      <h4>第一次心动的感觉 <span class="heart-icon">💓</span></h4>
      <p>牵手看电影，如果没记错的话，看的第一次电影是《哥，你好》</p>
    </div>
    <div class="story-item">
      <h4>难忘的旅行 🎈</h4>
      <p>🏞️🧗‍♂️趵突泉 [看泉水 tutututu~]</p>
      <p>🗺️🏃‍♀️大明湖 [大明湖畔夏雨荷 ]</p>
      <p>⛰️🎒✨千佛山 [勇闯千佛山，我就是那只最慢但最坚定的蜗牛！]</p>
      <p>🍖🤝 济南  [和你一起在济南吃把子肉]</p>
      <p>🛍️👫宽厚里 [排队买猪蹄被冻成🐕]</p>
    </div>
    <div class="story-item">
      <h4>难忘的生日 🎈</h4>
      <p>居然有人把步骤记错了，应该是先先许愿后吹蜡烛啊喂，红蛋🤪 </p>
    </div>
    <div class="story-item">
      <h4>难忘的旅行 🎈</h4>
      <p>那次去青岛旅行，我们一起在海边溜达，一起感受烫脚的沙子(是真的烫脚)，一起city
        walk，虽然很累，那是我最快乐的时光之一</p>
    </div>
    <div class="story-item">
      <h4>鼓励我的事 🌱</h4>
      <p>真的有人会觉得我做饭好吃吗？😋😋😋😋</p>
    </div>
    <p class="outro-text">我们的故事还在继续，未来由我们共同书写... <span class="highlight">未完待续...</span></p>
  </section>
</template>

<script setup>
// 如果需要props，可以在这里定义
// import { defineProps } from 'vue';
// const props = defineProps({ /* ... */ });
</script>

<style scoped>
.intro-text, .outro-text {
  text-align: center;
  margin-bottom: 25px;
  font-size: 1.15em;
}
.story-item {
  margin-bottom: 25px;
  padding: 20px;
  background-color: #fff8f9; /* 淡粉背景 */
  border-radius: 8px;
  border-left: 4px solid var(--secondary-color);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.story-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(233, 30, 99, 0.1);
}
.story-item:last-of-type { /* 修正选择器 */
  margin-bottom: 0; /* 最后一个故事项通常不需要底部外边距 */
}
.story-item h4 {
  font-size: 1.4em; /* 调整标题大小 */
  color: var(--accent-color);
  margin-top: 0; /* 移除顶部外边距 */
  margin-bottom: 10px;
}
.story-item p {
  font-size: 1.05em; /* 调整段落字体 */
  line-height: 1.7;
  margin-bottom: 0; /* 段落后通常没有额外边距，除非需要 */
}
</style>