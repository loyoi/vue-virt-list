<template>
  <div class="main">
    <Operate
      :virtListRef="virtGridRef"
      :length="list.length"
      :visible="visible"
      @toggleShow="visible = !visible"
    ></Operate>
    <!-- gridItems -->
    <div>
      <span>gridItems:</span>
      <span
        class="demo-btn"
        style="margin-left: 10px"
        @click="changeGridItems(2)"
        >2</span
      >
      <span
        class="demo-btn"
        style="margin-left: 10px"
        @click="changeGridItems(3)"
        >3</span
      >
      <span
        class="demo-btn"
        style="margin-left: 10px"
        @click="changeGridItems(4)"
        >4</span
      >
      <span
        class="demo-btn"
        style="margin-left: 10px"
        @click="changeGridItems(5)"
        >5</span
      >
      <span
        class="demo-btn"
        style="margin-left: 10px"
        @click="changeGridItems(6)"
        >6</span
      >
    </div>
    <div>&nbsp;</div>

    <!-- demo -->
    <!-- important: must set a height for Container or VirtList -->
    <!-- important: must set itemKey and keep id is unique -->
    <div class="demo-grid" style="width: 100%; height: 500px" v-show="visible">
      <VirtGrid
        ref="virtGridRef"
        :list="list"
        :minSize="70"
        :gridItems="gridItems"
        stickyHeaderStyle="text-align: center; height: 40px; background: #42b983;"
        @toTop="toTop"
      >
        <template #default="{ itemData, index, rowIndex }">
          <Item
            :itemData="itemData"
            :index="index"
            :rowIndex="rowIndex"
            @deleteItem="deleteItem"
          />
        </template>
      </VirtGrid>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, shallowRef } from 'vue';
import type { Ref, ShallowRef } from 'vue';
import { VirtGrid } from 'vue-virt-list';
import Operate from '../components/OperateGroup.vue';
import { getAvatarList } from '../utils/common';
import Item from './Item.vue';

const virtGridRef: Ref<typeof VirtGrid | null> = ref(null);
const visible = ref(true);

const gridItems = ref(2);

const list: Ref<any[]> = ref([]);
// const list: ShallowRef<any[]> = shallowRef([]);
list.value = getAvatarList(1000);

// console.log(list.value);
function deleteItem(itemData: any) {
  // console.log(itemData);
  const targetIndex = list.value.findIndex((item) => item.id === itemData.id);
  if (targetIndex > -1) {
    list.value.splice(targetIndex, 1);
    // 如果list为shallowRef，务必调用该方法用来更新list变化
    // virtGridRef?.value?.updateList();
  }
}

function toTop(e: Event) {
  console.log('grid-toTop', e);
}

function changeGridItems(number: number) {
  gridItems.value = number;
}
</script>

<style lang="scss" scoped>
.demo-grid {
  background-color: var(--vp-sidebar-bg-color);
  border: 1px solid var(--vp-c-border);
  overflow-x: scroll;
}
</style>
