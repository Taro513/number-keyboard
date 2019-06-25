<template>
  <div class="container">
    <div ref="form" :model="form" class="phone-form">
      <div >
        <span>手机号：</span>
        <input v-model="form.phone" maxlength="11" @focus="inputNumber($el,'phone')" type="number"
                  ref="phoneNum"/>
      </div>
      <div >
        <span>验证码：</span>
        <input v-model="form.verCode" maxlength="6" type="number"
               @focus="inputNumber($el,'verCode')"/>
        <button @click="countDown()"
                :disabled="count!==60">{{count===60?'发送验证码':count+'秒'}}
        </button>
      </div>
    </div>
    <!-- 数字软键盘 -->
    <number-keyboard :number-length="numberLength" ref="keyboard" @get-val="getVal"></number-keyboard>
  </div>
</template>

<script>
  import NumberKeyboard from './NumberKeyboard'

  export default {
    name: "PhoneVerification",
    components:{
      NumberKeyboard
    },
    data() {
      var validatePhone = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('请输入手机号'));
        }
        let reg = /^1[34578]\d{9}$/;
        if (value !== '') {
          if (reg.test(value)) {
            callback();
          } else {
            callback(new Error('手机号格式有误'));
          }
        }
      };
      return {
        form: {
          phone: '',
          verCode: '',
        },
        count: 60,
        timer: '',

        isVisible: false,
        numberList: [],
        curEl: '',
        inputVal:'',
        numberLength:0
      }
    },
    created() {
    },
    methods: {
      //倒计时
      countDown() {
        this.count = 59;
        this.timer = setInterval(() => {
          if (this.count > 1) {
            this.count--;
          } else {
            this.count = 60;
            clearInterval(this.timer);
          }
        }, 1000)
      },
      inputNumber(e,el) {
        this.$refs.keyboard.viewKeyboard();
        this.curEl = el;
        if (el === 'phone'){
          this.numberLength = 11
        } else {
          this.numberLength = 6
        }
        if (e && e.stopPropagation) {
          e.stopPropagation();
        } else {
          window.event.cancelBubble = true;
        }
      },
      getVal(data){
        this.inputVal = data;
      }
    },
    watch:{
      inputVal(newVal){
        this.form[this.curEl] = newVal;
        if (newVal.length === this.numberLength){
          this.$refs.keyboard.hideKeyboard();
        }
      }
    }
  }
</script>
<style>
.container{
  padding: 20px;
}
</style>
