<template>
  <div class="message" :class="[className, { senderMessage: sender }]">
    <div class="preloader" v-if="preloader && !sender">
      <div class="dot-flashing"></div>
    </div>
    <div class="content" v-if="!preloader || sender">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import { animate } from "@motionone/dom";

export default {
  props: {
    className: String,
    sender: Boolean,
  },
  data() {
    return {
      preloader: true,
    };
  },
  mounted() {
    let seqNumber = parseInt(this.className[4]);
    let waitTime = (seqNumber * 0.5 + seqNumber) * 1100;
    if (seqNumber < 3) {
      waitTime = seqNumber * 1100;
    }
    if (seqNumber === 3) {
      waitTime = seqNumber * 1300;
    }
    setTimeout(() => {
      this.preloader = false;
    }, waitTime);
  },
};
</script>

<style lang="scss">
@import "../assets/Sass/components/text-message";

.preloader {
  height: 20px;
  display: flex;
  align-items: center;
}
.dot-flashing {
  position: relative;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: #333;
  color: #333;
  animation: dotFlashing 1s infinite linear alternate;
  animation-delay: 0.2s;
  left: 15px;
}

.dot-flashing::before,
.dot-flashing::after {
  content: "";
  display: inline-block;
  position: absolute;
  top: 0;
}

.dot-flashing::before {
  left: -13px;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: #333;
  color: #333;
  animation: dotFlashing 1s infinite alternate;
  animation-delay: 0s;
}

.dot-flashing::after {
  left: 13px;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  background-color: #333;
  color: #333;
  animation: dotFlashing 1s infinite alternate;
  animation-delay: 0.3s;
}

@keyframes dotFlashing {
  0% {
    background-color: #333;
  }
  50%,
  100% {
    background-color: #b5b5b7;
  }
}
</style>
