<template>
  <section class="lessons" v-if="lessons.length > 0 && currentGroup != ''">
    <Lesson v-for="lesson in lessons" :key="lesson" :lesson="lesson"> </Lesson>
  </section>
  <section v-else-if="lessons.length == 0 && currentGroup != ''">
    <p>ПАР НЕТ</p>
  </section>
  <section v-else-if="currentGroup == ''">
    <p>НЕ ВЫБРАНА ГРУППА</p>
  </section>
</template>

<script>
import rasp from "../assets/rasp.json";
import Lesson from "./Lesson.vue";

export default {
  props: {
    currentDay: String,
    currentGroup: String,
  },
  components: {
    Lesson,
  },
  data() {
    return {
      rasp: rasp,
      selected: "",
    };
  },
  computed: {
    lessons: function () {
      console.log("." + this.currentGroup + ".");
      if (this.currentGroup == "") return 0;
      const weekType = localStorage.weekType;
      const group = this.currentGroup;
      const day = String(new Date(Date.parse(this.currentDay)).getDay());
      if (day == 0) return 0;
      let arr = new Object(rasp[group]["lessons"][weekType][day]);
      let lessons = new Array();
      for (let i = 0; i < 5; i++) {
        if (arr[i]["empty"] == false) lessons.push(arr[i]);
      }

      return lessons;
    },
  },
  methods: {
    checkIsEven: function (weekNumber) {
      return weekNumber % 2 === 0;
    },
    ForcesUpdateComponent() {
      this.$forceUpdate();
    },
  },
};
</script>

<style lang="scss">
select {
  padding: 5px 5px;
  text-align: center;
  font-size: 1.5rem;
}
.header {
  background-color: aqua;
  padding: 10px 0;
}
.week-type {
  font-weight: bold;
  text-transform: uppercase;
}
</style>
