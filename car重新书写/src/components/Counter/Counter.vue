<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click='sub'>-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{num}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click='add'>+</button>
  </div>
</template>

<script>
export default {
  props:{
    num:{
      default:0,
      type:Number
    },
    id:{
      default:0,
      type:Number
    }
  },
  methods: {
   add(){
      const obj = {id:this.id ,count:this.num+1}
      this.$bus.$emit('addnum',obj)
      //通过事件总线把数据传送出去
    },
    sub(){
      //防止商品数量少于0
      if(this.num - 1 < 0){
        return
      }
      const obj = {id:this.id,count:this.num-1};
      this.$bus.$emit('subnum',obj)
      //同add一样
    },
    }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
