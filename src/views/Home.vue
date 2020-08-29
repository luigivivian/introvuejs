<template>
  <div class="home">

    <div v-if="this.mensagem != null">
      <b-alert show variant="danger" v-if="this.mensagem.type == 'error'">{{this.mensagem.msg}}</b-alert>
      <b-alert show variant="danger" v-if="this.testepayload != null">{{this.testepayload}}</b-alert>
      <b-alert show variant="success" v-if="this.mensagem.type == 'success'">{{this.mensagem.msg}}</b-alert>
    </div>

    <b-jumbotron header="Introduçao vue js">
      <div class="row">
        <div class="col col-md-4 offset-3">
          <b-form-input
                  id="input-1"
                  v-model="username"
                  type="text"
                  required
                  placeholder="Digite alguma coisa"
          ></b-form-input>
        </div>
        <div class="col col-md-2">
          <button class="btn btn-primary btn-block" @click="pesquisarUsuario()">Pesquisar</button>
        </div>
      </div>
    </b-jumbotron>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
// import UserDetail from '@/components/UserDetail.vue';

export default {
  name: 'Home',
  components: {
    // HelloWorld
    //UserDetail
  },
  props: ["mensagem", "testepayload"],
  mounted() {

  },
  data: function() {
    return {
      username: null
    }
  },
  methods:{
    getUser(){
      // 'https://api.github.com/users/luigivivian'

    },
    pesquisarUsuario(){
      var vm = this;

      this.axios.get(`https://api.github.com/users/${this.username}`).then(function (response) {
        if(response.data){
          console.log(response);
          vm.$router.push({name: 'user-detail', params:{ usuario: response.data} });
        }
      }).catch(function (error) {
        vm.username = null;
        console.error(error);
        alert('Usuario não encontrado');
      });
    }


  }
}
</script>
