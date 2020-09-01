<template>
  <div>
    <section class="container">
      <div>
        <h1 class="title">Space X Launch Programs</h1>
        <div class="grid-row">
          <!-- Filter card  -->
          <div class="grid-column">
            <FilterCard @filterSelection="filterSelection($event)" />
          </div>
           <!-- Filter card  -->
          <div class="grid-card" v-if="isDataAvailable">
            <!-- Rocket Details Card -->
            <div class="grid-column" v-for="(details,index) in launchDetails" :key="index">
              <RocketDetailsCard :rocketDetails="details" />
            </div>
            <!-- Rocket Details Card -->
          </div>
          <!-- Error Messsage -->
          <p v-else class="error-message">{{message}}</p>
             <!-- Error Messsage -->
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import axios from "axios";
// Dynamic import of components
const FilterCard = () => ({
  component: import("../components/FilterCard")
});
const RocketDetailsCard = () => ({
  component: import("../components/RocketDetailsCard")
});
export default {
  components: {
    FilterCard,
    RocketDetailsCard
  },
  data() {
    return {
      launchDetails: [],
      isDataAvailable: false,
      message: "",
      baseUrl: "https://api.spaceXdata.com/v3/launches?limit=100"
    }; 
  },
  created() {
    this.getLaunchDetails();
  },
  methods: {
    //Fetch API data on intial fetch
    getLaunchDetails() {
      axios.get(this.baseUrl).then(response => {
        if (typeof response.data !== "undefined" && response.data.length > 0) {
          this.isDataAvailable = true;
          let filteredResponse = response.data.map(item => {
            return {
              mission_name:item.mission_name,
              mission_patch_small:item.links.mission_patch_small,
              flight_number: item.flight_number,
              mission_id:item.mission_id,
              launch_success:item.launch_success,
              launch_year:item.launch_year,
              land_success: item.rocket.first_stage.cores[0].land_success,
            };
          });
          this.launchDetails = filteredResponse;
        } else {
          this.isDataAvailable = false;
          this.message = "No Data Available";
        }
      });
    },
    //Construct API link based on filter response
    filterSelection(data) {
      if (data.launch != undefined && data.year && data.landing != undefined) {
        this.baseUrl = `https://api.spaceXdata.com/v3/launches?limit=100&launch_year=${data.year}&launch_success=${data.launch}&land_success=${data.landing}`;
      } else if (data.year && data.launch != undefined) {
        this.baseUrl = `https://api.spaceXdata.com/v3/launches?limit=100&launch_year=${data.year}&launch_success=${data.launch}`;
      } else if (data.year && data.landing != undefined) {
        this.baseUrl = `https://api.spaceXdata.com/v3/launches?limit=100&launch_year=${data.year}&land_success=${data.landing}`;
      } else if (data.launch != undefined && data.landing != undefined) {
        this.baseUrl = `https://api.spaceXdata.com/v3/launches?limit=100&launch_success=${data.launch}&land_success=${data.landing}`;
      } else if (data.landing != undefined) {
        this.baseUrl = `https://api.spaceXdata.com/v3/launches?limit=100&land_success=${data.landing}`;
      } else if (data.year) {
        this.baseUrl = `https://api.spaceXdata.com/v3/launches?limit=100&launch_year=${data.year}`;
      } else if (data.launch != undefined) {
        this.baseUrl = `https://api.spaceXdata.com/v3/launches?limit=100&launch_success=${data.launch}`;
      }
      this.getLaunchDetails();
    }
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: block;
  max-width: 1440px;
}
.grid-row {
  display: grid;
  grid-template-columns: 20% auto;
}
.grid-column {
  margin: 10px;
  /* background: red; */
}
.grid-card {
  display: grid;
  grid-template-columns: 25% 25% 25% 25%;
}
.title {
  margin: 10px;
}
.error-message {
  text-align: center;
  margin: auto;
  font-size: 16px;
}
.ma-0 {
  margin: 0px;
}
.my-xs {
  margin: 5px 0;
}
@media (max-width: 1024px) and (min-width: 700px) {
  .grid-card {
    display: grid;
    grid-template-columns: 50% 50%;
  }
}
@media (max-width: 700px) {
  .grid-row {
    display: grid;
    grid-template-columns: auto;
  }
  .grid-card {
    display: grid;
    grid-template-columns: auto;
  }
}
</style>
