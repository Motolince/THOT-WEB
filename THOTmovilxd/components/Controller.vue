<template>
  <div id="controller-container" @keydown="moveWithKeyboard" tabindex="0">
    <div class="row align-items-center">
      <!-- ====================================================== -->
      <!-- Lights Switch Col -->
      <div class="col-12 col-md-6">
        <!-- LIGHTS -->
        <div class="d-flex align-items-center justify-content-center py-3">
          <h5 class="display-4">Led</h5>
          <button-1>
            <div class="col-4 text-center">
              <b-icon icon="brightness-high" font-scale="2.0"></b-icon>
            </div>
          </button-1>
          <label class="switch mt-2 mx-3">
            <input
              type="checkbox"
              v-model="powerLights"
              @change="turnLights($event)"
            />
            <div class="slider round"></div>
            <small v-if="!powerLights" class="text-muted on"></small>
            <small v-if="powerLights" class="text-muted off"></small>
          </label>
        </div>
      </div>
      <div>
        <h4 aling="center">Politécnico Grancolombiano</h4>
        <h5>Jhon Alejandro Ospina Romero</h5>
        <h5>Nicolas Silva Muñoz</h5>
        <p aling="center">THOT V1.0 - 2021</p>
      </div>
      <!-- ====================================================== -->
      <!-- Direction Buttons Col -->
      <div class="col-12 col-md-6">
        <div class="row justify-content-center justify-content-md-start">
          <div class="col-10 py-3">
            <div class="row align-items-center">
              <div class="col-4 text-right">
                <!-- Empty Col1 -->
              </div>
              <div class="col-4 text-center">
                <!-- FORWARD BUTTON Col2 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.top
                  v-on:click="move(MOVEMENT_DIRECTION.FORWARD)"
                >
                  <b-icon icon="box-arrow-in-up" font-scale="5.0"></b-icon>
                </button>
              </div>
              <div class="col-4">
                <!-- Empty Col3 -->
              </div>
            </div>

            <div class="row">
              <div class="col-4 text-right">
                <!-- LEFT BUTTON Col4 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.left
                  v-on:click="move(MOVEMENT_DIRECTION.LEFT)"
                >
                  <b-icon icon="box-arrow-in-left" font-scale="5.0"></b-icon>
                </button>
              </div>
              <div class="col-4">
                <!-- Empty Col5 -->
              </div>
              <div class="col-4 text-left">
                <!-- RIGHT BUTTON Col6 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.right
                  v-on:click="move(MOVEMENT_DIRECTION.RIGHT)"
                >
                  <b-icon icon="box-arrow-in-right" font-scale="5.0"></b-icon>
                </button>
              </div>
            </div>

            <div class="row">
              <div class="col-4">
                <!-- Empty Col7 -->
              </div>
              <div class="col-4 text-center">
                <!-- BACKWARD BUTTON Col8 -->
                <button
                  type="button"
                  class="direction-button btn btn-ak"
                  v-b-tooltip.hover.bottom
                  v-on:click="move(MOVEMENT_DIRECTION.BACKWARD)"
                >
                  <b-icon icon="box-arrow-in-down" font-scale="5.0"></b-icon>
                </button>
              </div>
              <div class="col-4">
                <!-- Empty Col9 -->
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  MOVEMENT_DIRECTION,
  MOVEMENT_ORIGIN,
  SOCKET_EVENT,
} from "../env/constants";

export default {
  name: "ControllerComponent",
  data() {
    return {
      MOVEMENT_DIRECTION: MOVEMENT_DIRECTION,
      powerLights: false,
    };
  },
  methods: {
    // --- Move
    move(direction) {
      this.socket.emit(SOCKET_EVENT.CREATE_MOVEMENT, {
        direction: direction,
        origin: MOVEMENT_ORIGIN.WEB,
      });
    },
    // --- Move with keyboard
    moveWithKeyboard(event) {
      switch (event.key) {
        case "ArrowUp":
          this.move(MOVEMENT_DIRECTION.FORWARD);
          break;
        case "ArrowDown":
          this.move(MOVEMENT_DIRECTION.BACKWARD);
          break;
        case "ArrowLeft":
          this.move(MOVEMENT_DIRECTION.LEFT);
          break;
        case "ArrowRight":
          this.move(MOVEMENT_DIRECTION.RIGHT);
          break;
      }
    },
    // --- Turn the lights on/off
    turnLights(event) {
      this.socket.emit(SOCKET_EVENT.TURN_LIGHTS, {
        power: event.target.checked,
      });

      this.powerLights = event.target.checked;
    },
  },
  mounted() {
    this.socket = this.$nuxtSocket({
      name: "main",
      channel: "/",
      reconnection: false,
    });

    this.socket.on(SOCKET_EVENT.TURN_LIGHTS, (data) => {
      this.powerLights = data.power;
    });
  },
};
</script>

<style>
.direction-button {
  width: 95px;
  height: 95px;
  border-radius: 10%;
  padding: 0;
  transition: all 800s;
  color: rgb(0, 0, 0);
  cursor: pointer;
  background-color: #b2b2b2;
}

.display-4 {
  font-size: 32px;
}

/* ======================== SLIDER ======================== */

/* Formateamos el label que servirá de contenedor */
.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 40px;
}

/* Ocultamos el checkbox html */
.switch input {
  display: none;
}

.switch small {
  position: absolute;
  top: 35px;
}

.switch .on {
  margin-left: 10px;
}

.switch .ff {
  margin-left: 10px;
}
/* Formateamos la caja del interruptor sobre la cual se deslizará la perilla de control o slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgb(215, 215, 215);
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

/* Pintamos la perilla de control o slider usando el selector before */
.slider:before {
  position: absolute;
  content: "";
  height: 0px;
  width: 0px;
  left: 0px;
  bottom: 0px;
  background-color: rgb(188, 187, 187);
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

/* Cambiamos el color de fondo cuando el checkbox esta activado */
input:checked + .slider {
  background-color: rgb(220, 85, 75);
}

/* Deslizamos el slider a la derecha cuando el checkbox esta activado */
input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Aplicamos efecto de bordes redondeados en slider y en el fondo del slider */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 10%;
}
</style>