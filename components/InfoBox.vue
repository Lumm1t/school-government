<template>
  <div class="info-box">
    <div class="info-box__header">
      <h2>
        <slot name="header"></slot>
      </h2>

      <div class="info-box__content">
        <slot name="content"></slot>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import './assets/sass/variables';

.info-box {
  margin: 40px auto;
  width: 80vw;
  background-color: var(--backgroundColor-primary);
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  text-align: center;

  @media (max-width: 600px) {
    width: 95vw;
  }

  &__header {
    background: linear-gradient(270deg, $mainColor, $secondColor);
    color: white;
    width: 100%;
    padding: 10px;
  }

  &__content {
    font-size: 16px;
    padding-top: 20px;
  }
}
</style>
