<template>
  <div id="canvasContainer">
    <!-- <div class="col-12">
      <button v-on:click="printMovement('FORWARD')">Arriba</button>
      <button v-on:click="printMovement('LEFT')">Izquierda</button>
      <button v-on:click="printMovement('BACKWARD')">Abajo</button>
      <button v-on:click="printMovement('RIGHT')">Derecha</button>
    </div> -->

    <canvas
      id="robothPathCanvas"
      class="robot-path-canvas"
      height="480"
    ></canvas>
  </div>
</template>

<script>
import { MOVEMENT_DIRECTION, SOCKET_EVENT } from "../env/constants";

export default {
  name: "TrajectoryComponent",
  data() {
    return {
      xFin: 1,
      yFin: 1
    };
  },
  methods: {
    printMovement(direction) {
      var canvas = document.getElementById("robothPathCanvas");

      if (canvas !== null) {
        var canvasCtx = canvas.getContext("2d");

        canvasCtx.lineWidth = 1;
        canvasCtx.moveTo(this.xFin, this.yFin);

        switch (direction) {
          case MOVEMENT_DIRECTION.FORWARD:
            this.yFin -= 50;
            break;
          case MOVEMENT_DIRECTION.BACKWARD:
            this.yFin += 50;
            break;
          case MOVEMENT_DIRECTION.LEFT:
            this.xFin -= 50;
            break;
          case MOVEMENT_DIRECTION.RIGHT:
            this.xFin += 50;
            break;
        }

        canvasCtx.lineTo(this.xFin, this.yFin);
        canvasCtx.stroke();
      }
    },
    setupCanvas() {
      var canvasContainer = document.getElementById("canvasContainer");

      if (canvasContainer !== null) {
        this.xFin = canvasContainer.offsetWidth / 2;
        this.yFin = canvasContainer.offsetHeight / 2;

        init();

        window.addEventListener("resize", init, false);

        function init() {
          var canvas = document.getElementById("robothPathCanvas");

          if (canvas !== null) {
            var canvasCtx = canvas.getContext("2d");

            var canvasWidth =
              document.getElementById("canvasContainer").offsetWidth - 5;
            var canvasHeight =
              document.getElementById("canvasContainer").offsetHeight - 5;

            canvasCtx.canvas.width = canvasWidth;
            canvasCtx.canvas.height = canvasHeight;
          }
        }
      }
    }
  },
  mounted() {
    this.setupCanvas();

    this.socket = this.$nuxtSocket({
      name: "main",
      channel: "/",
      reconnection: false
    });

    this.socket.on(SOCKET_EVENT.CREATE_MOVEMENT, data => {
      this.printMovement(data.direction);
    });
  },
  destroyed() {
    // this.socket.removeAllListeners("ROBOT_MOVE");
  }
};
</script>

<style>
.robot-path-canvas {
  border: solid #444 1px;
  /* height: 100px; */
}
</style>
