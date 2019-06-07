<template>
  <div class="about page">
    <h1 v-if="is_started">click in order of number</h1>
    <h1 v-else>{{message}}</h1>
    <p>
      <input type=number v-model="number"></input>
      <button @click="start">スタート</button>
    </p>
    <transition-group tag="ul" class="list"
      @before-enter="beforeEnter"
      @after-enter="afterEnter"
      @enter-cancelled="afterEnter">
      <li v-for="(item, index) in filteredList"
        :data-index="index"
        :key="item"
        class="item"
        :style="{color: font_color}"
        @click="doRemove(item)">{{ item }}</li>
    </transition-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      is_started: false,
      number: 10,
      list: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      memory: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      font_color: "black",
      message: "memory game"
    }
  },
  computed: {
    filteredList() {
      return this.list
    }
  },
  methods: {
    start() {
      this.fontcolorChange("black")
      this.is_started = false
      this.message = "remember the number"
      var white = function(){
        this.list = []
      } 
      // 追加ならフラグを立てる
      this.list = []
      for( x = 1; x <= this.number; x++ ){
        this.list.splice(x-1, 0, x)
      }
      for(var y =0 ; y < this.list.length;y++){
        const index = Math.floor(Math.random() * this.list.length)
        const indexx = Math.floor(Math.random() * this.list.length)
        var x = this.list[indexx]
        this.list.splice(indexx, 1, this.list[index])
        this.list.splice(index, 1, x)
      }
      setTimeout(this.fontcolorChange,3000,"transparent")
      setTimeout(this.started,5000)
      
    },
    fontcolorChange(color) {
      this.font_color= color
    },
    started(){
      this.is_started = true
    },
    doRemove(item) {
      if(!this.is_started){
        return 0
      }
      if(item == Math.min.apply(null, this.list)){
        this.list.splice(this.list.indexOf(item), 1)
      }
      else {
        this.message = "miss!restart!"
        this.is_started = false
        this.font_color= "black"
        setTimeout(this.start,3000)
      }
      if(!this.list.length){
        this.is_started = false
        this.message = "congratulations!!!"
      }
    },
    // トランジション開始でインデックス*100ms分のディレイを付与
    beforeEnter(el) {
      this.font_color= "black"
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
<style src="./style.css" scoped>
</style>