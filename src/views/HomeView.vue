<template>
  <div>
    <nav class="bg-gray-800">
      <div class="container mx-auto px-8">
        <div class="flex items-center justify-between h-16">
          <div class="flex items-center">
            <a class="text-white text-lg font-semibold" href="#">{Kod,ilan}</a>
          </div>
          <div class="flex items-center">
            <a class="text-gray-300 hover:text-white" href="#">Ilanlar</a>
            <a class="ml-4 text-gray-300 hover:text-white" href="#">Github</a>
          </div>
        </div>
      </div>
    </nav>
    <div class="flex justify-between mx-8 mt-8">
      <input
        class="px-4 py-2 border border-gray-700 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        type="text"
        placeholder="Search"
      />
      <div class="relative">
        <button class="text-gray-300 hover:text-black focus:outline-none">
          Pick City
        </button>
        <div
          class="absolute right-0 w-48 bg-white border border-gray-300 rounded-md shadow-lg hidden"
        >
          <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200" href="#"
            >Dropdown Link 1</a
          >
          <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200" href="#"
            >Dropdown Link 2</a
          >
          <a class="block px-4 py-2 text-gray-800 hover:bg-gray-200" href="#"
            >Dropdown Link 3</a
          >
        </div>
      </div>
    </div>

    <div class="mt-8 mx-8">
      <ul class="space-y-4">
        <li class="flex items-center" @click="goDetail">
          <img
            class="w-12 h-12 rounded-full"
            src="../../public/meta-icon.png"
            alt="Avatar"
          />
          <div class="ml-4">
            <h4 class="text-lg font-semibold">Başlık</h4>
            <div class="flex gap-1">
              <p class="text-gray-600">Firma Adı</p>
              <span>|</span>
              <p class="text-gray-600">Şehir</p>
            </div>
          </div>
          <button class="ml-auto px-4 py-2 bg-blue-500 text-white rounded-md">
            Kaydet
          </button>
        </li>
        <!-- Diğer liste öğelerini buraya ekleyebilirsiniz -->
      </ul>
    </div>

    <div class="flex justify-center mt-8">
      <button
        class="px-4 py-2 bg-blue-500 text-white rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
      >
        Daha Fazla Göster
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeView",
  components: {},
  mounted() {
    const dropdownButton = document.querySelector(".relative button");
    const dropdownMenu = document.querySelector(".relative .absolute");

    dropdownButton.addEventListener("click", () => {
      dropdownMenu.classList.toggle("hidden");
    });

    window.addEventListener("click", (event) => {
      if (!dropdownButton.contains(event.target)) {
        dropdownMenu.classList.add("hidden");
      }
    });
  },
  methods: {
    goDetail() {
      this.$router.push("/info");
    },
  },
};
</script>
