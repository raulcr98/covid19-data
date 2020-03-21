
<template>
  
  <div>
       
    <div class="limiter">    
      
      <div class="container-table100">
        
        <div class="header">
          <h1>Covid19 World Status </h1>          
          <p> Last update: {{last_update}}</p>          
        </div>
        
        <div class="wrap-table100">
          <div class="table100">
            <table>
              <thead>
                <tr class="table100-head"  style="cursor: pointer;">
                  <th class="column1"> 
                    <span v-on:click="order_by('Country')">Country</span> 
                    <input placeholder="Search by Country"
                           v-model="filter_name"/>
                  </th>
                  <th class="column2" 
                    v-on:click="order_by('newConfirmed')">New Confirmed</th>
                  <th class="column3"
                    v-on:click="order_by('totalConfirmed')">Total Confirmed</th>
                  <th class="column4"
                    v-on:click="order_by('totalDeaths')">Total Deaths</th>
                  <th class="column5" 
                    v-on:click="order_by('newRecovered')">New Recovered</th>
                  <th class="column6" 
                    v-on:click="order_by('totalRecovered')">Total Recovered</th>
                </tr>
              </thead>
              <tbody>              
                  <!-- CHECKING IF FILTER_NAME IS PREFIX OF THE COUNTRY NAME IN ANY CASE-->                  
                    <tr v-for="country in filteredItems" v-bind:key="country.Country">
                      <td class="column1">{{ country.Country }}</td>
                      <td class="column2">{{ country.NewConfirmed }}</td>
                      <td class="column3">{{ country.TotalConfirmed }}</td>
                      <td class="column4">{{ country.TotalDeaths }}</td>
                      <td class="column5">{{ country.NewRecovered }}</td>
                      <td class="column6">{{ country.TotalRecovered }}</td>
                    </tr>
                  
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CovidTable',
  data() {
    return {
      countries: [
        /*{
          Country: "Cuba",
          NewConfirmed: 1,
          TotalConfirmed: 11, 
          TotalDeaths: 1,
          NewRecovered: 0,
          TotalRecovered: 0 
        },
        {
          Country: "Italia",
          NewConfirmed: 200,
          TotalConfirmed: 30000, 
          TotalDeaths: 3000,
          NewRecovered: 100,
          TotalRecovered: 1000
        }*/
      ],
      filter_name: '',
      last_update: '',
      // ORDER DIRECTION
      country_order: -1,
      new_confirmed_order: -1,
      total_confirmed_order: -1,
      total_deaths_order: -1,
      new_recovered_order: -1,
      total_recovered_order: -1,
      // END ORDER DIRECTION
    };
  },
  computed: {
    filteredItems() {
      return this.countries.filter(country => {
        return country.Country.toLowerCase().indexOf(this.filter_name.toLowerCase())>-1
      })
    }
  },
  mounted:function(){
    this.getCases()
  },
  methods:{
    getCases:function(){
      axios
      .get(
        "https://api.covid19api.com/summary"
      )
      .then(response => {
        response.data.Countries.map(country => {
           this.countries.push(country);
           console.log(this.countries)          
        });
        this.last_update = response.data.Date;
      })
      .catch(e => {
        console.log(e);
      });
    },
    order_by: function(param) {      
      if ( param === "Country" ) {
        this.countries.sort((a, b) => (a.Country > b.Country) ? 
                                      this.country_order : (a.Country < b.Country) ?
                                      -1*this.country_order : 0);
        this.country_order *= -1;
      }
      if ( param === "newConfirmed" ) {
        this.countries.sort((a, b) => (a.NewConfirmed > b.NewConfirmed) ?
                                      this.new_confirmed_order : (a.NewConfirmed < b.NewConfirmed) ?
                                      -1*this.new_confirmed_order : 0);
        this.new_confirmed_order *= -1;                                    
      }
      if ( param === "totalConfirmed" ) {
        this.countries.sort((a, b) => (a.TotalConfirmed > b.TotalConfirmed) ? 
                                      this.total_confirmed_order : (a.TotalConfirmed < b.TotalConfirmed) ?
                                      -1*this.total_confirmed_order : 0);
        this.total_confirmed_order *= -1;                                    
      }
      if ( param === "totalDeaths" ) {
        this.countries.sort((a, b) => (a.TotalDeaths > b.TotalDeaths) ? 
                                      this.total_deaths_order: (a.TotalDeaths < b.TotalDeaths) ?
                                      -1*this.total_deaths_order : 0);
        this.total_deaths_order *= -1;                                      
      }
      if ( param === "newRecovered" ) {
        this.countries.sort((a, b) => (a.NewRecovered > b.NewRecovered) ? 
                                      this.new_recovered_order : (a.NewRecovered < b.NewRecovered) ?
                                      -1*this.new_recovered_order : 0);
        this.new_recovered_order *= -1;                                      
      }
      if ( param === "totalRecovered") {
        this.countries.sort((a, b) => (a.TotalRecovered > b.TotalRecovered) ? 
                                      this.total_recovered_order : (a.TotalRecovered < b.TotalRecovered) ?
                                      -1*this.total_recovered_order : 0)
        this.total_recovered_order *= -1;                                      
      }
    }
  },
  props: {
    
  }
  // 
}
</script>

<style scoped>
  .header {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 1em;
  }

  .table100-head .column1 {
    display: flex;
    flex-direction: row;
    padding-top: 1em;
  }

  .table100-head .column1 input{
    margin-left: 1em;
    border-radius: 15px;
    padding-left: .5em;
  }

</style>
