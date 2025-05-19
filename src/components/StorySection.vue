<template>
  <section id="story" class="content-section">
    <h2 class="section-title">我们的故事 <span class="heart-icon">📜</span></h2>
    <p class="intro-text">回忆我们一起走过的点点滴滴，每一刻都那么珍贵...</p>

    <div class="story-item">
      <h4>初遇的那一天 ✨</h4>
      <p>（【描述你们相遇的场景和你的感受... 例如：还记得那天阳光正好，你穿着[描述]，笑容[如何]...】）</p>
    </div>
    <div class="story-item">
      <h4>第一次心动的感觉 <span class="heart-icon">💓</span></h4>
      <p>（【分享第一次让你心动的瞬间... 例如：当我们第一次[共同做的事]，我感觉心跳都漏了一拍...】）</p>
    </div>
    <div class="story-item">
      <h4>难忘的[某个瞬间/旅行/纪念日] 🎈</h4>
      <p>（【详细描述一个你们共同的难忘经历... 例如：那次去[地点]旅行，我们一起[做了什么]，那是我最快乐的时光之一...】）</p>
    </div>
    <div class="story-item">
      <h4>你教会我的事 🌱</h4>
      <p>（【写下她对你的积极影响... 例如：因为你，我变得更加[积极的改变]，学会了[新技能/道理]...】）</p>
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