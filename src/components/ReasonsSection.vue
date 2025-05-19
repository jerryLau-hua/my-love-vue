<template>
  <section id="reasons" class="content-section">
    <h2 class="section-title">为什么我如此爱你 <span class="heart-icon">💌</span></h2>
    <p class="intro-text">有太多太多的理由，让我深深地爱着你。这里只是其中的一小部分：</p>
    <div class="reasons-cards">
      <div class="reason-card">
        <h4>你的笑容 😊</h4>
        <p>你的笑容像阳光一样温暖，能驱散我所有的阴霾。</p>
      </div>
      <div class="reason-card">
        <h4>你的善良 ❤️</h4>
        <p>你对待世界的温柔和善良，让我觉得这个世界都美好了许多。</p>
      </div>
      <div class="reason-card">
        <h4>我们共同的默契 🤝</h4>
        <p>很多时候，一个眼神，一个动作，我们就能明白彼此的心意。</p>
      </div>
      <div class="reason-card">
        <h4>你的陪伴 🚶‍♀️🚶‍♂️</h4>
        <p>有你在身边，无论做什么都觉得安心和快乐。</p>
      </div>
      <div class="reason-card">
        <h4>你做的【她擅长的事，例如：美味佳肴】 🍲</h4>
        <p>（【具体描述，例如：每次吃到你做的[菜名]，都觉得是世界上最幸福的事。】）</p>
      </div>
      <div class="reason-card">
        <h4>你的【某个特质，例如：上进心/幽默感】 ✨</h4>
        <p>（【具体描述，例如：我欣赏你的[特质]，它总是能激励我/逗乐我。】）</p>
      </div>
    </div>
  </section>
</template>

<script setup>
// No props needed for this component based on current design
</script>

<style scoped>
.intro-text {
  text-align: center;
  margin-bottom: 30px;
}
.reasons-cards {
  display: grid; /* 使用 grid 布局 */
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); /* 响应式列 */
  gap: 25px; /* 卡片之间的间隙 */
  justify-content: center;
}
.reason-card {
  background-color: #fff3f6;
  border: 1px solid var(--secondary-color);
  border-radius: 10px;
  padding: 25px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.06);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column; /* 使内容垂直排列 */
  /* min-width: 250px; /* 移到 grid-template-columns 中管理 */
}
.reason-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 8px 16px rgba(233, 30, 99, 0.15);
}
.reason-card h4 {
  color: var(--primary-color);
  margin-top: 0;
  margin-bottom: 12px;
  font-size: 1.3em;
  display: flex; /* 让图标和文字在同一行 */
  align-items: center;
}
/* 如果 h4 标签内有图标，可以通过 ::before 或直接在模板中加 span 来美化 */
/* 例如： h4::before { content: "💖 "; margin-right: 8px; } */

.reason-card p {
  font-size: 1em;
  color: #5c5c5c; /* 稍微软化文字颜色 */
  line-height: 1.6;
  margin-bottom: 0;
  flex-grow: 1; /* 让p标签填充剩余空间，如果卡片高度不一时有用 */
}
</style>