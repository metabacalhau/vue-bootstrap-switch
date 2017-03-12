<template>
  <div id="app">
    <toggle-button
      :value="value"
      :true-text="trueText"
      :false-text="falseText"
      :on-changed="onChanged"
      :disabled="false">
    </toggle-button>
    <div>
      True text: <input type="text" v-model="trueText">
    </div>
    <div>
      False text:<input type="text" v-model="falseText">
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import ToggleButton from '../ToggleButton'

export default {
  name: 'app',
  components: {
    ToggleButton
  },
  data() {
    return {
      value: true,
      trueText: 'ON',
      falseText: 'OFF'
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