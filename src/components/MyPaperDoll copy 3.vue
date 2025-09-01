<template>
  <q-page class="flex flex-center">
    <div class="paper-doll">
      <!-- 底圖 (娃娃) -->
      <!-- <img src="@/assets/base-doll.jpg" class="base" /> -->
      <!-- <img :src="baseDoll" class="base" /> -->
      <img src="/base-doll.jpg" class="base" />
      <img v-if="selectedCloth" :src="`/clothes/${selectedCloth}.jpg`" class="clothes" />


      <!-- 衣服 (可切換) -->
      <!-- <img
        v-if="selectedCloth"
        :src="`/src/assets/clothes/${selectedCloth}.jpg`"
        class="clothes"
      /> -->

      <!-- 選擇衣服按鈕 -->
      <div class="controls">
        <q-btn
          v-for="cloth in clothes"
          :key="cloth"
          color="primary"
          :label="cloth"
          @click="selectedCloth = cloth"
          class="q-mx-sm"
        />
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
// ---------
// 直接 import 圖片
// import baseDoll from '@/assets/base-doll.jpg'
// ------------
const clothes = ["dress1", "dress2", "dress3"]; // 對應 JPG 檔名
const selectedCloth = ref(null);
</script>

<style scoped>
.paper-doll {
  position: relative;
  width: 300px;
  height: 400px;
}
.base {
  width: 100%;
  height: 100%;
}
.clothes {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.controls {
  margin-top: 16px;
  text-align: center;
}
</style>
