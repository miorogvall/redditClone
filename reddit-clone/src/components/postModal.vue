<template>
        <div>
           <div>
               <h2>{{dataRecieved}}</h2>
            <h1></h1>
           </div>
        </div>
     </template>
     
     
     <script>
        export default {
            name: 'postModal',
            props: {
                dataRecieved: Object
            },
            created () {
                console.log('hey')
                console.log(this.dataRecieved)
      }
}
     </script>
    
      <style scoped lang="scss">
      </style>