<template>
<v-container fill-height fluid  class="w-100 p-4 d-flex align-items-center justify-content-center"
    style="height: 550px;">
  <v-row class="d-flex justify-center mb-6" >
      <v-col
            cols="12"
            sm="6"
            md="3"
          >
            <v-col>
              <v-text-field
              label="Type or paste your URL here...."
              rounded
              filled
              class = "searchBar"
              background-color="black"
              v-model = "searchBar"
            ></v-text-field>
            <div class="d-flex justify-center mb-6">
              <v-btn
                  rounded
                  color="green"
                  dark
                  @click="searchURL(searchBar)"
                >
                  Search
                </v-btn>
            </div>
          </v-col> 
          </v-col>
  </v-row>
  <v-row v-if="responseCard">
    <v-col  class="d-flex justify-center mb-6">
      <v-card
      class="mx-auto"
      max-width="344"
    >
      <v-card-title>
        Response
      </v-card-title>
          <v-card-text>
            URL : {{this.response.URL}}
           </v-card-text>
           <v-card-text>
            Status : {{this.response.Status}}
           </v-card-text>
    </v-card>
    </v-col>
  </v-row>
  <v-row style = "display: none;">

  </v-row>
</v-container>

</template>

<script>
import axios from "axios"
  export default {
    name: 'SearchBar',

    data: () => ({
      searchBar : "",
      responseCard : false,
      response : {
          URL : this.data.URL,
          Status : this.data.Status,
      }
    }),

    mounted : async function () {
    this.instance =  axios.create({ baseURL: "http://127.0.0.1:5000" , headers:{'Content-type' : "application/json",}})
    },

    methods:{
      async searchURL(searchBar){
        console.log(searchBar , 'before axios')
        const URL = this.searchBar
        const data = await this.instance.get("/predict",{params: {URL : URL}})
        console.log('after axios' , URL);
        this.searchBar = ""
        this.responseCard = true
        return this.response = data
      }
    }
  }
</script>
