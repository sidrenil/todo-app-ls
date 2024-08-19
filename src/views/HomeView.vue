<template>
  <div class="home">
    <NavbarFilter @filterDurum="aktifSekme = $event" :aktifSekme="aktifSekme" />
    <div v-if="yapilacaklar.length">
      <div v-for="yap in filtrelenmisYapilacaklar" :key="yap.id">
        <Yapilacak :yapilacak="yap" @sil="silHandle" @yapildi="yapildiHandle" />
      </div>
    </div>
    <div v-else>
      <p class="mt-5">Yapilacaklar yok</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Yapilacak from "../components/Yapilacak.vue";
import NavbarFilter from "../components/NavbarFilter.vue";

const yapilacaklar = ref([]);
const aktifSekme = ref("hepsi");

const fetchData = () => {
  yapilacaklar.value = JSON.parse(localStorage.getItem("yapilacaklar")) || [];
};

const silHandle = (id) => {
  let yapilacaklar = JSON.parse(localStorage.getItem("yapilacaklar")) || [];
  yapilacaklar = yapilacaklar.filter((yap) => yap.id !== id);
  localStorage.setItem("yapilacaklar", JSON.stringify(yapilacaklar));
  fetchData();
};

const yapildiHandle = (id) => {
  let yapilacaklar = JSON.parse(localStorage.getItem("yapilacaklar")) || [];
  const yap = yapilacaklar.find((yapilacak) => yapilacak.id === id);
  if (yap) {
    yap.yapildi = !yap.yapildi;
    localStorage.setItem("yapilacaklar", JSON.stringify(yapilacaklar));
    fetchData();
  }
};

const filtrelenmisYapilacaklar = computed(() => {
  if (aktifSekme.value === "tamamlandi") {
    return yapilacaklar.value.filter((yapilacak) => yapilacak.yapildi);
  }
  if (aktifSekme.value === "yapiliyor") {
    return yapilacaklar.value.filter((yapilacak) => !yapilacak.yapildi);
  }
  return yapilacaklar.value;
});

fetchData();
</script>
