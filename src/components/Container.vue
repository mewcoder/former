<template>
  <el-container class="fm-container">
    <!-- 拖拽区 -->
    <el-aside width="300px" class="fm-left">
      <draggable
        class="list-group"
        tag="ul"
        v-model="componentList"
        v-bind="{
          animation: 200,
          group: { name: 'people', pull: 'clone', put: false },
          disabled: false,
          ghostClass: 'ghost',
        }"
      >
        <li
          class="list-group-item"
          v-for="element in componentList"
          :key="element.type"
        >
          {{ element.name }}
        </li>
      </draggable></el-aside
    >
    <!-- 编辑区 -->
    <el-main class="fm-main">
      <el-form>
        <draggable
          class="list-group"
          tag="ul"
          v-model="widgetList"
          v-bind="{
            animation: 200,
            group: 'people',
            disabled: false,
            ghostClass: 'ghost',
          }"
          @add="onAdd"
        >
          <el-form-item
            v-for="element in widgetList"
            :key="element.key"
            :label="element.name"
            class="list-form-item"
          >
            <template v-if="element.type == 'input'">
              <el-input
                :style="{ width: element.options.width }"
                :placeholder="element.options.placeholder"
                v-model="element.options.defaultValue"
              ></el-input>
            </template>
          </el-form-item>
        </draggable>
      </el-form>
    </el-main>
    <!-- 配置区 -->
    <el-aside width="300px" class="fm-right">配置区</el-aside>
  </el-container>
</template>
<script>
import Draggable from "vuedraggable";

export default {
  components: {
    Draggable,
  },
  data() {
    return {
      componentList: [
        {
          type: "input",
          name: "单行文本",
          options: {
            width: "100%",
            defaultValue: "",
            placeholder: "",
          },
        },
      ],
      widgetList: [],
    };
  },
  methods: {
    onAdd(evt) {
      // evt： 事件对象，具体可以参考上个实验《2.4 拖拽事件》中有详细描述
      const newIndex = evt.newIndex;
      const key = new Date().getTime();
      this.$set(this.widgetList, newIndex, {
        ...this.widgetList[newIndex],
        options: {
          ...this.widgetList[newIndex].options,
        },
        key,
      });
    },
  },
};
</script>

<style lang="scss">
html,
body,
#app {
  height: 100%;
  margin: 0;
  padding: 0;
}
ul,
li {
  margin: 0;
  padding: 0;
  list-style: none;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
  list-style: none;
}
.list-group-item {
  cursor: move;
  height: 30px;
  line-height: 30px;
  border: 1px solid #ccc;
}
.list-form-item {
  border: 1px dashed blue;
  padding: 5px;

  &.el-form-item {
    margin: 2px;
  }
}
.fm-container {
  height: 100%;
  border: 1px solid #ccc;
  text-align: center;

  .fm-main {
    padding: 0;
  }
  .fm-left {
    border-right: 1px solid #ccc;
  }
  .fm-right {
    border-left: 1px solid #ccc;
  }
}
</style>
