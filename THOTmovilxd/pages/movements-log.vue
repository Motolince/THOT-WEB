<template>
  <div class="p-4">
    <div class="row">
      <div class="col-12 text-left">
        <!-- Paginator -->
        <b-pagination
          align="left"
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="movements-table"
          class="pagination-container"
        >
          <template #first-text>
            <span v-b-tooltip.hover.top title="Primera página">
              <b-icon icon="arrow-left-square"></b-icon>
            </span>
          </template>
          <template #prev-text>
            <span v-b-tooltip.hover.top title="Página anterior">
              <b-icon icon="arrow-left"></b-icon>
            </span>
          </template>
          <template #next-text>
            <span v-b-tooltip.hover.top title="Siguente página">
              <b-icon icon="arrow-right"></b-icon>
            </span>
          </template>
          <template #last-text>
            <span v-b-tooltip.hover.top title="Última página">
              <b-icon icon="arrow-right-square"></b-icon>
            </span>
          </template>
        </b-pagination>
      </div>
    </div>

    <!-- Movements Table -->
    <b-table
      id="movements-table"
      striped
      responsive
      borderless
      outlined
      hover
      bordered
      head-variant="orange"
      :per-page="perPage"
      :current-page="currentPage"
      :small="true"
      :items="movements"
      :fields="fields"
      :empty-text="'No hay registros.'"
      :show-empty="true"
    ></b-table>
  </div>
</template>

<script>
import moment from "moment";

import {
  MOVEMENT_DIRECTION,
  MOVEMENT_ORIGIN,
  SOCKET_EVENT,
} from "../env/constants";

export default {
  data() {
    return {
      socket: null,
      perPage: 20,
      currentPage: 1,
      fields: [
        {
          key: "direction",
          label: "Movimiento",
          sortable: true,
          formatter: "formatDirection",
        },
        {
          key: "origin",
          label: "Origen",
          sortable: true,
          formatter: "formatOrigin",
        },
        {
          key: "datetime",
          label: "Fecha y Hora",
          sortable: true,
          formatter: "formatDate",
        },
      ],
      movements: [],
    };
  },
  computed: {
    rows() {
      if (this.movements) {
        return this.movements.length;
      }
    },
  },
  methods: {
    formatDate: function (value) {
      if (value) {
        return moment(String(value)).format("DD/MM/YYYY - HH:mm:ss");
      }
    },
    formatDirection: function (value) {
      switch (value) {
        case MOVEMENT_DIRECTION.FORWARD:
          return "Adelante";
        case MOVEMENT_DIRECTION.BACKWARD:
          return "Atrás";
        case MOVEMENT_DIRECTION.LEFT:
          return "Izquierda";
        case MOVEMENT_DIRECTION.RIGHT:
          return "Derecha";
      }
    },
    formatOrigin: function (value) {
      switch (value) {
        case MOVEMENT_ORIGIN.WEB:
          return "App web";
        case MOVEMENT_ORIGIN.MOB:
          return "App móvil";
        case MOVEMENT_ORIGIN.MUS:
          return "Sensor";
      }
    },
  },
  mounted() {
    this.socket = this.$nuxtSocket({
      name: "main",
      channel: "/",
      reconnection: false,
    });

    this.socket.emit(SOCKET_EVENT.GET_MOVEMENTS);
    this.socket.on(SOCKET_EVENT.GET_MOVEMENTS, (data) => {
      this.movements = this.movements.concat(data);
      this.movements
        .sort((a, b) => new Date(a.datetime) - new Date(b.datetime))
        .reverse();
    });

    this.socket.on(SOCKET_EVENT.CREATE_MOVEMENT, (data) => {
      this.movements.unshift(data);
    });
  },
};
</script>

<style>
/* ----- GENERAL PAGINATOR STYLES ----- */
.pagination-container .page-item.active .page-link {
  background-color: rgb(0, 0, 0) !important;
  border: solid rgb(6, 5, 5) 1px !important;
  color: #fff !important;
  box-shadow: none;
}

.pagination-container .page-link {
  color: rgb(0, 0, 0) !important;
}

.pagination-container .page-link:focus {
  box-shadow: none;
}
</style>
