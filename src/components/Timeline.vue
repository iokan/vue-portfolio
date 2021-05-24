<template>
  <div class="timeline">
    <p class="timeline__description">Визуализация длительности и этапов жизни. Каждая клетка — четверть месяца. В строке 48 клеток, или 1 год.</p>
    <div class="timeline__table">
      <div class="timeline__table-item"
           v-for="n in amountItem(birthDate, lifetimeDate)"
           :key="n"
           :class="addClass(n)">
        <span v-if="n === 48">Год</span>
        <span v-else-if="n === 960">20</span>
        <span v-else-if="n === 1920">40</span>
        <span v-else-if="n === 2880">60</span>
        <span v-else-if="n === 3840">80</span>
      </div>
      <div class="timeline__table-period">{{timelinePeriod}} {{timelinePeriodText}}</div>
    </div>

    <nav class="timeline__nav">

      <ul class="timeline__nav-list timeline__nav-list_life">
        <li class="timeline__nav-item" @click="timeline = 'life', timelineNav = false">Жизнь</li>
        <transition name="shiftRight">
          <ul class="timeline__nav-sublist timeline__nav-sublist_life" v-if="timeline === 'life'">
            <li class="timeline__nav-item" @click="timelineNav = 'wedding'" key="wedding">Женился
            </li>
            <li class="timeline__nav-item" @click="timelineNav = 'parent'" key="parent">Родилась
              дочка
            </li>
          </ul>
        </transition>
      </ul>

      <ul class="timeline__nav-list timeline__nav-list_job">
        <li class="timeline__nav-item" @click="timeline = 'job', timelineNav = false">Работа</li>
        <transition name="shiftLeft">
          <ul class="timeline__nav-sublist timeline__nav-sublist_job" v-if="timeline === 'job'">
            <li class="timeline__nav-item" @click="timelineNav = 'building'"
                key="building">Строительство
            </li>
            <li class="timeline__nav-item" @click="timelineNav = 'it'" key="it">ИТ</li>
          </ul>
        </transition>
      </ul>

    </nav>

  </div>
</template>

<script>
export default {
  name: 'Timeline',
  data: function () {
    return {
      timeline: 'life',
      timelineNav: false,
      timelinePeriod: false,
      timelinePeriodText: '',
      birthDate: new Date('06-28-1990'),
      lifetimeDate: new Date('06-28-2070'),
      weddingDate: new Date('02-28-2018'),
      birthDateDaughter: new Date('07-05-2019'),
      workDateBuilding: new Date('09-10-2013'),
      workDateIt: new Date('09-18-2017'),
      currentDate: new Date()
    }
  },
  mounted () {
    this.getPeriod()
  },
  watch: {
    timeline: function () {
      console.log('таймлайн: ' + this.timeline)
      this.getPeriod()
    },
    timelineNav: function () {
      this.getPeriod()
    }
  },
  methods: {
    amountItem (start, end) {
      const k = 7.609 // дней в 1/4 месяца
      const amount = Math.floor(Math.abs(end.getTime() - start.getTime()) / (1000 * 3600 * 24 * k))
      return amount
    },

    amountPeriod (start, end) {
      const k = 0.083 / 4 // годов в 1/4 месяца
      this.timelinePeriod = Math.floor(this.amountItem(start, end) * k)

      if (this.timelinePeriod >= 5 && this.timelinePeriod <= 20) {
        this.timelinePeriodText = 'лет'
      } else {
        let txt = this.timelinePeriod
        txt = txt % 10
        if (txt === 1) {
          this.timelinePeriodText = 'год'
        } else if (txt >= 2 && txt <= 4) {
          this.timelinePeriodText = 'года'
        } else {
          this.timelinePeriodText = 'лет'
        }
      }
    },

    getPeriod () {
      if (this.timeline === 'life') {
        this.amountPeriod(this.birthDate, this.currentDate)

        if (this.timelineNav === 'wedding') {
          this.amountPeriod(this.weddingDate, this.currentDate)
        } else if (this.timelineNav === 'parent') {
          this.amountPeriod(this.birthDateDaughter, this.currentDate)
        }
      } else if (this.timeline === 'job') {
        this.amountPeriod(this.workDateBuilding, this.currentDate)

        if (this.timelineNav === 'building') {
          this.amountPeriod(this.workDateBuilding, this.workDateIt)
        } else if (this.timelineNav === 'it') {
          this.amountPeriod(this.workDateIt, this.currentDate)
        }
      }
    },

    addClass (n) {
      let color
      if (this.timeline === 'life' && n <= this.amountItem(this.birthDate, this.currentDate)) {
        color = 'life'
      }

      if (this.timeline === 'job' && n <= this.amountItem(this.birthDate, this.currentDate) && n >= (this.amountItem(this.birthDate, this.currentDate) - this.amountItem(this.workDateBuilding, this.currentDate))) {
        color = 'job'
      }

      if ((this.timelineNav === 'wedding' && n <= (this.amountItem(this.birthDate, this.currentDate) - this.amountItem(this.weddingDate, this.currentDate))) ||
      (this.timelineNav === 'parent' && n <= (this.amountItem(this.birthDate, this.currentDate) - this.amountItem(this.birthDateDaughter, this.currentDate))) ||
      (this.timelineNav === 'building' && n <= this.amountItem(this.birthDate, this.currentDate) && n >= (this.amountItem(this.birthDate, this.currentDate) - this.amountItem(this.workDateIt, this.currentDate))) ||
      (this.timelineNav === 'it' && n <= this.amountItem(this.birthDate, this.workDateIt) && n >= (this.amountItem(this.birthDate, this.currentDate) - this.amountItem(this.workDateBuilding, this.currentDate)))) {
        return color + ' ' + 'opacity'
      } else {
        return color
      }
    }
  }
}
</script>

<style scoped>

</style>
