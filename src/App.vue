<template>
  <div>
    <TreeMap
      :array="array"
      :key="treeMapKey"
      @getDetails="getDetails"/>

    <ModalCountry
      @close="close"
      @deleteCountry="deleteCountry"
      @saveCountry="saveCountry"
      :obj="obj"
      v-if="modal"/>
  </div>
</template>

<script>
import TreeMap from "@/components/TreeMap.vue";
import ModalCountry from "./components/ModalCountry.vue";
import array from "./services/array";

export default {
  name: 'App',
  components: { TreeMap, ModalCountry },
  data() {
    return {
      obj: {},
      modal: false,
      array,
      treeMapKey: 0  //Serve para forcar para renderizar de novo o TreeMap
    };
  },
  methods: {
    getDetails(value) {
      this.obj = value;
      this.modal = true;
    },
    close() {
      this.modal = false;
    },
    saveCountry(data) {
      const id = (data.id === 0) ? 0 : data.id - 1;
        this.array[id].size = data.size
        this.array[id].text = data.text
        alert("Atualizado com sucesso");
        this.close()
        this.treeMapKey += 1;         
    },
    deleteCountry(index) {
        this.array.splice(index, 1);
        alert("Apagado com sucesso");
        this.close()
        this.treeMapKey += 1; 
      
    }
  }
};
</script>
