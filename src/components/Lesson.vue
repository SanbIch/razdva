<template>
  <div class="lesson">
    <div class="time">
      <span>{{ timeStart }}</span>
      <span>-</span>
      <span>{{ timeEnd }}</span>
    </div>
    <div
      class="lesson-info"
      v-if="lesson['double'] != true && lesson['multiple'] != true"
    >
      <p class="name">
        {{ lesson["info"]["name"] }}
      </p>
      <p class="type">
        {{ lesson["info"]["type"] }}
      </p>
      <div class="teacher-aud">
        <p class="teacher">
          {{ lesson["info"]["teacher"] }}
        </p>
        <p class="aud">
          {{ lesson["info"]["aud"] }}
        </p>
      </div>
    </div>
    <div class="lesson-info double" v-if="lesson['double'] == true">
      <p class="p1">
        <span class="name">{{ lesson["info"]["a"]["name"] }}</span> <br />
        <span class="type">{{ lesson["info"]["a"]["type"] }}</span> <br />
        <span>{{ lesson["info"]["a"]["teacher"] }}</span> <br />
        <span class="aud">{{ lesson["info"]["a"]["aud"] }}</span>
      </p>
      <p class="p2">
        <span class="name">{{ lesson["info"]["b"]["name"] }}</span> <br />
        <span class="type">{{ lesson["info"]["b"]["type"] }}</span> <br />
        <span>{{ lesson["info"]["b"]["teacher"] }}</span> <br />
        <span class="aud">{{ lesson["info"]["b"]["aud"] }}</span>
      </p>
    </div>
    <div class="lesson-info-multiple" v-if="lesson['multiple'] == true">
      <div class="multiple" v-for="info in lesson['info']" :key="info">
        <p class="name">
          {{ info["name"] }}
          <span v-if="info['optionally'] == true">(по выбору)</span>
        </p>
        <p class="type">
          {{ info["type"] }}
        </p>
        <div class="teacher-aud">
          <p class="teacher">
            {{ info["teacher"] }}
          </p>
          <p class="aud">
            {{ info["aud"] }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import time from "../assets/times.json";

export default {
  props: {
    lesson: Object,
  },
  data() {
    return {
      selected: "",
    };
  },
  computed: {
    timeStart: function () {
      return time[this.lesson["time"]]["start"];
    },
    timeEnd: function () {
      return time[this.lesson["time"]]["end"];
    },
  },
};
</script>

<style lang="scss">
.time {
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-right: 1px solid black;
  padding: 5px;
}
.lesson {
  display: flex;
  border: 1px solid black;
  border-radius: 10px;
  margin-bottom: 10px;
}
.lesson-info {
  width: 100%;
  padding: 10px;
}
.lesson-info-multiple {
  padding: 10px 10px 0 10px;
}
.multiple {
  padding-bottom: 10px;
}
.double {
  background: linear-gradient(
    to top left,
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 0) calc(50% - 0.8px),
    rgba(0, 0, 0, 1) 50%,
    rgba(0, 0, 0, 0) calc(50% + 0.8px),
    rgba(0, 0, 0, 0) 100%
  );
}
.teacher-aud {
  display: flex;
  justify-content: space-between;
}
.name,
.type,
.teacher,
.aud {
  margin: 0;
  text-align: left;
}
.name {
  font-weight: bold;
}
.type {
  text-decoration: underline;
}
.aud {
  font-weight: bold;
}
.p1 {
  text-align: left;
}
.p2 {
  text-align: right;
}
</style>
