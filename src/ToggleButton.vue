<template>
  <div :class="css" v-on:click="toggle" ref="wrapper">
    <div class="bootstrap-switch-container" ref="container">
      <span class="bootstrap-switch-handle-on bootstrap-switch-primary"
        v-text="trueText"
        ref="on"
      ></span>
      <span class="bootstrap-switch-label" ref="label">&nbsp;</span>
      <span
        class="bootstrap-switch-handle-off bootstrap-switch-default"
        v-text="falseText"
        ref="off"
      ></span>
      <input type="checkbox" :id="id" :disabled="disabled" v-model="value" />
    </div>
  </div>
</template>

<script>
import 'vue';

export default {
  name: 'toggle-button',
  props: {
    id: {
      required: true,
    },
    value: {
      type: Boolean,
      required: true,
    },
    onChanged: {
      type: Function,
      required: true,
    },
    trueText: {
      type: String,
    },
    falseText: {
      type: String,
    },
    disabled: {
      required: false,
      default() {
        return false;
      },
    },
    animate: {
      required: false,
      default() {
        return true;
      },
    },
  },
  data() {
    return {
      localValue: this.value,
      localAnimate: this.animate,
    };
  },
  watch: {
    trueText() {
      this.$nextTick(function () {
        this.adjustWidth();
      });
    },
    falseText() {
      this.$nextTick(function () {
        this.adjustWidth();
      });
    },
    animate(val) {
      this.localAnimate = val;
    },
    value() {
      if (this.localAnimate) {
        this.$el.classList.add('bootstrap-switch-animate');
      }

      if (this.value) {
        this.$refs.container.style.marginLeft = 0;
      } else {
        const width = this.getWidth();

        this.$refs.container.style.marginLeft = `${-1 * width.on}px`;
      }
    },
  },
  computed: {
    css() {
      return {
        'bootstrap-switch': true,
        'bootstrap-switch-animate': this.localAnimate,
        'bootstrap-switch-disabled': this.disabled,
      };
    },
  },
  methods: {
    toggle() {
      if (!this.disabled) {
        this.localValue = !this.localValue;

        if (this.localValue) {
          this.$refs.container.style.marginLeft = 0;
        } else {
          this.$refs.container.style.marginLeft = `${-1 *
            this.$refs.on.offsetWidth}px`;
        }

        this.onChanged(this.localValue);
      }
    },
    getWidth() {
      const el = this.$el;
      const width = { on: 0, label: 0, off: 0 };
      const isVisible = this.$el.offsetParent != null;

      if (isVisible) {
        width.on = this.$refs.on.getBoundingClientRect().width;
        width.label = this.$refs.label.getBoundingClientRect().width;
        width.off = this.$refs.off.getBoundingClientRect().width;
      } else {
        // get width and height of the label element
        const tempContainer = document.body;
        const elCloned = el.cloneNode(true);

        // make it invisible in the temporary DOM container
        elCloned.style.setProperty('display', '');
        elCloned.style.visibility = 'hidden';
        elCloned.style.position = 'absolute';

        tempContainer.appendChild(elCloned);

        width.on = elCloned
          .getElementsByClassName('bootstrap-switch-handle-on')[0]
          .getBoundingClientRect().width;
        width.label = elCloned
          .getElementsByClassName('bootstrap-switch-label')[0]
          .getBoundingClientRect().width;
        width.off = elCloned
          .getElementsByClassName('bootstrap-switch-handle-off')[0]
          .getBoundingClientRect().width;

        tempContainer.removeChild(elCloned);
      }

      return width;
    },
    adjustWidth() {
      if (this.localAnimate) {
        // remove animation to avoid side-effects
        this.$el.classList.remove('bootstrap-switch-animate');
      }

      this.$refs.container.style.width = 0;

      const width = this.getWidth();

      const handleWidth = Math.max(width.on, width.off);

      this.$refs.on.style.width = `${handleWidth}px`;
      this.$refs.label.style.width = `${handleWidth}px`;
      this.$refs.off.style.width = `${handleWidth}px`;

      let labelWidth = 0;

      if (width.label < handleWidth) {
        labelWidth = handleWidth;
      } else {
        labelWidth = width.label;
      }

      if (this.localValue) {
        this.$refs.container.style.marginLeft = 0;
      } else {
        this.$refs.container.style.marginLeft = `${-1 * labelWidth}px`;
      }

      this.$refs.label.style.width = `${labelWidth}px`;
      this.$refs.container.style.width = `${(handleWidth * 2) + labelWidth}px`;
      this.$refs.wrapper.style.width = `${handleWidth + labelWidth}px`;
    },
  },
  mounted() {
    this.adjustWidth();
  },
};
</script>

