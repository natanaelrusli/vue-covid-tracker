<template>
  <main v-if="!loading">
    <DataHeader :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>
    <CountrySelectComponent @get-country="getCountryData" :countries="countries" />
    <button @click="clearCountryData" v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">Clear Country</button>
  </main>
  <main class="flex flex-column align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <div>
      <img :src="loadingImage" alt="Loading" class="w-24 m-auto">
    </div>
  </main>
</template>

<script>
// @ is an alias to /src
  import DataHeader from "@/components/DataHeader"
  import DataBoxes from "@/components/DataBoxes"
  import CountrySelectComponent from "@/components/CountrySelectComponent"

  export default {
    name: 'Home',
    components: {
      DataHeader,
      DataBoxes,
      CountrySelectComponent,
    },
    data() {
      return {
        loading: true,
        title: "Global",
        dataDate: "",
        stats: {},
        countries: [],
        loadingImage: require("../assets/hourglass.gif"),
      }
    },
    methods: {
      async fetchCovidData() {
        const res = await fetch("https://api.covid19api.com/summary")
        const data = await res.json()
        return data
      },
      getCountryData(country) {
        this.stats = country
        this.title = country.Country
      },
      async clearCountryData() {
        this.loading = true
        const data = await this.fetchCovidData()
        this.title = "Global"
        this.stats = data.Global
        this.loading = false
      }
    },
    async created() {
      const data = await this.fetchCovidData()
      
      this.dataDate = data.Date
      this.stats = data.Global
      this.countries = data.Countries
      this.loading = false
      console.log(data)
    }
  }
</script>
