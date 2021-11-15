<template>
  <div class="plateNumer">
    <!-- 车牌号码输入框 -->
    <div class="carNum__area">
      <!-- 车牌号前七位 -->
      <ul class="carNumber-items">
        <li
          v-for="(item, index) in carnum.length - 1"
          :key="index"
          class="carNumber-item"
          :class="{ 'active-carNumber': activeList[index] }"
          @click="openKeyboard(index)"
        >
          {{ carnum[index] }}
        </li>

        <span class="point">·</span>
        <!-- 新能源 -->
        <div @click="openKeyboard(7)">
          <li
            v-if="!showNewPower && carnum[7] === ''"
            class="carNumber-item flex flex-column"
            :class="{ 'carNumber-item-newpower': !showNewPower }"
          >
            <span class="carNumber-newpower-add">+</span>
            <div>新能源</div>
          </li>
          <li
            v-else
            class="carNumber-item"
            :class="{ 'active-carNumber': activeList[7] }"
          >
            {{ carnum[7] }}
          </li>
        </div>
      </ul>
    </div>

    <!-- 虚拟键盘 -->
    <div
      class="keyboard1"
      :class="{ keyboard: pageState === 0, keyboard1: pageState === 1 }"
      v-show="KeyboardState"
    >
      <div class="keyboardClose">
        <div class="keyboardClose_btn" @click="closeKeyboard">关闭</div>
        <div
          class="keyboardClose_btn"
          @click="clearKeyboard"
          v-show="activeList[0]"
        >
          清空
        </div>
      </div>
      <!-- 省份简写键盘 -->
      <div class="keyboard-item" v-show="activeList[0]">
        <ul
          class="keyboard-line"
          v-for="(itemList, index) in provinces"
          :key="index"
        >
          <li
            v-for="item in itemList"
            :key="item"
            class="keyboard-btn1"
            @click="bindChoose(item)"
          >
            {{ item }}
          </li>
        </ul>
      </div>
      <!-- 车牌后几位选择键盘 -->
      <div class="keyboard-item iscarnumber" v-show="!activeList[0]">
        <ul
          class="keyboard-line"
          v-for="(itemList, index) in numbers"
          :key="index"
        >
          <li
            v-for="item in itemList"
            :key="item"
            class="keyboard-btn"
            :class="{ 'delete-btn': item === 'delete' }"
            @click="bindChoose(item)"
          >
            {{ item === "delete" ? "" : item
            }}<img
              class="delete-icon"
              src="@/assets/icon/delete.png"
              v-if="item === 'delete'"
            />
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PlateNumer",
  props: {
    carnum: {
      type: Array,
      default: () => [],
    },
    pageState: {
      type: Number,
    },
  },
  data() {
    return {
      // 省份简写
      provinces: [
        ["京", "沪", "浙", "苏", "粤", "鲁", "晋", "冀", "豫", "川"],
        ["渝", "辽", "吉", "黑", "皖", "鄂", "津", "贵", "云"],
        ["桂", "琼", "青", "新", "藏", "蒙", "宁"],
        ["甘", "陕", "闽", "赣", "湘", "无"],
      ],
      // 车牌输入
      numbers: [
        ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
        ["Q", "W", "E", "R", "T", "Y", "U", "P", "港", "澳"],
        ["A", "S", "D", "F", "G", "H", "J", "K", "L", "学"],
        ["Z", "X", "C", "V", "B", "N", "M", "领", "delete"],
      ],
      activeList: [false, false, false, false, false, false, false, false],
      showNewPower: false,
      KeyboardState: false,
      selectIndex: 0,
    };
  },
  methods: {
    // 打开键盘
    openKeyboard(i) {
      this.selectIndex = i;
      this.KeyboardState = true;
      this.activeList = [...new Array(8).fill(false)]
      this.activeList[i] = true;
      if (i === 7) {
        this.showNewPower = true;
      }
      console.log(this.activeList);
    },
    // 清空车牌
    clearKeyboard() {
      this.showNewPower = false;
      this.activeList = [true, ...new Array(7).fill(false)];
      this.$emit("clearPlateNum")
    },
    // 关闭
    closeKeyboard() {
      this.KeyboardState = false;
      this.showNewPower = false;
      this.activeList = [
        false,
        false,
        false,
        false,
        false,
        false,
        false,
        false,
      ];
    },
    // 自定义键盘的点击事件
    bindChoose(e) {
      if (this.selectIndex === 1) {
        let numArr = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];
        if (this.activeList[1] && numArr.includes(e)) {
          alert("车牌第二位不能输入数字");
          return;
        }
      }

      if (e === "delete") {
        this.bindDelChoose();
      } else {
        this.activeList.map((e) => {
          console.log(e);
          e = false;
        });

        let carnum = this.carnum;
        let activeList = this.activeList.map((item) => {
          if (item) item = false;
          return item;
        });

        carnum[this.selectIndex] = e;
        activeList[this.selectIndex + 1] = true;
        if (carnum.length <= 8 && activeList[8] === undefined) {
          this.selectIndex++;
          this.activeList = [...activeList];
        } else {
          this.KeyboardState = false;
          this.showNewPower = false;
          this.activeList = activeList.map((item) => {
            if (item) item = false;
            return item;
          });
        }
        if (this.selectIndex === 7) {
          this.showNewPower = true;
        }
        this.$emit("getCarNum", carnum);
      }
    },
    // 自定义键盘的删除事件
    bindDelChoose() {
      this.activeList = [...new Array(8).fill(false)]
      let carnum = this.carnum;
      let activeList = this.activeList;
      carnum[this.selectIndex] = "";
      activeList[this.selectIndex - 1] = true;
      this.selectIndex--;
      this.activeList = activeList;
      this.$emit("getCarNum", carnum);
    },
  },
};
</script>

