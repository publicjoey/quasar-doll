<template>
  <q-card class="q-pa-md q-ma-md">
    <q-card-section>
      <div class="text-h6">紙娃娃元件</div>
      <div>這裡可以放換衣服遊戲的舞台</div>
    </q-card-section>
    <q-separator />
    <q-card-actions align="right">
      <q-btn label="隨機穿搭" color="primary" @click="randomize" />
      <q-btn label="重置" flat color="negative" @click="reset" />
    </q-card-actions>
  </q-card>
</template>

<script setup>
import { ref } from "vue";

const outfit = ref("無");

// 假裝有隨機換裝
function randomize() {
  const list = ["上衣", "褲子", "鞋子", "帽子"];
  outfit.value = list[Math.floor(Math.random() * list.length)];
}

function reset() {
  outfit.value = "無";
}
</script>
