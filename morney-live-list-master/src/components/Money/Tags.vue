<template>
  <div class="tags">
    <div class="new">
      <button @click="createTag">新增标签</button>
    </div>
    <ul class="current">
      <li v-for="tag in tagList" :key="tag.id"
          :class="{selected: selectedTags.indexOf(tag)>=0}"
          @click="toggle(tag)">{{tag.name}}
      </li>
    </ul>
  </div>

</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Prop} from 'vue-property-decorator';
  import {mixins} from 'vue-class-component';
  import TagHelper from '@/mixins/TagHelper';

  @Component
  export default class Tags extends mixins(TagHelper) {
    selectedTags: string[] = [];

    get tagList() {
      return this.$store.state.tagList;
    }

    created() {
      this.$store.commit('fetchTags');
    }

    toggle(tag: string) {
      const index = this.selectedTags.indexOf(tag);
      if (index >= 0) {
        this.selectedTags.splice(index, 1);
      } else {
        this.selectedTags.push(tag);
      }
      this.$emit('update:value', this.selectedTags);
    }
  }
</script>

<style lang="scss" scoped>
  .tags {
    background: white;
    font-size: 12px;
    padding: 10px 10px 0px 15px;
    flex-grow: 1;
    display: flex;
    flex-direction: column-reverse;
    > .current {
      display: flex;
      overflow:auto;
      flex-wrap: wrap;
      > li {
        /*width: 20%;*/
        $bg: #dadede;
        background: $bg;
        $h: 22px;
        height: $h;
        line-height: $h;
        border-radius: $h/2;
        margin-right: 13px;
        padding: 0 20px;
        margin-top: 10px;
        margin-bottom: 20px;
        &.selected {
          background: darken($bg, 50%);
          color: white;
        }
      }
    }
    > .new {
      padding-top: 16px;
      button {
        background: transparent;
        border: none;
        color: #999;
        border-bottom: 1px solid;
        padding: 0 4px;
      }
    }
  }

</style>