<style scoped lang="less">
@blue: #1890ff;

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 5px;
}

/* 虚拟键盘 */
.keyboard {
  height: auto;
  background: #d1d5d9;
  position: fixed;
  bottom: 0;
  width: 100%;
  left: 0;
  z-index: 999;
}
.keyboard1 {
  height: auto;
  background: #d1d5d9;
  position: fixed;
  bottom: 15px;
  width: 100%;
  left: 0;
  z-index: 999;
}

.keyboard-item {
  padding: 5px 0 2.5px 0;
  position: relative;
  display: block;
}

/* 关闭虚拟键盘 */
.keyboardClose {
  height: 35px;
  background-color: #f7f7f7;
  overflow: hidden;
  display: flex;
  flex-direction: row-reverse;
}

.keyboardClose_btn {
  line-height: 35px;
  font-size: 15px;
  padding: 0 15px;
}

/* 虚拟键盘-省缩写 */

/* 虚拟键盘-行 */
.keyboard-line {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  text-align: center;
}

.iscarnumber .keyboard-line {
  text-align: left;
  margin-left: 2.5px;
}

/* 虚拟键盘-单个按钮 */
.keyboard-btn {
  font-size: 20px;
  /* font-weight: 700; */
  color: #333333;
  background: #fff;
  display: inline-block;
  padding: 6px 0;
  width: 30px;
  text-align: center;
  box-shadow: 0 1px 0 0 #999999;
  border-radius: 2px;
  margin: 2.5px;
}
.keyboard-btn1 {
  font-size: 22px;
  /* font-weight: 700; */
  color: #333333;
  background: #fff;
  display: inline-block;
  padding: 6px 3px;
  width: 30px;
  text-align: center;
  box-shadow: 0 1px 0 0 #999999;
  border-radius: 5px;
  margin: 2.5px;
}
/* 虚拟键盘-删除按钮 */
.delete-btn {
  color: #333333;
  background: #fff;
  display: inline-block;
  width: 67px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 1px 0 0 #999999;
  border-radius: 5px;
  margin: 2.5px 3px;
  padding: 2px 0px 1px;
}
/* 点击的效果 */
.keyboard-bover {
  background: @blue;
}
.delete-icon {
  width: 30px;
  height: 30px;
}

.keyboard-del-font {
  font-size: 25px;
}

/* 车牌号码 */
.carNumber-items {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  .point {
    color: #333333;
    font-size: 40px;
    padding-bottom: 4px;
    position: absolute;
    left: 106px;
    top: 0;
  }
  .carNumber-item {
    width: 36px;
    color: #666666;
    height: 45px;
    font-size: 21px;
    text-align: center;
    border: 1px solid #cccccc;
    border-radius: 6px;
    line-height: 45px;
    display: inline-block;
    margin: 0 3px;
    vertical-align: middle;
    &:nth-child(2) {
      margin-right: 30px;
    }
  }
  .active-carNumber {
    width: 36px;
    color: #666666;
    height: 45px;
    font-size: 21px;
    text-align: center;
    border-radius: 6px;
    line-height: 45px;
    display: inline-block;
    margin: 0 3px;
    vertical-align: middle;
    border: 1px solid @blue;
  }

  /* 新能源 */
  .carNumber-item-newpower {
    border: 1px dashed #a8bff3;
    background-color: #f6f9ff;
    color: #a8bff3;
    font-size: 10px;
    line-height: 23px;
    color: #00cc99;
  }

  .carNumber-newpower-add {
    font-size: 18px;
  }
}
</style>
