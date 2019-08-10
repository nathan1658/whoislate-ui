<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-light">邊個遲到</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <v-dialog v-model="staffDialog" width="500">
        <v-card>
          <v-card-title class="headline grey lighten-2" primary-title>{{staffComboBoxSelected}}</v-card-title>
          <v-flex v-if="!selectedStaff" align-center justify-center row fill-height >  
            <h1  >未返工</h1>
                 </v-flex>
            <v-list v-else dense>
              <v-list-item>
                <v-list-item-content>Location:</v-list-item-content>

                <v-list-item-content>
                  <v-layout>
                    <location-chip :location="selectedStaff.location"></location-chip>
                  </v-layout>
                </v-list-item-content>
              </v-list-item>

              <v-list-item>
                <v-list-item-content>Time:</v-list-item-content>

                <v-list-item-content>{{getMoment(selectedStaff.time)}}</v-list-item-content>
              </v-list-item>
            </v-list>
     
          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="staffDialog = false">Close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-container>
        <v-flex xs12>
          <v-combobox v-model="staffComboBoxSelected" :items="staffComboBox"></v-combobox>
        </v-flex>
        <v-data-table :headers="headers" :items="staffList" :items-per-page="-1">
          <!-- <template v-slot:item.time="{ item }">{{getMoment(item.time)}}</template>
        
          <template v-slot:item.isLate="{ item }">{{item.isLate}}</template>-->
          <template v-slot:item="{item}">
            <tr>
              <td>
                <location-chip :location="item.location"></location-chip>
                <!-- <v-chip
                  :color="item.location == 'HV'?'#2196F3':'#9C27B0'"
                  text-color="white"
                >{{item.location}}</v-chip>-->
              </td>
              <td>
                <span :class="item.isLate?'late':''">{{item.name}}</span>
              </td>
              <td>
                <span :class="item.isLate?'late':''">{{getMoment(item.time)}}</span>
              </td>
            </tr>
          </template>
        </v-data-table>
      </v-container>
    </v-content>
  </v-app>
</template>
 

<script>
const axios = require("axios");
const moment = require("moment");
import locationChip from "./LocationChip.vue";
export default {
  name: "App",
  components: { locationChip },
  data: () => ({
    headers: [
      {
        text: "Location",
        value: "location"
      },
      {
        text: "Name",
        value: "name"
      },

      {
        text: "Time",
        value: "time"
      }
      // {
      //   text: "isLate",
      //   value: "isLate"
      // }
    ],
    info: null,
    staffList: [],
    staffComboBox: [],
    staffComboBoxSelected: null,
    staffDialog: false,
    selectedStaff: { name: "Peter", location: "HV", time: "123456" }
  }),
  methods: {
    getMoment(val) {
      //TODO fix this -8 hours
      return moment(val)
        .add(-8, "hours")
        .format("DD/MM/YYYY HH:mm");
    },
    arrayUnique(array) {
      var a = array.concat();
      for (var i = 0; i < a.length; ++i) {
        for (var j = i + 1; j < a.length; ++j) {
          if (a[i] === a[j]) a.splice(j--, 1);
        }
      }

      return a;
    }
  },
  created() {},
  mounted() {
    // axios.get("http://localhost:8887/whoislate.json").then(response => {
    axios.get("https://quiet-spire-21167.herokuapp.com/").then(response => {
      this.staffList = response.data;

      var tmp = JSON.parse(localStorage.getItem("staffList"));
      if (tmp == null) {
        let newList = this.staffList.map(x => x.name);
        newList = newList.sort((a, b) => {
          return a[0].toLowerCase() > b[0].toLowerCase() ? 1 : -1;
        });
        //If not set, use current list
        localStorage.setItem("staffList", JSON.stringify(newList));
        this.staffComboBox = newList;
      } else {
        let newList = this.arrayUnique(
          this.staffList.map(x => x.name).concat(tmp)
        );

        newList = newList.sort((a, b) => {
          return a[0].toLowerCase() > b[0].toLowerCase() ? 1 : -1;
        });

        localStorage.setItem("staffList", JSON.stringify(newList));
        this.staffComboBox = newList;
      }
    });
  },
  watch: {
    staffComboBoxSelected: function(val) {
      this.selectedStaff = this.staffList.filter(x => x.name == val)[0];
      this.staffDialog = true;
    }
  }
};
</script>
<style>
.late {
  color: red;
}
</style>
