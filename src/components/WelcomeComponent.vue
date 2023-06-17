<template>
    <h1>{{ txtmsg }}</h1>
    </template>
    
    <script>
    export default {
      name: 'WelcomeComponent',
      props: {
        txtmsg:{
            type:String,
            required: true,
        },
      },
    }
    </script>
    
    <!-- Add "scoped" attribute to limit CSS to this component only -->
    <style scoped>
    h1{
        color:black;
    }
    </style>
    