<style>
/**
    * bootstrap-switch - Turn checkboxes and radio buttons into toggle switches.
    *
    * @version v3.3.4
    * @homepage https://bttstrp.github.io/bootstrap-switch
    * @author Mattia Larentis <mattia@larentis.eu> (http://larentis.eu)
    * @license Apache-2.0
    */

.bootstrap-switch {
  display: inline-block;
  direction: ltr;
  cursor: pointer;
  border-radius: 4px;
  border: 1px solid;
  border-color: #ccc;
  position: relative;
  text-align: left;
  overflow: hidden;
  line-height: 8px;
  z-index: 0;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  vertical-align: middle;
  -webkit-transition: border-color ease-in-out 0.15s,
    box-shadow ease-in-out 0.15s;
  -o-transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
  transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
}

.bootstrap-switch .bootstrap-switch-container {
  display: table;
  top: 0;
  border-radius: 0;
  -webkit-transform: translate3d(0, 0, 0);
  transform: translate3d(0, 0, 0);
}

.bootstrap-switch .bootstrap-switch-handle-on,
.bootstrap-switch .bootstrap-switch-handle-off,
.bootstrap-switch .bootstrap-switch-label {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  cursor: pointer;
  display: table-cell;
  vertical-align: middle;
  padding: 6px 12px;
  font-size: 11px;
  text-transform: uppercase;
  line-height: 20px;
}

.bootstrap-switch .bootstrap-switch-handle-on,
.bootstrap-switch .bootstrap-switch-handle-off {
  text-align: center;
  z-index: 1;
}

.bootstrap-switch .bootstrap-switch-handle-on {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}

.bootstrap-switch .bootstrap-switch-handle-off {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}

.bootstrap-switch .bootstrap-switch-handle-on.bootstrap-switch-primary,
.bootstrap-switch .bootstrap-switch-handle-off.bootstrap-switch-primary {
  color: #fff;
  background: #337ab7;
}

.bootstrap-switch .bootstrap-switch-handle-on.bootstrap-switch-default,
.bootstrap-switch .bootstrap-switch-handle-off.bootstrap-switch-default {
  color: #000;
  background: #eeeeee;
}

.bootstrap-switch .bootstrap-switch-label {
  text-align: center;
  margin-top: -1px;
  margin-bottom: -1px;
  z-index: 100;
  color: #333;
  background: #fff;
}

.bootstrap-switch.bootstrap-switch-disabled {
  cursor: not-allowed;
}

.bootstrap-switch.bootstrap-switch-disabled .bootstrap-switch-handle-on,
.bootstrap-switch.bootstrap-switch-disabled .bootstrap-switch-handle-off,
.bootstrap-switch.bootstrap-switch-disabled .bootstrap-switch-label {
  opacity: 0.5;
  filter: alpha(opacity=50);
  cursor: not-allowed;
}

.bootstrap-switch.bootstrap-switch-mini .bootstrap-switch-handle-on,
.bootstrap-switch.bootstrap-switch-mini .bootstrap-switch-handle-off,
.bootstrap-switch.bootstrap-switch-mini .bootstrap-switch-label {
  padding: 5px 6px;
  line-height: 1.4em;
}

.bootstrap-switch.bootstrap-switch-inverse .bootstrap-switch-handle-on,
.bootstrap-switch.bootstrap-switch-inverse .bootstrap-switch-handle-off {
  border-radius: 0;
}

.bootstrap-switch.bootstrap-switch-on .bootstrap-switch-label,
.bootstrap-switch.bootstrap-switch-inverse.bootstrap-switch-off
  .bootstrap-switch-label {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}

.bootstrap-switch.bootstrap-switch-off .bootstrap-switch-label,
.bootstrap-switch.bootstrap-switch-inverse.bootstrap-switch-on
  .bootstrap-switch-label {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}

.bootstrap-switch input[type="checkbox"] {
  position: absolute !important;
  top: 0;
  left: 0;
  margin: 0;
  z-index: -1;
  opacity: 0;
  filter: alpha(opacity=0);
  visibility: hidden;
}

.bootstrap-switch.bootstrap-switch-animate .bootstrap-switch-container {
  -webkit-transition: margin-left 0.5s;
  -o-transition: margin-left 0.5s;
  transition: margin-left 0.5s;
}
</style>
