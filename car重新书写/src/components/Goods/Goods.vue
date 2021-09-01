<template>
  <div>
  <!-- v-for 把获得数据用v-for 遍历 -->
  <div class="goods-container" v-for='items in list' :key="items.id">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <!-- 注意id和for 要用:绑定 并把item.id赋值给他 才能有不同的类名 -->
        <input type="checkbox" class="custom-control-input" :id="'cb'+items.id" 
        :checked="items.goods_state" :data-id='items.id' @change='stachange' />
        <label class="custom-control-label" :for="'cb'+items.id">
          <!-- 商品的缩略图 -->
          <img :src="items.goods_img" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{items.goods_name}}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">{{items.goods_price}}</span>
        <!-- 商品的数量 -->
        <Counter :id='items.id' :num='items.goods_count'></Counter>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import Counter from '@/components/Counter/Counter.vue'
export default {
  props:{
    list:{
      default:[],
      type:Array
    }
  },
  components:{
    Counter
  },
  methods: {
    stachange(e){
      //获取目标的勾选状态
      const sta = e.target.checked;
      // 获取目标的id
      const id = e.target.getAttribute('data-id')
      // 自定义事件，把数据传送给父组件
      this.$emit('sta-change',{id:id,state:sta})
    }
  },
}
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
