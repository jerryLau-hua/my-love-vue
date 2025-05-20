<template>
  <section id="wish" class="content-section">
    <h2 class="section-title">特别的期许 <span class="heart-icon">🌟</span></h2>
    <div class="wish-text">
      <p>亲爱的<span class="highlight">{{ girlfriendName }}</span>，</p>
      <p>在这个特别的日子，我不仅想对你说“我爱你”，更想许下对我们未来的美好期许。</p>
      <p>我希望我们的爱情能够长长久久，如同星辰一般永恒。愿我们能一起经历更多人生的风景，一起克服未来的挑战，一起分享每一个喜悦的瞬间。</p>
      <p>然后呢 old papa is coming~ 👨‍🦳,希望未来的你：</p>
      <p>好好吃饭，多吃肉肉 🍖，身体棒棒，充满能量！</p>
      <p>舒缓心情，告别焦虑 😊，每天都有阳光照进心里。</p>
      <p>早睡早起，精神饱满 ☀️，像向日葵一样追逐光芒。</p>
      <p>坚持热爱，发展兴趣 🎨🎸，让生活多姿多彩，闪闪发光。</p>
      <p>保持好奇，不断学习 📚，世界那么大，我们一起去看看。</p>
      <p>未来的每一天，我都希望能有你相伴。</p>
      <p class="signature">永远爱你的，<span class="highlight">{{ yourName }}</span> <span class="heart-icon">💖</span></p>
    </div>
  </section>
</template>

<script setup>
import { defineProps } from 'vue';

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  girlfriendName: String,
  yourName: String
});
</script>

<style scoped>
.wish-text {
  font-size: 1.2em;
  text-align: center;
  padding: 30px 25px; /* 增加内边距 */
  background: linear-gradient(135deg, #fff0f5, #ffe6ee);
  border-radius: 12px; /* 更大的圆角 */
  border-left: 6px solid var(--primary-color); /* 加粗左边框 */
  box-shadow: 0 5px 15px rgba(233, 30, 99, 0.1); /* 添加阴影 */
  line-height: 1.8; /* 增加行高 */
}
.wish-text p {
  margin-bottom: 20px; /* 段落间距 */
  color: #4a4a4a; /* 调整文字颜色使其更柔和 */
}
.wish-text p:last-child {
  margin-bottom: 0;
}
.signature {
  font-weight: bold;
  margin-top: 30px; /* 与上方内容有更多间距 */
}
</style>