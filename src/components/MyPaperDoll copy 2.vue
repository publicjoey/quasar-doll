<template>
  <q-card class="q-pa-md q-ma-md" flat bordered>
    <q-card-section>
      <div class="text-h6">紙娃娃換衣服遊戲</div>
      <div class="text-subtitle2 text-grey">點擊服裝即可穿上，再次點擊卸下</div>
    </q-card-section>

    <q-separator />

    <!-- 舞台 -->
    <q-card-section>
      <div class="row justify-center">
        <svg id="paper-stage" :width="300" :height="500" viewBox="0 0 300 500">
          <!-- 基本人型 -->
          <defs>
            <g id="doll">
              <circle cx="150" cy="70" r="38" fill="#ffd9b3" />
              <rect
                x="130"
                y="108"
                width="40"
                height="70"
                rx="18"
                fill="#ffd9b3"
              />
              <rect
                x="118"
                y="170"
                width="64"
                height="118"
                rx="22"
                fill="#ffd9b3"
              />
              <!-- 手臂 -->
              <rect
                x="94"
                y="175"
                width="22"
                height="96"
                rx="10"
                fill="#ffd9b3"
              />
              <rect
                x="184"
                y="175"
                width="22"
                height="96"
                rx="10"
                fill="#ffd9b3"
              />
              <!-- 腿 -->
              <rect
                x="132"
                y="290"
                width="20"
                height="110"
                rx="10"
                fill="#ffd9b3"
              />
              <rect
                x="168"
                y="290"
                width="20"
                height="110"
                rx="10"
                fill="#ffd9b3"
              />
              <!-- 臉 -->
              <circle cx="138" cy="70" r="4" fill="#333" />
              <circle cx="162" cy="70" r="4" fill="#333" />
              <path
                d="M138,84 Q150,92 162,84"
                stroke="#e17055"
                stroke-width="3"
                fill="none"
              />
            </g>
          </defs>

          <use href="#doll" />

          <!-- 動態穿著層 -->
          <g v-html="activeLayers.hair?.svg"></g>
          <g v-html="activeLayers.top?.svg"></g>
          <g v-html="activeLayers.bottom?.svg"></g>
          <g v-html="activeLayers.shoes?.svg"></g>
        </svg>
      </div>
    </q-card-section>

    <q-separator />

    <!-- 控制區 -->
    <q-card-section>
      <div class="row q-col-gutter-sm">
        <div class="col-3" v-for="item in items" :key="item.id">
          <q-btn
            class="full-width"
            :color="isEquipped(item) ? 'primary' : 'grey-4'"
            :text-color="isEquipped(item) ? 'white' : 'black'"
            @click="toggleEquip(item)"
          >
            {{ item.name }}
          </q-btn>
        </div>
      </div>
    </q-card-section>

    <q-separator />

    <q-card-actions align="right">
      <q-btn flat label="重置" color="negative" @click="resetAll" />
      <q-btn unelevated label="隨機穿搭" color="primary" @click="randomize" />
    </q-card-actions>
  </q-card>
</template>

<script setup>
import { reactive } from "vue";

// 簡單的服裝項目
const items = [
  {
    id: "hair-long",
    cat: "hair",
    name: "長髮",
    svg: '<path d="M120,46 q30,-18 60,0 v10 q18,14 18,44 v66 q0,18 -16,28 l-10,6 v-60 q0,-12 -12,-18 l-16,-6 -16,6 q-12,6 -12,18 v60 l-10,-6 q-16,-10 -16,-28 v-66 q0,-30 18,-44 z" fill="#3c3c3c"/>',
  },
  {
    id: "top-shirt",
    cat: "top",
    name: "T恤",
    svg: '<rect x="118" y="170" width="64" height="60" rx="12" fill="#74b9ff"/>',
  },
  {
    id: "bottom-skirt",
    cat: "bottom",
    name: "裙子",
    svg: '<path d="M118,230 l32,90 l32,-90 z" fill="#fab1a0"/>',
  },
  {
    id: "shoes-red",
    cat: "shoes",
    name: "紅鞋",
    svg: '<rect x="128" y="400" width="24" height="20" rx="4" fill="#d63031"/><rect x="168" y="400" width="24" height="20" rx="4" fill="#d63031"/>',
  },
];

// 當前穿著
const activeLayers = reactive({
  hair: null,
  top: null,
  bottom: null,
  shoes: null,
});

function toggleEquip(item) {
  if (activeLayers[item.cat]?.id === item.id) {
    activeLayers[item.cat] = null; // 卸下
  } else {
    activeLayers[item.cat] = item;
  }
}

function isEquipped(item) {
  return activeLayers[item.cat]?.id === item.id;
}

function resetAll() {
  activeLayers.hair = null;
  activeLayers.top = null;
  activeLayers.bottom = null;
  activeLayers.shoes = null;
}

function randomize() {
  resetAll();
  const cats = ["hair", "top", "bottom", "shoes"];
  cats.forEach((cat) => {
    const pool = items.filter((i) => i.cat === cat);
    if (pool.length) {
      activeLayers[cat] = pool[Math.floor(Math.random() * pool.length)];
    }
  });
}
</script>
