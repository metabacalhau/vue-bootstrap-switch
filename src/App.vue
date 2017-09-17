<template>
  <div id="app">
    <toggle-button
      :id="'toggle-button-1'"
      :value="value"
      :true-text="trueText"
      :false-text="falseText"
      :on-changed="onChanged"
      :disabled="disabled"
      :animate="animate"
      v-if="vIf"
      v-show="vShow">
    </toggle-button>
    <div>
      Result: <span style="font-weight: bold;" v-text="value"></span>
    </div>
    <div>
      Value: <input type="checkbox" v-model="value">
    </div>
    <div>
      <span>v-if:</span><input type="checkbox" v-model="vIf"> <span>v-show:</span><input type="checkbox" v-model="vShow">
    </div>
    <div>
      True text: <input type="text" v-model="trueText">
    </div>
    <div>
      False text: <input type="text" v-model="falseText">
    </div>
    <div>
      Disabled: <input type="checkbox" v-model="disabled">
    </div>
    <div>
      Animate: <input type="checkbox" v-model="animate">
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
      animate: true,
      vIf: true,
      vShow: true
    }
  },
  methods: {
    onChanged(value) {
      this.value = value;
    }
  }
}
</script>