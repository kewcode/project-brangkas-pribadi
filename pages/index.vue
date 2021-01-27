<template>
  <div class="w-full pt-20 bg-theme_primary_light flex flex-wrap p-4 lg:px-40">
  
    <div class="w-full lg:w-1/2 p-4 shadow-lg rounded-xl mt-3">
      <h1 class="w-full text-lg font-bold" @click="formTambah = !formTambah">
        Tambah Data Pribadi
      </h1>
      <div :class="(formTambah) ? 'block' : 'hidden lg:block'">
        <div>
            <label class="flex w-full">Title</label>
            <input type="text" v-model="d.title" class="my-2 rounded-lg bg-theme_primary_dark p-2 px-3 w-full">
        </div>
        <div>
            <label>Data </label>
          <textarea v-model="d.data" class="my-2 rounded-lg bg-theme_primary_dark p-2 px-3 w-full"></textarea>
        </div>
        <div>
            <label class="flex w-full">Key
              <button class="ml-auto ">default (password user)</button>
              </label>
            <input type="text" v-model="d.key" class="my-2 rounded-lg bg-theme_primary_dark p-2 px-3 w-full">
        </div>

        <div>
            <button @click="tambah"  class="rounded-lg bg-primary p-2 px-3 w-full mt-3 text-white">Simpan</button>
        </div>
      </div>
      

    </div>

  <ul class="w-full lg:w-1/2 p-4 shadow-lg rounded-xl overflow-scroll mt-3" style="max-height:60vh">
    <h1 class="w-full text-lg font-bold">
      Data Pribadi Saya
    </h1>
    <li v-for="(b,index) in dataEncrypt" :key="index" class="shadow-sm py-2 flex flex-wrap">
      <h1 class="p-2">
        {{ index+1}}. 
        {{ b.title}}</h1>
      <p class="p-2 w-full mb-2">{{ b.encrypt}} </p>
      <button class="rounded-xl w-1/2 bg-primary p-1 px-5 text-sm text-white" @click="showEncrypt = b.encrypt;lihat()">Lihat</button>
      <button class="rounded-xl w-1/2 bg-danger p-1 px-5 text-sm text-white" @click="hapus(index)">hapus</button>
    </li>
  </ul>
  <div class="w-full bg-theme_primary p-4 text-center shadow-lg rounded-xl absolute bottom-0 right-0 left-0 border" v-if="show">
    <h1 class="w-full text-lg font-bold">
     Detail
    </h1>
    <p class="p-2">{{ show }}</p>
    <button class="rounded-xl w-full bg-primary p-1 px-5 text-sm text-white" @click="show = ''">Tutup</button>
  </div>




<div v-if="modalKey" class="font-bold bg-theme_primary text-center w-full h-screen fixed top-0 right-0 p-3 rounded-lg flex justify-center items-center flex-wrap" style="overflow:scroll">
    
    <div @click="modalKey = false" class=" opacity-75 w-full h-screen bg-theme_primary_light absolute top-0 right-0  z-10"></div>
    
    <div class="z-20 w-full bg-theme_primary p-5 shadow-lg rounded-xl neu-out text-left" style="max-width:600px">
         
        <h1 class="w-full text-2xl font-bold text-center">
           Masukan Key
        </h1>
      
        <div>
            <label>Key</label>
            <input type="text" v-model="key" class="rounded-lg bg-theme_primary_dark p-2 px-3 w-full">
        </div>

        <div>
            <button @click="modalKey = false;lihat('with-key')"  class="rounded-lg bg-primary p-2 px-3 w-full mt-3 text-white">Lihat</button>
        </div>

    </div>

    </div>
    
  </div>
</template>

<script>
export default {
  scrollToTop: true,
  layout: 'app',
  data(){
    return {
      formTambah: false,
      d:{
        title: '',
        data : '',
        key: ''
      },
      dataEncrypt : (JSON.parse(localStorage.getItem('_dataEncrypt'))) || [],
      modalKey: '',
      showEncrypt: '',
      key: '',
      show: ''
    }
  },
  created(){
    if(!JSON.parse(localStorage.getItem('_user'))){
      this.$store.commit('toggleSetting')
    }
  },
  methods:{
    tambah(){
      if(this.d.data){
        if(!this.d.key){
          this.d.key = JSON.parse(localStorage.getItem('_user')).password
        }
        let str = this.d.data
        const encryptedText = this.CryptoJS.AES.encrypt(JSON.stringify(str), this.d.key).toString()
        this.dataEncrypt.push({
          encrypt: encryptedText,
          title: this.d.title
        })
        localStorage.setItem('_dataEncrypt',JSON.stringify(this.dataEncrypt))
        this.d ={
          title: '',
          data : '',
          key: ''
        }
        this.formTambah = false
      }
    },
    lihat(withKey){
      let key = ''
      if(withKey){
       key = this.key
      }else{
       key = JSON.parse(localStorage.getItem('_user')).password
      }
       const decryptedText = this.CryptoJS.AES.decrypt(this.showEncrypt.toString(),key).toString(this.CryptoJS.enc.Utf8)
       if(!decryptedText){
         this.modalKey = true
       }
       this.show = decryptedText;
       this.key = ''
    },
    hapus(index){
      this.dataEncrypt.splice(index,1)
      localStorage.setItem('_dataEncrypt',JSON.stringify(this.dataEncrypt))
    }
  },
}
</script>


    