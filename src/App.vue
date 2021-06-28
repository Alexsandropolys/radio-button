<template>
  <div id="app">
    <div class="wrapper">
      <radio-button-group
        v-model="radioButtonGroupValue"
        :items="radioButtonGroupItems"
        :disabled="radioButtonGroupDisabled"
      />
    </div>
  </div>
</template>

<script>
import RadioButtonGroup from "./components/RadioButtonGroup";
export default {
  name: "App",
  components: { RadioButtonGroup },
  data() {
    return {
      radioButtonGroupItems: [
        { title: "item1", value: "item-id-1" },
        { title: "item2", value: "item-id-2", disabled: true },
        { title: "item3", value: "item-id-3" },
        { title: "item4", value: "item-id-4" }
      ],
      radioButtonGroupValue: "item-id-2",
      radioButtonGroupDisabled: false
    };
  }
};
</script>

<style>
.wrapper {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
