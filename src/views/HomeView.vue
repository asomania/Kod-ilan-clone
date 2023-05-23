<template>
  <div>
    <Navbar />
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
          class="absolute right-0 w-48 bg-white border border-gray-300 rounded-md shadow-lg hidden max-h-60 overflow-y-auto"
        >
          <a
            class="block px-4 py-2 text-gray-800 hover:bg-gray-200"
            @click="getJobsByLocation('Remote')"
            href="#"
            >Remote</a
          >
          <a
            class="block px-4 py-2 text-gray-800 hover:bg-gray-200"
            href="#"
            @click="getJobsByLocation(location.location)"
            v-for="location in locations"
            :key="location.id"
            >{{ location.location }}</a
          >
        </div>
      </div>
    </div>

    <div class="mt-8 mx-8">
      <ul class="space-y-4">
        <li
          v-for="job in jobs"
          :key="job.id"
          class="flex items-center justify-between px-4 py-2 border border-gray-300 rounded-md hover:bg-gray-100"
          @click="goDetail(job)"
        >
          <div class="flex">
            <img
              class="w-12 h-12 rounded-full"
              :src="job.company.logo"
              alt="Avatar"
            />
            <div class="ml-4">
              <h4 class="text-lg font-semibold">{{ job.position }}</h4>
              <div class="flex gap-1">
                <p class="text-gray-600">{{ job.company.name }}</p>
                <span>|</span>
                <p class="text-gray-600">{{ job.location }}</p>
              </div>
            </div>
          </div>

          <div>
            <button
              v-for="tags in job.tags"
              class="ml-auto px-4 py-2 mx-2 bg-blue-500 text-white rounded-md disabled"
            >
              {{ tags.name }}
            </button>
          </div>
        </li>
      </ul>
    </div>

    <div class="flex justify-center mt-8">
      <button
        class="px-4 py-2 bg-blue-500 text-white rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        @click="getJobsNext"
      >
        Daha Fazla Göster
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Navbar from "../components/Navbar.vue";
export default {
  name: "HomeView",
  components: {
    Navbar,
  },
  data() {
    return {
      locations: [],
      jobs: [],
      pageNumber: 1,
      pageSize: 10,
    };
  },
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
    this.getLocations();
    this.getJobs();
  },
  methods: {
    goDetail(job) {
      const jobSlug = job.slug;
      this.$router.push({ path: "/info", query: { jobSlug } });
    },

    async getLocations() {
      const response = await axios.get(
        "https://api.kodilan.com/api/posts/locations"
      );
      this.locations = response.data;
    },
    async getJobsByLocation(location) {
      if (location === "Remote") {
        this.getJobs(); // Tüm verileri al
      } else {
        const response = await axios.get(
          `https://api.kodilan.com/api/search?query=php&location=${location}`
        );
        this.jobs = response.data.data;
      }
    },

    async getJobs() {
      const response = await axios.get(
        `https://api.kodilan.com/api/posts?get=${this.pageSize}&page=1`
      );
      this.jobs = response.data.data;
    },
    async getJobsNext() {
      this.pageNumber++;
      this.pageSize = this.pageSize + 10;
      const response = await axios.get(
        `https://api.kodilan.com/api/posts?get=${this.pageSize}&page=1`
      );
      this.jobs = response.data.data;
    },
  },
};
</script>
