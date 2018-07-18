<template>
  <div class="container">
    <report-harvested-followers :chart-data="datacollection">></report-harvested-followers>
  </div>
</template>

<script>
/* eslint semi: "error" */
/* eslint space-before-function-paren: ["error", "never"] */
/* eslint-env es6 */
import axios from 'axios';
import moment from 'moment';
import ReportHarvestedFollowers from './reportHarvestedFollowers';
export default {
  name: 'HelloWorld',
  components: {
    ReportHarvestedFollowers
  },
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      datacollection: null
    };
  },
  mounted() {
    var _this = this;
    var url =
      'http://insupapi.azurewebsites.net/userRequestReports?username=quierobesarte.es';
    var config = {
      url: url,
      method: 'get',
      respnseType: 'json',
      headers: {
        'Content-Type': 'application/json',
        Authorization:
          'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjViNDNkMzFlZTkxZGIxMzg0NmNiOTVjMiIsImlhdCI6MTUzMTE3MjAyM30.IVHTvnZwef5QdeTVL9TWxd0Ug6BmZlH2yIj26K8Hy0g'
      }
    };

    axios.request(config).then(respose => {
      console.log(respose.data);
      this.datacollection = _this.$_HelloWorld_getPlotData(respose.data);
    });
  },
  methods: {
    $_HelloWorld_getPlotData: serverData => {
      var lineSuccess = {
        label: 'Success',
        backgroundColor: '#00cc44',
        data: []
      };
      var linePending = {
        label: 'Pending',

        data: []
      };
      var lineTimeout = {
        label: 'Timeout',
        backgroundColor: '#f44b42',
        data: []
      };
      var lineCancel = {
        label: 'Cancel',
        data: []
      };

      var lineNoMachineFollowers = {
        label: 'noMachineSuccess',
        backgroundColor: '#87c176',
        data: []
      };

      var labels = [];
      serverData.forEach(element => {
        if (element.days.length > 0) {
          element.days.forEach(item => {
            labels.push(moment(item.date).format('LL'));
            lineSuccess.data.push(item.success);
            linePending.data.push(item.pending);
            lineTimeout.data.push(item.timeout);
            lineCancel.data.push(item.cancel);
            lineNoMachineFollowers.data.push(item.noMachineFollowers);
          });
        }
      });

      var datasets = [
        lineSuccess,
        linePending,
        lineTimeout,
        lineCancel,
        lineNoMachineFollowers
      ];

      return {
        labels: labels,
        datasets: datasets
      };
    }
  }
};
// # sourceURL=settings.vue
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
