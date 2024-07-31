<template>
<div class="w-full h-[100vh] justify-center items-center flex bg-blue-500">
  <div class="w-[50%] h-[75%]  bg-red-500 flex flex-wrap">
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-red-800 w-[40%] h-[40%]">China</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>

        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>


        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        <p class="bg-green-500 w-[20%] h-[20%]">Brazil</p>
        
  </div>
</div>

</template>
<script>
export default{
    name:"TreeMap",
    props:{
        array:{
            required:true
        }
    },
    data(){
        return {
            data:[],
            
        }
    },
    created(){
        // sum will be the 100%
        /*
        sum---100%
        item ---x
        avg = item*100/sum
        */
        var sum = this.array.reduce((avg, item) => {
            return avg + item.size;
        }, 0);
        var avg = 0
        this.array.forEach((item)=>
        {

            avg = (item.size*100)/sum

            this.data.push({text:item.text,color:item.color,size:avg})
        })

    },
    mounted(){
        console.log(this.data)
    }
}
</script>