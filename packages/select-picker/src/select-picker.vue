<template>
  <mt-popup v-model="visible" position="bottom" class="mint-datetime">
    <mt-picker
      :slots="dateSlots"
      @change="onChange"
      :visible-item-count="visibleItemCount"
      class="mint-datetime-picker"
      ref="picker"
      show-toolbar>
      <span class="mint-datetime-action mint-datetime-cancel" @click="visible = false">{{ cancelText }}</span>
      <span class="mint-datetime-action mint-datetime-confirm" @click="confirm">{{ confirmText }}</span>
    </mt-picker>
  </mt-popup>
</template>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component datetime {
      width: 100%;

      .picker-slot-wrapper, .picker-item {
        backface-visibility: hidden;
      }

      .picker-toolbar {
        border-bottom: solid 1px #eaeaea;
      }

      @descendent action {
        display: inline-block;
        width: 50%;
        text-align: center;
        line-height: 40px;
        font-size: 16px;
        color: $color-blue;
      }

      @descendent cancel {
        float: left;
      }

      @descendent confirm {
        float: right;
      }
    }
  }
</style>

<script>
  import picker from 'mint-ui/packages/picker/index.js';
  import popup from 'mint-ui/packages/popup/index.js';
  if (process.env.NODE_ENV === 'component') {
    require('mint-ui/packages/picker/style.css');
    require('mint-ui/packages/popup/style.css');
  }

  export default {
    name: 'mt-select-picker',

    props: {
      cancelText: {
        type: String,
        default: '取消'
      },
      confirmText: {
        type: String,
        default: '确定'
      },
      data: {
        type: Array,
        default: () => {
          return [];
        }
      },
      visibleItemCount: {
        type: Number,
        default: 7
      },
      value: null
    },

    data() {
      return {
        visible: false,
        currentValue: null,
        dateSlots: [{
          values: this.data,
          textAlign: 'center'
        }]
      };
    },

    components: {
      'mt-picker': picker,
      'mt-popup': popup
    },

    methods: {
      open() {
        this.visible = true;
      },

      close() {
        this.visible = false;
      },

      onChange(picker) {
        let values = picker.$children.filter(child => child.currentValue !== undefined).map(child => child.currentValue);
        this.currentValue = values[0];
      },

      confirm() {
        this.visible = false;
        this.$emit('confirm', this.currentValue);
        this.$emit('input', this.currentValue);
      }
    },

    watch: {
      value(val) {
        this.currentValue = val;
      }
    },

    mounted() {
      if (!this.value) {
        this.currentValue = this.data[0];
      } else {
        this.currentValue = this.value;
        this.$refs.picker.setSlotValue(0, this.value);
      }
    }
  };
</script>
