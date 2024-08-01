<template>
    <div class="fixed inset-0 flex items-center justify-center bg-opacity-50 z-40">
        <div class="relative p-4 w-[50%]">
          <div class="relative bg-white rounded-lg shadow border border-gray-400">
            <div class="flex items-center justify-between p-4 md:p-5 border-b flex-col">
              <div class="w-full flex border-b border-gray-200 mb-2">
                <h3 class="text-xl font-semibold text-gray-900">
                  Detalhes do {{ obj.text }}
                </h3>
                <button type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-black-200 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center" data-modal-hide="default-modal" @click="this.$emit('close')">
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                  </svg>
                  <span class="sr-only">Close modal</span>
                </button>
              </div>
    
              <div class=" w-full mb-2">
                <div class="w-full flex gap-3 text-center">
                    <div class="w-full">
                        <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900">Nome do Pais</label>
                        <input type="text" id="first_name" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" v-model="data.text" />
                    </div>
                    <div class="w-full">
                        <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900">Tamanho</label>
                        <input type="number" id="first_name" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" v-model="data.size" />
                    </div>
                </div>
              </div>
    
              <div class="flex items-center justify-center pt-2 border-t border-gray-200 rounded-b w-full">
                <button type="button"  class="focus:outline-none text-white bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2" @click=" this.$emit('saveCountry',data); ">Salvar</button>
                <button type="button"  class="focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2" @click=" this.$emit('deleteCountry',this.obj.id); ">Apagar</button>  
            </div>
            </div>
          </div>
        </div>
      </div>
</template>
<script>

export default{
    name:"ModalCountry",
    emits: ['close','deleteCountry','saveCountry'],
    props:{
        obj:{
            required:true
        }
    },
    data(){
        return{
            data:this.obj
        }
    },
  
}
</script>