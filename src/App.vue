<template>
  <div id="app">
    <div class="office">
      <Map
        @setCurrentUser="setCurrentUser"
        @setIsUserOpenned="setIsUserOpenned"
        :tables="normalizeTables"
        :people="people"
        :legend="getLegend"
      />
      <SideMenu
        :isUserOpenned="isUserOpenned"
        :person="currentUser"
        :legend="getLegend"
        @update:isUserOpenned="setIsUserOpenned"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import legend from "./assets/data/legend.json";
import people from "./assets/data/people.json";
import tables from "./assets/data/tables.json";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data() {
    return {
      currentUser: null,
      isUserOpenned: false,
      people: [],
      tables: [],
      legend: [],
    };
  },
  methods: {
    setCurrentUser(user) {
      this.currentUser = user;
    },
    setIsUserOpenned(isOpen) {
      this.isUserOpenned = isOpen;
    },
    loadTables() {
      this.tables = tables;
    },
    loadPeople() {
      this.people = people;
    },
    loadLegend() {
      this.legend = legend;
    },
    getLegendCounter(id) {
      return this.tables.filter(({ group_id }) => group_id === id).length;
    },
    normalizeLegend(legendItem) {
      return {
        ...legendItem,
        counter: this.getLegendCounter(legendItem.group_id),
      };
    },
  },
  computed: {
    getUsersId() {
      return this.people.map((user) => user._id);
    },
    normalizeTables() {
      return tables.map((table) => ({
        ...table,
        isEmpty: !this.getUsersId.includes(table._id),
      }));
    },
    getLegend() {
      return this.legend.map(this.normalizeLegend);
    },
  },
  created() {
    this.loadLegend();
    this.loadPeople();
    this.loadTables();
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
