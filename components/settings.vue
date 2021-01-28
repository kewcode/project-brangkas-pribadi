<template>
    <div v-if="$store.state.settings" class="font-bold  text-center w-full h-screen fixed top-0 right-0 p-3 rounded-lg flex justify-center items-center flex-wrap" style="overflow:scroll">
    
    <div @click="$store.commit('toggleSetting')" class=" opacity-75 w-full h-screen bg-theme_primary_light absolute top-0 right-0  z-10"></div>
    
    <div class="z-20 w-full bg-theme_primary p-5 shadow-lg rounded-xl neu-out text-left" style="max-width:600px">
         
        <h1 class="w-full text-2xl font-bold text-center">
            User
        </h1>
        
        <div>
            <label>Nama </label>
            <input type="text" v-model="d.nama" class="rounded-lg bg-theme_primary_dark p-2 px-3 w-full">

        </div>
        <div>
            <label>Password  (Default Key)</label>
            <input type="text" v-model="d.password" class="rounded-lg bg-theme_primary_dark p-2 px-3 w-full">
        </div>

        <div>
            <button @click="simpan"  class="rounded-lg bg-primary p-2 px-3 w-full mt-3 text-white">Simpan</button>
        </div>

    </div>

    </div>
</template>
<script>
export default {
    data(){
       return{
           d: (JSON.parse(localStorage.getItem('_user'))) || {
               nama : '',
               password: ''
           }
       }
    },
    methods:{
        simpan(){
            localStorage.setItem('_user',JSON.stringify(this.d))
            this.$store.commit('toggleSetting')
        }
    }
}
</script>