<template>
  <div class="paper-doll relative">
    <!-- 底圖 -->
    <img :src="bases[selectedBase]" class="base-doll" />

    <!-- 頭 -->
    <img
      v-if="selectedHead"
      :src="heads[selectedHead]"
      class="layer head"
      :style="getHeadStyle()"
    />

    <!-- 衣服 joey -->
    <!-- <img
      v-if="selectedCloth"
      :src="clothes[selectedCloth]"
      class="layer clothes"
      :style="getStyle(clothPosition)"
    /> -->
    <img v-if="selectedCloth" :src="clothes[selectedCloth]" class="layer clothes"
    :style="getStyle(clothPosition)"
    />

    <!-- 褲子 -->
    <img
      v-if="selectedPant"
      :src="pants[selectedPant]"
      class="layer pants"
      :style="getStyle(pantPosition)"
    />

    <!-- 鞋子 -->
    <img
      v-if="selectedShoe"
      :src="shoes[selectedShoe]"
      class="layer shoes"
      :style="getStyle(shoePosition)"
    />

    <!-- 控制面板 -->
    <div class="controls q-mt-lg">
      <!-- 更換底圖 -->
      <div>
        <h6>底圖</h6>
        <q-btn
          v-for="(src, key) in bases"
          :key="key"
          flat
          @click="selectedBase = key"
        >
          <img :src="src" width="50" />
        </q-btn>
      </div>

      <!-- 頭部 -->
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

      <!-- 衣服 -->
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

      <!-- 褲子 -->
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

      <!-- 鞋子 -->
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

      <!-- 調整頭部大小 -->
      <div class="q-mt-md">
        <h6>調整頭部大小</h6>
        <q-slider v-model="headSize.width" :min="50" :max="200" label="寬度" />
        <q-slider v-model="headSize.height" :min="50" :max="200" label="高度" />
      </div>

      <!-- 調整頭部左右位置 -->
      <div class="q-mt-md">
        <h6>調整頭部位置</h6>
        <q-slider
          v-model="headPosition.x"
          :min="-100"
          :max="100"
          label="左右"
        />
        <q-slider
          v-model="headPosition.y"
          :min="-100"
          :max="100"
          label="上下"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

// 載入圖片
const basesModules = import.meta.glob("../assets/bases/*.jpg", { eager: true });
const clothesModules = import.meta.glob("../assets/clothes/*.jpg", {  eager: true,});
const pantsModules = import.meta.glob("../assets/pants/*.jpg", { eager: true });
const shoesModules = import.meta.glob("../assets/shoes/*.jpg", { eager: true });
const headsModules = import.meta.glob("../assets/heads/*.jpg", { eager: true });

function loadImages(modules) {
  const images = {};
  for (const file in modules) {
    const key = file.split("/").pop().replace(".jpg", "");
    images[key] = modules[file].default;
  }
  return images;
}

const bases = loadImages(basesModules);
const clothes = loadImages(clothesModules);
const pants = loadImages(pantsModules);
const shoes = loadImages(shoesModules);
const heads = loadImages(headsModules);

const selectedBase = ref(Object.keys(bases)[0]);
const selectedHead = ref(null);
// const selectedHead = ref(Object.keys(bases)[0]);
const selectedCloth = ref(null);
const selectedPant = ref(null);
const selectedShoe = ref(null);

// 頭部位置與大小
const headPosition = ref({ x: 0, y: 0 });
const headSize = ref({ width: 120, height: 120 });

// 其他部位位置
const clothPosition = ref({ x: 0, y: 222 });
const pantPosition = ref({ x: 0, y: 200 });
const shoePosition = ref({ x: 0, y: 280 });

// 取得樣式
function getStyle(pos) {
  return {
    transform: `translate(${pos.x}px, ${pos.y}px)`,
  };
}

function getHeadStyle() {
  return {
    width: headSize.value.width + "px",
    height: headSize.value.height + "px",
    transform: `translate(${headPosition.value.x}px, ${headPosition.value.y}px)`,
  };
}
</script>

<style scoped>
.paper-doll {
  width: 800px;
  margin: auto;
  position: relative;
}
.base-doll,
.layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 300px;
  user-select: none;
}
.head {
  z-index: 10;
}
.controls {
  position: relative;
  margin-top: 340px;
  text-align: center;
}
</style>
