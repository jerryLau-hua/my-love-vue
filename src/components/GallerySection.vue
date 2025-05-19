<template>
  <section id="gallery" class="content-section">
    <h2 class="section-title">甜蜜相册 <span class="heart-icon">🖼️</span></h2>
    <p>这里是我们一些美好的瞬间，每一张照片都承载着一份甜蜜的回忆。</p>

    <div class="gallery-grid">
      <div class="gallery-item">
        <img src="/image1.jpg" alt="我们的合照1" @error="imageLoadError" />
        <div class="caption">（图片描述1：例如，初次约会）</div>
      </div>

      <div class="gallery-item">
        <img src="/image2.jpg" alt="我们的合照2" @error="imageLoadError" />
        <div class="caption">（图片描述2：例如，一起看过的风景）</div>
      </div>

      <div class="gallery-item placeholder-item" v-if="false"> <div class="img-placeholder">照片 (待添加)</div>
        <div class="caption">（图片描述）</div>
      </div>
    </div>
  </section>
</template>

<script setup>
// 图片加载错误处理函数 (可选)
function imageLoadError(event) {
  console.warn("图片加载失败:", event.target.src);
  // 你可以在这里设置一个默认的占位图，或者隐藏图片元素
  event.target.style.display = 'none'; // 例如，直接隐藏错误的图片
  // 或者 event.target.src = '/placeholder_image.jpg'; // 替换为占位图
  const caption = event.target.nextElementSibling;
  if (caption && caption.classList.contains('caption')) {
    caption.textContent = "图片加载失败";
  }
}
</script>

<style scoped>
.gallery-tip {
  font-size: 0.9em;
  color: #777;
  text-align: center;
  margin-bottom: 25px;
}
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); /* 调整minmax使图片不会太小 */
  gap: 20px;
  margin-top: 20px;
}
.gallery-item {
  border-radius: 8px;
  overflow: hidden;
  background-color: #fff; /* 给item一个背景色，以防图片透明 */
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex; /* 用于占位符居中 */
  flex-direction: column; /* 使caption在图片下方 */
}
.gallery-item:hover {
  transform: translateY(-6px) scale(1.03);
  box-shadow: 0 8px 20px rgba(0,0,0,0.18);
}
.gallery-item img {
  width: 100%;
  height: 200px; /* 固定高度，让相册更整齐 */
  object-fit: cover; /* 保持图片比例，裁剪多余部分 */
  display: block;
  background-color: var(--light-gray); /* 图片加载时的背景 */
}
.gallery-item .caption {
  padding: 12px 15px;
  font-size: 0.95em;
  text-align: center;
  color: #444;
  background-color: #f9f9f9; /* caption背景色 */
  border-top: 1px solid #eee; /* 图片和caption之间的分割线 */
  min-height: 40px; /* 给caption一个最小高度 */
  display: flex;
  align-items: center;
  justify-content: center;
}
/* 占位符样式 */
.img-placeholder {
  width: 100%;
  height: 200px; /* 与图片高度一致 */
  background-color: var(--light-gray);
  display: flex;
  justify-content: center;
  align-items: center;
  color: #999;
  font-size: 1em;
  border-radius: 8px 8px 0 0; /* 如果caption在下面，顶部圆角 */
}
.placeholder-item { /* 如果使用单独的占位符item */
  justify-content: center;
  align-items: center;
}
</style>