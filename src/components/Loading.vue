<template>
  <div>
    <section class="heart-loading flex flex-center">
      <ul style="--line-count: 9">
        <li
          v-for="v in 9"
          :key="v"
          :class="`line-${v}`"
          :style="`--line-index: ${v}`"
        ></li>
      </ul>
    </section>
    <!-- <div class="heart-loading flex-ct-x">
      <div class="ul">
        <div v-for="item in 9" :key="item"></div>
        <div class="li line-1" style="--line-index: 1"></div>
        <div class="li line-2" style="--line-index: 2"></div>
        <div class="li line-3" style="--line-index: 3"></div>
        <div class="li line-4" style="--line-index: 4"></div>
        <div class="li line-5" style="--line-index: 5"></div>
        <div class="li line-6" style="--line-index: 6"></div>
        <div class="li line-7" style="--line-index: 7"></div>
        <div class="li line-8" style="--line-index: 8"></div>
        <div class="li line-9" style="--line-index: 9"></div>
      </div>
    </div> -->
    <div class="loading-text">加载中</div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="less" scoped>
@blue: #1890ff;

.heart-loading {
  width: 100px;
  height: 100px;
  align-items: center;
  ul {
    display: flex;
    justify-content: space-between;
    padding: 0px;
    width: 75px;
    height: 5px;
    list-style: none;
  }
  li {
    // --trans: calc(var(--line-index) / var(--line-count) * 0.5turn);
    // filter: hue-rotate(var(--trans));
    --time: calc((var(--line-index) - 1) * 40ms);
    border-radius: 2.5px;
    width: 5px;
    height: 5px;
    background-color: @blue;
    animation-duration: 1s;
    animation-delay: var(--time);
    animation-iteration-count: infinite;
  }
  .line-1,
  .line-9 {
    animation-name: line-move-1;
  }
  .line-2,
  .line-8 {
    animation-name: line-move-2;
  }
  .line-3,
  .line-7 {
    animation-name: line-move-3;
  }
  .line-4,
  .line-6 {
    animation-name: line-move-4;
  }
  .line-5 {
    animation-name: line-move-5;
  }
  @keyframes line-move-1 {
    0%,
    10%,
    90%,
    100% {
      height: 5px;
    }

    45%,
    55% {
      height: 15px;
      transform: translate3d(0, -7.5px, 0);
    }
  }

  @keyframes line-move-2 {
    0%,
    10%,
    90%,
    100% {
      height: 5px;
    }

    45%,
    55% {
      height: 30px;
      transform: translate3d(0, -15px, 0);
    }
  }

  @keyframes line-move-3 {
    0%,
    10%,
    90%,
    100% {
      height: 5px;
    }

    45%,
    55% {
      height: 40px;
      transform: translate3d(0, -20px, 0);
    }
  }

  @keyframes line-move-4 {
    0%,
    10%,
    90%,
    100% {
      height: 5px;
    }
    45%,
    55% {
      height: 45px;
      transform: translate3d(0, -15px, 0);
    }
  }

  @keyframes line-move-5 {
    0%,
    10%,
    90%,
    100% {
      height: 5px;
    }

    45%,
    55% {
      height: 45px;
      transform: translate3d(0, -10px, 0);
    }
  }
}

.loading-text {
  font-size: 12px;
  color: @blue;
}
</style>