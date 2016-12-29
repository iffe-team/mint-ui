<template>
  <div class="page-datetime">
    <h1 class="page-title">Datetime Picker</h1>
    <div class="page-datetime-wrapper">
      <mt-button @click.native="open('picker1')" size="large">点击弹出 Select Picker</mt-button>
    </div>
    <mt-select-picker
      ref="picker1"
      :data="pickerData"
      :visibleItemCount="7"
      v-model="selectValue"
      value="郑成功"
      @confirm="handleChange">
    </mt-select-picker>
  </div>
</template>

<style>
  @component-namespace page {
    @component datetime {
      @descendent wrapper {
        padding: 0 20px;
        position: absolute 50% * * *;
        width: 100%;
        transform: translateY(-50%);

        button:not:(last-child) {
          margin-bottom: 20px;
        }
      }
    }
  }
</style>

<script>
  import { Toast } from 'src/index';

  export default {
    data() {
      return {
        selectValue: '',
        pickerData: ['李健', '王林', '郑成功', '胡汉三', '蒋介石', '郑鑫', '陈天灿', '刘志龙', '梅艳芳']
      };
    },

    watch: {
      selectValue(val) {
        console.log(val);
      }
    },

    methods: {
      open(picker) {
        this.$refs[picker].open();
      },

      handleChange(value) {
        Toast({
          message: '已选择 ' + value.toString(),
          position: 'bottom'
        });
      }
    }
  };
</script>
