<template>
  <div class="lava-bg">
    <div class="lava-blobs" />
    <div class="lava-blobs-2" />
  </div>
</template>

<style scoped>
@keyframes bgShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}
@keyframes lavaFlow {
  0% { transform: translate(-5%, -5%) rotate(0deg) scale(1); }
  25% { transform: translate(3%, -3%) rotate(90deg) scale(1.04); }
  50% { transform: translate(-2%, 4%) rotate(180deg) scale(0.96); }
  75% { transform: translate(5%, 1%) rotate(270deg) scale(1.02); }
  100% { transform: translate(-5%, -5%) rotate(360deg) scale(1); }
}
@keyframes lavaFlow2 {
  0% { transform: translate(3%, 3%) rotate(0deg) scale(1); }
  25% { transform: translate(-4%, 5%) rotate(-90deg) scale(0.97); }
  50% { transform: translate(5%, -2%) rotate(-180deg) scale(1.03); }
  75% { transform: translate(-3%, -4%) rotate(-270deg) scale(0.98); }
  100% { transform: translate(3%, 3%) rotate(-360deg) scale(1); }
}

.lava-bg {
  position: fixed;
  inset: 0;
  background: linear-gradient(135deg, #865757, #F8F6F3, #EFECE7, #E4E0DA, #D8D3CB);
  background-size: 300% 300%;
  animation: bgShift 100s ease infinite;
  overflow: hidden;
  pointer-events: none;
}
.lava-blobs {
  position: absolute;
  width: 150%; height: 150%; top: -25%; left: -25%;
  background:
    radial-gradient(circle at 20% 20%, rgba(255, 255, 255, 0.9) 0%, transparent 30%),
    radial-gradient(circle at 75% 25%, rgba(226, 221, 214, 0.6) 0%, transparent 35%),
    radial-gradient(circle at 65% 65%, rgba(206, 200, 192, 0.5) 0%, transparent 35%),
    radial-gradient(circle at 25% 75%, rgba(214, 209, 201, 0.5) 0%, transparent 30%),
    radial-gradient(circle at 50% 45%, rgba(236, 232, 226, 0.4) 0%, transparent 35%);
  animation: lavaFlow 25s ease-in-out infinite;
  filter: blur(40px);
}
.lava-blobs-2 {
  position: absolute;
  width: 130%; height: 130%; top: -15%; left: -15%;
  background:
    radial-gradient(circle at 60% 30%, rgba(233, 229, 222, 0.5) 0%, transparent 30%),
    radial-gradient(circle at 30% 60%, rgba(218, 213, 205, 0.4) 0%, transparent 35%),
    radial-gradient(circle at 80% 70%, rgba(255, 255, 255, 0.6) 0%, transparent 25%);
  animation: lavaFlow2 30s ease-in-out infinite;
  filter: blur(50px);
}
</style>