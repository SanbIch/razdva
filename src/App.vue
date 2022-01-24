<template>
  <Header @currentGroup="setCurGroup"></Header>
  <section class="selectDay">
    <button
      v-for="day in week"
      :key="day"
      class="dayBtn"
      v-on:click="BtnClick(day[2])"
      :class="[day[2] == curDay ? 'active' : '']"
    >
      <span class="hidden">{{ day[2] }}</span>
      <span class="dayBtn__num">{{ day[0] }}</span>
      <span>{{ day[1] }}</span>
    </button>
  </section>
  <Lessons :currentDay="curDay" :currentGroup="currentGroup"></Lessons>
</template>

<script>
import Header from "./components/Header.vue";
import Lessons from "./components/Lessons.vue";

export default {
  name: "App",
  components: {
    Header,
    Lessons,
  },
  data() {
    return {
      curDay: this.curDayFunc(new Date()),
      currentGroup: "",
    };
  },
  computed: {
    week: function () {
      let current = new Date();
      let week = new Array();
      current.setDate(current.getDate() - current.getDay() + 1);
      for (var i = 0; i < 6; i++) {
        week.push(this.fromatingDate(new Date(current)));
        current.setDate(current.getDate() + 1);
      }
      return week;
    },
  },
  methods: {
    curDayFunc: function (current) {
      let month = current.getMonth() + 1;
      let day = current.getDate();
      if (month < 10) month = "0" + month;
      if (day < 10) day = "0" + day;
      return `${current.getFullYear()}-${month}-${day}`;
    },
    fromatingDate: function (day) {
      let shortNames = ["ВС", "ПН", "ВТ", "СР", "ЧТ", "ПТ", "СБ"];
      let dayNum = day.getDate();
      let shortName = shortNames[day.getDay()];

      let month = day.getMonth() + 1;
      if (month < 10) month = "0" + month;
      if (dayNum < 10) dayNum = "0" + dayNum;

      return [dayNum, shortName, `${day.getFullYear()}-${month}-${dayNum}`];
    },
    BtnClick: function (value) {
      this.curDay = value;
    },
    setGroup: function (group) {
      this.currentGroup = group.group;
      localStorage.currentGroup = group.group;
    },
    setCurGroup: function (group) {
      this.currentGroup = group.group;
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap");
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Roboto, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}
.selectDay {
  padding: 3% 0;
  display: flex;
  justify-content: space-evenly;
}
.dayBtn {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #e4e4e4;
  border: none;
  padding: 5px 10px;
  border-radius: 8px;
  cursor: pointer;
}
.dayBtn__num {
  font-weight: bold;
  font-size: 1.2rem;
}
.hidden {
  display: none;
}
.active {
  background-color: #8b8b8b;
  color: white;
}
.lessons {
  padding: 0 5px;
}
</style>
