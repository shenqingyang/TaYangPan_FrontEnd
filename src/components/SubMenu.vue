<template>
  <el-menu-item :class="'menu-item'" :index="parentIndex" ref="menuRef">
    <template class="menu-item-style" #title>
      <el-icon><div :class="['iconfont', 'icon-' + item.icon]"></div></el-icon>
      <div class="text">
        {{ item.name }}
      </div>
    </template>
  </el-menu-item>
  <el-popover
    placement="right"
    trigger="hover"
    ref="popoverRef"
    :virtual-ref="menuRef"
    virtual-triggering
  >
    <el-menu>
      <template v-for="(sub, sub_index) in item.children">
        <el-menu-item
          @click="jump(sub)"
          class="menu-item-sub"
          :index="parentIndex + '-' + sub_index"
        >
          <span
            :class="['iconfont', 'icon-' + sub.icon]"
            v-if="sub.icon"
          ></span>
          <span class="text">{{ sub.name }}</span>
        </el-menu-item>
      </template>
    </el-menu>
  </el-popover>
</template>
<script setup>
import { ref, unref } from "vue";
import { ClickOutside as vClickOutside } from "element-plus";

const props = defineProps({
  item: {
    type: Object,
    default: () => ({}),
  },
  parentIndex: {
    type: Number,
    default: () => -1,
  },
});
const emit = defineEmits(["jump"]);

const item = props.item;
const parentIndex = props.parentIndex;

const jump = (path) => {
  emit("jump", path);
};

const menuRef = ref();
const popoverRef = ref();
const changeVisible = () => {
  unref(popoverRef).popperRef?.delayHide?.();
};
</script>
<style lang="scss" scoped>
.menu-list {
  // height: calc(100vh - 56px);
  // width: 80px;
  // box-shadow: 0 3px 10px 0 rgb(0 0 0 / 6%);
  // border-right: 1px solid #f1f2f4;

  .menu-item {
    text-align: center;
    margin: auto;
    // font-size: 14px;
    // font-weight: bold;
    padding: 8px 0px;
    cursor: pointer;
    .menu-item-style {
      margin: auto;
      display: flex;
      align-items: center;
      height: 54px;
      // width:80%;
      border-radius: 8px;
      &:hover {
        background: #ffffff;
        // box-shadow: 3px rgb(0 0 0 / 6%);
        box-shadow: 0px 3px 16px 1px rgb(0 0 0 / 8%);
      }

      .iconfont {
        margin-left: 20%;
        font-weight: normal;
        font-size: 22px;
      }
      .text {
        font-size: 16px;
        margin-left: 10px;
      }
    }
  }

  .active {
    .menu-item-style {
      // box-shadow: 0px 3px 16px 1px rgb(0 0 0 / 10%);
      background-color: #eef9fe;
      .iconfont {
        color: #06a7ff;
      }

      .text {
        color: #06a7ff;
      }
    }
  }
}
menu-sub-list {
  // width: 200px;
  // padding: 20px 10px 0px;
  position: relative;
  margin-top: 18%;
  .menu-item-sub {
    text-align: center;
    line-height: 40px;
    border-radius: 5px;
    cursor: pointer;

    &:hover {
      background: #f3f3f3;
    }

    .iconfont {
      font-size: 14px;
      margin-right: 20px;
    }

    .text {
      font-size: 13px;
    }
  }

  .active {
    background: #eef9fe;

    .iconfont {
      color: #05a1f5;
    }

    .text {
      color: #05a1f5;
    }
  }
}
</style>
