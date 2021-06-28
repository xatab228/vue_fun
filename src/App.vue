<template>
   <v-app>
    <v-container fluid>
      <v-app-bar color = "blue">
        <v-icon>mdi-vuetify</v-icon>
      </v-app-bar>
    </v-container>
    <v-main>
      <v-container >
        <v-btn outlined class="ml-2" @click="generated_names(first_name,second_name)">Создать данные для работы</v-btn>
        <v-btn outlined class="ml-2" @click="get_names()">Получить пользователей из API</v-btn>
      </v-container>
      <v-container>
        <v-label>Мои пользователи: </v-label>
        <!-- eslint-disable-next-line vue/no-confusing-v-for-v-if  -->
        <v-chip
         class="ma-2" 
         close
         @click="name_to_text_field(item.name,index,item._id)"
         @click:close="visible(index,item._id)" 
         v-for="(item,index) in full_name" 
         v-bind:key="item._id"
         v-bind:index="index" 
         v-bind:id = "index"
         >
         {{item.name}}
         </v-chip>
      </v-container>
      <v-container>
        <v-text-field outlined id="text_edit" v-model="name_for_change"></v-text-field>
        <v-btn outlined class="ml-2" @click="change_name()">Сохранить</v-btn>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios, axios)

export default {
  name: 'App',

  components: {
    
  },

  data: () => ({
    api_url : 'https://crudcrud.com/api/090d690756da4a3ba95575b89735b8c2/name',
    first_name : "Александр;Захар;Варвара;Кирилл;Тимофей;Вероника;Кира;Максим;Ярослав;Марк;Тихон;Макар;Матвей;Екатерина;Даниил;Ксения;Алексей;Артём;Юлия;Дарья;Мирон;Жасмин;Маргарита;Александра;Дмитрий;София;Агата;Софья;Егор;Злата",
    second_name : "Петровский;Фомин;Воробьева;Мальцев;Грачева;Степанов;Яковлев;Майоров;Власов;Соколов;Хомяков;Александрова;Кузьмина;Уткин;Плотников;Дмитриева;Григорьева;Лаврова;Некрасова;Митрофанов;Чернышев;Филиппова;Демидов;Назарова;Родионов;Смирнов;Пономарев;Орлов;Кузнецов;Чесноков",
    full_name: [],
    name_for_change: null,
    index_for_change: null,
    single_api_index: null
  }),
  methods: {
    generated_names(first_name,second_name){
      this.api_url += Math.floor(Math.random() * (1000 - 0) + 0)
      first_name = first_name.split(';');
      second_name = second_name.split(';');
      for (let i = 0;i<10;++i){
        let full_name = first_name[Math.floor(Math.random() * (first_name.length - 0) + 0)] + " " + second_name[Math.floor(Math.random() * (second_name.length - 0) + 0)] 
        axios.post(this.api_url,{name: full_name});
      }
      console.log(this.api_url)
    },
    get_names(){
      axios
        .get(this.api_url)
        .then(response => {
          this.full_name = response.data;
        });
    },
    visible(index,index_g){
      document.getElementById(index).style.display = "none"
      axios.delete(this.api_url + '/' + index_g)
    },
    name_to_text_field(name,index,index_g){
      this.name_for_change = name;
      this.index_for_change = index;
      this.single_api_index = index_g;
    },
    change_name(){
      let name = document.getElementById("text_edit").value
      this.full_name[this.index_for_change].name = name
      axios.put(this.api_url + '/' + this.single_api_index,{name: name})
    }
  }
};
</script>
