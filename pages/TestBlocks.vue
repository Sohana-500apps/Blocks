<template>
    <!-- start of Blog tag -->
  <CollectionBlocksMain :BlogUrl="BlogUrl" :url="url"/>
    <!-- end of Blog tag -->
</template>
<script setup lang="ts">
//importing the BlogUrl url and url
import {BlogUrl,url}  from "../js/data"
</script>