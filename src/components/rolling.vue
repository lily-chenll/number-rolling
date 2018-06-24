<template>
  <div>
    <!--js: time-s-->
    <div class="number-box number-border">
      <transition name="rolling">
        <div v-if="on" key="current" class="number-box rolling-box">{{ timer[currentIndex] }}</div>
        <div v-else key="next" class="number-box rolling-box">{{ timer[currentIndex] }}</div>
      </transition>
    </div>
    <br>
    <!--css: time-ms-->
    <div class="number-box number-border msecond-box">

    </div>
    <br>
    <!--test transition-->
    <button @click="setTestTimer">test</button>
    <div class="number-box number-border">
      <div id="testCur" class="number-box, test-box">{{ timer[testIndex] }}</div>
      <!--<div id="testNext" class="number-box, test-box, test-enter-animation">{{ timer[testIndex] }}</div>-->
    </div>
  </div>

</template>

<script>
  export default {
    data() {
      return {
        on: true,
        onTest: true,
        timer: [
          0, 1, 2, 3, 4, 5, 6, 7, 8, 9
        ],
        currentIndex: 0,
        testIndex: 0,
        testafter: ['test-leave', 'test-leave-animation'],
        testbefore: ['test-enter', 'test-enter-animation'],
      };
    },
    created() {
      this.setTimer();
//      this.setTestTimer();
      this.setTest();
    },
    methods: {
      setTimer() {
        setInterval(() => {
          this.on = !this.on;
          this.currentIndex = this.currentIndex + 1 >= 10 ? 0 : this.currentIndex + 1;
        }, 1000);
      },
      setTestTimer() {
        setTimeout(() => {
          document.getElementById('testCur').className = 'number-box test-box test-leave-animation';
        }, 0);
        setTimeout(() => {
          document.getElementById('testCur').className = 'number-box test-box test-enter-animation';
        }, 800);
        setTimeout(() => {
          document.getElementById('testCur').className ='number-box test-box';
          this.testIndex = this.testIndex + 1 >= 10 ? 0 : this.testIndex + 1;
        }, 1800);
      },
      setTest() {
        setInterval(() => {
          this.setTestTimer();
        }, 2800);
      },
    },
  };
</script>

<style lang="scss" scoped>
  .number-border {
    border: 1px solid #000000;
    position: relative;
  }

  .number-box {
    width: 80px;
    height: 80px;
    line-height: 80px;
  }

  .rolling-box {
    position: absolute;
    left: 0px;
  }

  .rolling-enter-active, .rolling-leave-active {
    transition: all 1s;
  }

  .rolling-enter, .rolling-leave-active {
    opacity: 0;
  }

  .rolling-enter{
    transform: translateY(35px);
  }

  .rolling-leave-active {
    transform: translateY(-35px);
  }

  .msecond-box:before {
    content: '0';
    /* safari 4.0 - 8.0 */
    -webkit-animation-name: mill-second;
    -webkit-animation-duration: 1s;
    -webkit-animation-timing-function: linear;
    -webkit-animation-iteration-count: infinite;
    /* standard */
    animation-name: mill-second;
    animation-duration: 1s;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
  }

  /* safari 4.0 - 8.0 */
  @-webkit-keyframes mill-second {
    @for $i from 0 through 9 {
      $number: $i * 10;
      $percent: $number + '%';
      #{$percent} {
        content: $i + "";
      }
    }
    100% {
      content: '0';
    }
  }

  /* standard */
  @keyframes mill-second {
    @for $i from 0 through 9 {
      $number: $i * 10;
      $percent: $number + '%';
      #{$percent} {
        content: $i + "";
      }
    }
    100% {
      content: '0';
    }
  }

  .test-box {
    position: absolute;
    top: 0;
    opacity: 1;
    -webkit-transition: all 1s;
    transition: all 1s;
  }

  .test-enter-animation {
    -webkit-transform: translateY(35px);
    transform: translateY(35px);
    opacity: 0;
  }

  .test-leave-animation {
    -webkit-transform: translateY(-35px);
    transform: translateY(-35px);
    opacity: 0;
  }
</style>
