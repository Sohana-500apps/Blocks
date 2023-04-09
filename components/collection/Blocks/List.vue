<template>
    <!-- start of showing Blocks in the table -->
    <div class="overflow-auto">
       <table class="shadow-2xl border-2 border-gray-800 w-full">
        <thead class="bg-gray-800 text-white text-left">
            <tr>
                <th class="py-3">Name</th>
                <th class="py-3">Category</th>
                <th class="py-3">Edit Action</th>
                <th class="py-3">Delete Action</th>
            </tr>
        </thead>
        <tbody class="divide-y  divide-gray-800">
            <tr v-for="(block,index) in BlockUrlData" :key="block" class="text-left cursor-pointer">
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editBlockIndex===index" v-model="editBlockForm.name" class="focus:border-purple-500" cols="10" rows="1">{{block.name}}</textarea>
                <p v-else>{{block.name}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editBlockIndex===index" v-model="editBlockForm.category" class="focus:border-purple-500" cols="10" rows="1">{{block.category}}</textarea>
                <p v-else>{{block.category}}</p>
              </td>
            
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionBlocksEdit @edit="editBlock(block,index)"/>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionBlocksDelete @delete="deleteBlock(block)"/>
              </td>
            </tr>
        </tbody>
       </table>
          
    </div>
     <!-- End of showing Blocks in the table -->
</template>
<script setup lang="ts">
//Defining the schema of BlockSchema
interface BlockSchema{
 name:string,
 category:string,
}
// Getting the props of BlockUrlData
let props = defineProps<{ BlockUrlData:BlockSchema }>()
const emits = defineEmits(['edit','delete']);
const editBlockIndex=ref("0")


//Defining the schema of reactive BlockEditForm
interface BlockEditForm {
 name:string,
 category:string,
 
 
}

//Declaring in the empty form and use it in v-model
const editBlockForm:BlockEditForm=reactive({
 name:'',
 category:'',
 
})

//Emitting the delete and pass it in the main
const deleteBlock=(block:any)=>{
   emits('delete',block)
 
}
//Emitting the edit and pass it in the main
const editBlock=(block:any,index:any)=>{
   editBlockIndex.value=index
 emits('edit',block,editBlockForm)
}
</script>