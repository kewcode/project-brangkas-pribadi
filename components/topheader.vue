<template>
    <div >
           <!-- Header -->
            <nav class="w-full p-3 pt-4 lg:px-40 flex justify-between flex-wrap fixed bg-theme_primary_light" >
                
                

                    <nuxt-link to="/home" class="flex text-primary text-md items-start" >
                        <span class=" font-bold text-xl"> 
                            Berangkas Pribadi
                        </span> 
                    </nuxt-link>

                    <div @click="$store.commit('toggleSetting')" class="text-primary cursor-pointer ml-auto relative flex">
                       User
                    </div>
            </nav>
    </div>
</template>