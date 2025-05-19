<template>
  <section id="home" class="content-section"> <h2 class="section-title">亲爱的 <span class="highlight">{{ girlfriendName }}</span>，520快乐！</h2>
    <p>在这个特别的日子里，我的心里充满了对你的爱意与感激。 <span class="heart-icon">💖</span></p>
    <p>从我们相遇的那一刻起，我的世界就因你而变得更加精彩。你的笑容，你的温柔，你的一切，都深深吸引着我。希望这个小小的页面，能带给你一丝丝的惊喜和温暖。</p>
    <p>该说不说，时间过的可真快啊，转眼已经过了两三个年头，回想起过往的点点滴滴，彷佛确实是在昨天发生的一样，当年那个</p>
    <p>愿我们的爱情，像永不凋谢的花朵，永远鲜艳芬芳。 <span class="heart-icon">🌹</span></p>
  </section>
</template>

<script setup>
import { defineProps } from 'vue';

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  girlfriendName: String
});
</script>

<style scoped>
/* HomeSection 特有的样式可以放在这里 */
.content-section p { /* 示例：如果想让此区块的p居中 */
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  max-width: 600px; /* 控制段落宽度，使其更易读 */
}
</style>