<template>
  <div>
    <b-form @submit.prevent="update">
      <b-form-group
        id="threshold-group"
        label="Temperature Threshold"
        label-for="threshold-input"
      >
        <b-form-input
          id="threshold-input"
          required
          v-model="threshold"
          :placeholder="this.threshold"
        />
      </b-form-group>
      <b-form-group
        id="number-group"
        label="Notify Number"
        label-for="number-input"
      >
        <b-form-input
          id="number-input"
          required
          v-model="number"
          :placeholder="this.number"
        />
      </b-form-group>
      <b-form-group
        id="location-group"
        label="Sensor Location"
        label-for="location-input"
      >
        <b-form-input
          id="location-input"
          required
          v-model="location"
          :placeholder="this.location"
        />
      </b-form-group>
      <b-form-group
        id="name-group"
        label="Sensor Name"
        label-for="name-input"
      >
        <b-form-input
          id="name-input"
          required
          v-model="name"
          :placeholder="this.name"
        />
      </b-form-group>
      <div class="buttons">
        <b-button-group>
          <b-button type="submit" variant="success">Update</b-button>
          <b-button @click="close()" variant="danger">Cancel</b-button>
        </b-button-group>
      </div>
    </b-form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: "ProfileModal",
  data() {
    return {
      threshold: null,
      number: null,
      location: null,
      name: null,
    };
  },
  async created() {
    try {
      let response = await axios.get("http://localhost:3000/sky/cloud/ckkumnzus000f7plad5q156kf/sensor_profile/profile");
      this.threshold = response.data.threshold;
      this.number = response.data.notify_number;
      this.location = response.data.location;
      this.name = response.data.name;
    } catch(error) {
      console.log(error);
    }
  },
  methods: {
    close() {
      this.$nextTick(() => {
        this.$bvModal.hide("profile-modal");
      });
    },
    async update() {
      try {
        await axios.post("http://localhost:3000/sky/event/ckkumnzus000f7plad5q156kf/none/sensor/profile_updated", {
          new_threshold: parseInt(this.threshold),
          new_number: parseInt(this.number),
          new_location: this.location,
          new_name: this.name
        });
        this.close();
      } catch(error) {
        console.log(error);
      }
    }
  },
};
</script>

<style scoped>
.buttons {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
}

.error {
  margin-top: 20px;
  display: inline;
  padding: 5px 20px;
  border-radius: 30px;
  font-size: 10px;
  background-color: #d9534f;
  color: #fff;
}
</style>
