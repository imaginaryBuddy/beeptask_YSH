<script>
import Autocomplete from '../components/Autocomplete.vue';
import AutocompleteAsync from '../components/AutocompleteAsync.vue';
import ToggleButton from '../components/ToggleButton.vue';

// export default{
//     data(){
//         return{
//             asyncVersion: false,
//         }
//     },

// }
</script>

<template>
  
    <!-- <ToggleButton @click="asyncVersion = !asyncVersion"/> -->
    <!-- <autocomplete/> -->
    <!-- <div class="relative h-10 w-20 bg-blue-200 rounded bottom-7">
        <button @click="this.asyncVersion = !this.asyncVersion" class="flex items-center justify-center h-full w-full">toggle</button>
    </div>
    <AutocompleteAsync v-if="this.asyncVersion"/>
    <Autocomplete v-else/> -->

    <AutocompleteAsync/>

</template>

<style scoped>

</style>
