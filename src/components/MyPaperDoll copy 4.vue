<template>
  <q-page class="flex flex-center">
    <div class="paper-doll">
      <!-- 底圖 (身體) -->
      <img src="/base-doll.jpg" class="base" />

      <!-- 頭部 -->
      <img
        v-if="selectedHead"
        :src="`/head/${selectedHead}.jpg`"
        class="layer"
      />

      <!-- 上衣 -->
      <img
        v-if="selectedCloth"
        :src="`/clothes/${selectedCloth}.jpg`"
        class="layer"
      />

      <!-- 褲子 -->
      <img
        v-if="selectedPants"
        :src="`/pants/${selectedPants}.jpg`"
        class="layer"
      />

      <!-- 鞋子 -->
      <img
        v-if="selectedShoes"
        :src="`/shoes/${selectedShoes}.jpg`"
        class="layer"
      />

      <!-- 控制區 -->
      <div class="controls">
        <div class="q-mb-md">
          <span>頭部：</span>
          <q-btn
            v-for="h in heads"
            :key="h"
            :label="h"
            color="primary"
            flat
            @click="selectedHead = h"
          />
        </div>

        <div class="q-mb-md">
          <span>上衣：</span>
          <q-btn
            v-for="c in clothes"
            :key="c"
            :label="c"
            color="secondary"
            flat
            @click="selectedCloth = c"
          />
        </div>

        <div class="q-mb-md">
          <span>褲子：</span>
          <q-btn
            v-for="p in pants"
            :key="p"
            :label="p"
            color="accent"
            flat
            @click="selectedPants = p"
          />
        </div>

        <div>
          <span>鞋子：</span>
          <q-btn
            v-for="s in shoes"
            :key="s"
            :label="s"
            color="positive"
            flat
            @click="selectedShoes = s"
          />
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";

// 各部分的選項 (對應 JPG 檔名，不含副檔名)
const heads = ["head1", "head2"];
const clothes = ["cloth1", "cloth2"];
const pants = ["pants1", "pants2"];
const shoes = ["shoes1", "shoes2"];

// 狀態
const selectedHead = ref(null);
const selectedCloth = ref(null);
const selectedPants = ref(null);
const selectedShoes = ref(null);
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

.layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.controls {
  margin-top: 20px;
  text-align: center;
}
</style>
