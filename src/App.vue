<script setup>

import {computed, ref} from "vue";

const currentDate = ref({
  year: new Date().getFullYear(),
  month: new Date().getMonth(),
  day: new Date().getDate(),
})

const monthNames = {
  en: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
  ru: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь']
}

function prevMonth() {
  if (currentDate.value.month === 0) {
    currentDate.value.year -= 1;
    currentDate.value.month = 11;
  }
  else {
    currentDate.value.month -= 1;
  }
}

function nextMonth() {
  if (currentDate.value.month === 11) {
    currentDate.value.year += 1;
    currentDate.value.month = 0;
  }
  else {
    currentDate.value.month += 1;
  }
}

const daysInMonth = computed(() => {
  const totalDays = new Date(currentDate.value.year, currentDate.value.month + 1, 0).getDate();
  return Array.from({length: totalDays}, (_, index) => index + 1)
})

const currentLanguage = ref('en');
const currentMonthNames = ref([]);
const currentDayNames = ref([]);

function changeLanguage(language) {
  currentLanguage.value = language;
  currentMonthNames.value = monthNames[language];

  if (language === 'en') {
    currentDayNames.value = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
  } else if (language === 'ru') {
    currentDayNames.value = ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
  }
  localStorage.setItem('selectedLanguage', language);
}

const savedLanguage = localStorage.getItem('selectedLanguage');
if (savedLanguage) {
  changeLanguage(savedLanguage)
}

function selectDay(day) {
  const selectedDate = new Date(currentDate.value.year, currentDate.value.month, day);
  alert(`Выбранная дата: ${selectedDate.toDateString()}`)
}

</script>

<template>
  <div class="calendar">
    <div class="header">
      <button class="left" @click="prevMonth"><</button>
      <span class="center"> {{ monthNames[currentLanguage][currentDate.month]}} {{ currentDate.year }}</span>
      <button class="right" @click="nextMonth">></button>
    </div>
    <div class="days">
      <div class="data" v-for="day in currentDayNames"> {{ day }}</div>
      <div class="currentDays" v-for="day in daysInMonth" :key="day" @click="selectDay(day)">{{ day }}</div>
    </div>
    <button class="change" @click="changeLanguage('en')">Смена языка на Английский</button>
    <button class="change" @click="changeLanguage('ru')">Смена языка на Русский</button>
  </div>
</template>

<style scoped>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.calendar {
  display: inline-block;
  justify-content: center;
  align-items: center;
  text-align: center;
  height: 230px;
  width: 260px;
  border: 1px solid gray;
  margin: 10px;
}

.left {
  background-color: transparent;
  border: none;
  height: 16px;
}

.data {
  margin-bottom: 10px;
}
.currentDays {
  font-size: 15px;
  height: 30px;
  width: 30px;
}

.center {
  align-items: center;
  text-align: center;
  width: 200px;
}

.right {
  background-color: transparent;
  border: none;
  height: 16px;
}

.change {
  margin-top: 10px;
}

.header {
  text-align: center;
  justify-content: center;
  align-items: center;
  display: flex;
  margin: 10px;
}
.currentDays:hover {
  border: 1px solid deepskyblue;
}
.currentDays:active {
  border: 2px solid deepskyblue;
}

.days {
  margin: 10px;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
  align-items: center;
  justify-content: center;
}
</style>
