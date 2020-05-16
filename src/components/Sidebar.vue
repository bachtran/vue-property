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
        <label for="commenceDateFrom">Commence Date From</label>
        <b-form-datepicker v-model="commenceDateFrom" name="commenceDateFrom"></b-form-datepicker>
        <label for="commenceDateTo">Commence Date To</label>
        <b-form-datepicker v-model="commenceDateTo" name="commenceDateTo"></b-form-datepicker>
        <label for="completionDateFrom">Completion Date</label>
        <b-form-datepicker v-model="completionDateFrom" name="completionDateFrom"></b-form-datepicker>
        <label for="completionDateTo">Completion Date</label>
        <b-form-datepicker v-model="completionDateTo" name="completionDateFrom"></b-form-datepicker>
        <label for="lastUpdatedFrom">Last Update From</label>
        <b-form-datepicker v-model="lastUpdatedFrom" name="lastUpdatedFrom"></b-form-datepicker>
        <label for="lastUpdatedTo">Last Update To</label>
        <b-form-datepicker v-model="lastUpdatedTo" name="lastUpdatedTo"></b-form-datepicker>
        <label for="value">Value</label>
        <vue-slider v-model="value" :min="valueRange.min" :max="valueRange.max" name="value"></vue-slider>
        
        <b-form-input v-model="description" placeholder="Description"></b-form-input>
        <b-form-input v-model="notes" placeholder="Notes"></b-form-input>
        
        <b-button v-on:click.prevent="filter" class="submitButton">Filter</b-button>
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

            KEY: {
                Title: "Title",
                Type: "Type",
                Address: "Address",
                Suburb: "Suburb",
                State: "State",
                Stage: "Stage",
                Category: "Category",
                SubCategory: "SubCategory",
                Status: "Status",
                Council: "Council",
                DevType: "Dev. Type",
                FloorArea: "Floor Area",
                SiteArea: "Site Area",
                Storeys: "Storeys",
                Units: "Units",
                CommenceDate: "Commence Date",
                CompletionDate: "Completion Date",
                LastUpdated: "Last Updated",
                Value: "Value",
                Ownership: "Ownership",
                Description: "Description",
                Notes: "Notes",
                AdditionalDetails: "Addtional Details",
                Lat: "Lat",
                Long: "Long"
            },

            coordinatesInterval: 0.0001,
            
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
            commenceDateFrom: "", // date picker 
            commenceDateTo: "",
            completionDateFrom: "", // date picker 
            completionDateTo: "",
            lastUpdatedFrom: "", // date picker
            lastUpdatedTo: "",
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
        this.typeChoices = this.getChoices(this.filterData, this.KEY.Type);

        this.suburbChoices = this.getChoices(this.filterData, this.KEY.Suburb);

        this.stateChoices = this.getChoices(this.filterData, this.KEY.State);

        this.stageChoices = this.getChoices(this.filterData, this.KEY.Stage);

        this.categoryChoices = this.getChoices(this.filterData, this.KEY.Category);

        this.subCategoryChoices = this.getChoices(this.filterData, this.KEY.SubCategory);

        this.statusChoices = this.getChoices(this.filterData, this.KEY.Status);

        this.councilChoices = this.getChoices(this.filterData, this.KEY.Council);

        this.devTypeChoices = this.getChoices(this.filterData, this.KEY.DevType);

        this.ownershipChoices = this.getChoices(this.filterData, this.KEY.Ownership);

        this.valueRange = this.getRange(this.filterData, this.KEY.Value);
        this.value = this.getRangeArray(this.valueRange);

        this.floorAreaRange = this.getRange(this.filterData, this.KEY.FloorArea);
        this.floorArea = this.getRangeArray(this.floorAreaRange);

        this.siteAreaRange = this.getRange(this.filterData, this.KEY.SiteArea);
        this.siteArea = this.getRangeArray(this.siteAreaRange);

        this.storeysRange = this.getRange(this.filterData, this.KEY.Storeys);
        this.storeys = this.getRangeArray(this.storeysRange);

        this.unitsRange = this.getRange(this.filterData, this.KEY.Units);
        this.units = this.getRangeArray(this.unitsRange);

        this.latRange = this.getRange(this.filterData, this.KEY.Lat);
        this.lat = this.getRangeArray(this.latRange);

        this.longRange = this.getRange(this.filterData, this.KEY.Long);
        this.long = this.getRangeArray(this.longRange);
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
        filterChoices: function (data, value, key) {
            if (value != null) {
                return data.filter(item => item.properties.project[key] == value);
            } else {
                return data;
            }
        },
        filterRange: function (data, value, key) {
            return data.filter(item => item.properties.project[key] >= value[0] && item.properties.project[key] <= value[1]);
        },
        filterText: function (data, value, key) {
            if (value != "") {
                return data.filter(item => item.properties.project[key].toUpperCase().includes(value.toUpperCase()));
            } else {
                return data;
            }
        },
        filterDateFrom: function (data, value, key) {
            if (value == "") {
                return data;
            }      
            return data.filter(function (item) {
                let dateSplit = item.properties.project[key].split('/');
                // As the year in date is in short format yy, I assume the year is 20yy
                let dateRef = new Date(parseInt(dateSplit[2]) + 2000, dateSplit[1], dateSplit[0]);
                let dateValue = new Date(value);
                if (dateRef > dateValue) {
                    return true;
                } else {
                    return false;
                }
            });
        },
        filterDateTo: function (data, value, key) {    
            if (value == "") {
                return data;
            }       
            return data.filter(function (item) {
                let dateSplit = item.properties.project[key].split('/');
                // As the year in date is in short format yy, I assume the year is 20yy
                let dateRef = new Date(parseInt(dateSplit[2]) + 2000, dateSplit[1], dateSplit[0]);
                let dateValue = new Date(value);
                if (dateRef < dateValue) {
                    return true;
                } else {
                    return false;
                }
            });
        },
        filter: function () {
            let result = this.filterData;
            result = this.filterText(result, this.title, this.KEY.Title);

            result = this.filterChoices(result, this.type, this.KEY.Type);

            result = this.filterChoices(result, this.suburb, this.KEY.Suburb);

            result = this.filterChoices(result, this.state, this.KEY.State);

            result = this.filterChoices(result, this.stage, this.KEY.Stage);

            result = this.filterChoices(result, this.category, this.KEY.Category);

            result = this.filterChoices(result, this.subCategory, this.KEY.SubCategory);

            result = this.filterChoices(result, this.status, this.KEY.Status);

            result = this.filterChoices(result, this.council, this.KEY.Council);

            result = this.filterChoices(result, this.devType, this.KEY.DevType);

            result = this.filterRange(result, this.floorArea, this.KEY.FloorArea);

            result = this.filterRange(result, this.siteArea, this.KEY.SiteArea);

            result = this.filterRange(result, this.storeys, this.KEY.Storeys);

            result = this.filterRange(result, this.units, this.KEY.Units);

            result = this.filterRange(result, this.value, this.KEY.Value);

            result = this.filterDateFrom(result, this.commenceDateFrom, this.KEY.CommenceDate);
            result = this.filterDateTo(result, this.commenceDateTo, this.KEY.CommenceDate);

            result = this.filterDateFrom(result, this.completionDateFrom, this.KEY.CompletionDate);
            result = this.filterDateTo(result, this.completionDateTo, this.KEY.CompletionDate);

            result = this.filterDateFrom(result, this.lastUpdatedFrom, this.KEY.LastUpdated);
            result = this.filterDateTo(result, this.lastUpdatedTo, this.KEY.LastUpdated);

            this.$emit('filter', result);
        }
    }
}
</script>
<style scoped>
#sidebar {
    padding: 10px;
}
.submitButton {
  position: fixed;
  top: 20px;
  left: 20px;
}
</style>