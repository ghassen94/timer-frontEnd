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
      v-bind:periodIndexProp="time.index" 
      v-bind:startAt="time.startAt"
      v-bind:endAt="time.endAt"
      v-bind:hh="time.hour"
      v-bind:mm="time.minute"
      v-bind:ss="time.second"
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
        
        // Ajouter la période à la liste  
        this.times.unshift({
          index: this.times.length +1 ,
          hour: this.h,
          minute: this.min,
          second: this.sec,
          startAt: this.startAt,
          endAt: hours + ":" + miniutes,
        });

        fetch("http://192.168.1.54:3000/period/saveTodayTimers", {
          method: "Post",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ 
            index: this.times.length ,
            hour: this.h,
            minute: this.min,
            second: this.sec,
            startAt: this.startAt,
            endAt: hours + ":" + miniutes,
            createdAt: new Date()
          })
        })
          .then(response => response.json())

        this.isRunning = false;
        (this.h = 0), (this.min = 0), (this.sec = 0), clearInterval(this.timer);
        this.timer = 0;
      }
    },
    getTimes() {
      let totalHours : number 
      totalHours = 0
      
      let totalMinutes : number 
      totalMinutes = 0
      
      let totalSeconds : number 
      totalSeconds = 0

      fetch("http://192.168.1.54:3000/period/getTodayTimers", {
          "method": "GET",
          "headers": {
          }
      })
      .then(response => { 
          if(response.ok){
              return response.json()    
          }            
      })
        .then(response => {
          if (response && response.times) {
            this.times = response.times.reverse();
            
            response.times.forEach((value: any) => {
                totalHours += value.hour; 
                totalMinutes += value.minute; 
                totalSeconds += value.second;
            });

            if (totalSeconds > 59) {
              totalMinutes += Math.floor(totalSeconds / 60)
              totalSeconds -=  (Math.floor(totalSeconds / 60)*60)
            }

            if (totalMinutes > 59) {
              totalHours += Math.floor(totalMinutes / 60)
              totalMinutes -=  (Math.floor(totalMinutes / 60) * 60)
            }
            
            this.totalH = totalHours
            this.totalMin = totalMinutes
            this.totalSec = totalSeconds
          }
      })
      .catch(err => {
          console.log(err);
      });
    },
  },

  created() {
    // stocker la date d'aujourd'hui dans la variable today
    const now: Date = new Date();
    const month = now.toLocaleString("default", { month: "short" });
    const date = now.getDate() < 10 ? "0" + now.getDate() : now.getDate();
    this.today = date + " " + month;

    this.getTimes()
 
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
</style>
