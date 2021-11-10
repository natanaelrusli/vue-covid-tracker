<template>
    <select @change="onChange" class="form-select mt-10 block w-full border p-3 rounded" v-model="selected">
        <option value="0">Select Country</option>
        <option v-for="country in countries" v-bind:key="country.ID" :value="country.ID">
            {{ country.Country }}
        </option>
    </select>
</template>

<script>
export default {
    nam: "CountrySelectComponent",
    props: ["countries"],
    data() {
        return {
            selected: 0
        }
    },
    methods: {
        onChange() {
            const country = this.countries.find((item) => item.ID === this.selected)

            this.$emit("get-country", country)
        }
    }
}
</script>