<template>
  <v-container>
    <v-row class="text-center">
      <v-col v-if="data" cols="12">
        <h3 class="display-2 font-weight-bold mb-3">
          {{data.title}}
        </h3>

        <p class="subheading font-weight-regular">
          <b>Fecha Publicación:</b> {{data.year}}-{{data.month}}-{{data.day}}
        </p>
      </v-col>

      <v-col class="mb-5" cols="12">
        <v-row justify="center">
          <v-tooltip bottom><template v-slot:activator="{ on, attrs }">
            <v-btn @click="getSpecific(1)" :disabled="disable.back" v-bind="attrs" v-on="on"><v-icon>mdi-chevron-double-left</v-icon></v-btn>
          </template><span>Ir al primero</span></v-tooltip>
          <v-tooltip bottom><template v-slot:activator="{ on, attrs }">
            <v-btn @click="getBackNext('back')" :disabled="disable.back" v-bind="attrs" v-on="on">Ant.</v-btn>
          </template><span>Anterior</span></v-tooltip>
          <v-tooltip bottom><template v-slot:activator="{ on, attrs }">
            <v-btn @click="getRandom" color="primary" v-bind="attrs" v-on="on">Random</v-btn>
          </template><span>Buscar aleatorio</span></v-tooltip>
          <v-tooltip bottom><template v-slot:activator="{ on, attrs }">
            <v-btn @click="getBackNext('next')" :disabled="disable.next" v-bind="attrs" v-on="on">Sig.</v-btn>
          </template><span>Siguiente</span></v-tooltip>
          <v-tooltip bottom><template v-slot:activator="{ on, attrs }">
            <v-btn @click="getLastest" :disabled="disable.next" v-bind="attrs" v-on="on"><v-icon>mdi-chevron-double-right</v-icon></v-btn>
          </template><span>Ir al último</span></v-tooltip>
        </v-row>
      </v-col>

      <v-col class="mb-4" cols="12">
        <v-row justify="center">
          <v-img
            :src="data.img"
            class="my-3"
            contain
            max-height="800"
            max-width="700"
            aspect-ratio="1.4"
            @click="openImg"
          />
        </v-row>
      </v-col>

      <v-col class="mb-4" cols="12">
        <v-row justify="center">
          <p class="subheading font-weight-regular">
          Califica este comic:
          </p>
        </v-row>
        <v-row justify="center">
          <v-rating
          v-model="rating"
          color="yellow darken-3"
          background-color="grey darken-1"
          empty-icon="$ratingFull"
          hover
          large
          ></v-rating>
        </v-row>
      </v-col>
      
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'consult-comic',

    data() {
      return{
        rating: 4,
        idlastest: 1,
        idActual: 1,
        data: {},
        disable: {
          back: false,
          next: true
        },
      }
    },
    watch: {
      idActual: function (val) {
        if(val <= 1){
          this.disable.back = true;
          this.disable.next = false;
        }
        else if(val == this.idlastest){
          this.disable.back = false;
          this.disable.next = true;
        }else{
          this.disable.back = false;
          this.disable.next = false;
        }
      },
    },
    methods:{
      getLastest(){
        this.axios.get('https://xkcd.now.sh/?comic=latest')
        .then(result =>{
          console.log("Lastest: ", result.data);
          this.idlastest = result.data.num;
          this.idActual = result.data.num;
          this.data = result.data;
        })
      },
      getSpecific(idComic){
        this.axios.get('https://xkcd.now.sh/?comic=' + idComic)
        .then(result =>{
          console.log("result "+ idComic, result.data);
          this.idActual = result.data.num;
          this.data = result.data;          
        })
      },
      getBackNext(option){
        switch(option){
          case "back":
            if(!this.disable.back){
              this.getSpecific(this.idActual - 1);
            }
            break;
          case "next":
            if(!this.disable.next){
              this.getSpecific(parseInt(this.idActual) + 1);
            }
            break;
        }
      },
      getRandom(){
        this.getSpecific(this.getRndInteger(1,this.idlastest));
      },
      openImg(){
        window.open(this.data.img);
      },
      getRndInteger(min, max) {
        return Math.floor(Math.random() * (max - min + 1) ) + min;
      }
    },
    mounted() {            
      this.getLastest();
    },
  }
</script>
