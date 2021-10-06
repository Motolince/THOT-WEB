<template>
  <div>
    <ControllerComponent />
    <TrajectoryComponent />
  </div>
</template>

<script>
import ControllerComponent from "../components/Controller.vue";
import TrajectoryComponent from "../components/Trajectory.vue";

export default {
  name: "MainComponent",
  components: {
    ControllerComponent,
    TrajectoryComponent
  },
  data() {
    return {};
  },
  methods: {},
  mounted() {}
};
</script>

<style></style>
