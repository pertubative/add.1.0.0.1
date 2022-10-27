<template>
  <div class="content">
    <div class="row">
      <div class="col-lg-3 col-md-12">
        <card type="tasks">
          <template slot="header">
            <div>
              <select v-model="selected" v-on:change="get_indicator_list(true)">
                <option disabled value="">Data Sources</option>
                <option>World Bank</option>
                <option>UNSTATS</option>
                <option>Freedom House</option>
                <option>Lexical Index</option>
              </select>
            </div>
            <SearchAutoComplete
            :items="info"
            />
          </template>

          <div class="table-full-width table-responsive">
            <base-table :data="indicators" thead-classes="text-primary">
              <template slot-scope="{ row }">
                <td>
                  <input
                    type="checkbox"
                    :id="row.id"
                    :value="row.id"
                    v-model="indicator_data_table.indicator_codes"
                  />
                </td>
                <td class="text-left">
                  <p class="title">{{ row.id }}</p>
                  <p class="text-muted">{{ row.name }}</p>
                </td>
                <td class="td-actions text-right">
                  <base-button type="link" artia-label="edit button">
                  </base-button>
                </td>
              </template>
            </base-table>
          </div>
          <template v-if="page_control.show">
            <a href="#" class="previous round"  v-on:click="decrement_active_index">&lt;</a>
            <label> {{page_control.active_index}} </label>
            <a href="#" class="next round" v-on:click="increment_active_index">&gt;</a>
          </template>
        </card>
      </div>
      <div class="col-lg-3">
        <card type="tasks">
          <template slot="header">
            <span>Countries </span>
          </template>
          <div class="table-full-width table-responsive">
            <base-table :data="country_data" thead-classes="text-primary">
              <template slot-scope="{ row }">
                <td>
                  <input
                    type="checkbox"
                    :id="row.iso3code"
                    :value="row.iso3code"
                    v-model="selected_country_codes"
                  />
                </td>
                <td class="text-left">
                  <p class="text-muted">{{ row.country }}</p>
                </td>
              </template>
            </base-table>
          </div>
        </card>
      </div>

      <div class="col-lg-6 col-md-12">
        <card type="tasks">
          <span slot="header" class="ex3">Time Series: </span>
          <input
            slot="header"
            type="number"
            class="number"
            v-model="indicator_data_table.from_year"
            min="1700"
            max="2021"
          />
          <span slot="header" class="ex3"> - </span>
          <input
            slot="header"
            type="number"
            class="number"
            v-model="indicator_data_table.to_year"
            min="1700"
            max="2021"
          />
         
          <JsonExcel
            :data="indicator_data_table.data"
            worksheet="My Worksheet"
            name="data.xls"
          >
            <button class="download">
              <img
                src="excel-png-office-xlsx-icon-3.png"
                width="30px"
                height="20px"
              />
            </button>
          </JsonExcel>
          <button slot="header" class="ex1" type="submit" v-on:click="get_data">
            Run
          </button>

          <div class="table-full-width table-responsive">
            <base-table
              :data="indicator_data_table.data"
              :columns="indicator_data_table.columns"
              thead-classes="text-primary"
            >
            </base-table>
          </div>
        </card>
      </div>
    </div>
    <!-- This is the indicators series chart -->
    <div class="row">
      <div class="col-12">
        <card type="chart">
          <template slot="header">
            <div class="row">
              <div class="col-sm-6" :class="isRTL ? 'text-right' : 'text-left'">
                <template v-if="!isRTL">
                  <h5 class="card-category">API Aggregator</h5>
                </template>
                <template v-if="!isRTL">
                  <h2 class="card-title">Indicators Time Series</h2>
                </template>
              </div>
              <div class="col-sm-6">
                <div
                  class="btn-group btn-group-toggle"
                  data-toggle="buttons"
                  :class="isRTL ? 'float-left' : 'float-right'"
                >
                  <div>
                    <select
                      v-model="selected_filter_country"
                      v-on:change="
                        get_data_points_group_by_country(
                          indicator_data_table.indicator_codes,
                          indicator_data_table.data
                        )
                      "
                    >
                      <option disabled value="">Countries</option>
                      <option
                        v-for="country in filter_countries"
                        :value="country"
                        v-bind:key="country"
                        >{{ country }}</option
                      >
                    </select>
                  </div>
                </div>
              </div>
            </div>
          </template>
          <line-chart
            class="chart-area"
            ref="bigChart"
            chart-id="big-line-chart"
            :chart-data="bigLineChart.chartData"
            :gradient-colors="bigLineChart.gradientColors"
            :gradient-stops="bigLineChart.gradientStops"
            :options="bigLineChart.options"
          >
          </line-chart>
        </card>
      </div>
    </div>
        <!-- This is the countries series chart -->
    <div class="row">
      <div class="col-12">
        <card type="chart">
          <template slot="header">
            <div class="row">
              <div class="col-sm-6" :class="isRTL ? 'text-right' : 'text-left'">
                <template v-if="!isRTL">
                  <h5 class="card-category">API Aggregator</h5>
                </template>
                <template v-if="!isRTL">
                  <h2 class="card-title">Countries Time Series</h2>
                </template>
              </div>
              <div class="col-sm-6">
                <div
                  class="btn-group btn-group-toggle"
                  data-toggle="buttons"
                  :class="isRTL ? 'float-left' : 'float-right'"
                >
                  <div>
                    <select v-model="countries_series.selected_indicator_filter"
                             v-on:change="get_data_points_group_by_indicators(
                              indicator_data_table.data)
                        "
                    >
                      <option disabled value="">Indicators</option>
                      <option
                        v-for="indicator in countries_series.filter_indicators"
                        :value="indicator"
                        v-bind:key="indicator"
                        >{{ indicator }}</option
                      >
                    </select>
                  </div>
                </div>
              </div>
            </div>
          </template>
          <line-chart
            class="chart-area"
            ref="bigChart"
            chart-id="big-line-chart"
            :chart-data="countriesBigLineChart.chartData"
            :gradient-colors="countriesBigLineChart.gradientColors"
            :gradient-stops="countriesBigLineChart.gradientStops"
            :options="countriesBigLineChart.options"
          >
          </line-chart>
        </card>
      </div>
    </div>
  </div>
