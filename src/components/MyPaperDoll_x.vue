<template>
  <div class="paper-doll">
    <!-- 娃娃區域 -->
    <div class="doll-container">
      <!-- 固定大小的 base-doll -->
      <img src="@/assets/base-doll.jpg" class="base" />

      <!-- 各部位 -->
      <img v-if="selectedHead" :src="heads[selectedHead]" class="part" />
      <img v-if="selectedCloth" :src="clothes[selectedCloth]" class="part" />
      <img v-if="selectedPant" :src="pants[selectedPant]" class="part" />
      <img v-if="selectedShoe" :src="shoes[selectedShoe]" class="part" />
    </div>

    <!-- 選擇按鈕區 -->
    <div class="controls">
      <div class="control-group">
        <div class="label">頭部</div>
        <q-btn
          v-for="(src, key) in heads"
          :key="key"
          flat
          @click="selectedHead = key"
        >
          <img :src="src" class="preview" />
        </q-btn>
      </div>

      <div class="control-group">
        <div class="label">衣服</div>
        <q-btn
          v-for="(src, key) in clothes"
          :key="key"
          flat
          @click="selectedCloth = key"
        >
          <img :src="src" class="preview" />
        </q-btn>
      </div>

      <div class="control-group">
        <div class="label">褲子</div>
        <q-btn
          v-for="(src, key) in pants"
          :key="key"
          flat
          @click="selectedPant = key"
        >
          <img :src="src" class="preview" />
        </q-btn>
      </div>

      <div class="control-group">
        <div class="label">鞋子</div>
        <q-btn
          v-for="(src, key) in shoes"
          :key="key"
          flat
          @click="selectedShoe = key"
        >
          <img :src="src" class="preview" />
        </q-btn>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

// 自動載入 assets 圖片
const heads = import.meta.glob("@/assets/heads/*.jpg", {
  eager: true,
  import: "default",
});
const clothes = import.meta.glob("@/assets/clothes/*.jpg", {
  eager: true,
  import: "default",
});
const pants = import.meta.glob("@/assets/pants/*.jpg", {
  eager: true,
  import: "default",
});
const shoes = import.meta.glob("@/assets/shoes/*.jpg", {
  eager: true,
  import: "default",
});

// 選中的部位
const selectedHead = ref(null);
const selectedCloth = ref(null);
const selectedPant = ref(null);
const selectedShoe = ref(null);
</script>

<style scoped>
.paper-doll {
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* 娃娃容器固定大小 */
.doll-container {
  position: relative;
  width: auto;
  height: 300px; /* 固定高度 */
}

/* base 固定大小 */
.doll-container .base {
  height: 100%;
  width: auto;
  display: block;
}

/* 疊加的配件，保持原圖比例 */
.doll-container .part {
  position: absolute;
  top: 0;
  left: 0;
  max-width: 100%;
  max-height: 100%;
}

/* 控制按鈕區 */
.controls {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.control-group {
  display: flex;
  align-items: center;
  gap: 8px;
}

.label {
  font-weight: bold;
  min-width: 50px;
}

.preview {
  max-width: 50px;
  max-height: 50px;
}
</style>
