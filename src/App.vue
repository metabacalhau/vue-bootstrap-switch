<template>
  <div id="app">
    <toggle-button
      :value="value"
      :true-text="trueText"
      :false-text="falseText"
      :on-changed="onChanged"
      :disabled="disabled"
      :animate="animate">
    </toggle-button>
    <div>
      True text: <input type="text" v-model="trueText">
    </div>
    <div>
      False text:<input type="text" v-model="falseText">
    </div>
    <div>
      Disabled:<input type="checkbox" v-model="disabled">
    </div>
    <div>
      Animate:<input type="checkbox" v-model="animate">
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import ToggleButton from './ToggleButton'

export default {
  name: 'app',
  components: {
    ToggleButton
  },
  data() {
    return {
      value: true,
      trueText: 'ON',
      falseText: 'OFF',
      disabled: false,
      animate: true
    }
  },
  watch: {
    value() {
      console.log(this.value);
    }
  },
  methods: {
    onChanged(value) {
      this.value = value;
    }
  }
}
</script>