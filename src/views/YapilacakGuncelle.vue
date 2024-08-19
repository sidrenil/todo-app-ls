<template>
  <h1>Yapilacak Guncelle {{ route.params.id }}</h1>
  <form @submit.prevent="handleSubmit">
    <label>Başlık:</label>
    <input type="text" v-model="baslik" />

    <label>İçerik:</label>
    <input type="text" v-model="icerik" />

    <button>Güncelle</button>
  </form>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();
const baslik = ref("");
const icerik = ref("");
const id = route.params.id;

onMounted(() => {
  let yapilacaklar = JSON.parse(localStorage.getItem("yapilacaklar")) || [];
  const yap = yapilacaklar.find((item) => item.id === id);
  if (yap) {
    baslik.value = yap.baslik;
    icerik.value = yap.icerik;
  }
});

const handleSubmit = () => {
  let yapilacaklar = JSON.parse(localStorage.getItem("yapilacaklar")) || [];
  const index = yapilacaklar.findIndex((item) => item.id === id);
  if (index !== -1) {
    yapilacaklar[index] = {
      ...yapilacaklar[index],
      baslik: baslik.value,
      icerik: icerik.value,
    };
    localStorage.setItem("yapilacaklar", JSON.stringify(yapilacaklar));
    router.push("/");
  }
};
</script>

<style></style>
