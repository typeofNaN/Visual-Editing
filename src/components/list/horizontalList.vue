<template>
  <div
    :class="['comp-content', component.active ? 'active' : '']"
    :style="getStyle"
  >
    <div class="page-list">
      <div
        v-for="(item, index) in list"
        :key="index"
        class="page-list-item"
        :style="getItemStyle('horizontal-list-item')"
      >
        <div
          class="page-list-item__hd"
          :style="getItemStyle('horizontal-list-logo_')"
        >
          <div
            class="page-list-item__img"
            :style="{ backgroundImage: 'url(' + item.val + ')' }"
          ></div>
        </div>
        <div class="page-list-item__bd">
          <div
            class="page-list-item__title"
            :style="getItemStyle('horizontal-list-title_')"
          >{{ item.title }}</div>
          <p
            class="page-list-item__desc"
            :style="getItemStyle('horizontal-list-desc_')"
          >{{ item.desc }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component({
  name: 'HorizontalList'
})
export default class HorizontalList extends Vue {
  @Prop({ default: null })
  private component: any


  private list: any = this.component.action.config

  private get getStyle (): string {
    const ret: Array<string> = []
    this.component.style.forEach((item: any) => {
      const unit: string = item.unit || ''
      if (item.val) {
        if (item.attr === 'background-image') {
          ret.push(item.attr + ':url(' + item.val + ')')
        } else {
          ret.push(item.attr + ':' + item.val + unit)
        }
      }
    })
    return ret.join(';')
  }


  private getItemStyle (key: string): string {
    const ret: Array<string> = []
    this.component.others.config.forEach((item: any) => {
      const isItem: number = item.attr.indexOf(key)
      const idx: number = item.attr.indexOf('_')
      if (isItem === 0) {
        const unit: string = item.unit || ''
        item.val && ret.push(item.attr.substring(idx + 1, item.attr.length) + ':' + item.val + unit)
      }
    })
    return ret.join(';')
  }
}
</script>

<style lang="scss" scoped>
.comp-content {
  background-repeat: no-repeat;
}
.page-list {
  background-color: #fff;

  .page-list-item {
    display: flex;
    align-items: center;
    position: relative;

    &:after {
      position: absolute;
      left: 0;
      bottom: 0;
      right: 0;
      content: " ";
      height: 1px;
      background-color: #E5E5E5;
      -webkit-transform-origin: 0 0;
      transform-origin: 0 0;
      -webkit-transform: scaleY(0.5);
      transform: scaleY(0.5);
    }

    &:last-child {
      &:after {
        display: none;
      }
    }

    .page-list-item__hd {
      margin-right: .8em;
      text-align: center;

      .page-list-item__img {
        margin: 0 auto;
        height: 100%;
        background-color: #e9e9eb;
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
      }
    }

    .page-list-item__bd {
      flex: 1;
      min-width: 60px;

      .page-list-item__title {
        margin-bottom: .3em;
        width: auto;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        word-wrap: normal;
        word-wrap: break-word;
        word-break: break-all;
      }

      .page-list-item__desc {
        margin: 0;
        line-height: 1.5;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
      }
    }
  }
}
</style>
