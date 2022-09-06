<template>
  <div id="app">
    <CustomTable :titles="titles" :tableData="tableData" />
  </div>
</template>

<script>
import CustomTable from './components/CustomTable.vue'

export default {
  name: 'App',
  components: {
    CustomTable
  },
  data(){
    return{
      titles:[
        {title:'başlık',field:'name'},
        {title:'kalori',field:'cal'},
        {title:'renk',field:'color'},
      ],
      tableData:[
        {name:'çilek',cal: 40,color:"red"},
        {name:'muz',cal: 30,color:"yellow"},
        {name:'elma',cal: 20,color:"redish"},
        {name:'salat',cal: 10,color:"yeş"},
      ],
    }
  }
}
</script>

<style>
</style>
