<template>  
    <div id="filme">

        <div v-if="loading">
           <Loading/> 
        </div>

        <div class="container" v-else>
            <h2>{{filme.nome}}</h2>
            <img :src="filme.foto"/>
            <h3>Sinopse</h3>
            <p>{{filme.sinopse}}</p>

            <div class="botaos">
                <router-link tag="button" to="/">Ver mais filmes</router-link>
                <button @click="salvarFilme">Salvar</button>
                <button>
                    <a :href="`https://youtube.com/results?search_query=${filme.nome}`" target="blank">
                        Trailer
                    </a>
                </button>
            </div>
        </div>
    </div>
</template>

<script>

import api from '../service/api';
import Loading from '../components/Loading.vue';
export default {
    props:['id'],
    components:{
        Loading,
    },
    data(){
        return{
            filme:{},
            loading: true,
        }
    },
    async created(){
        const response = await api.get(`?api=filmes/${this.id}`);
        this.filme = response.data;
        this.loading = false;
    },
    methods:{
        salvarFilme(){
            const minhaLista = localStorage.getItem('myFilme');

            let filmes = JSON.parse(minhaLista) || [];

            //Se tiver um filme com id igaul ao que queremos adicionar devolve true
            const hasFilme = filmes.some( (filme) => filme.id === this.filme.id );
            if(hasFilme){
                return;
                //parar o código aqui
            }

            filmes.push(this.filme);
            localStorage.setItem('myFilme', JSON.stringify(filmes));
            alert('Filme salvo')
        }
    }
}
</script>

<style scoped>
    .container{
        display: flex;
        justify-content: center;
        flex-direction: column;
        max-width: 900px;
    }

    h2{
        color: #fff;
        margin-bottom: 0px;
        background: brown;
        padding: 15px;
        border-top-left-radius: 20px;
        border-top-right-radius: 20px;
    }

    .container img{
        width: 100%;
        border-top-left-radius: 20px;
        border-top-right-radius: 20px;
    }

    button{
        margin-right: 5px;
        border-radius: 5px;
        margin-top: 15px;
        cursor: pointer;
        border: 0;
        padding: 12px;
        font-size: 20px;
        transition: all 0.5s;
        outline: none;
    }

    button:hover{
        background: brown;
        color: #fff;
    }

    a{
        text-decoration: none;
        color: #000;
        transition: all 0.5s;
    }

    a:hover{
        color: #fff;
    }
</style>