<template>
    <div class="col-span-1 h-10 w-20 bg-blue-200 rounded mb-3">
      <button class="flex items-center justify-center h-full w-full text-xs" @click="$emit(parentFunctionName)">{{ buttonLabel }}</button>
    </div>
</template>

<script>
  export default {

    props:{
      parentFunctionName: String,
      buttonLabel: String,
    }
  }
</script>