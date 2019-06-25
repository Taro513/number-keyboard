<template>
  <div class="keyboard" v-if="isVisible">
    <ul>
      <li v-for="item in 10" :key="item">
        <button @click="addNumber(item-1)">{{item-1}}</button>
      </li>
      <li >
        <button @click="delNumber"><span class="text">回退</span></button>
      </li>
      <li>
        <button @click="hideKeyboard"><span class="text">收起</span></button>
      </li>

    </ul>
  </div>
</template>

<script>
  export default {
    name: "NumberKeyboard",
    props:['numberLength'],
    data() {
      return {
        numberList: [],
        isVisible: false
      }
    },
    created() {
    },
    methods: {
      viewKeyboard() {
        this.isVisible = true;
        this.numberList = [];
      },
      hideKeyboard() {
        this.numberList = [];
        this.isVisible = false;
      },
      addNumber(num) {
        if (this.numberList.length < this.numberLength) {
          this.numberList.push(String(num));
        }
        this.setVal()
      },
      delNumber() {
        this.numberList = this.numberList.slice(0, this.numberList.length - 1);
        this.setVal()
      },
      setVal(){
        this.$emit('get-val',this.numberList.join(''))
      }
    }
  }
</script>

<style>

  .keyboard {
    width: 220px;
    position: absolute;
    right: 40px;
    top: 50%;
    background: #ffffff;
    padding: 20px 20px 10px;
    display: flex;
    align-items: center;
    z-index: 99999;
    transform: translateY(-25%);
  }

  .keyboard ul li{
    width: 33%;
    float: left;
    display: inline-block;
    font-size: 32px;
    text-align: center;
    margin-bottom: 10px;
  }

  .keyboard  ul li button {
    width: 100%;
    height: 46px;
    font-size: 26px;
    padding: 0;
    cursor: pointer;
  }

  .keyboard  ul li button .text {
    font-size: 16px;
  }


</style>
