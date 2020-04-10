<template>
  <div class="numberPad">
    <div class="output">{{output}}</div>
    <div class="buttons">
      <button @click="inputContent">1</button>
      <button @click="inputContent">2</button>
      <button @click="inputContent">3</button>
      <button @click="remove">
        <svg class="kell">
          <use xlink:href="#kell"/>
        </svg>
      </button>
      <button @click="inputContent">4</button>
      <button @click="inputContent">5</button>
      <button @click="inputContent">6</button>
      <button @click="inputContent">+</button>
      <button @click="inputContent">7</button>
      <button @click="inputContent">8</button>
      <button @click="inputContent">9</button>
      <button @click="inputContent">-</button>
      <button @click="inputContent">.</button>
      <button @click="inputContent">0</button>
      <button @click="dengyu">=</button>
      <button @click="ok" class="over">记录</button>
    </div>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Prop} from 'vue-property-decorator';

  @Component
  export default class NumberPad extends Vue {
    @Prop(Number) readonly value!: number;
    output = this.value.toString();

    inputContent(event: MouseEvent) {
      const button = (event.target as HTMLButtonElement);
      const input = button.textContent!;
      if (this.output.length === 16) { return; }
      if (this.output === '0') {
        if ('0123456789'.indexOf(input) >= 0) {
          this.output = input;
        } else {
          this.output += input;
        }
        return;
      }
      if (this.output.indexOf('.') >= 0 && input === '.') {return;}
      this.output += input;
    }

    remove() {
      if (this.output.length === 1) {
        this.output = '0';
      } else {
        this.output = this.output.slice(0, -1);
      }
    }
    dengyu(){this.output=eval(this.output)}

    clear() {
      this.output = '0';
    }

    ok() {
      const number = parseFloat(this.output);
      this.$emit('update:value', number);
      this.$emit('submit', number);
      this.output = '0';
    }
  }
</script>

<style lang="scss" scoped>
  @import "~@/assets/style/helper.scss";
  .numberPad {
    /*.output{*/
    /*  display:flex;*/
      justify-content: space-between;
    /*}*/
    /*padding:auto;*/
    flex-direction: column;
    font-size:22px;
    .output {
      justify-content: space-between;
      @extend %clearFix;
      @extend %innerShadow;
      font-size: 22px;
      font-family: Consolas, monospace;
      padding: 12px 16px;
      text-align: right;
      height: 45px;
    }
    .buttons {
      @extend %clearFix;
      > button {
        width: 25%;
        height: 54px;
        float: left;
        background: transparent;
        border: none;
        &.ok {
          height: 64*2px;
          float: right;
        }
        &.over {
          background: #ffda47;
        }
        $bg: #f2f2f2;
        & {
          border:0.1px solid #F5F5F5;
        }
      }
    }
  }
</style>
<style lang="scss" >

  .kell{
    width: 1em; height: 1em;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;

  }
</style>
<style lang="scss" scoped>
  .notes {
    max-width: 30%;
    font-size: 14px;
    padding-left: 16px;
    align-items: center;
    display: flex;
    flex-direction: row;
    .name {
      padding-right: 15px;
      display: flex;
      flex-direction: row;
      font-size: 20px;

    }
    input {
      height: 50px;
      flex-grow: 0;
      background: transparent;
      border: none;
      padding-right: 15px;
    }
  }
</style>

<!--<style lang="scss" scoped>-->
<!--  @import "~@/assets/style/helper.scss";-->
<!--  .numberPad {-->
<!--    .output {-->
<!--      @extend %clearFix;-->
<!--      @extend %innerShadow;-->
<!--      font-size: 36px;-->
<!--      font-family: Consolas, monospace;-->
<!--      padding: 9px 16px;-->
<!--      text-align: right;-->
<!--      height: 72px;-->
<!--    }-->
<!--    .buttons {-->
<!--      @extend %clearFix;-->
<!--      > button {-->
<!--        width: 25%;-->
<!--        height: 64px;-->
<!--        float: left;-->
<!--        background: transparent;-->
<!--        border: none;-->
<!--        &.ok {-->
<!--          height: 64*2px;-->
<!--          float: right;-->
<!--        }-->
<!--        &.zero {-->
<!--          width: 25*2%;-->
<!--        }-->
<!--        $bg: #F2F2F2;-->
<!--        &:nth-child(1) {-->
<!--          background: $bg;-->
<!--        }-->
<!--        &:nth-child(2), &:nth-child(5) {-->
<!--          background: darken($bg, 4%);-->
<!--        }-->
<!--        &:nth-child(3), &:nth-child(6), &:nth-child(9) {-->
<!--          background: darken($bg, 4*2%);-->
<!--        }-->
<!--        &:nth-child(4), &:nth-child(7), &:nth-child(10) {-->
<!--          background: darken($bg, 4*3%);-->
<!--        }-->
<!--        &:nth-child(8), &:nth-child(11), &:nth-child(13) {-->
<!--          background: darken($bg, 4*4%);-->
<!--        }-->
<!--        &:nth-child(14) {-->
<!--          background: darken($bg, 4*5%);-->
<!--        }-->
<!--        &:nth-child(12) {-->
<!--          background: darken($bg, 4*6%);-->
<!--        }-->
<!--      }-->
<!--    }-->
<!--  }-->
<!--</style>-->
