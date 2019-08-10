<template>
  <v-chip 
                  :color="location == 'HV'?'#2196F3':'#9C27B0'"
                  text-color="white" 
                >{{location}}</v-chip>
</template>

<script>
    export default {
        props:['location'],
      
    }
</script>

<style scoped>

</style>