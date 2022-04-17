<template>
  <div class="flex between-h m-b97">
    <div>
      <DefaultText v-bind:text="'Today, '+today" textStyle="today-txt-style margin-auto" />
    </div>
    <div class="flex">
      <DefaultButton v-bind:action="stop" btn-style="m-r13 red-text red-border" v-bind:withIcon="true" btn-text="Stop" :icon-src="require('@/assets/images/play.svg')" />
      <DefaultButton v-bind:action="start" btn-style="m-r32 green-text green-border" v-bind:withIcon="true" btn-text="Start new" :icon-src="require('@/assets/images/clock.svg')" />
      <TimeText v-bind:hour="totalH" v-bind:minutes="totalMin" v-bind:seconds="totalSec" textStyle="time-style green-text margin-auto" secondesStyle="weight-300 robotoFontLight"/>
    </div>
  </div>
  <PeriodItem 
    v-if="isRunning"
      isCurrent="true"
      v-bind:periodIndexProp="times.length+1" 
      v-bind:startAt="startAt"
      v-bind:endAt="endAt"
      textStyle="begin-end-txt-style" 
      v-bind:hh="h"
      v-bind:mm="min"
      v-bind:ss="sec"
      periodeContainerStyle="green-boder-top-bottom"
      periodStyleProp = "period-number-txt-style m-b5"
      timeTextStyleProps = "green-text"
      secondesStyleProps = "green-text"
  />
  <div v-for="(time, index) in times" :key="index" >
    <PeriodItem 
      v-bind:periodIndexProp="time.index+1" 
      v-bind:startAt="time.startAt"
      v-bind:endAt="time.endAt"
      v-bind:hh="time.h"
      v-bind:mm="time.min"
      v-bind:ss="time.sec"
      periodStyleProp = "period-number-txt-style"
    />
  </div>

  <div class="total-container">
    <DefaultText text="Total" textStyle="total-time-txt-style m-r40 m-b0 m-t0" />
    <TimeText v-bind:hour="totalH" v-bind:minutes="totalMin" v-bind:seconds="totalSec"  textStyle="time-style green-text m-b0 m-t0" secondesStyle="weight-300 robotoFontLight"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import DefaultButton from "./components/uikit/DefaultButton.vue";
import DefaultText from "./components/uikit/DefaultText.vue";
import TimeText from "./components/uikit/TimeText.vue";
import PeriodItem from "./components/uikit/PeriodItem.vue";
import Period from '@/types/Period'

