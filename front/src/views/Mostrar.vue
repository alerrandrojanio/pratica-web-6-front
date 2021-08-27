<template>
 <div class="campos">

          <h4>Mostrar Posts</h4>
            <button @click="mostarPosts()">Posts</button><br /><br />
                <ul>
                    <li v-for="post in posts " :key="post.id">
                        
                        <div class="infor">
                        <p>Id: {{ post.id }}</p>
                        <p>Corpo: {{ post.corpo }}</p>
                        <p>Data: {{ post.data }}</p>
                        <p>Hora: {{ post.hora }}</p>
                        </div>
                        <br/>
                        
                        <div class="campos">
                            <h6>Comentar</h6>
                            <p> Comentario: <input type="text" v-model="comentario" /><br /></p> 
                            <p> Data: <input type="text" v-model="data" /><br /></p>
                            <p> Hora: <input type="text" v-model="hora" /><br /></p>
                            
                            <div class="butaos">
                                <button @click="post(post.id)">Publicar</button><br /><br />
                            </div>
                        </div>        
                    </li>
                </ul>

        </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            id: 0,
            comentario: "",
            data: "",
            hora: "",
            id_post: "",
            posts: [],
            baseURIPosts:"http://localhost:3000/posts",
            baseURIFeedbacks:"http://localhost:3000/feedbacks"
        }
    },

    methods: {
        mostarPosts(){
            axios.all([
                axios.get(baseURIPosts),
                axios.get(baseURIFeedbacks),
            ]).then(axios.spread((postsRes, feedbacksRes)=>{
                this.posts = postsRes.data
                this.feedbacks = feedbacksRes.data
            }))
        },

        post(id){
            let feedback ={
                corpo: this.corpo,
                data: this.data,
                hora: this.hora,
                id_post: id
            };

            axios.post(this.baseURIFeedbacks, feedback).then((resul) =>{
                this.posts = resul.data
            })
        },
    },
}
</script>

<style>
.Campos {
    margin: auto;
    width: 80%;
        
}
button{
    background-color: rgb(23, 91, 192);
    color: white;
    margin: 0 20px;
    height: 30px;
    cursor: pointer;
}
.infor {
    display: flex;
   
}
.infor p {
    margin: 10px 10px;
    
    
}
</style>