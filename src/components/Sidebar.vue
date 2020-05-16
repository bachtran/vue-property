<template>
    <div id="sidebar">
        <b-form-input v-model="title" placeholder="Title"></b-form-input>
        <b-form-select v-model="type" :options="typeChoices"></b-form-select>
        <b-form-input v-model="address" placeholder="Address"></b-form-input>
        <b-form-select v-model="suburb" :options="suburbChoices"></b-form-select>
        <b-form-select v-model="state" :options="stateChoices"></b-form-select>
        <b-form-select v-model="stage" :options="stageChoices"></b-form-select>
        <b-form-select v-model="category" :options="categoryChoices"></b-form-select>
        <b-form-select v-model="subCategory" :options="subCategoryChoices"></b-form-select>
        <b-form-select v-model="status" :options="statusChoices"></b-form-select>
        <b-form-select v-model="council" :options="councilChoices"></b-form-select>
        <b-form-select v-model="devType" :options="devTypeChoices"></b-form-select>
        <b-form-select v-model="ownership" :options="ownershipChoices"></b-form-select>
        <label for="floorArea">Floor Area</label>
        <vue-slider v-model="floorArea" :min="floorAreaRange.min" :max="floorAreaRange.max" name="floorArea"></vue-slider>
        <label for="siteArea">Site Area</label>
        <vue-slider v-model="siteArea" :min="siteAreaRange.min" :max="siteAreaRange.max" name="siteArea"></vue-slider>
        <label for="storeys">Storeys</label>
        <vue-slider v-model="storeys" :min="storeysRange.min" :max="storeysRange.max" name="storeys"></vue-slider>
        <label for="units">Units</label>
        <vue-slider v-model="units" :min="unitsRange.min" :max="unitsRange.max" name="units"></vue-slider>
        <label for="value">Value</label>
        <vue-slider v-model="value" :min="valueRange.min" :max="valueRange.max" name="value"></vue-slider>
        
        <b-form-input v-model="description" placeholder="Description"></b-form-input>
        <b-form-input v-model="notes" placeholder="Notes"></b-form-input>
        <label for="lat">Latitude</label>
        <vue-slider v-model="lat" :min="latRange.min" :max="latRange.max" name="lat"></vue-slider>
        <label for="long">Longitude</label>
        <vue-slider v-model="long" :min="longRange.min" :max="longRange.max" name="long"></vue-slider>
        <b-button v-on:click.prevent="filter">Filter</b-button>
    </div>
</template>

<script>
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/default.css'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
export default {
    name: 'Sidebar',
    components: {
        VueSlider
    },
    props: {
        filterData: {
            type: Array,
            required: true,
        },
    },
    data: function () {
        return {
            name: "sidebar",
            
            title: "", // text 
            type: null, // dropdown
            typeChoices: [],
            address: "", // text
            suburbChoices: [], // dropdown
            suburb: null,
            state: null, // dropdown
            stateChoices: [],
            stage: null, // dropdown
            stageChoices: [],
            category: null, // dropdown
            categoryChoices: [],
            subCategory: null, // dropdown
            subCategoryChoices: [],
            status: null, // dropdown
            statusChoices: [],
            council: null, // dropdown
            councilChoices: [],
            devType: null, // dropdown
            devTypeChoices: [],
            floorArea: "", // range slider
            floorAreaRange: {}, 
            siteArea: "", // range slider
            siteAreaRange: {},
            storeys: "", // range slider
            storeysRange: {},
            units: "", // range slider
            unitsRange: {},
            commenceDate: "", // date picker 
            commenceDataRange: {},
            completionDate: "", // date picker 
            completionDateRange: {},
            lastUpdate: "", // date picker
            lastUpdateRange: {},
            value: [],
            valueRange: {},
            ownership: null, // dropdown
            ownershipChoices: [],
            description: "", // text
            notes: "", // text
            additionalDetails: "", // text
            lat: "", // range slider
            latRange: {},
            long: "", // range slider
            longRange: {},
        }
    },
    created: function () {
        this.typeChoices = this.getChoices(this.filterData, 'Type');

        this.suburbChoices = this.getChoices(this.filterData, 'Suburb');

        this.stateChoices = this.getChoices(this.filterData, 'State');

        this.stageChoices = this.getChoices(this.filterData, 'Stage');

        this.categoryChoices = this.getChoices(this.filterData, 'Category');

        this.subCategoryChoices = this.getChoices(this.filterData, 'SubCategory');

        this.statusChoices = this.getChoices(this.filterData, 'Status');

        this.councilChoices = this.getChoices(this.filterData, 'Council');

        this.devTypeChoices = this.getChoices(this.filterData, 'Dev. Type');

        this.ownershipChoices = this.getChoices(this.filterData, 'Ownership');

        this.valueRange = this.getRange(this.filterData, 'Value');
        this.value = this.getRangeArray(this.valueRange);

        this.floorAreaRange = this.getRange(this.filterData, 'Floor Area');
        this.floorArea = this.getRangeArray(this.floorAreaRange);
    },
    methods: {
        getChoices: function (filterData, key) {
            let choices = Array.from(new Set(filterData.map(x => x.properties.project[key])));
            choices.unshift({ value: null, text: "Please choose a " + key});
            return choices;
        },
        getRange: function (filterData, key) {
            return {
                min: Math.min.apply(Math, filterData.map(function(x) { return x.properties.project[key]; })),
                max: Math.max.apply(Math, filterData.map(function(x) { return x.properties.project[key]; }))
            };
        },
        getRangeArray: function (rangeObject) {
            return [rangeObject.min, rangeObject.max];
        },
        filter: function () {
            let result = this.filterData;
            if (this.title != "") {
                result = result.filter(item => item.properties.project.Title.toUpperCase().includes(this.title.toUpperCase()));
            }

            if (this.suburb != null) {
                result = result.filter(item => item.properties.project.Suburb == this.suburb);
            }
            
            if (this.state != null) {
                result = result.filter(item => item.properties.project.State == this.state);
            }

            result = result.filter(item => item.properties.project.Value >= this.value[0] && item.properties.project.Value <= this.value[1]);
            this.$emit('filter', result);
        }
    }
}
</script>
<style scoped>
#sidebar {
    padding: 10px;
}
</style>