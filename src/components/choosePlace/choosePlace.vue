<template>
  <div class="choosePlace" ref="choosePlace">
    <form action="" id="myForm">
      <div v-for="(cp,index) in seller.choosePlace" class="cpItems">
        <input type="radio" :id="index" class="magic-radio" name="cp"  :value="cp" v-model="picked">
        <label :for="index">{{cp}}</label> 
      </div>
      <br>
      <div class="selectInfo"><span>你选择的是：</span><span class="selEle">{{ picked }}</span></div>
      <button @submit.stop.prevent="submit" :disabled="!picked">确认选桌</button>
    </form>
  </div>
</template>

<script type="text/ecmascript-6">
  const ERR_OK = 0;

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        choosePlace: [],
        picked: ''
      };
    },
    created() {
      this.$http.get('/api/seller').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.choosePlace = response.data.choosePlace;
          this.$nextTick(() => {
            window.console.log(response.data.choosePlace);
          });
        }
      });
    },
    methods: {
      submit() {
        window.console.log(this.picked);
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .choosePlace
    width:100%;
    form
      .cpItems
        position:relative;
        line-height:4em;
        label
        position: relative;
        display: block;
        padding-left: 30px;
        cursor: pointer;
        vertical-align: middle;
        input
          display:none;
      .selectInfo
        line-height:1.5em;
        text-indent:30px;
        .selEle
          color:red;
          font-weight:bold;
      button
        display:block;
        margin:0 auto;
        margin-top:1rem;
        padding: .5em;
        font-size: 1em;
  @keyframes hover-color {
  from {
    border-color: #c0c0c0; }
  to {
    border-color: #3e97eb; } }

.magic-radio {
  position: absolute;
  display: none; }

.magic-radio[disabled] {
  cursor: not-allowed; }

.magic-radio + label {
  position: relative;
  display: block;
  padding-left: 30px;
  cursor: pointer;
  vertical-align: middle; }
  .magic-radio + label:hover:before {
    animation-duration: 0.4s;
    animation-fill-mode: both;
    animation-name: hover-color; }
  .magic-radio + label:before {
    position: absolute;
    top: 1.2rem;
    left: 0;
    display: inline-block;
    width: 20px;
    height: 20px;
    content: '';
    border: 1px solid #c0c0c0; }
  .magic-radio + label:after{
    position: absolute;
    display: none;
    content: ''; }

.magic-radio[disabled] + label {
  cursor: not-allowed;
  color: #e4e4e4; }
  .magic-radio[disabled] + label:hover, .magic-radio[disabled] + label:before, .magic-radio[disabled] + label:after{
    cursor: not-allowed; }
  .magic-radio[disabled] + label:hover:before{
    border: 1px solid #e4e4e4;
    animation-name: none; }
  .magic-radio[disabled] + label:before {
    border-color: #e4e4e4; }

.magic-radio:checked + label:before{
  animation-name: none; }

.magic-radio:checked + label:after {
  display: block; }

.magic-radio + label:before {
  border-radius: 50%; }

.magic-radio + label:after {
  top: 1.6rem;
  left: 7px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #3e97eb; }

.magic-radio:checked + label:before {
  border: 1px solid #3e97eb; }

.magic-radio:checked[disabled] + label:before {
  border: 1px solid #c9e2f9; }

.magic-radio:checked[disabled] + label:after {
  background: #c9e2f9; }
</style>