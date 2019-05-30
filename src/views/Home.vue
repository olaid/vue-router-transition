<template>
  <div class="home page">
    <h1>This is an home page</h1>
    <p>
      <button @click="doAdd">追加</button>
      <button @click="randomize">シャッフル</button>
      <button @click="current=1">すべて</button>
      <button @click="current=n" v-for="n in [3,5,6]" :key="n">
        {{n}}の倍数
      </button>
    </p>
    <draggable>
    <transition-group tag="ul" class="list"
      @before-enter="beforeEnter"
      @after-enter="afterEnter"
      @enter-cancelled="afterEnter">å
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
      this.current=1
      // 追加ならフラグを立てる
      this.addEnter = true
      const newNumber = Math.max.apply(null, this.list) + 1
      const index = (this.list.length + 1)
      this.list.splice(index, 0, newNumber)
    },
    doRemove(item) {
      this.list.splice(this.list.indexOf(item), 1)
    },
    randomize(){
      this.current=1
      for(var y =0 ; y < this.list.length;y++){
        var x = this.list[0]
        const index = Math.floor(Math.random() * this.list.length)
        this.list.splice(0, 1, this.list[index])
        this.list.splice(index, 1, x)
      }
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