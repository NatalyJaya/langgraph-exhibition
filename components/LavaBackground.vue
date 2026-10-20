<template>
  <div class="lava-bg" aria-hidden="true">
    <span class="blob b1" />
    <span class="blob b2" />
    <span class="blob b3" />
    <span class="blob b4" />
    <span class="veil" />
  </div>
</template>

<style scoped>
.lava-bg {
  position: absolute;
  inset: 0;
  z-index: -1;
  overflow: hidden;
  pointer-events: none;
  background: #fff;
}

.blob {
  position: absolute;
  width: 60%;
  aspect-ratio: 1;
  border-radius: 42% 58% 63% 37% / 45% 38% 62% 55%;
  filter: blur(64px);
  opacity: 0.7;
  mix-blend-mode: multiply;
  animation: lava-drift 18s ease-in-out infinite alternate;
  will-change: transform;
}

.b1 {
  top: -18%;
  left: -14%;
  background: #f0f0f0;
  animation-duration: 16s;
}

.b2 {
  top: 4%;
  right: -16%;
  background: #e8e8e8;
  animation-duration: 21s;
  animation-delay: -7s;
}

.b3 {
  bottom: -20%;
  left: 8%;
  background: #ececec;
  animation-duration: 19s;
  animation-delay: -11s;
}

.b4 {
  bottom: -6%;
  right: -8%;
  background: #f3f3f3;
  animation-duration: 24s;
  animation-delay: -15s;
}

.veil {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    ellipse at 50% 45%,
    rgba(255, 255, 255, 0.6),
    transparent 70%
  );
}

@keyframes lava-drift {
  0% {
    transform: translate3d(-6%, 4%, 0) scale(1) rotate(-4deg);
    border-radius: 42% 58% 63% 37% / 45% 38% 62% 55%;
  }
  50% {
    transform: translate3d(14%, -10%, 0) scale(1.18) rotate(10deg);
    border-radius: 58% 42% 38% 62% / 55% 62% 38% 45%;
  }
  100% {
    transform: translate3d(-10%, 12%, 0) scale(0.92) rotate(-8deg);
    border-radius: 45% 55% 52% 48% / 60% 40% 60% 40%;
  }
}

@media (prefers-reduced-motion: reduce) {
  .blob {
    animation: none;
  }
}
</style>
