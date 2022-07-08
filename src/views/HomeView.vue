<template>
  <div class="container">
    <div class="foodie-wrapper">
      <div class="left-box">
        <img :src='imgUrl' alt="" />
        <div class="random-machine" :style="{'--activity-foods-height': foodListHeight}">
          <div class="random-result" v-show="!isShowFoodList && disabled">{{ food }}</div>
          <ul ref="foodList" v-show="isShowFoodList">
            <li v-for="(food,index) in foods" :key="index">
              <span class="food">{{ food }}</span>
            </li>
          </ul>
        </div>
      </div>
      <div class="right-box">
        <div class="winner-area">
          <h3>摇摇机动态</h3>
          <div class="lottery-content" :style="{'--activity-lottery-height': lotteryListHeight, '--activity-lottery-time': lotteryListAnimationTimeText}">
            <ul ref="lotteryList">
              <li v-for="(item, index) in scrollNames" :key="index">
                <span>{{ item.name }} 今天吃 </span><span class="food">{{ `#${item.food}#` }}</span>
              </li>
            </ul>
          </div>
        </div>
        <el-button size="medium" @click="beginShake" class="start" :disabled="disabled">{{countDownText}}</el-button>
      </div>
    </div>
  </div>
</template>
<script>
import initStatus from '@/imgs/lottery/init_status.png'
import lottering from '@/imgs/lottery/lottering.png'

export default {
  name: "lottery",
  data() {
    return {
      imgUrl: initStatus,
      isShowFoodList: false,
      isChangeFood: true,
      disabled: false,
      foodListHeight: '',
      lotteryListHeight: '-135px',
      lotteryListAnimationTime: 4,
      lotteryListAnimationTimeText: '4s',
      food: '',
      countDownText: '摇一摇',
      scrollNames: [
        { name: '吴多萍', food: '烧烤' },
        { name: '卢佳佳', food: '外婆家的菜外婆家的菜外婆家的菜' },
        { name: '吴多萍', food: '嘉陵食堂' },
        { name: '卢佳佳', food: '烧烤' },
        { name: '吴多萍', food: '烧烤' },
      ],
      foods: [
        '兰州拉面',
        '海口麻辣大拌',
        '北京烤鸭',
        '重庆酸辣粉',
        '武汉热干面',
        '广东肠粉',
        '广西桂林米粉',
        '西安羊肉泡馍',
        '宁夏手抓羊肉'
      ],
    };
  },
  methods: {
    beginShake() {
      this.imgUrl = lottering
      this.isShowFoodList = true;
      this.countDown()

      // 获取食品列表所在盒子高度
      this.$nextTick(() => {
        this.foodListHeight = '-' + this.$refs.foodList.offsetHeight + 'px'
      })
      this.disabled = true;

      // 5秒后随机产生一种食物
      setTimeout(() => {
        this.food = this.foods[this.getRandomIntInclusive(0,3)]
        this.scrollNames.push({name: 'Cyan', food: this.food})
        // 刷新获奖名单所在盒子高度
        this.lotteryListHeight = '-' + this.$refs.lotteryList.offsetHeight + 'px'
        this.lotteryListAnimationTimeText = ++this.lotteryListAnimationTime +'s'
        this.isShowFoodList = false
      }, 5000)

      setTimeout(() => {
        this.imgUrl = initStatus
        this.disabled = false
      }, 7000)
    },

    countDown() {
      let countDownNumber = 5
      this.countDownText = countDownNumber
      const countDownInterval = setInterval(() => {
        this.countDownText = countDownNumber === 0 ? '摇一摇' : --countDownNumber
        if (this.countDownText === '摇一摇') clearInterval(countDownInterval)
      }, 1000)
    },

    getRandomIntInclusive(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1) + min); //The maximum is inclusive and the minimum is inclusive
    }
  },
};
</script>
<style lang="scss" scoped>
.foodie-wrapper {
  display: flex;
  width: 500px;
  padding: 20px;
  padding-bottom: 10px;
  background-color: #ffcd00;
  border-radius: 10px;
  .left-box {
    position: relative;
    .random-machine {
      width: 120px;
      height: 75px;
      line-height: 75px;
      overflow: hidden;
      position: absolute;
      left: 50%;
      top: 50px;
      transform: translateX(-50%);
      @keyframes activity-foods-slider {
        0% {
        }
        100% {
          transform: translateY(var(--activity-foods-height));
        }
      }
      .random-result, ul {
        text-align: center;
        font-size: 18px;
        color: #fff;
      }
      ul {
        animation: 1s activity-foods-slider linear infinite;
      }
    }
  }
  .right-box {
    margin-left: 20px;
    .winner-area {
      padding: 10px 20px 10px 10px;
      border-radius: 8px;
      background-color: #ffdc49;
      h3 {
        font-size: 18px;
        margin-bottom: 10px;
      }
      .lottery-content {
        height: 135px;
        overflow: hidden;
        position: relative;
        @keyframes activity-lottery-slider {
          0% {
          }
          100% {
            transform: translateY(var(--activity-lottery-height));
          }
        }
      }
      ul {
        margin-left: 15px;
        animation: var(--activity-lottery-time) activity-lottery-slider linear infinite;
        li {
          margin: 10px 0;
          font-size: 14px;
          .food {
            margin-left: 6px;
            color: #1e25ac;
          }
          &::before {
            content: "";
            display: inline-block;
            width: 4px;
            height: 4px;
            vertical-align: middle;
            border-radius: 50%;
            margin-right: 5px;
            background-color: #000;
          }
          &:last-child {
            margin-bottom: 0;
          }
        }
      }
    }
    .el-button {
      border: 1px solid transparent;
      padding: 0;
    }
    ::v-deep .el-button--medium {
      padding: 0;
      font-size: 16px;
    }
    .start {
      display: block;
      margin: 10px auto 0;
      width: 120px;
      background-color: #f560a5;
      color: #fff;
      font-weight: bold;
      line-height: 36px;
      border-radius: 20px;
      text-align: center;
      cursor: pointer;
    }
  }
}
</style>
