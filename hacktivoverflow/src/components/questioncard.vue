<template>

<v-card v-show="show" >
    <v-container fill-height>
        <v-layout row wrap align-center>
            <v-flex xs12 sm1>
                <div>
                    <v-btn @click.prevent="upvote(question._id)" icon flat class="green--text mx-1 pa-1">
                    <v-icon>thumb_up</v-icon>
                    <span class="total">{{question.upVotes.length}}</span>
                    </v-btn> 
                </div>
                <div>
                    <v-btn @click.prevent="downvote(question._id)" icon class="red--text mx-1 pa-1">
                    <v-icon>thumb_down</v-icon>
                    <span class="total">{{question.downVotes.length}}</span>
                    </v-btn>
                </div>
            </v-flex>
            <v-flex xs12 sm10>
            <h3>{{question.title}}</h3>
            <p>
                {{question.description}}
            </p>
            </v-flex>
            <!-- <h1>card content</h1>  -->
            <v-card-actions>
                <v-btn @click.prevent="answerQuestion(question._id)" flat color="orange">Answer</v-btn>
                <span>{{question.answerList.length}}</span>
                <v-btn flat color="orange" @click.prevent="editQuestion(question._id,question.userId)">Edit</v-btn>
                <v-btn flat color="orange" @click.prevent="deleteQuestion(question._id,question.userId)">Delete</v-btn>                
                <v-btn flat color="orange" @click.prevent="reroute(question._id)">Detail</v-btn>

            </v-card-actions>
        </v-layout>
    </v-container>
</v-card>
</template>

<script>
import axios from 'axios'

export default {
    props: ['question','index'],
    data(){
        return{
            show : true,
        }
    },
    methods : {
        reroute(id){
            console.log('ke clickkkkkkkk');
            this.$router.push(`/questionDetail/${id}`)
            this.$store.dispatch('getQuestionAnswer',id)
        },
        editQuestion(id,userId){
            if(userId === localStorage.userId){
                console.log('ini id nya question',id);
                this.$router.push(`/editQuestion/${id}`)
            }else{
                console.log(`tidak berhak mengedit`);
                this.$swal('you are not authorized to edit this content', '', 'error')
            }
        },
        deleteQuestion(id,userId){
            if(userId === localStorage.userId){
                this.show = false
                this.$store.dispatch('deleteQuestion',id)
            }else{
                this.$swal(`youre not authorized!`,'','error')
            }
        },
        upvote(id){
            console.log('ini id',id);
            
            this.$store.dispatch('upvoteQuestion',id)
        },
        downvote(id){
            console.log('ini id',id);
            
            this.$store.dispatch('downvoteQuestion',id)
        },
        answerQuestion(id){
            if(localStorage.token){
                this.$router.push(`/addAnswer/${id}`)
            }else{
                this.$swal('you must login first','','error')
            }
        }
    },
    created(){
            this.$store.dispatch('getQuestion') 
    }
}
</script>

<style>
.clickable{
    cursor : pointer
}
</style>

