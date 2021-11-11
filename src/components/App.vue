<template>
<div class="newPost">
    <h1>Відділення Нової Пошти</h1>
        <select v-model="areas" v-on:change="chooseCity">
            <option value="0" selected disabled>Оберiть область</option>
            <option v-for="(area, index) in allArea" :key="index" :value="index+1"> {{area.Description}}</option>
        </select>
        <br>
        <select v-model="cities" v-on:change="chooseDepert" v-if="allCity[0]">
            <option value="0" selected disabled>Оберiть мiсто</option>
            <option v-for="(city, index) in allCity" :key="index" :value="index+1">{{city.Description}}</option>
        </select>
        <br>      
        <select v-model="departs" v-if="allDepart[0]">
            <option value="0" selected disabled>Оберiть відділення</option>
            <option v-for="(depart, index) in allDepart" :key="index" :value="index+1">{{depart.Description}}</option>
        </select>
</div>
</template>

<script>

import axios from 'axios'

export default {
    
    data: function() {
        return {
           url: 'https://api.novaposhta.ua/v2.0/json/',
           data: [],
           allArea: [],
           allCity: [],
           allDepart: [], 
           areas: '0',
           cities: '0',
           departs: '0',
        }
    },

    mounted: function(){
        axios.post(this.url, {
            "modelName": "AddressGeneral",
            "calledMethod": "getAreas",
            "methodProperties": {},
            "apiKey": "31445003fe9f423020f8d7f914532e23"
        })
        .then (res => {
            console.log(res.data.data)
            this.allArea = res.data.data;
        })
    },

    methods: {        
        chooseCity: function() {
            console.log(this.allArea[this.areas-1].Ref)
            axios.post(this.url, {
                "modelName": "AddressGeneral",
                "calledMethod": "getCities",
                "methodProperties": {
                    "AreaRef": this.allArea[this.areas-1].Ref.trim(),
                },
                "apiKey": '31445003fe9f423020f8d7f914532e23',
            })
            .then (res => {
                console.log(res.data.data)
                this.allCity = res.data.data
            })
            .then (() => {
                this.cities = '0';
            })
        },       
        
        chooseDepert: function() {
            console.log(this.allCity[this.cities-1].Ref)
            axios.post(this.url, {
                "modelName": "AddressGeneral",
                "calledMethod": "getWarehouses",
                "methodProperties": {
                    "CityRef": this.allCity[this.cities-1].Ref,
                },
                "apiKey": '31445003fe9f423020f8d7f914532e23',
            })
            .then (res => {
                console.log(res.data.data)
                this.allDepart = res.data.data
                
            })

        },
    }
}
</script>

<style scoped>

</style>