<template>
<!-- Rocket Details Card -->
  <article class="card-rocket">
    <div :style="imgcontainer"></div>
    <h6 class="card-title">{{rocketDetails.mission_name}} #{{rocketDetails.flight_number}}</h6>
    <h5 class="my-xs">
      Mission Id:
      <ul v-for="(id,index) in this.rocketDetails.mission_id" :key='index'>
          <li>{{id}}</li>
      </ul>
    </h5>
    <h5 class="my-xs">
      Launch Year:
      <span>{{rocketDetails.launch_year}}</span>
    </h5>
    <h5 class="my-xs">
      Successful Launch:
      <span>{{rocketDetails.launch_success}}</span>
    </h5>
    <h5 class="my-xs">
      Successful Landing:
      <span>{{rocketDetails.land_success}}</span>
    </h5>
  </article>
  <!-- Rocket Details Card -->
</template>

<script>
export default {
    props:["rocketDetails"],
    data(){
        return{
        }
    },

    computed:{
        imgcontainer(){
            return{
            background: `url(${this.rocketDetails.mission_patch_small}) 40% 0% / contain no-repeat #F1F1F2`,
            height:'165px',
            }
        },
    },
};
</script>

<style scoped>
.card-rocket {
  background: #fff;
  padding: 10px;
  margin: 0 5px 5px 5px;
  min-height: 385px;
}

.card-title {
  margin: 3px 0 12px 0px;
  color: navy;
  font-size: 14px;
}
</style>