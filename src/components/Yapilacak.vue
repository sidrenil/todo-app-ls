<template>
  <div class="yapilacak" :class="{ yapildi: yapilacak.yapildi }">
    <div class="baslik">
      <h3 @click="detayGoster = !detayGoster">{{ yapilacak.baslik }}</h3>
      <div class="icon">
        <router-link
          :to="{ name: 'YapilacakGuncelle', params: { id: yapilacak.id } }"
        >
          <span class="material-icons">edit</span>
        </router-link>
        <span class="material-icons" @click="yapilacakSil">delete</span>
        <span class="material-icons" @click="toggle">done</span>
      </div>
    </div>
    <div v-if="detayGoster" class="detay">
      <p>{{ yapilacak.icerik }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["yapilacak"],
  data() {
    return {
      detayGoster: false,
    };
  },
  methods: {
    yapilacakSil() {
      this.$emit("sil", this.yapilacak.id);
    },
    toggle() {
      this.$emit("yapildi", this.yapilacak.id);
    },
  },
};
</script>

<style scoped>
.yapilacak {
  @apply m-5 bg-[#4b4b4b] px-3 py-8 rounded-xl border-2 border-red-500 text-white;
}
.yapilacak:hover {
  @apply shadow-lg;
}
h3 {
  @apply cursor-pointer;
}
.baslik {
  @apply flex justify-between content-center;
}
.material-icons {
  @apply text-xl ml-0 text-stone-500 cursor-pointer;
}
.material-icons:hover {
  @apply text-stone-500;
}
.yapilacak.yapildi {
  @apply border-4 border-solid border-lime-600;
}
.yapilacak.yapildi .tick {
  @apply text-teal-800;
}
</style>
