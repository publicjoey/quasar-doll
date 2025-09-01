<template>
  <div class="paper-doll relative">
    <!-- 底圖 (娃娃本體) -->
    <!-- <img src="@/assets/base-doll.jpg" class="base-doll" /> -->
    <img src="/src/assets/base-doll.jpg" class="base-doll" />

    <!-- 頭 -->
    <img
      v-if="selectedHead"
      :src="heads[selectedHead]"
      class="layer head"
    />

    <!-- 衣服 -->
    <img
      v-if="selectedCloth"
      :src="clothes[selectedCloth]"
      class="layer clothes"
    />

    <!-- 褲子 -->
    <img
      v-if="selectedPant"
      :src="pants[selectedPant]"
      class="layer pants"
    />

    <!-- 鞋子 -->
    <img
      v-if="selectedShoe"
      :src="shoes[selectedShoe]"
      class="layer shoes"
    />

    <!-- 控制按鈕 -->
    <div class="controls q-mt-lg">
      <div>
        <h6>頭部</h6>
        <q-btn
          v-for="(src, key) in heads"
          :key="key"
          flat
          @click="selectedHead = key"
        >
          <img :src="src" width="50" />
        </q-btn>
      </div>
      <div>
        <h6>衣服</h6>
        <q-btn
          v-for="(src, key) in clothes"
          :key="key"
          flat
          @click="selectedCloth = key"
        >
          <img :src="src" width="50" />
        </q-btn>
      </div>
      <div>
        <h6>褲子</h6>
        <q-btn
          v-for="(src, key) in pants"
          :key="key"
          flat
          @click="selectedPant = key"
        >
          <img :src="src" width="50" />
        </q-btn>
      </div>
      <div>
        <h6>鞋子</h6>
        <q-btn
          v-for="(src, key) in shoes"
          :key="key"
          flat
          @click="selectedShoe = key"
        >
          <img :src="src" width="50" />
        </q-btn>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 載入 assets 資料夾中的圖檔
// const clothesModules = import.meta.glob('@/assets/clothes/*.jpg', { eager: true })
// const pantsModules   = import.meta.glob('@/assets/pants/*.jpg', { eager: true })
// const shoesModules   = import.meta.glob('@/assets/shoes/*.jpg', { eager: true })
// const headsModules   = import.meta.glob('@/assets/heads/*.jpg', { eager: true })


const clothesModules = import.meta.glob("../assets/clothes/*.jpg", {
  eager: true,
});
const pantsModules = import.meta.glob("../assets/pants/*.jpg", { eager: true });
const shoesModules = import.meta.glob("../assets/shoes/*.jpg", { eager: true });
const headsModules = import.meta.glob("../assets/heads/*.jpg", { eager: true });

function loadImages(modules) {
  const images = {}
  for (const file in modules) {
    const key = file.split('/').pop().replace('.jpg', '')
    images[key] = modules[file].default
  }
  return images
}

const clothes = loadImages(clothesModules)
const pants   = loadImages(pantsModules)
const shoes   = loadImages(shoesModules)
const heads   = loadImages(headsModules)

const selectedHead  = ref(null)
const selectedCloth = ref(null)
const selectedPant  = ref(null)
const selectedShoe  = ref(null)
</script>

<style scoped>
.paper-doll {
  width: 300px;
  margin: auto;
}
.base-doll,
.layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 300px; /* 統一尺寸 */
}
.controls {
  position: relative;
  margin-top: 320px; /* 推到娃娃下方 */
  text-align: center;
}
</style>
