<template>
  <h1>{{ message }}</h1>
  <HelloWorld />
</template>

<script>
import HelloWorld from '~/components/HelloWorld'

export default {
    components: {        
        HelloWorld
    },
    //data : function () {
    data() {
        return {
            message : 'Hello Vue!!'
        }
    }
}
</script>
