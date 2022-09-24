<script lang="ts">
  import { defineComponent } from 'vue';
  import axios from "axios"
  import {useI18n} from "vue-i18n";

export default  defineComponent({
  name : 'app',
    setup(){
      const {t, locale} = useI18n();
      return {t, locale};
    },
    data(){
      return{
        text:'',
        lang: 'en'
      }
    },
    async mounted(){
      await this.load();
    },
    methods : {
      async load(){
       // axios.defaults.headers.put['Accept-Language'] = this.lang;
        const  response = await axios.get('http://localhost:8000/text', {
          headers: {
            'Accept-Language':  this.lang
          }
        });
        this.text = response.data.text;

      },
      async change(lang: string){
        this.lang = lang;
        await this.load();
      },
      async click(){
        const  response = await axios.get('http://localhost:8000/like');
        //alert(response.data.text);
        const text = this.t(response.data.text, {}, {locale : this.lang});
        alert(text);
      }

    }
  });

</script>

<template>
  <main class="container">
    <div class="row py-3">
      <div class="col-10"></div>
      <div class="col-2">
        <select class="form-control" @change="change($event.target.value)">
          <option value="en">English</option>
          <option value="de">Deutsh</option>
          <option value="es">Español</option>
        </select>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <h2>{{ t('title', {}, {locale: lang}) }}</h2>
        <p>{{ text }}</p>
        <button class="btn btn-outline-primary" @click="click()">{{ t('like', {}, {locale: lang}) }}</button>
      </div>
     </div>
  </main>
</template>

