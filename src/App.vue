<template>
  <div id="app">
    <div class="office">
      <Map @select="handleTableSelect" />
      <SideMenu
        :person="person"
        :isUserOpened="isTableSelected"
        @update:isUserOpened="hideUser"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import people from "./assets/data/people.json";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data: () => {
    return {
      person: null,
      isTableSelected: false,
    };
  },
  methods: {
    handleTableSelect(tableId) {
      if (tableId === undefined) {
        this.isTableSelected = false;
        this.person = null;
      } else {
        this.isTableSelected = true;
        this.person =
          people.find((employee) => employee.tableId === Number(tableId)) ??
          null;
      }
    },
    hideUser() {
      this.person = null;
      this.isTableSelected = false;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 320px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
