<template>
  <h1 ref="header">App Counter</h1>
  <CounterValue
    v-if="isShown"
    class="counter"
    v-for="obj in [{ index: 1, text: 'Clicked' }]"
    :title="obj.text"
    :value="counter"
    :key="obj.index"
  />
  {{ isShown ? "More than 10" : "Less than 10" }}
  <vs-button color="dark" v-on:click="onPlus">+</vs-button>
  <vs-button color="dark" v-if="canRenderMinusButton" @click="onMinus"
    >-</vs-button
  >
</template>
<script>
import CounterValue from "./components/CounterValue.vue";

const LOCAL_KEY_COUNTER = "counter";
const LOCAL_KEY_TEXT = "text";
const LOCAL_KEY_LIST = "list";

const save = (key, value) => localStorage.setItem(key, value);

let counterWatcher = null;

export default {
  name: "AppCounter",
  components: {
    CounterValue,
  },
  data() {
    return {
      counter: 0,
    };
  },
  created() {
    console.log("> created: ", this.counter);
    this.counter = localStorage.getItem(LOCAL_KEY_COUNTER) || 0;
    counterWatcher = this.$watch(
      () => this.counter,
      (newValue, oldValue) => {
        console.log("> counter watched:", { newValue, oldValue });
        save(LOCAL_KEY_COUNTER, newValue);
      }
    );
  },
  mounted() {
    console.log("> mounted: ", this.counter);
  },
  computed: {
    isShown() {
      return this.counter < 15;
    },
    canRenderMinusButton() {
      return this.counter > 0;
    },
    isAllowedToSave() {
      return this.inputText.length === 0;
    },
  },
  methods: {
    onPlus() {
      this.counter++;
      console.log("> Counter -> onPlus:", this.counter, this);
    },
    onMinus() {
      this.counter--;
      if (this.counter === 0) {
        this.$refs.header.innerText = `Header: ${this.counter}`;
      }
      console.log("> Counter -> onMinus:", this.counter);
    },
  },
  unmounted() {
    counterWatcher();
  },
};
</script>
<style lang="scss" scoped>
.counter {
  color: green;
}
</style>
