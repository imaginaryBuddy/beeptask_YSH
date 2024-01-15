<script setup>
  import TaskView from './views/TaskView.vue';
  import AutocompleteAsync from './components/AutocompleteAsync.vue';
  import Autocomplete from './components/Autocomplete.vue';
  import ToggleButton from './components/ToggleButton.vue';
  import {ref, watch} from 'vue';


  let asyncVersion = ref(false);
  let noInput = ref(false);

  watch(asyncVersion, () => {});
  watch(noInput, () => {});

  const handleClick = () => {
    asyncVersion.value = !asyncVersion.value;
    noInput.value = false;
  }

  const handleDisableInput = () => {
    noInput.value = !noInput.value; 
  }

</script>

<template>
  <div class="grid grid-cols-1 items-center justify-items-center min-h-screen bg-gradient-to-r from-cyan-500 to-blue-500 min-w-screen">
  <!-- <div class="grid h-10 w-40 bg-blue-200 rounded mb-3">
      <button class="flex items-center justify-center h-full w-full" @click="handleClick">sync/async</button>
  </div> -->
  <div class="grid-rows-2 w-full">
    <div class="grid grid-cols-1 justify-items-center justify-center">
      <ToggleButton parentFunctionName="onClick" buttonLabel="sync/async" @onClick="handleClick"/>
    </div>
  
    <div class="grid col-span-1 w-full justify-items-center justify-center">
      <AutocompleteAsync disableInput={{noInput.value}} v-if="asyncVersion"/>
      <Autocomplete disableInput={{noInput.value}} v-else/>
    </div>
  </div>
 
</div>

  

</template>

<style scoped>

</style>
