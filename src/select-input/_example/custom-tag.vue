<template>
  <t-space direction="vertical" class="tdesign-demo-select-input-custom-tag">
    <!-- 单选，使用 valueDisplay 插槽定义选中的某一项的内容，也可使用同名渲染函数 props.valueDisplay -->
    <t-select-input
      :popup-props="{ overlayInnerStyle: { padding: '6px' } }"
      :value="selectValue1"
      placeholder="Please Select"
      clearable
      @clear="onClear"
    >
      <template #valueDisplay>
        <span v-if="selectValue1" class="displaySpan">
          <control-platform-icon class="tdesign-demo-select-input__img" />
          {{ selectValue1.label }}
        </span>
      </template>
      <template #panel>
        <ul class="tdesign-demo__select-input-ul-single">
          <li v-for="item in options" :key="item.value" @click="() => onOptionClick(item)">
            {{ item.label }}
          </li>
        </ul>
      </template>
    </t-select-input>

    <!-- 多选，第一种方式：使用 tag 插槽定义选中的某一项的内容，也可使用同名渲染函数 props.tag -->
    <t-select-input :value="selectValue2" clearable placeholder="Please Select" multiple @tag-change="onTagChange2">
      <template #tag="{ value }">
        <span class="displaySpan">
          <control-platform-icon />
          {{ value }}
        </span>
      </template>
      <template #panel>
        <div class="tdesign-demo__select-empty-custom">暂无示意数据</div>
      </template>
    </t-select-input>

    <!-- 多选，第二种方式：使用 valueDisplay 插槽定义全部选中项的内容，也可使用同名渲染函数 props.valueDisplay -->
    <t-select-input :value="selectValue3" placeholder="Please Select" multiple @tag-change="onTagChange3">
      <template #valueDisplay="{ value, onClose }">
        <!-- <span><LayersIcon />{{ value }}</span> -->
        <t-tag
          v-for="(item, index) in value"
          :key="item"
          closable
          style="margin-right: 4px"
          @close="() => onClose(index)"
        >
          <span class="displaySpan">
            <control-platform-icon />
            <span>{{ item }}</span>
          </span>
        </t-tag>
      </template>
      <template #panel>
        <div class="tdesign-demo__select-empty-custom">暂无示意数据</div>
      </template>
    </t-select-input>
  </t-space>
</template>
<script>
import { ControlPlatformIcon } from 'tdesign-icons-vue';

const options = [
  { label: 'tdesign-vue', value: 1 },
  { label: 'tdesign-react', value: 2 },
  { label: 'tdesign-miniprogram', value: 3 },
  { label: 'tdesign-angular', value: 4 },
  { label: 'tdesign-mobile-vue', value: 5 },
  { label: 'tdesign-mobile-react', value: 6 },
];
export default {
  components: { ControlPlatformIcon },
  data() {
    return {
      options,
      selectValue1: { label: 'tdesign-vue', value: 1 },
      selectValue2: ['tdesign-vue', 'tdesign-react'],
      selectValue3: ['tdesign-vue', 'tdesign-react', 'tdesign-mobile-vue'],
    };
  },
  methods: {
    onOptionClick(item) {
      this.selectValue1 = item;
    },
    onClear() {
      this.selectValue1 = undefined;
    },
    onTagChange2(val) {
      this.selectValue2 = val;
    },
    onTagChange3(val) {
      this.selectValue3 = val;
    },
  },
};
</script>
<style lang="less" scoped>
.tdesign-demo__select-input-ul-single {
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.tdesign-demo__select-input-ul-single > li {
  display: block;
  border-radius: 3px;
  line-height: 22px;
  cursor: pointer;
  padding: 3px 8px;
  color: var(--td-text-color-primary);
  transition: background-color 0.2s linear;
  white-space: nowrap;
  word-wrap: normal;
  overflow: hidden;
  text-overflow: ellipsis;
}

.tdesign-demo__select-input-ul-single > li:hover {
  background-color: var(--td-bg-color-container-hover);
}

.tdesign-demo-select-input-custom-tag .tdesign-demo-select-input__img {
  font-size: 16px;
  margin-right: 4px;
}

.tdesign-demo__select-empty-custom {
  text-align: center;
  color: var(--td-text-color-disabled);
  line-height: 32px;
}
.displaySpan {
  line-height: 24px;
}
</style>
