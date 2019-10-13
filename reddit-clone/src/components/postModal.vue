<template>
        <div>
           <div v-if="isOpen">
               <h1>Modal heading</h1>
               <p>This my first modal using vue.js</p>
           </div>
        </div>
     </template>
     
     
     <script>
        export default {
            name: 'postModal',
            data:function(){
               return {
                    isOpen: false,
                };
            },
            mounted() {
            this.$root.$on('eventing', data => {
                console.log(data);
                });
            }
}
     </script>
    
      <style scoped lang="scss">
      </style>