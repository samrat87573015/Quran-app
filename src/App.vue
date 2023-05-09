

<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container mx-auto"> 
      <div class="wrapper bg-white shadow rounded">
        <div class="sm:flex block py-5 px-8 items-center">
          <div class="flex-1 mb-3 sm:mb-0 text-center">
            <select @change="getSpecificSura" class="border border-gray-200 py-2 px-4" name="" id="">
              <option  value="">select sura</option>
              <option :key="sura" v-for="sura in suras"  :value="sura.number">{{ sura.name}} - {{ sura.englishName }} </option>
              
            </select>
          </div>
          <div class="flex-1 text-center">
            <div>
              <h2 class="text-3xl font-bold mb-1">{{ currentSura.name }}</h2>
              <span>{{ currentSura.englishName }} - Ayahs: {{ currentSura.numberOfAyahs }}</span>
            </div>
          </div>
        </div>
      </div>
      <div class="bg-white mt-7 shadow-md rounded p-8">
          <div>
            <div v-if="loading" class="flex items-center w-full min-h-screen justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 animate-spin">
                <path stroke-linecap="round" stroke-linejoin="round" d="M16.712 4.33a9.027 9.027 0 011.652 1.306c.51.51.944 1.064 1.306 1.652M16.712 4.33l-3.448 4.138m3.448-4.138a9.014 9.014 0 00-9.424 0M19.67 7.288l-4.138 3.448m4.138-3.448a9.014 9.014 0 010 9.424m-4.138-5.976a3.736 3.736 0 00-.88-1.388 3.737 3.737 0 00-1.388-.88m2.268 2.268a3.765 3.765 0 010 2.528m-2.268-4.796a3.765 3.765 0 00-2.528 0m4.796 4.796c-.181.506-.475.982-.88 1.388a3.736 3.736 0 01-1.388.88m2.268-2.268l4.138 3.448m0 0a9.027 9.027 0 01-1.306 1.652c-.51.51-1.064.944-1.652 1.306m0 0l-3.448-4.138m3.448 4.138a9.014 9.014 0 01-9.424 0m5.976-4.138a3.765 3.765 0 01-2.528 0m0 0a3.736 3.736 0 01-1.388-.88 3.737 3.737 0 01-.88-1.388m2.268 2.268L7.288 19.67m0 0a9.024 9.024 0 01-1.652-1.306 9.027 9.027 0 01-1.306-1.652m0 0l4.138-3.448M4.33 16.712a9.014 9.014 0 010-9.424m4.138 5.976a3.765 3.765 0 010-2.528m0 0c.181-.506.475-.982.88-1.388a3.736 3.736 0 011.388-.88m-2.268 2.268L4.33 7.288m6.406 1.18L7.288 4.33m0 0a9.024 9.024 0 00-1.652 1.306A9.025 9.025 0 004.33 7.288" />
              </svg><span class="ml-1">Loading....</span>              
            </div>
            <div v-else >
              <ul v-if="currentSura.hasOwnProperty('ayahs')">
                <li class="mb-5" :key="ayah" v-for="ayah in currentSura.ayahs"><span class="ayah_number"> {{ ayah.numberInSurah }} </span> <span class="md:text-4xl sm:text-3xl text-2xl ml-3">{{ ayah.text }}</span></li>
              </ul>
            </div>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
    import axios from 'axios'
    export default{
      name: 'App',
      data() {
        return {
          suras: [],
          currentSura: [],
          loading: true
        }
      },
      mounted() {
        axios.get('https://api.alquran.cloud/v1/surah')
        .then(response =>{
          this.suras = response.data.data
        })

        this.querySpecificSura(1);

      },
      methods: {
        getSpecificSura(e){
          this.querySpecificSura(e.target.value);
        },
        querySpecificSura(suraNumber){
          axios.get('https://api.alquran.cloud/v1/surah/' + suraNumber)
          .then(response =>{
            this.loading = true;
            this.currentSura = response.data.data;
            this.loading = false;
          })
        }
      },
    }
</script>



