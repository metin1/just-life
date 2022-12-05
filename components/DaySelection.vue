<template>
  <ElContainer class="package-container" id="app">
    <h1>JUST LIFE {{ !isScheduled ? 'Day Selection' : '' }}</h1>
    <div v-if="isScheduled" class="success-card">
      <el-button
        size="large"
        type="success"
        icon="el-icon-check"
        circle
      ></el-button>
      <h2>Thank you for your purchase!</h2>
      <br />
      <h3>Your scheduled {{ selectedPackage?.visit }} {{ getDayContent() }}</h3>
      <h4>Package: {{ selectedPackage?.name }}</h4>
      <h4>Booked {{ getDayContent() }}:</h4>
      <ul>
        <li
          v-for="(day, index) in dayArray"
          v-bind:key="index"
          v-show="day.isSelected"
        >
          {{ day?.fullDate.toLocaleDateString('en-US') }} -
          {{ day?.weekDay }} {{ day?.time ? ` - ${day?.time}` : '' }}
        </li>
      </ul>
      <NuxtLink to="/">
        <button class="button next-button large-button" @click="handleReset">
          Book another package
        </button>
      </NuxtLink>
    </div>
    <div v-else-if="selectedPackage?.name" class="selection">
      <h4
        :style="
          selectedPackage?.name
            ? 'color: var(--dark-color)'
            : 'color: var(--secondary-color)'
        "
      >
        Selected Package: {{ selectedPackage?.name || 'none' }}
      </h4>
      <p>
        You can select up to {{ selectedPackage?.visit }} {{ getDayContent() }}.
        You can select appropriate {{ getDayContent() }}.
      </p>
      <div>
        <button
          class="button day-button"
          v-for="(day, index) in dayArray"
          v-bind:key="index"
          :class="{
            'selected-day': day.isSelected,
            'unselected-day': !day.isSelected,
          }"
          @click="selectDay(day)"
          :disabled="day.isDisabled"
        >
          {{ day?.fullDate.toLocaleDateString('en-US') }} -
          {{ day?.weekDay }} {{ day?.time ? ` - ${day?.time}` : '' }}
        </button>
      </div>
      <div
        v-if="selectedPackage?.isOneTime || !selectedPackage?.name"
        style="height: 1.1rem"
      />
      <br />
      <p v-if="!!selectedPackage?.name">
        If you are ready to proceed, please click the button below.
      </p>
      <p v-else>Please select a package to proceed.</p>
      <button
        class="button next-button"
        :disabled="this.selectedPackage?.visit > this.totalSelectedDays"
        @click="handleSchedule"
      >
        Book Now
      </button>
    </div>
    <div v-else class="danger-card">
      <el-button
        size="large"
        type="danger"
        icon="el-icon-check"
        circle
      ></el-button>
      <h2>Please select a package to proceed.</h2>
      <NuxtLink to="/">
        <button class="button next-button large-button">
          Go Package Selection Page
        </button>
      </NuxtLink>
    </div>
    <el-dialog
      title="Please select cleaning time"
      :visible.sync="open"
      width="30%"
      center
    >
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="handleSelectTime('9AM - 1PM')"
          >9AM - 1PM</el-button
        >
        <el-button type="primary" @click="handleSelectTime('5PM - 9PM')"
          >5PM - 9PM</el-button
        >
      </span>
    </el-dialog>
  </ElContainer>
</template>

