<template>
  <div id="app">
    <div>
      <b-button @click="getData()">Get Data</b-button>
      <b-button @click="$bvModal.show('profile-modal')" variant="success">Profile</b-button>
    </div>
    <h1>Current Temperature: {{currentTemperature}} C</h1>
    <h1>Recent Readings</h1>
    <div v-for="item in recent" :key="item.id">
      <h4>Temperature: {{item.temperatureC}}</h4>
      <h5>Timestamp: {{item.timestamp}}</h5>
    </div>
    <h1>Violations Log</h1>
    <div v-for="item in violations" :key="item.id">
      <h4>Temperature: {{item.temperatureC}}</h4>
      <h5>Timestamp: {{item.timestamp}}</h5>
    </div>
    
    <b-modal id="profile-modal" title="Profile" hide-footer>
      <ProfileModal />
    </b-modal>
  </div>
</template>

<script>
import ProfileModal from '@/components/ProfileModal'
import axios from 'axios';

export default {
  name: 'App',
  components: {
    ProfileModal
  },
  data() {
    return {
      currentTemperature: 0,
      recent: [],
      violations: []
    };
  },
  methods: {
    async getData() {
      try {
        let temperatures = await axios.get("http://localhost:3000/sky/cloud/ckkumnzus000f7plad5q156kf/temperature_store/temperatures");
        let violations = await axios.get("http://localhost:3000/sky/cloud/ckkumnzus000f7plad5q156kf/temperature_store/threshold_violations");
        this.currentTemperature = temperatures.data[0].temperatureC;
        this.recent = temperatures.data;
        this.violations = violations.data;
      } catch(error) {
        console.log(error);
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
