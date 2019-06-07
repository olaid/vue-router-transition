<template>
  <div class="home page">
    <h1>vue toy</h1>
    <p>
      <button @click="doAdd">追加</button>
      <button @click="sort">整列</button>
      <button @click="randomize">シャッフル</button>
      <button @click="current=1">すべて</button>
      <button @click="current=n" v-for="n in [2,3]" :key="n">
        {{n}}の倍数
      </button>
    </p>
    <draggable v-model="filteredList">
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
//https://cr-vue.mio3io.com/examples/delay-transition.html
//https://taroken.org/vuejs-transition-router/
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
    filteredList:{
      get: function() {
        return this.list.filter(el => el % this.current === 0)
      },
      set: function (newValue) {
        this.list = newValue
      }
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
    sort(){
      this.current=1
      for(var x =0 ; x < this.list.length;x++){
        for(var y =0 ; y < this.list.length;y++){
          if(this.list[x] < this.list[y]){
            var xnum = this.list[x]
            this.list.splice(x, 1, this.list[y])
            this.list.splice(y, 1, xnum)
          }
        }
      }
    },
    randomize(){
      this.current=1
      for(var y =0 ; y < this.list.length;y++){
        const index = Math.floor(Math.random() * this.list.length)
        const indexx = Math.floor(Math.random() * this.list.length)
        var x = this.list[indexx]
        this.list.splice(indexx, 1, this.list[index])
        this.list.splice(index, 1, x)
      }
    },
    // トランジション開始でインデックス*100ms分のディレイを付与
    beforeEnter(el) {
      //倍数表示ボタンの切り替えでは要素が一気に追加されるため段階的にディレイを挿入
      //追加は遅くする必要がないためそのまま挿入
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