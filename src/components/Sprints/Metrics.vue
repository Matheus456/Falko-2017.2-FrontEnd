<template>
  <div>
    <button class="tablink" id="defaultOpen" v-on:click="openMetrics($event, 'Stories')"><h3>Kanban</h3></button>
    <button class="tablink" v-on:click="openMetrics($event, 'Velocity')"><h3>Velocity</h3></button>
    <button class="tablink" v-on:click="openMetrics($event, 'Burndown')"><h3>Burndown</h3></button>

    <div id="Velocity" class="tabcontent active">
      <br><br><br>
      <velocity ref="velocity"></velocity>
    </div>

    <div id="Burndown" class="tabcontent">
      <br><br><br>
      <burndown ref="burndown"></burndown>
    </div>

    <div id="Stories" class="tabcontent">
      <br><br><br>
      <div class="kanban-style">
        <stories></stories>
      </div>
    </div>

  </div>
</template>


<script>
import { mapState } from 'vuex';
import { HTTP } from '../../http-common';
import Velocity from './Velocity.vue';
import Burndown from './Burndown.vue';
import Stories from '../Stories/Stories.vue'

export default {
  data () {
    return{
      metric: 'Velocity',
    }
  },
  components: {
    'burndown': Burndown,
    'velocity': Velocity,
    'stories': Stories,
  },
  methods: {
    openMetrics(evt, metric) {
      console.log(evt);
      var i, tabcontent, tablinks;
      this.metric = metric;
      tabcontent = document.getElementsByClassName("tabcontent");
      for (i = 0; i < tabcontent.length; i++) {
          tabcontent[i].style.display = "none";
      }

      tablinks = document.getElementsByClassName("tablink");
      for (i = 0; i < tablinks.length; i++) {
          tablinks[i].className = tablinks[i].className.replace(" active", "");
          tablinks[i].style.background = "";
          tablinks[i].style.border = "none";
      }

      document.getElementById(metric).style.display = "block";
      evt.currentTarget.className += " active";
      evt.currentTarget.style.background = "white";
      evt.currentTarget.style.border = "3px solid #496C78";
      evt.currentTarget.style.borderWidth = "0.4px 0.4px 0px 0.4px";
      this.$refs.velocity.getVelocityData();
      this.$refs.burndown.getBurndownData();
    },
  },
}
</script>

<style scoped>
.tablink {
    background-color: #E0E8EB ;
    color: #01161E;
    float: left;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 14px 16px;
    width: 33%;
    margin-bottom: 50px;
}

.tablink:not(.active):hover {
    filter: brightness(90%);
}

.tabcontent {
    display: none;
    background-color: white ;
    text-align: left;
}
.kanban-style {
  width: 100%;
}
.tab-style {
  background-color: #AEC3B0;
}
h3 {
  font-size: 20px;
}
</style>
