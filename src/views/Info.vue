<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="flex mx-4 grid grid-cols-3 mt-8">
        <div class="col-span-2 flex">
          <img
            class="w-12 h-12 rounded-full"
            :src="jobsCompany.logo"
            alt="Avatar"
          />
          <div class="ml-4">
            <h4 class="text-lg font-semibold">{{ jobs.position }}</h4>
            <div class="ml-4">
              <div class="flex gap-1">
                <p class="text-gray-600">{{ jobsCompany.name }}</p>
                <span>|</span>
                <p class="text-gray-600">{{ jobs.location }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="flex mt-8 mx-4">
          <div class="flex-1">
            <h4 class="text-lg font-semibold">İş Tanımı</h4>
            <p class="text-gray-600" v-html="jobs.description"></p>
          </div>
          <div class="flex-1">
            <div class="flex justify-center">
              <div>
                <button
                  v-for="tags in jobs.tags"
                  class="ml-auto px-4 py-2 mx-2 bg-blue-500 text-white rounded-md disabled"
                >
                  {{ tags.name }}
                </button>
              </div>
            </div>
            <div class="flex justify-center">
              <button class="px-4 py-2 m-8 bg-lime-500 text-white rounded-md">
                Basvur
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Navbar from "../components/Navbar.vue";
import axios from "axios";

export default {
  name: "info",
  components: { Navbar },
  data() {
    return {
      jobs: {},
      jobsCompany: {},
    };
  },
  async created() {
    const jobSlug = this.$route.query.jobSlug;
    async function getJob() {
      const response = await axios.get(
        `https://api.kodilan.com/api/posts/${jobSlug}`
      );

      return response.data;
    }

    await getJob().then((data) => {
      this.jobs = data;
      console.log(this.jobs);
      this.jobsCompany = this.jobs.company;
    });
  },
  methods: {},
};
</script>
