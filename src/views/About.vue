<template>
  <div class="about page">
    <h1>This is an about page</h1>
    <p>
      <button @click="doAdd">追加</button>
      <button @click="current=1">すべて</button>
      <button @click="current=n" v-for="n in [3,5,6]" :key="n">
        {{n}}の倍数
      </button>
    </p>
    <draggable>
    <transition-group tag="ul" class="list"
      @before-enter="beforeEnter"
      @after-enter="afterEnter"
      @enter-cancelled="afterEnter">
      <li v-for="(item, index) in filteredList"
        :data-index="index"
        :key="item"
        class="item"
        @click="doRemove(item)">{{ item }}</li>
    </transition-group>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  components: {
    draggable,
  },
  data() {
    return {
      addEnter: false,
      current: 1,
      list: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    }
  },
  computed: {
    filteredList() {
      return this.list.filter(el => el % this.current === 0)
    }
  },
  methods: {
    doAdd() {
      // 追加ならフラグを立てる
      this.addEnter = true
      const newNumber = Math.max.apply(null, this.list) + 1
      const index = (this.list.length + 1)
      this.list.splice(index, 0, newNumber)
    },
    doRemove(item) {
      this.list.splice(this.list.indexOf(item), 1)
    },
    // トランジション開始でインデックス*100ms分のディレイを付与
    beforeEnter(el) {
      this.$nextTick(() => {
        if (!this.addEnter) {
          // 追加でなければディレイを付与
          el.style.transitionDelay = 100 * parseInt(el.dataset.index, 10) + 'ms'
        } else {
          // 追加ならフラグを消すだけ
          this.addEnter = false
        }
      })
    },
    // トランジション完了またはキャンセルでディレイを削除
    afterEnter(el) {
      el.style.transitionDelay = ''
    }
  }
}
</script>

<style src="./style.css" scoped></style>