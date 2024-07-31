<template>
<TreeMap
:array="array"
/>
</template>

<script>
import TreeMap from "@/components/TreeMap.vue"
export default {
  name: 'App',
  components:{TreeMap},
  data(){
    return{
      array:[
        {text:"Estados Unidos",color:"blue-500", size:50},
        {text:"Mexico",color:"red-500",size:20},
        {text:"Brazil",color:"green-800",size:30},
        {text:"Russian",color:"black-800",size:25},
      ]
    }
  }
  
}
</script>