export default defineComponent({
  name: "App",
  data() {
    return {
      h: 0,
      min: 0,
      sec: 0,
      totalH: 0,
      totalMin: 0,
      totalSec: 0,
      startAt: "",
      endAt: "",
      isRunning: false,
      time: 0,
      timer: 0,
      times: [] as Period[],
      today: "",
    };
  },
  components: {
    DefaultButton,
    DefaultText,
    TimeText,
    PeriodItem,
  },
  methods: {
    // Démarrer le chronomètre
    start() {
      if (!this.isRunning) {
        const now = new Date();
        let hours : string = now.getHours().toString();
        if (now.getHours() < 10) {
          hours = "0" + hours;
        }
        let miniutes : string = now.getMinutes().toString();
        if (now.getMinutes() < 10) {
          hours = "0" + miniutes;
        }
        this.startAt = hours + ":" + miniutes;
        this.isRunning = true;
        if (!this.timer) {
          this.timer = setInterval(() => {
            this.sec++;
            this.totalSec++;

            // controle si le nombre totale des secondes dépasse 59
            if (this.totalSec > 59) {
              this.totalSec = 0;
              this.totalMin++;
            }
            // controle si le nombre des secondes dépasse 59
            if (this.sec > 59) {
              this.sec = 0;
              this.min++;
            }

            // controle si le nombre totale des minutes dépasse 59
            if (this.totalMin > 59) {
              this.totalMin = 0;
              this.totalH++;
            }

            // controle si le nombre des minutes dépasse 59
            if (this.min > 59) {
              this.min = 0;
              this.h++;
            }
          }, 1000);
        }
      }
    },

    // Arrêter le chronomètre
    stop() {
      if (this.isRunning) {
        const now = new Date();
        let hours : string = now.getHours().toString();
        if (now.getHours() < 10) {
          hours = "0" + hours;
        }
        let miniutes : string = now.getMinutes().toString();
        if (now.getMinutes() < 10) {
          hours = "0" + miniutes;
        }
        
        this.times.unshift({
          index: this.times.length ,
          h: this.h,
          min: this.min,
          sec: this.sec,
          startAt: this.startAt,
          endAt: hours + ":" + miniutes,
        });

        this.isRunning = false;
        (this.h = 0), (this.min = 0), (this.sec = 0), clearInterval(this.timer);
        this.timer = 0;
      }
    },
  },

  created() {
    // stocker la date d'aujourd'hui dans la variable today
    const now: Date = new Date();
    const month = now.toLocaleString("default", { month: "short" });
    const date = now.getDate() < 10 ? "0" + now.getDate() : now.getDate();
    this.today = date + " " + month;
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 10% 20%;
}

.total-container {
  display: flex;
  justify-content: flex-end;
  padding: 14px 64px;
  border-top: 1px solid $light-gray-color;
  border-bottom: 1px solid $light-gray-color;
  margin-top: 100px;
}

.m-r40 {
  margin-right: 40px;
}

.m-t20 {
  margin-top: 20px;
}

.green-boder-top-bottom {
  border-top: 1px solid $green-color !important;
  border-bottom: 1px solid $green-color;
}

.margin-auto {
  margin: auto;
}

.m-b97 {
  margin-bottom: 97px !important;
}

.m-b0 {
  margin-bottom: 0px !important;
}

.m-r13 {
  margin-right: 13px !important;
}

.m-r32 {
  margin-right: 32px !important;
}

.flex {
  display: flex;
}

.initial-display {
  display: initial;
}

.between-h {
  justify-content: space-between;
}

.weight-300 {
  font-weight: 300 !important;
}

.robotoFontLight {
  font-family: $robotoFontLight !important;
}

.red-text {
  color: $red-color !important;
}

.red-border {
  border-color: $red-color !important;
}

.green-text {
  color: $green-color !important;
}

.left-text {
  text-align: left;
}

.right-text {
  text-align: right;
}

.m-b5 {
  margin-bottom: 5px !important;
}

.m-t0 {
  margin-top: 0px;
}
.green-border {
  border-color: $green-color !important;
}

.no-bottom-border {
  border-bottom: none;
}

.today-txt-style {
  color: $gray-color-level-5;
  font-family: $robotoFont;
  font-style: normal;
  font-weight: 900;
  font-size: 30px;
  line-height: 35px;
}

.period-number-txt-style {
  padding-top: 6px;
  margin: 0px;
  color: $gray-color-level-4;
  font-family: $robotoFont;
  font-style: normal;
  font-weight: 900;
  font-size: 16px;
  line-height: 19px;
}

.current-period-txt-style {
  color: $gray-color-level-1;
  font-family: $robotoFont;
  font-style: normal;
  font-weight: 900;
  font-size: 16px;
  line-height: 19px;
}

.begin-end-txt-style {
  color: $gray-color-level-3;
  font-family: $robotoFontMeduim;
  font-style: normal;
  font-weight: 900;
  font-size: 14px;
  line-height: 16px;
}

.time-style {
  color: $gray-color-level-5;
  font-family: $robotoFontMeduim;
  font-style: normal;
  font-weight: 900;
  font-size: 25px;
  line-height: 29px;
}

.total-time-txt-style {
  color: $gray-color-level-2;
  font-family: $robotoFont;
  font-style: normal;
  font-weight: 900;
  font-size: 25px;
  line-height: 29px;
}

// Period item
.defaultPeriodeContainerStyle {
  padding: 23px 64px;
  border-top: 1px solid $light-gray-color;
}

.light-gray-bottom-border {
  border-bottom: 1px solid $light-gray-color;
}

.period-number-txt-style {
  color: $gray-color-level-4;
  font-family: $robotoFont;
  font-style: normal;
  font-weight: 900;
  font-size: 16px;
  line-height: 19px;
}
</style>
