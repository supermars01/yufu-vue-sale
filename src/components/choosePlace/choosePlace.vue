<template>
  <div class="choosePlace" ref="choosePlace">
    <div class="list-header">
        <h1 class="title">购物车</h1>
    </div>
    <div class="list-content" ref="listContent">
       <ul>
          <li class="foodsList" v-for="food in selectFoods">
            <span class="name">{{food.name}}</span>
              <div class="price">
                <span>￥{{food.price}}</span>
                <span>{{food.count}}</span>
              </div>
          </li>
        </ul>     
    </div>
    <form action="" id="myForm">
      <div v-for="(cp,index) in choosePlace" class="cpItems">
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
        picked: '',
        selectFoods: []
      };
    },
    created() {
      this.$http.get('/api/seller').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.choosePlace = response.data.choosePlace;
          this.$nextTick(() => {
            window.eventBus.$on('eventBusName', function(val) { window.console.log(val); });
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
    .list-header
      height: 40px;
      line-height: 40px;
      padding: 0 18px;
      background: #f3f5f7;
      border-bottom: 1px solid rgba(7,17,27,0.1);
    .foodsList
      padding: 12px 18px;
      box-sizing: border-box;
      position: relative;
      left: 0;
      top: 0;
      bottom: 48px;
      z-index: 30;
      width: 100%;
      background: #fff;
      -webkit-transform: translate3d(0, 0, 0);
      transform: translate3d(0, 0, 0);
      :after
        display: block;
        position: absolute;
        left: 0;
        bottom: 0;
        width: 100%;
        border-top: 1px solid rgba(7,17,27,0.1);
        content: ' ';
      .name
        line-height: 24px;
        font-size: 14px;
        color: #07111b;
      .price
        position: absolute;
        right: 30px;
        bottom: 12px;
        line-height: 24px;
        font-size: 14px;
        font-weight: 700;
        color: #f01414;
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