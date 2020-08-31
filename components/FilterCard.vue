<template>
  <article class="card">
    <h4 class="ma-0">Filter</h4>
    <h5 class="ma-0 card-subtitle">Launch Year</h5>
    <div class="filter-option">
      <!-- Year Buttom -->
      <button
        class="year-chip"
        @click="getFilterSelection(year,'year',index)"
        v-for="(year,index) in yearsData"
        :key="index"
        :class="{active:index === activeItemYear}"
      >{{year}}</button>
      <!-- Year Buttom -->
    </div>
    <h5 class="card-subtitle">Success Launch</h5>
    <div class="filter-option">
      <!-- Launch Buttom -->
      <button
        class="year-chip"
        @click="getFilterSelection(launch,'launch',index)"
        v-for="(launch,index) in successLaunch"
        :key="index"
        :class="{active:index === activeItemLaunch}"
      >{{launch}}</button>
      <!-- Launch Buttom -->
    </div>
    <h5 class="card-subtitle">Success Landing</h5>
    <div class="filter-option">
      <!-- Landing Buttom -->
      <button
        class="year-chip"
        @click="getFilterSelection(landing,'landing',index)"
        v-for="(landing,index) in successLanding"
        :key="index"
        :class="{active:index === activeItemLanding}"
      >{{landing}}</button>
      <!-- Launch Buttom -->
    </div>
  </article>
</template>

<script>
export default {
  data() {
    return {
      filterSelection: {},
      activeItemYear: null,
      activeItemLaunch: null,
      activeItemLanding: null,
      yearsData: [
        2006,
        2007,
        2008,
        2009,
        2010,
        2011,
        2012,
        2013,
        2014,
        2015,
        2016,
        2017,
        2018,
        2019,
        2020
      ],
      successLaunch: [true, false],
      successLanding: [true, false]
    };
  },
  methods: {
    //get Filter selection and pass to parent layout(Pages/index.vue)
    getFilterSelection(data, type, index) {
      if (type === "year") {
        this.filterSelection.year = data;
        this.activeItemYear = index;
      } else if (type === "launch") {
        this.filterSelection.launch = data;
        this.activeItemLaunch = index;
      } else if (type === "landing") {
        this.filterSelection.landing = data;
        this.activeItemLanding = index;
      }
      this.$emit("filterSelection", this.filterSelection);
    }
  }
};
</script>

<style scoped>
.card {
  padding: 5px 10px;
  background: #fff;
}
.card-subtitle {
  text-align: center;
  border-bottom: 1px solid;
  font-weight: 500;
  margin: 0 20%;
}
.filter-option {
  display: grid;
  grid-template-columns: auto auto;
  margin-bottom: 10px;
}
.year-chip {
  background: #7ad67a;
  padding: 5px 0px;
  text-align: center;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  margin-top: 10px;
}
.active {
  background: #0cbb0c;
}
.year-chip:nth-child(odd) {
  margin-right: 15px;
}
.year-chip:nth-child(even) {
  margin-left: 15px;
}

@media (max-width: 700px) {
  .year-chip:nth-child(odd) {
    margin-right: 25px;
  }
  .year-chip:nth-child(even) {
    margin-left: 25px;
  }
}
</style>