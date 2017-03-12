<template>
    <div :class="css"
        v-on:click="toggle"
        ref="wrapper">
        <div class="bootstrap-switch-container"
            ref="container">
            <span class="bootstrap-switch-handle-on bootstrap-switch-primary"
                v-text="trueText"
                ref="on"></span>
            <span class="bootstrap-switch-label"
                ref="label">&nbsp;</span>
            <span class="bootstrap-switch-handle-off bootstrap-switch-default"
                v-text="falseText"
                ref="off"></span>
            <input id="switch-state"
                type="checkbox"
                v-model="value">
        </div>
    </div>
</template>

<script>
import Vue from 'vue'

export default {
    name: 'toggle-button',
    props: {
        value: {
            type: Boolean,
            required: true
        },
        onChanged: {
            type: Function,
            required: true
        },
        trueText: {
            type: String
        },
        falseText: {
            type: String
        },
        disabled: {
            required: false,
            default: function () {
                return false;
            }
        },
        animate: {
            required: false,
            default: function () {
                return true;
            }
        }
    },
    data () {
        return {
            localValue: this.value,
            localAnimate: this.animate
        }
    },
    watch: {
        trueText(val) {
            this.$nextTick(function () {
                this.width();
            })
        },
        falseText(val) {
            this.$nextTick(function () {
                this.width();
            })
        },
        animate(val) {
            this.localAnimate = val;
        }
    },
    computed: {
        css() {
            return {
                'bootstrap-switch': true,
                'bootstrap-switch-wrapper': false,
                'bootstrap-switch-animate': this.localAnimate,
                'bootstrap-switch-disabled': this.disabled
            }
        }
    },
    methods: {
        toggle() {
            if (!this.disabled) {
                this.localValue = !this.localValue;

                if (this.localValue) {
                    this.$refs.container.style.marginLeft = 0;
                }
                else {
                    this.$refs.container.style.marginLeft = (-1 * this.$refs.on.offsetWidth) + 'px';
                }

                this.onChanged(this.localValue);
            }
        },
        width() {
            const animate = this.localAnimate;
            
            this.localAnimate = false;

            this.$refs.on.style.width = 0;
            this.$refs.label.style.width = 0;
            this.$refs.off.style.width = 0;

            const handleWidth = Math.round(Math.max(this.$refs.on.offsetWidth, this.$refs.off.offsetWidth));

            this.$refs.on.style.width = handleWidth + 'px';
            this.$refs.label.style.width = handleWidth + 'px';
            this.$refs.off.style.width = handleWidth + 'px';

            let labelWidth = 0;

            if (this.$refs.label.offsetWidth < handleWidth) {
                labelWidth = handleWidth;
            }
            else {
                labelWidth = this.$refs.label.offsetWidth;
            }

            if (this.localValue) {
                this.$refs.container.style.marginLeft = 0;
            }
            else {
                this.$refs.container.style.marginLeft = (-1 * labelWidth) + 'px';
            }

            this.$refs.label.style.width = labelWidth + 'px';
            this.$refs.container.style.width = ((handleWidth * 2) + labelWidth) + 'px';
            this.$refs.wrapper.style.width = (handleWidth + labelWidth) + 'px';

            setTimeout(function () {
                this.localAnimate = animate;
            }.bind(this), 0.5);
        }
    },
    mounted() {
        this.width();
    }
}
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
        /*width: 106px;*/
        z-index: 0;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
        vertical-align: middle;
        -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
        -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
        transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
    }

    .bootstrap-switch .bootstrap-switch-container {
        display: inline-block;
        top: 0;
        border-radius: 4px;
        -webkit-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0);
        /*width: 159px;*/
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
        font-size: 14px;
        line-height: 20px;
    }

    .bootstrap-switch .bootstrap-switch-handle-on,
    .bootstrap-switch .bootstrap-switch-handle-off {
        text-align: center;
        z-index: 1;
    }

    .bootstrap-switch .bootstrap-switch-handle-on.bootstrap-switch-primary,
    .bootstrap-switch .bootstrap-switch-handle-off.bootstrap-switch-primary {
        color: #fff;
        background: #337ab7;
    }

    .bootstrap-switch .bootstrap-switch-handle-on.bootstrap-switch-info,
    .bootstrap-switch .bootstrap-switch-handle-off.bootstrap-switch-info {
        color: #fff;
        background: #5bc0de;
    }

    .bootstrap-switch .bootstrap-switch-handle-on.bootstrap-switch-success,
    .bootstrap-switch .bootstrap-switch-handle-off.bootstrap-switch-success {
        color: #fff;
        background: #5cb85c;
    }

    .bootstrap-switch .bootstrap-switch-handle-on.bootstrap-switch-warning,
    .bootstrap-switch .bootstrap-switch-handle-off.bootstrap-switch-warning {
        background: #f0ad4e;
        color: #fff;
    }

    .bootstrap-switch .bootstrap-switch-handle-on.bootstrap-switch-danger,
    .bootstrap-switch .bootstrap-switch-handle-off.bootstrap-switch-danger {
        color: #fff;
        background: #d9534f;
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

    .bootstrap-switch span::before {
        content: "\200b";
    }

    .bootstrap-switch .bootstrap-switch-handle-on {
        border-bottom-left-radius: 3px;
        border-top-left-radius: 3px;
    }

    .bootstrap-switch .bootstrap-switch-handle-off {
        border-bottom-right-radius: 3px;
        border-top-right-radius: 3px;
    }

    .bootstrap-switch input[type='radio'],
    .bootstrap-switch input[type='checkbox'] {
        position: absolute !important;
        top: 0;
        left: 0;
        margin: 0;
        z-index: -1;
        opacity: 0;
        filter: alpha(opacity=0);
        visibility: hidden;
    }

    .bootstrap-switch.bootstrap-switch-disabled,
    .bootstrap-switch.bootstrap-switch-readonly,
    .bootstrap-switch.bootstrap-switch-indeterminate {
        cursor: default !important;
    }

    .bootstrap-switch.bootstrap-switch-disabled .bootstrap-switch-handle-on,
    .bootstrap-switch.bootstrap-switch-readonly .bootstrap-switch-handle-on,
    .bootstrap-switch.bootstrap-switch-indeterminate .bootstrap-switch-handle-on,
    .bootstrap-switch.bootstrap-switch-disabled .bootstrap-switch-handle-off,
    .bootstrap-switch.bootstrap-switch-readonly .bootstrap-switch-handle-off,
    .bootstrap-switch.bootstrap-switch-indeterminate .bootstrap-switch-handle-off,
    .bootstrap-switch.bootstrap-switch-disabled .bootstrap-switch-label,
    .bootstrap-switch.bootstrap-switch-readonly .bootstrap-switch-label,
    .bootstrap-switch.bootstrap-switch-indeterminate .bootstrap-switch-label {
        opacity: 0.5;
        filter: alpha(opacity=50);
        cursor: default !important;
    }

    .bootstrap-switch.bootstrap-switch-animate .bootstrap-switch-container {
        -webkit-transition: margin-left 0.5s;
        -o-transition: margin-left 0.5s;
        transition: margin-left 0.5s;
    }

    .bootstrap-switch.bootstrap-switch-inverse .bootstrap-switch-handle-on {
        border-bottom-left-radius: 0;
        border-top-left-radius: 0;
        border-bottom-right-radius: 3px;
        border-top-right-radius: 3px;
    }

    .bootstrap-switch.bootstrap-switch-inverse .bootstrap-switch-handle-off {
        border-bottom-right-radius: 0;
        border-top-right-radius: 0;
        border-bottom-left-radius: 3px;
        border-top-left-radius: 3px;
    }

    .bootstrap-switch.bootstrap-switch-focused {
        border-color: #66afe9;
        outline: 0;
        -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
        box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
    }

    .bootstrap-switch.bootstrap-switch-on .bootstrap-switch-label,
    .bootstrap-switch.bootstrap-switch-inverse.bootstrap-switch-off .bootstrap-switch-label {
        border-bottom-right-radius: 3px;
        border-top-right-radius: 3px;
    }

    .bootstrap-switch.bootstrap-switch-off .bootstrap-switch-label,
    .bootstrap-switch.bootstrap-switch-inverse.bootstrap-switch-on .bootstrap-switch-label {
        border-bottom-left-radius: 3px;
        border-top-left-radius: 3px;
    }
</style>