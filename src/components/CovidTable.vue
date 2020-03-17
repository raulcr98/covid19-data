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
							<tr class="table100-head">
								<th class="column1">Country</th>
								<th class="column2">New Confirmed</th>
								<th class="column3">Total Confirmed</th>
								<th class="column4">Total Deaths</th>
								<th class="column5">New Recovered</th>
								<th class="column6">Total Recovered</th>
							</tr>
						</thead>
						<tbody>
              <tr v-bind:key="country.Country" v-for="country in countries">
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
      countries: [],
      last_update: ''
    };
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
        console.log(response);
        response.data.Countries.map(country => {
          this.countries.push(country);
        });
        this.last_update = response.data.Date;
      })
      .catch(e => {
        console.log(e);
      });
    }
  },
  props: {
  }
}
</script>

<style scoped>
.header {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1em;
}
</style>
