
<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-card>
        <b-card>
          <b-row>
            <b-col sm>
              <label for="status">Status</label>
              <b-form-select v-model="status" :options="options" @change="changeStatus($event)"></b-form-select>
            </b-col>
            <b-col sm>
              <label for="from-datepicker">From date</label>
              <b-form-datepicker id="from-datepicker" v-model="from_date" class="mb-2"></b-form-datepicker>
            </b-col>
            <b-col sm>
              <label for="to-datepicker">To date</label>
              <b-form-datepicker id="to-datepicker" v-model="to_date" class="mb-2"></b-form-datepicker>
            </b-col>
            <b-col>
              <b-button class="mt-4" variant="outline-primary" @click="DateFilter()">Search</b-button>
            </b-col>
          </b-row>
        </b-card>
        <b-row>
          <b-col sm><h3>Call Date</h3></b-col>
          <b-col sm><h3>Phone Number</h3></b-col>
          <b-col sm><h3>Call Duration</h3></b-col>
          <b-col sm><h3>Status</h3></b-col>
        </b-row>
        <b-row v-for="(item, index) in list" :key="index">
          <b-col sm>{{ item.call_date }}</b-col>
          <b-col sm>{{ item.phone_number }}</b-col>
          <b-col sm>{{ item.call_duration }}</b-col>
          <b-col sm>{{ item.status }}</b-col>
        </b-row>
      </b-card>
    </b-container>
  </div>
</template>

<script>

import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

 
Vue.use(VueAxios, axios)

export default {
  name: 'App',
 

  data () {
    return {
      list: [],
      status: null,
      from_date:'',
      to_date:'',
      options: [
        { value: null, text: 'Please select an option' },
        { value: 'in-call', text: 'in call' },
        { value: 'hold', text: 'hold' },
        { value: 'do not call', text: 'do not call' },
        { value: 'call back', text: 'call back' },
      ]
    }
  },
  
  created () {
    this.axios.get('http://axilweb.test/api/get-logs').then((response) => {
      this.list = response.data.data
    })
  },
  methods: {
    // changeStatus(event) {
    //   let status = event;
    //   this.axios.get('http://axilweb.test/api/get-logs?status='+status).then((response) => {
    //     this.list = response.data.data
    //   })
    // },

    DateFilter(){
      this.axios.get('http://axilweb.test/api/get-logs?status='+this.status+'&from_date='+this.from_date+'&to_date='+this.to_date).then((response) => {
        this.list = response.data.data
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
