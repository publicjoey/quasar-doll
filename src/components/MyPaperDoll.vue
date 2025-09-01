<template>
  <div class="paper-doll relative">
    <!-- 底圖 (娃娃本體) -->
    <img src="/src/assets/base-doll.jpg" class="base-doll" />

    <!-- 底圖 (娃娃本體) -->
    <!-- <img v-if="selectedBase" :src="bases[selectedBase]" class="base-doll" /> -->

    <!-- 頭 -->
    <img
      v-if="selectedHead"
      :src="heads[selectedHead]"
      class="layer head"
      :style="getHeadStyle(headPosition)"
      @mousedown="startDrag($event, 'head')"
    />

    <!-- 衣服 -->
    <img
      v-if="selectedCloth"
      :src="clothes[selectedCloth]"
      class="layer clothes"
      :style="getStyle(clothPosition)"
      @mousedown="startDrag($event, 'cloth')"
    />

    <!-- 褲子 -->
    <img
      v-if="selectedPant"
      :src="pants[selectedPant]"
      class="layer pants"
      :style="getStyle(pantPosition)"
      @mousedown="startDrag($event, 'pant')"
    />

    <!-- 鞋子 -->
    <img
      v-if="selectedShoe"
      :src="shoes[selectedShoe]"
      class="layer shoes"
      :style="getStyle(shoePosition)"
      @mousedown="startDrag($event, 'shoe')"
    />

    <!-- 控制按鈕 -->
    <div class="controls q-mt-lg">
      <!-- 底圖選擇 -->
      <div>
        <h6>角色底圖</h6>
        <q-btn
          v-for="(src, key) in bases"
          :key="key"
          flat
          @click="selectedBase = key"
        >
          <img :src="src" width="50" />
        </q-btn>
      </div>

      <!-- 頭部選擇 -->
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

      <!-- 衣服選擇 -->
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

      <!-- 褲子選擇 -->
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

      <!-- 鞋子選擇 -->
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

      <!-- 頭部大小調整滑桿 -->
      <div class="q-mt-md">
        <h6>調整頭部大小</h6>
        <q-slider
          v-model="headSize.value"
          :min="60"
          :max="200"
          :step="1"
          label
          color="primary"
          class="q-mx-md"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

// 載入 assets 中的圖檔
// const baseModules = import.meta.glob("../assets/base-dolls/*.jpg", { eager: true });

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

const clothes = loadImages(clothesModules);
const pants = loadImages(pantsModules);
const shoes = loadImages(shoesModules);
const heads = loadImages(headsModules);


// joey -------------
// const selectedBase = ref(Object.keys(bases)[0] || null);
// joey -------------


const selectedHead = ref(null);
const selectedCloth = ref(null);
const selectedPant = ref(null);
const selectedShoe = ref(null);

// 各部位位置（預設座標）joey
const headPosition = ref({ x: 90, y: 0 }); // 頭部預設稍微偏右
const clothPosition = ref({ x: 0, y: 120 });
const pantPosition = ref({ x: 0, y: 300 });
const shoePosition = ref({ x: 0, y: 555 });

// 預設頭部大小 (單位：px)
const headSize = ref({ value: 120 });

// 拖曳狀態
const dragging = ref(null); // 正在拖曳的部位
const offset = ref({ x: 0, y: 0 });

function getStyle(pos) {
  return {
    transform: `translate(${pos.x}px, ${pos.y}px)`,
    cursor: "move",
  };
}

// 專門給 head 用的 style（含大小）
function getHeadStyle(pos) {
  return {
    width: headSize.value.value + "px",
    height: headSize.value.value + "px",
    transform: `translate(${pos.x}px, ${pos.y}px)`,
    cursor: "move",
  };
}

function startDrag(event, part) {
  dragging.value = part;
  offset.value = {
    x: event.clientX - getPosition(part).x,
    y: event.clientY - getPosition(part).y,
  };
  window.addEventListener("mousemove", onDrag);
  window.addEventListener("mouseup", stopDrag);
}

function onDrag(event) {
  if (!dragging.value) return;
  const pos = getPosition(dragging.value);
  pos.x = event.clientX - offset.value.x;
  pos.y = event.clientY - offset.value.y;
}

function stopDrag() {
  dragging.value = null;
  window.removeEventListener("mousemove", onDrag);
  window.removeEventListener("mouseup", stopDrag);
}

function getPosition(part) {
  if (part === "head") return headPosition.value;
  if (part === "cloth") return clothPosition.value;
  if (part === "pant") return pantPosition.value;
  if (part === "shoe") return shoePosition.value;
}
</script>

<style scoped>
.paper-doll {
  width: 666px;
  margin: auto;
  position: relative;
}
.base-doll,
.layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 222px; /* 統一底圖尺寸 */
  user-select: none;
}
.head {
  z-index: 10; /* 頭部在最上層 */
}
.controls {
  position: relative;
  margin-top: 11px; /* 推到娃娃下方 */
  text-align: center;
}
</style>
