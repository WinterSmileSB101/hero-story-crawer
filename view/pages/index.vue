<template>
  <section class="container">
    <h2>英雄列表</h2>
    <div class="nav">
      <span v-for="(item, index) in hero.nav"
            :class="index === tab ? 'active' : ''"
            :key="index" @click="toFilter(item.type, index)">{{item.text}}</span>
    </div>
    <div class="imagebox">
      <img v-for="(item, index) in hero.heroList" :key="index" 
           v-lazyload="item.heroimg" 
           :alt="item.title" 
           @click="toDetail(index)">
    </div>
  </section>
</template>

<script>
import {mapState} from 'vuex'
import index from 'vue';
export default {
  head () {
    return {
      title: `首页-英雄列表`
    }
  },
  data () {
    return{
      hero: {},
      allHero: [],
      tab: 0
    }
  },
  methods: {
    toDetail(index) {
      this.$router.push(`/herodetail?index=${index}`)
    },
    toFilter(type, index) {
      if (type === 'all') {
        this.hero.heroList = this.allHero;
      } else {
        this.hero.heroList = this.allHero.filter(item => item.camptype == type)
      }
      this.tab = index
    }
  },
  beforeCreate () {
    this.$store.dispatch('getHero').then(data=>{
      this.hero = data;
      this.allHero = data.heroList
    })
  }
}
</script>

<style scoped lang="less">
.title{
  margin: 50px 0;
}
.nav{
  padding: 10px 0;
  span{
    display: inline-block;
    padding: 3px 12px;
    border: 1px solid #0698e0;
    border-radius: 30px;
    margin: 10px;
    cursor: pointer;
  }

  .active{
    background: #0698e0;
    color: #ffffff;
  }
}
img{
  cursor: pointer;
}
</style>
