<template>
  <div>
    <p class="error" v-if="err"> {{err}} </p>
      <div
        class="table center"
        v-for=" (table,index) in tableNumbers"
          v-bind:key="index"
      > 
        <Table
          v-bind:tableNumbers="tableNumbers[index]"
          v-bind:table="table"
          v-bind:scores="scores[index]"
        />
      </div>
  </div>
</template>

<script>
import Table from '../components/Table';
import APIService from "../APIService";

export default {
  name: 'Home',
  components: {
    Table
  },
    //attributes of the class
    data() {
      return {
        tableNumbers: [ 2 ,3 ,4 ,5 ,6 ,7 ,8 ,9 ,10 ,11 ,12, 13],
        scores: [],
        status:"",
        err: "",
      }
    },

    // get the api at the time page start
    async created() {
      try {
        this.scores = await APIService.getAllHighScore();
      } catch (err) {
        this.err = err.message;
      }
    },

    }
  
</script>

<style scoped>
.table {
  padding: 10px;
  margin-top: 15px;
}
</style>