</template>
<script>
import { Card } from "@/components/index";

import LineChart from "@/components/Charts/LineChart";
import * as chartConfigs from "@/components/Charts/config";

import BaseTable from "@/components/BaseTable";
import config from "@/config";
import axios from "axios";
import JsonExcel from "vue-json-excel";
import SearchAutoComplete from '../components/SearchAutoComplete.vue';

const tableColumns = ["indicatorcode", "country", "year", "value"];
export default {
  components: {
    Card,
    LineChart,
    BaseTable,
    JsonExcel,
    SearchAutoComplete,
  },
  data() {
    return {
      info: null,
      page_control : {
        active_index : 1,
        show : false
      },
      countries_series: {
        indicators : [],
        selected_indicator_filter : null,
        filter_indicators : [],
        labels : []
      },
      labels : [],
      selected_filter_country: null,
      filter_countries: [],
      selected_country_codes: [],
      selected_country_names: [],
      selected_country: "",
      indicators: [],
      country_data: [],
      indicator_data_table: {
        title: "Data",
        columns: [...tableColumns],
        data: [],
        from_year: 2000,
        to_year: 2010,
        indicator_codes: [],
      },
      selected: "",
      bigLineChart: {
        chartData: { datasets: [{}] },
        options: chartConfigs.greenChartOptions,
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.4, 0],
      },
      countriesBigLineChart: {
        chartData: { datasets: [{}] },
        options: chartConfigs.greenChartOptions,
        gradientColors: config.colors.primaryGradient,
        gradientStops: [1, 0.4, 0],
      },
    };
  },
  created() {
    axios
      .get("https://gga-add-dev-api-staging.azurewebsites.net/api/countries")
      .then((response) => {
        this.country_data = response.data;
      })
      .catch((e) => {
        this.errors.push(e);
      });
  },
  computed: {
    enableRTL() {
      return this.$route.query.enableRTL;
    },
    isRTL() {
      return this.$rtl.isRTL;
    },
  },
  methods: {
    increment_active_index : function() {
        this.page_control.active_index++;
        this.get_indicator_list(false);
    },

    decrement_active_index : function(){
       if(this.page_control.active_index > 1)
        {
            this.page_control.active_index--;
            this.get_indicator_list(false);
        } 
        

    },
    get_indicator_list: async function(datasource_change) {
      
      let url = "#";
      switch (this.selected) {
        case "World Bank":
          url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/worldbank?page_number=" + this.page_control.active_index;
          break;
        case "UNSTATS":
          url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/unstats";
          break;
        case "Freedom House":
          url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/freedomhouse";
          break;
        case "Lexical Index":
          url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/lexicalindex";
          break;
      }
      console.log(url);
      await axios
        .get(url)
        .then((response) => {
          this.indicators = response.data;
        })
        .catch((e) => {
          this.errors.push(e);
        });

        if(datasource_change){
          this.page_control.active_index = 1;
          this.indicator_data_table.indicator_codes = []
        }
        if(this.indicators.length > 0){
           this.page_control.show = true;
        }else{
           this.page_control.show = false;
        }
      return this.indicators;
    },

    get_data_points: function(codes, in_data) {
      console.log(codes);

      let points = [];
      codes.forEach((code) => {
        points.push({
          indicator: code,
          values: in_data
            .filter((item) => item.indicatorcode == code)
            .map((element) => element.value),
        });
        console.log(points);
      });
      return points;
    },

    get_data_points_group_by_country: function(codes, in_data) {
      console.log(codes);

      this.filter_countries = [...new Set(in_data.map((item) => item.country))];
      this.labels = [...new Set(in_data.map((item) => item.year))];
      if (!this.selected_filter_country) {
        this.selected_filter_country = this.filter_countries[0];
        in_data = in_data.filter(
          (item) => item.country == this.selected_filter_country
        );

      } else {
        in_data = in_data.filter(
          (item) => item.country == this.selected_filter_country
        );
      }
      console.log(in_data);
      let points = [];
      codes.forEach((code) => {
        points.push({
          indicator: code,
          values: in_data
            .filter((item) => item.indicatorcode == code)
            .map((element) => element.value),
        });
        console.log(points);
        this.Indicators_time_series(points, this.labels);
      });

      return points;
    },

    Indicators_time_series: function(points, x_labels) {
      let pre_datasets = [];
      let color_index = 0;
      points.forEach((point) => {
        console.log("point.indicator_code = " + point.indicator);
        console.log("point.values = " + point.values);
        pre_datasets.push({
          label: point.indicator,
          fill: false,
          borderColor: chartConfigs.namedColor(color_index),
          borderWidth: 2,
          borderDash: [],
          borderDashOffset: 0.0,
          pointBackgroundColor: chartConfigs.namedColor(color_index),
          pointBorderColor: "rgba(255,255,255,0)",
          pointHoverBackgroundColor: chartConfigs.namedColor(color_index),
          pointBorderWidth: 20,
          pointHoverRadius: 4,
          pointHoverBorderWidth: 15,
          pointRadius: 4,
          data: point.values,
        });
        color_index++;
      });

      let chartData = {
        datasets: pre_datasets,
        labels: x_labels
      };
      this.bigLineChart.chartData = { datasets: [{}] };
      this.bigLineChart.chartData = chartData;
    },

    get_data_points_group_by_indicators: function(in_data) {

      this.countries_series.filter_indicators = [...new Set(in_data.map((item) => item.indicatorcode))];
      this.countries_series.labels = [...new Set(in_data.map((item) => item.year))];

      if (!this.countries_series.selected_indicator_filter) {
        this.countries_series.selected_indicator_filter = this.countries_series.filter_indicators[0];
        in_data = in_data.filter(
          (item) => item.indicatorcode == this.countries_series.selected_indicator_filter 
        );
      } else {
    
        in_data = in_data.filter(
          (item) => item.indicatorcode == this.countries_series.selected_indicator_filter 
        );
      }
      console.log(in_data);
      let countries = [...new Set(in_data.map((item) => item.country))];
      let points = [];
      countries.forEach((country) => {
        points.push({
          indicator: country,
          values: in_data
            .filter((item) => item.country == country)
            .map((element) => element.value),
        });
        console.log(points);
        this.countries_time_series(points, this.countries_series.labels);
      });

      return points;
    },

    countries_time_series: function(points, x_labels) {
      let pre_datasets = [];
      let color_index = 0;
      points.forEach((point) => {
        console.log("point.indicator_code = " + point.indicator);
        console.log("point.values = " + point.values);
        pre_datasets.push({
          label: point.indicator,
          fill: false,
          borderColor: chartConfigs.namedColor(color_index),
          borderWidth: 2,
          borderDash: [],
          borderDashOffset: 0.0,
          pointBackgroundColor: chartConfigs.namedColor(color_index),
          pointBorderColor: "rgba(255,255,255,0)",
          pointHoverBackgroundColor: chartConfigs.namedColor(color_index),
          pointBorderWidth: 20,
          pointHoverRadius: 4,
          pointHoverBorderWidth: 15,
          pointRadius: 4,
          data: point.values,
        });
        color_index++;
      });
      console.log("Predata sets" + pre_datasets);

      let chartData = {
         datasets: pre_datasets,
        labels: x_labels
      };
      this.countriesBigLineChart.chartData = { datasets: [{}] };
      this.countriesBigLineChart.chartData = chartData;
    },

    get_data: async function() {
      let base_url = "#";
      switch (this.selected) {
        case "World Bank":
          base_url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/data/worldbank/";
          break;
        case "UNSTATS":
          base_url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/data/unstats/";
          break;
        case "Freedom House":
          base_url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/data/freedomhouse/";
          break;
        case "Lexical Index":
          base_url = "https://gga-add-dev-api-staging.azurewebsites.net/api/indicators/data/lexicalindex/";
          break;
      }

      let query =
        this.indicator_data_table.indicator_codes +
        "?from_year=" +
        this.indicator_data_table.from_year;
      query +=
        "&to_year=" +
        this.indicator_data_table.to_year +
        "&countries=" +
        this.selected_country_codes;

      let url = base_url + query;

      await axios
        .get(url)
        .then((response) => {
          this.indicator_data_table.data = response.data;
        })
        .catch((e) => {
          this.errors.push(e);
        });
      this.indicator_data_table.data = this.indicator_data_table.data.sort(
        (a, b) => {
          return a.year - b.year;
        }
      );

      this.get_data_points_group_by_country(
        this.indicator_data_table.indicator_codes,
        this.indicator_data_table.data
      );

      this.get_data_points_group_by_indicators(this.indicator_data_table.data);
    },
  },
  mounted() {
  axios
    .get('https://cognitive-add-svc.search.windows.net/indexes/azureblob-index/docs?api-version=2021-04-30-Preview&search=gdp',{
       headers: {
        Authorization: '061F4E30D779DD3195B2010A2F3E53E2'
      }
    })
      .then (response => (this.info =response))
    this.i18n = this.$i18n;
    if (this.enableRTL) {
      this.i18n.locale = "ar";
      this.$rtl.enableRTL();
    }
    // this.Indicators_time_series(0);
  },
  beforeDestroy() {
    if (this.$rtl.isRTL) {
      this.i18n.locale = "en";
      this.$rtl.disableRTL();
    }
  },
};
</script>
<style>
button.ex1 {
  margin-left: 10px;
  width: 100px;
  background: lightgoldenrodyellow;
}

button.ex2 {
  margin-left: 10px;
  width: 100px;
  background: lightgoldenrodyellow;
}
span.ex2 {
  margin-left: 20px;
}
/* span.ex3 {
    margin-left: 20px;
  } */
input.number {
  width: 80px;
  height: 25px;
  text-align: center;
}

input.ex4 {
  width: 150px;
  height: 25px;
}

.download {
  float: right;
  flex: 0%;
  margin-top: 0px;
  background: rgb(39, 139, 64);
}


a {
  text-decoration: none;
  display: inline-block;
  padding: 8px 16px;
  font-size: 11px;
}

label {
  padding: 8px 16px;

}a:hover {
  background-color: #ddd;
  color: black;
}

.previous {
  background-color: #04AA6D;
  color: white;
}

.next {
  background-color: #04AA6D;
  color: white;
}

.round {
  border-radius: 50%;
}
</style>
