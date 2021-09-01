<template>
  <div class="app-container">
    <Header></Header>
    <Goods :list='carList' @sta-change='getstate'></Goods>
    <Footer :allcheck='allcheck' :pricesum='pricesum'
    :shopsum='shopsum'  @stafull='getfull'></Footer>
  </div>
</template>

<script>
import axios from "axios"
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
export default {
  components:{
    Header,Goods,Footer
  },
  data() {
    return {
      carList:[]
    }
  },
  methods: {
    //创建axios请求 
    async initList(){
      const {data : res} = await axios.get('https://www.escook.cn/api/cart');
      if(res.status == 200){
        //获取成功后 把数据赋值给data中carlist保存
        this.carList = res.list;
        console.log(this.carList);
      }
    },
    getstate(obj){
      // console.log(obj);
      //调用自定义事件把数据改变，因为prop的值只能读写
      this.carList.some(item => {
        if(item.id == obj.id){
          item.goods_state = obj.state
        }
      })
    },
    getfull(sta){
      //利用forEach  把全选框的状态遍历给每一个
      this.carList.forEach(item => item.goods_state = sta)
    } 
  },
  computed:{
    //全选状态 利用every遍历每个状态框的勾选情况，有false就false，全true为true
    allcheck(){
       return this.carList.every(val => val.goods_state)
    },
    //总价 先用filter过滤掉state为false的商品，然后再用reduce计算合并成一个总价
    pricesum(){
       return this.carList.filter(item => item.goods_state).reduce((pro,item)=>{
         return pro += item.goods_price * item.goods_count
       },0)
    },
    //购买商品总数 原理和总价的原理一样
    shopsum(){
       return this.carList.filter(item => item.goods_state).reduce((pro,item)=>{
         return pro += item.goods_count
       },0)
    }
  },
  created(){
    //调用axios获取数据到数组
    this.initList();
  },
 mounted() {
    //事件总线接收数据
    this.$bus.$on('addnum',(obj)=>{
      //找到触发点击事件的事件源，并更改值
      this.carList.some(item => {
          if(item.id == obj.id){
            item.goods_count = obj.count;
            return true
          }
        })
    }),
    //同上理
    this.$bus.$on('subnum',(obj)=>{
      this.carList.some(item => {
          if(item.id == obj.id){
            item.goods_count = obj.count;
            return true
          }
        })
    })
  },
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
