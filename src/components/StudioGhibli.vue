<template>
    <div>
        <button @click="agregarDatos" class="ingresarDatos">Ingresar Datos</button>
        <div v-if="user.name!=''">
            <section class="box">
                <h1>Bienvenid@ {{fullname}}</h1>
                <p>Aquí podrás encontrar información de películas y animé de Studios Ghibli</p>
                <button class="limpiarDatos">Limpiar información</button>
            </section>
            
            <div class="alertPelis"> 
                <p>A continuación hay un listado con los nombres de las películas</p>
            </div>
            
            <div>
                <select v-model="selectFilm" @change="getFilm" class="inputPelis">
                    <option v-for="(film,index) in films" :key="index" :value="film">{{film}}</option>
                </select>
                
                <div v-if="film.title!=''" class="container">
                    <div class="imgBx"><img :src="film.poster" alt="">
                        <section class="content">
                            <h1>{{film.title}}</h1>
                            <h4>Descripción</h4>
                            <p>{{film.synopsis}}</p>
                            <h4>Director</h4>
                            <p>{{film.director}}</p>
                            <h4>Duración</h4>
                            <p>{{film.runtimeMinutes}} hrs</p>
                        </section>
                        
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'StudioGhibli-comp',
    // props: {},
    data: function(){
        return {
            user:{
                name:'',
                lastName:'',
            },
            films:[],
            selectFilm:'',
            film:{
                title:'',
                poster:[],
                synopsis:'',
                director:'',
                runtimeMinutes:''
            }
        }
    },
    computed: {
        
        fullname(){
            return this.user.name+' '+this.user.lastName
        }
    },
    methods: {
        async fetchFilm(){
            try{
                let data = await fetch(`https://studio-ghibli-films-api.herokuapp.com/api`)
                let json = await data.json()
                
                for(const key in json){
                    this.films.push(key)
                }
            }catch(error){
                console.log(error);
            }
        },
        agregarDatos(){
            this.user.name = prompt('Ingrese su nombre')
            this.user.lastName = prompt('Ingrese su apellido')
        },
        async getFilm(){
            try{
                let data = await fetch(`https://studio-ghibli-films-api.herokuapp.com/api/${this.selectFilm}`)
                let json = await data.json()
                    this.film = json
                
            }catch(error){
                console.log(error);
            }
        }
    },
    // watch: {},
    // components: {},
    // mixins: [],
    // filters: {},
    // -- Lifecycle Methods
/*     mounted(){
        alert('comp creado')
    } */
    created(){
        this.fetchFilm()
        //alert('comp creado')
    }
    // -- End Lifecycle Methods
}
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css?family=Poppins:400,500,600,800&display=swap');

body {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    font-family: 'Poppins', sans-serif;
    background: #000;
    
}
img{
    width: 400px;
}
section{
    padding: 20px;
    bottom: 20px;
}
.ingresarDatos{
    background-color:#198754;
    color:#fff;
    border: 1px solid;
    border-radius: 5px;
    height: 35px;
}
.limpiarDatos{
    background-color: #ffc107;
    color: #000;
    border:none;
    border-radius: 5px;
    height: 30px;
    width: 60%;
}

.alertPelis{
    background-color: #bee3ff;
    color: #000;
    border:none;
    margin: 0 auto;
    height: 50px;
    width: 80%;
    margin-top: 40px;
    padding-top: 5px;
    text-align: center;
    margin-bottom: 20px;
}
.inputPelis{
    border:none;
    width: 80%;
    box-shadow: 1px 1px 9px 5px #c1daed;
    /* -webkit-box-shadow: -2px 5px 20px #e7f3fe; */

}
.box{
    border:none;
    margin: 0 auto;
    margin-top: 15px;
    width: 60%;
    box-shadow: -2px 5px 20px #f9cbf9;

}
.container {
    position: relative;
    width: 20%;
    padding: 0;
    height: 600px;
    background: #fff;
    margin: 70px auto;

}

.container .imgBx {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin: 0 auto;
    box-shadow: -2px 5px 20px #f9cbf9;
    height: auto;
}

.container .details {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 50%;
    height: 100%;
    box-sizing: border-box;
    padding: 40px;
}

.container .details h2{
    margin: 0;
    padding: 0;
    font-size: 2.4em;
    line-height: 1em;
    color: #444;
}

.container .details h2 span {
    font-size: 0.4em;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: #999;
}

.container .details p {
    max-width: 85%;
    margin-left: 15%;
    color: #333;
    font-size: 15px;
    margin-bottom: 36px;
}

.container .details h3 {
    margin: 0;
    padding: 0;
    font-size: 2.5em;
    color: #a2a2a2;
    float: left;
}
.container .details button {
    background: #000;
    color: #fff;
    border: none;
    outline: none;
    padding: 15px 20px;
    margin-top: 5px;
    font-size: 16px;
    letter-spacing: 1px;
    text-transform: uppercase;
    font-weight: 600;
    border-radius: 40px;
    float: right;
}

.product-colors span {
    width: 26px;
    height: 26px;
    top: 7px;
    margin-right: 12px;
    left: 10px;
    border-radius: 50%;
    position: relative;
    cursor: pointer;
    display: inline-block;
}

.black {
    background: #000;
}

.red {
    background: #D5212E;
}

.orange {
    background: #F18557;
}

.product-colors .active:after{
    content: "";
    width: 36px;
    height: 36px;
    border: 2px solid #000;
    position: absolute;
    border-radius: 50%;
    box-sizing: border-box;
    left: -5px;
    top: -5px;
}
</style>