<script>
export default {
  name: 'app',
  components: {},
  data() {
    return {
      selectedPackage: {},
      startDay: new Date(),
      dayArray: [],
      totalSelectedDays: 0,
      isScheduled: false,
      open: false,
      selectedDay: {},
    }
  },
  mounted() {
    const today = new Date()
    for (let i = 1; i <= 7; i++) {
      const day = new Date()
      day.setDate(today.getDate() + i)
      this.dayArray.push({
        fullDate: day,
        weekDay: day.toLocaleDateString('en-US', { weekday: 'long' }),
        isSelected: false,
        isDisabled: false,
      })
    }
    this.selectedPackage = localStorage.getItem('selectedPackage')
      ? JSON.parse(localStorage.getItem('selectedPackage'))
      : {}
  },
  methods: {
    selectDay: function (day) {
      this.selectedDay = day
      if (day.time) {
        return this.handleSelectTime(null)
      }
      this.open = true
    },
    handleSelectTime: function (time) {
      const selectedDays = this.dayArray.map((d) => {
        return d?.weekDay === this.selectedDay?.weekDay
          ? { ...d, isSelected: !d.isSelected, time }
          : d
      })

      this.totalSelectedDays = selectedDays.filter((d) => d.isSelected).length

      const disabledDays = selectedDays.map((d) => {
        return d?.isSelected
          ? { ...d, isDisabled: false }
          : this.selectedPackage?.visit > this.totalSelectedDays
          ? { ...d, isDisabled: false }
          : { ...d, isDisabled: true }
      })

      this.dayArray = disabledDays
      this.open = false
    },
    getDayContent() {
      return `day${this.selectedPackage?.visit > 1 ? 's' : ''}`
    },
    handleSchedule: function (pack) {
      this.isScheduled = true
      localStorage.setItem('dayArray', JSON.stringify(this.dayArray))
    },
    handleReset: function () {
      this.isScheduled = false
      this.selectedPackage = {}
      this.dayArray = []
      this.totalSelectedDay = 0
      localStorage.removeItem('selectedPackage')
      localStorage.removeItem('dayArray')
    },
  },
}
</script>

<style scope>
.large-button {
  min-width: 300px;
}
.danger-card,
.success-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 1rem;
  border: 1px solid var(--secondary-color);
  border-radius: 0.5rem;
  margin: 1rem;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.danger-card {
  border: 1px solid var(--secondary-color);
  background-color: var(--danger-color);
}
.success-card {
  background-color: var(--light-color);
  border: 1px solid var(--light-color);
}
.day-button {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  background-color: var(--dark-color);
  color: var(--background-color);
  font-size: 1.2rem;
}
.selected-day {
  background-color: var(--primary-color);
  color: var(--background-color);
}
.unselected-day {
  background-color: var(--light-color);
  color: var(--dark-color);
}
.package-card-header {
  display: flex;
  justify-items: center;
  justify-content: space-between;
}
.package-container {
  max-width: 800px;
  flex-direction: column;
  justify-items: center;
}
.price {
  font-size: 30px;
  font-weight: 600;
  color: #006658;
}
.price-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.save-tag {
  font-size: 16px;
  font-weight: 400;
  color: #e4e4e4;
  background-color: #006658;
  border-radius: 50px;
  border: none;
}
.grey-text {
  margin: 32px 0 8px 0;
  font-size: 22px;
  font-weight: 500;
  color: #8f8f8f;
}
.button {
  display: flex;
  border-radius: 10px;
  margin-top: 16px;
  width: 100%;
  min-height: 45px;
  font-size: 0.9rem;
  cursor: pointer;
  justify-content: center;
  align-items: center;
}

.button:hover {
  font-size: 1.1rem;
}
.button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}
.select-button {
  color: #e4e4e4;
  background-color: var(--dark-color);
}
.next-button {
  color: #e4e4e4;
  background-color: var(--secondary-color);
}
.close-button,
.detail-button {
  color: #000000;
  border: 2px solid var(--dark-color);
  background-color: var(--light-color);
}
.close-button {
  color: #e4e4e4;
  background-color: var(--dark-color);
  position: absolute;
  bottom: 16px;
  width: 85%;
}
.front-select-button {
  margin-top: 8px;
}
.flipper,
.card {
  width: 400px;
  height: 360px;
  margin: 12px;
  border-radius: 13px;
}
.front,
.back {
  width: 390px;
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
}

.front-content,
.back-content {
  border-radius: 13px;
  height: 370px;
  min-height: 370px;
}

.front-content {
  background-color: var(--light-color);
}

.back-content {
  background-color: var(--primary-color);
  color: #f5f5f5;
}

.flip-enter-active,
.flip-leave-active {
  transition: all 0.6s ease;
}
.flip-enter,
.flip-leave-to {
  transform: rotateY(180deg);
}
</style>
