<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-light">邊撚個遲到</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <v-data-table :headers="headers" :items="staffList" :items-per-page="-1">
        <!-- <template v-slot:item.time="{ item }">{{getMoment(item.time)}}</template>
        
        <template v-slot:item.isLate="{ item }">{{item.isLate}}</template>-->
        <template v-slot:item="{item}">
          <tr>
            <td>{{item.location}}</td>
            <td>
              <span :class="item.isLate?'late':''">
                 {{item.name}}
              </span>
         
            </td>
            <td>{{getMoment(item.time)}}</td>
            <td>{{item.isLate}}</td>
          </tr>
        </template>
      </v-data-table>
    </v-content>
  </v-app>
</template>
 

<script>
const axios = require("axios");
const moment = require("moment");
export default {
  name: "App",
  components: {},
  data: () => ({
    headers: [
           {
        text: "location",
        value: "location"
      },
      {
        text: "name",
        value: "name"
      },
 
      {
        text: "time",
        value: "time"
      },
      {
        text: "isLate",
        value: "isLate"
      }
    ],
    info: null,
    staffList: []
    // staffList: [
    //   {
    //     name: "YU Chun Fai",
    //     location: "HV",
    //     time: "2019-08-08T06:06:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Daniel W.T. CHAN",
    //     location: "HV",
    //     time: "2019-08-08T02:55:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Patrick S.W. CHAU",
    //     location: "HV",
    //     time: "2019-08-08T01:29:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Mandy M.L. LAI",
    //     location: "AKN",
    //     time: "2019-08-08T01:27:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Louis L.Y. SHUN",
    //     location: "HV",
    //     time: "2019-08-08T01:18:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Keith K.L. CHAN",
    //     location: "HV",
    //     time: "2019-08-08T01:09:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Raymen Y.N. HO",
    //     location: "HV",
    //     time: "2019-08-08T01:07:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Rio T.H. KWOK",
    //     location: "HV",
    //     time: "2019-08-08T01:07:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Maggie Y.M. CHAN",
    //     location: "AKN",
    //     time: "2019-08-08T01:05:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Paul C.S. MAK",
    //     location: "HV",
    //     time: "2019-08-08T01:04:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Alex K.W. WONG",
    //     location: "HV",
    //     time: "2019-08-08T01:04:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Alan H.F. CHAN",
    //     location: "HV",
    //     time: "2019-08-08T01:04:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Bruce K.K. FONG",
    //     location: "AKN",
    //     time: "2019-08-08T01:04:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Alex H.Y. LAW",
    //     location: "HV",
    //     time: "2019-08-08T01:02:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "Adolf C.S. LAI",
    //     location: "HV",
    //     time: "2019-08-08T01:02:00.000Z",
    //     isLate: true
    //   },
    //   {
    //     name: "KO Kin Man",
    //     location: "HV",
    //     time: "2019-08-08T00:58:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Elaine W.L. CHENG",
    //     location: "AKN",
    //     time: "2019-08-08T00:55:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "YIU Mei Ching",
    //     location: "AKN",
    //     time: "2019-08-08T00:54:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "YIU Mei Ching",
    //     location: "AKN",
    //     time: "2019-08-08T00:54:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Kenny C.K. WONG",
    //     location: "HV",
    //     time: "2019-08-08T00:53:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Sarah K.W. YUEN",
    //     location: "AKN",
    //     time: "2019-08-08T00:53:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Ewen Y.W. FUNG",
    //     location: "HV",
    //     time: "2019-08-08T00:52:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "German C.M. CHAU",
    //     location: "HV",
    //     time: "2019-08-08T00:50:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Polly P.K. CHAN",
    //     location: "HV",
    //     time: "2019-08-08T00:47:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Matthew C.W. LEE",
    //     location: "AKN",
    //     time: "2019-08-08T00:46:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Jessie Y.C. CHEUNG",
    //     location: "HV",
    //     time: "2019-08-08T00:46:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Peter C.K. YU",
    //     location: "HV",
    //     time: "2019-08-08T00:43:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Joey C.Y. PANG",
    //     location: "HV",
    //     time: "2019-08-08T00:41:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Alan W.L. LAW",
    //     location: "HV",
    //     time: "2019-08-08T00:41:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Ivan W.Y. WAN",
    //     location: "AKN",
    //     time: "2019-08-08T00:41:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Patrick W.F. DAI",
    //     location: "HV",
    //     time: "2019-08-08T00:36:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "CHEUNG Shing Shun",
    //     location: "HV",
    //     time: "2019-08-08T00:30:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Jeff S.H. NGAN",
    //     location: "HV",
    //     time: "2019-08-08T00:28:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Thomas C.C. LEUNG",
    //     location: "HV",
    //     time: "2019-08-08T00:25:00.000Z",
    //     isLate: false
    //   },
    //   {
    //     name: "Nathan S.C. CHENG",
    //     location: "HV",
    //     time: "2019-08-08T00:13:00.000Z",
    //     isLate: false
    //   }
    // ]
  }),
  methods: {
    getMoment(val) {
      //TODO fix this -8 hours
      return moment(val)
        .add(-8, "hours")
        .format("DD/MM/YYYY HH:mm");
    }
  },
  mounted() {
    axios.get("https://quiet-spire-21167.herokuapp.com/").then(response => {
      this.staffList = response.data;
    });
  }
};
</script>
<style>

.late{
  color: red
}
</style>
