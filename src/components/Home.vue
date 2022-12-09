<template>
    <div>
        <img class="center" src="../assets/droid.jpeg" alt="droid"></img>
        <h1>Skull</h1>
        
        <div class="btn">
        <button v-on:click=gotoskull() type="button" class="btn btn-primary" >
            Skull 💀
        </button>
        
        <button  v-on:click=gotoapp()  type="button" class="btn btn-secondary" >
            Food 🍔
        </button>
        </div>
        <p>{{ msg }}</p>
    </div>
</template>
 
<script>
import axios from 'axios'
export default
{
    name: 'Skull',
    data()
    {
        return {
            msg: 'Welcome to this oddly primitive webapp!'
        }
    },
    methods: 
    {
        gotoskull(){
            this.$router.push('/skull')
        },

        gotoapp(){
            this.$router.push('/recipes/')
        }

    }

}
</script>
<style scoped>
h1 {
padding:10%;
  text-align: center;
}
.center {
  displaY: block;
  margin-left: auto;
  margin-right: auto;
  width: 10%;
}
p{
    padding:10%;
    text-align: center;
}

div.btn{
    padding: 20%;

  position: absolute;

  top: 50%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}
.btn-primary{
    margin: 20%;
}
</style>