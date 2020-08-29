<template>
    <div class="userDetail">


        <b-container class="bv-example-row">
            <b-row>
                <b-col cols="3">
                    <b-card :img-src="this.usuario.avatar_url" img-alt="Card image" img-top>
                        <b-card-text>
                            {{this.usuario.login}}
                        </b-card-text>
                    </b-card>
                </b-col>
                <b-col cols="9">
                    <div>
                        <b-tabs content-class="mt-3">
                            <b-tab title="Perfil" active>
                                <h5>Nome:{{this.usuario.login}}</h5>
                                <h5>Repositorios publicos:{{this.usuario.public_repos}}</h5>
                                <h5>Entrou em:{{new Date(this.usuario.created_at).toLocaleDateString('pt-BR', {timeZone: 'UTC'})}}</h5>
                                <h5>Perfil: <a :href="this.usuario.html_url">{{this.usuario.login}}</a></h5>
                            </b-tab>
                            <b-tab title="Repositorios" @click="buscar('repos')">
                                <b-card v-for="(item) in this.repositorios_computed" :key="item.id" class="mt-4 text-left">
                                    <b-card-title>
                                      <a target="_blank" :href="item.html_url">{{item.name}}</a>
                                    </b-card-title>
                                    <b-card-text>
                                        {{item.description}}
                                    </b-card-text>
                                    <b-card-text>Last Update - {{item.updated_at}}</b-card-text>
                                    <b-card-text class="small text-muted">
                                        <b-badge v-if="item.language == 'HTML'" variant="danger">HTML</b-badge>
                                        <b-badge v-if="item.language == 'PHP'" variant="primary">PHP</b-badge>
                                        <b-badge v-if="item.language == 'JavaScript'" variant="warning">JavaScript</b-badge>
                                        <b-badge v-if="item.language == 'JAVA'" variant="danger">JAVA</b-badge>
                                        <b-badge v-if="item.language == 'CSS'" variant="warning">CSS</b-badge>
                                        <b-badge v-if="!langs.includes(item.language)" variant="info">{{item.language}}</b-badge>
                                    </b-card-text>
                                </b-card>
                            </b-tab>
                            <b-tab title="Starred" @click="buscar('starred')">
                                <b-card v-for="(item) in this.repositorios_starred_computed" :key="item.id" class="mt-4 text-left">
                                    <b-card-title>
                                        <a target="_blank" :href="item.html_url">{{item.name}}</a>
                                    </b-card-title>
                                    <b-card-text>
                                        {{item.description}}
                                    </b-card-text>
                                    <b-card-text>Last Update - {{item.updated_at}}</b-card-text>
                                    <b-card-text class="small text-muted">
                                        <b-badge v-if="item.language == 'HTML'" variant="danger">HTML</b-badge>
                                        <b-badge v-if="item.language == 'PHP'" variant="primary">PHP</b-badge>
                                        <b-badge v-if="item.language == 'JavaScript'" variant="warning">JavaScript</b-badge>
                                        <b-badge v-if="item.language == 'JAVA'" variant="danger">JAVA</b-badge>
                                        <b-badge v-if="item.language == 'CSS'" variant="warning">CSS</b-badge>
                                        <b-badge v-if="!langs.includes(item.language)" variant="info">{{item.language}}</b-badge>
                                    </b-card-text>
                                </b-card>
                            </b-tab>
                        </b-tabs>
                    </div>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    export default {
        name: 'UserDetail',
        props: ["usuario"],
        data:function(){
            return {
                endpoint:{
                    baseUrl: "https://api.github.com/users/",
                    respos: "/repos",
                    starred: "/starred",
                },
                repositorios: [],
                repositorios_starred: [],
                langs: ['HTML', 'JavaScript', 'CSS', 'JAVA', 'PHP'],
            }
        },
        computed:{
            repositorios_computed: function(){
                return this.repositorios;
            },
            repositorios_starred_computed: function(){
                return this.repositorios_starred;
            }
        },
        methods:{
            buscar(opt){
                var vm = this;
                if(opt == 'repos'){
                    // https://api.github.com/users/luigivivian/respos
                    this.axios.get(`${this.endpoint.baseUrl + this.usuario.login + this.endpoint.respos}`).then(function (response) {
                        vm.repositorios = response.data;
                    }).catch(function (error) {
                        console.error(error);
                    });
                }
                if(opt == 'starred'){
                    this.axios.get(`${this.endpoint.baseUrl + this.usuario.login + this.endpoint.starred}`).then(function (response) {
                        vm.repositorios_starred = response.data;
                    }).catch(function (error) {
                        console.error(error);
                    });
                }
            },
        },
        mounted() {
            if(this.usuario == null){
                var mensagem = {
                    type: 'error',
                    msg: 'Usuario não encontrado !'
                };
                var payload = {
                    mensagem: mensagem,
                    testepayload: "testando parametro rota"
                }
                this.$router.push({name: 'Home', params: payload});
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    a {
        color: #42b983;
    }
</style>
