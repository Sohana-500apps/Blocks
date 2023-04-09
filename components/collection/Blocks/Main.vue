<template>
    <div>
        <!-- start of Block -->
       <div class="p-[50px] flex divide-y-2 border-y">
           
        <h1>Users</h1>
        <!-- start of add button -->
        <div class="ml-[56rem]">
            <button class="flex bg-slate-100" @click="openBlocks">
            <PlusIcon class="h-6 w-6"/>
            <p class="ml-3">Add User</p>
            </button>
        </div>
          <!-- end of add button -->
            <!-- opening the Block modal by clicking add Block -->
        <CollectionBlocksAdd v-show="isBlock" @cancel="isBlock=false" @save="saveBlocks"/>
       </div>
    <div class="text-center" v-if="!isBlock">
        <h1 class="text-3xl">Blocks List</h1>
         <div>
            <CollectionBlocksList   :BlockUrlData="BlockUrlData"   @delete="deleteBlockListener" @edit="editBlockListener"/>
        </div>
      </div>
    </div> 
     <!-- End of Block -->
</template>
<script setup lang="ts">
//importing the HeroIcon and  useAuthLazyFetch
import { PlusIcon } from "@heroicons/vue/24/outline"
import {useAuthLazyFetch}  from "../../../composables/useAuthLazyFetch"


//Define the schema of BlogUrl
interface BlogUrl{
    BlogUrl:string,
    url:string
}
// Get the props of BlogUrl and url
const props = withDefaults(defineProps<BlogUrl>(), {
    BlogUrl: "",
    url:""
})

//Showing the dynamic Block in the table
const getBlogUrl = async () => {
    try {
    const { data: BlockData } = await useAuthLazyFetch(props.BlogUrl, {});
    return BlockData;
  } catch (error) {
    console.error("Error fetching tag URL", error);
    return null;
  }
}
const BlockUrlData = await getBlogUrl();

const isBlock=ref(false)

//opening the  Add Block model
const openBlocks=()=>{
    isBlock.value=!isBlock.value
}

//saving the Block
const saveBlocks=(addBlocksForm)=>{
useAuthLazyFetchPost(`${props.url}/`, {
    body: {
    name:addBlocksForm.name,
    category:addBlocksForm.category,
    block_html:"string",
    block_css:"string"
    }
  });
}

//Deleting the blocks
const deleteBlockListener=(block)=>{
if(confirm('Are you sure to delete this record?')){
    useAuthLazyFetchDelete(`${props.url}/${block.uid}`, {});
    let BlogUrlIndex=BlockUrlData.value.findIndex(item => item.uid === block.uid)
    if(BlogUrlIndex!==-1){
        BlockUrlData.value.splice(BlogUrlIndex,1)
    }
}
}

//Editing the Block
const editBlockListener=(block,editBlockForm)=>{
 useAuthLazyFetchPut(`${props.url}/${block.uid}?name=${editBlockForm.name}&category=${editBlockForm.category}`, {
    body: {
      name:editBlockForm.name,
      category:editBlockForm.category,
      block_html:block.block_html,
      block_css:block.block_css,
      uid:block.uid
    },
  });
}

</script>