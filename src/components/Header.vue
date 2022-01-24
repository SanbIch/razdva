<template>
  <header class="header">
    <select name="" id="" v-model="selected" @change="store(selected)">
      <option disabled value="">Выберите группу</option>
      <option
        :value="group.value"
        v-for="group in groups.groups"
        :key="group.id"
      >
        {{ group.name }}
      </option>
    </select>
    <p class="week">
      Неделя
      <span class="week-type">{{
        weekType == "up" ? "верхняя" : "нижняя"
      }}</span>
    </p>
  </header>
</template>

<script>
import groups from "../assets/groups.json";

export default {
  emits: ["currentGroup"],
  data() {
    return {
      groups: groups,
    };
  },
  created() {
    if (localStorage.currentGroup == undefined) this.selected = "";
    else this.selected = localStorage.currentGroup;
    this.$emit("currentGroup", {
      group: this.selected,
    });
  },
  computed: {
    weekType: function () {
      let date = new Date();
      if (date.getDay() == 0) date.setDate(date.getDate() + 1);
      let type = "";

      date.setDate(date.getDate() + 3 - ((date.getDay() + 6) % 7));

      let week1 = new Date(date.getFullYear(), 0, 4);
      if (
        this.checkIsEven(
          1 +
            Math.round(
              ((date.getTime() - week1.getTime()) / 86400000 -
                3 +
                ((week1.getDay() + 6) % 7)) /
                7
            )
        )
      )
        type = "down";
      else type = "up";
      localStorage.weekType = type;
      return type;
    },
  },
  methods: {
    store: function (newValue) {
      this.$emit("currentGroup", {
        group: newValue,
      });
      localStorage.currentGroup = newValue;
      // console.log(localStorage.currentGroup);
    },
    checkIsEven: function (weekNumber) {
      return weekNumber % 2 === 0;
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
.week {
  font-size: 1.3rem;
}
</style>
