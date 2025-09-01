<template>
  <div class="paper-doll">
    <!-- 頭 -->
    <img v-if="selectedHead" :src="heads[selectedHead]" class="head" />

    <!-- 衣服 -->
    <img v-if="selectedCloth" :src="clothes[selectedCloth]" class="clothes" />

    <!-- 褲子 -->
    <img v-if="selectedPant" :src="pants[selectedPant]" class="pants" />

    <!-- 鞋子 -->
    <img v-if="selectedShoe" :src="shoes[selectedShoe]" class="shoes" />

    <!-- 按鈕區 -->
    <div class="controls">
      <div>
        <h5>頭部</h5>
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
        <h5>衣服</h5>
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
        <h5>褲子</h5>
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
        <h5>鞋子</h5>
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
import { ref } from "vue";

// 靜態 glob
// const clothesModules = import.meta.glob("@/assets/clothes/*.jpg", {
//   eager: true,
// });
// const pantsModules = import.meta.glob("@/assets/pants/*.jpg", { eager: true });
// const shoesModules = import.meta.glob("@/assets/shoes/*.jpg", { eager: true });
// const headsModules = import.meta.glob("@/assets/heads/*.jpg", { eager: true });

const clothesModules = import.meta.glob('../assets/clothes/*.jpg', { eager: true })
const pantsModules   = import.meta.glob('../assets/pants/*.jpg', { eager: true })
const shoesModules   = import.meta.glob('../assets/shoes/*.jpg', { eager: true })
const headsModules   = import.meta.glob('../assets/heads/*.jpg', { eager: true })
// 路徑要從 MyPaperDoll.vue 所在的資料夾 出發計算 ;; 那 ../assets/heads/*.jpg 就會對應到 src/assets/heads/


function loadImages(modules) {
  const images = {};
  for (const file in modules) {
    const key = file.split("/").pop().replace(".jpg", "");
    images[key] = modules[file].default;
  }
  return images;
}

const clothes = loadImages(clothesModules);
const pants = loadImages(pantsModules);
const shoes = loadImages(shoesModules);
const heads = loadImages(headsModules);

const selectedHead = ref(null);
const selectedCloth = ref(null);
const selectedPant = ref(null);
const selectedShoe = ref(null);
</script>
