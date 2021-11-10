<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" @click="onClick" />
      <TableSvg v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import MapSVG from "../assets/images/map.svg";
import TableSvg from "../assets/images/workPlace.svg";
import * as d3 from "d3";
import tables from "../assets/data/tables.json";
import legend from "../assets/data/legend.json";

export default {
  components: {
    MapSVG,
    TableSvg,
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      g: null,
      tables: [],
      tableSVG: null,
    };
  },
  mounted() {
    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select("g");
    this.tables = tables;
    this.tableSVG = d3.select(this.$refs.table);

    if (this.g) {
      this.drawTables();
    } else {
      console.log("Error");
    }
  },
  methods: {
    getColor(table) {
      return (
        legend.find((el) => el.group_id === table.group_id)?.color ??
        "transparent"
      );
    },
    drawTables() {
      const svgTablesGroup = this.g.append("g").classed("tables", true);
      this.tables.forEach((table) => {
        svgTablesGroup
          .append("g")
          .attr(
            "transform",
            `translate(${table.x}, ${table.y}) scale (0.5) rotate(${
              table.rotate || 0
            })`
          )
          .classed("workplace", true)
          .attr("id", table._id)
          .attr("fill", this.getColor(table))
          .html(this.tableSVG.html());
      });
    },
    onClick(e) {
      const tableId = e.target?.closest(".workplace")?.id;
      this.$emit("select", tableId);
    },
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
