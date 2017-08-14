<template>
  <date-picker @change="change" :date="date" :option="option"></date-picker>
</template>

<script>
  // import DatePicker from 'vue-datepicker'
  import DatePicker from './vue-datepicker/vue-datepicker.vue'
  import dateFormat from 'bz-lib/functions/dateFormat'
  export default {
    props: {
      value: {
        requried: true
      },
      inputStyle: {
        type: Object,
        default: function () {
          return {
            'display': 'inline-block',
            'padding': '.9em 1em;',
            'line-height': '22px',
            'font-size': '14px',
            'border': '1px solid #E1E6EB',
            'box-shadow': 'inset 0px 1px 4px 0px rgba(0,0,0,0.10)',
            'border-radius': '2px',
            'color': 'rgba(0,0,0,.87)'
          }
        }
      },
      placeholder: {
        type: String,
        default: '请选择日期'
      },
      option: {
        type: Object,
        default: function () {
          return {
            type: 'day',
            month: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
            format: 'YYYY-MM-DD',
            placeholder: this.placeholder,
            inputStyle: this.inputStyle,
            color: {
              header: '#54C6A8',
              headerText: '#fff'
            },
            buttons: {
              ok: '好的',
              cancel: '取消'
            },
            overlayOpacity: 0.5, // 0.5 as default
            dismissible: true // as true as default
          }
        }
      }
    },
    watch: {
      value: function (val) {
        this.setDate(val)
      }
    },
    components: {
      DatePicker
    },
    data: function () {
      return {
        same_date: '',
        date: {
          time: ''
        },
        multi_option: {
          type: 'multi-day',
          week: ['Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa', 'Su'],
          month: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
          format: 'YYYY-MM-DD',
          placeholder: '请选择不可用日期',
          inputStyle: {
            'display': 'inline-block',
            'padding': '.9em 1em;',
            'line-height': '22px',
            'font-size': '14px',
            'border': '1px solid #E1E6EB',
            'box-shadow': 'inset 0px 1px 4px 0px rgba(0,0,0,0.10)',
            'border-radius': '2px',
            'color': 'rgba(0,0,0,.87)'
          },
          color: {
            header: '#54C6A8',
            headerText: '#fff'
          },
          buttons: {
            ok: '好的',
            cancel: '取消'
          },
          overlayOpacity: 0.5, // 0.5 as default
          dismissible: true // as true as default
        }
      }
    },
    mounted: function () {
      this.setDate(this.value)
      this.$nextTick(function () {
        // code that assumes this.$el is in-document
      })
    },
    methods: {
      setDate: function (val) {
        if (typeof val === 'number') { this.date.time = dateFormat(val, 'YYYY-MM-dd') }
        if (typeof val === 'string') {
          if (this.option.type === 'day') { // 只要天
            val = window.Date.parse(val)
            val = dateFormat(val, 'YYYY-MM-dd')
          }
          this.date.time = val
        }
        this.same_date = this.date.time
      },
      change: function (date) {
        if (this.same_date === date) { // 如果选的是同一个, 那么清空, 实现不选中的功能
          this.$emit('input', '')
          this.$emit('change', '')
          this.same_date = ''
        } else {
          this.$emit('input', date)
          this.$emit('change', date)
          this.same_date = date
        }
      }
    }
  }
</script>
