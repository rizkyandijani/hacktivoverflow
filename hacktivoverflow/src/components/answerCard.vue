<template>

<v-card v-show="show">
    <v-container fill-height>
        <v-layout row wrap align-center>
            <v-flex xs12 sm1>
                <div>
                    <v-btn @click.prevent="upvote(answer._id)" icon flat class="green--text mx-1 pa-1">
                    <v-icon>thumb_up</v-icon>
                    <span class="total">{{answer.upVotes.length}}</span>
                    </v-btn> 
                </div>
                <div>
                    <v-btn @click.prevent="downvote(answer._id)" icon class="red--text mx-1 pa-1">
                    <v-icon>thumb_down</v-icon>
                    <span class="total">{{answer.downVotes.length}}</span>
                    </v-btn>
                </div>
            </v-flex>
            <v-flex xs12 sm10>

            <h1>
                {{answer.description}}
            </h1>
            </v-flex>
            <!-- <h1>card content</h1>  -->
            <v-card-actions>
                <v-btn flat color="orange" @click.prevent="editAnswer(answer._id,answer.userId)">Edit</v-btn>
                <v-btn flat color="orange" @click.prevent="deleteAnswer(answer._id,answer.userId)">Delete</v-btn>
            </v-card-actions>
        </v-layout>
    </v-container>
</v-card>
</template>

<script>
import axios from 'axios'

export default {
    props: ['answer','question'],
    data(){
        return{
            show : true
        }
    },
    methods : {
        reroute(){
            console.log('ke clickkkkkkkk');
        },
        editAnswer(id,userId){
            console.log('edit trigered',id,userId);
            
            if(userId === localStorage.userId){
                console.log('ini id nya answer',id);
                this.$store.dispatch('getOneAnswer',id)
                this.$router.push(`/editAnswer/${this.question._id}/${id}`)
            }else{
                console.log(`tidak berhak mengedit`);
                this.$swal('you are not authorized to edit this content', '', 'error')
            }
        },
        upvote(id){
            let data = {
                answerId : id,
                questionId : this.question._id
            }
            console.log('waduh trigerred');
            console.log('ini id',id);
            this.$store.dispatch('upvoteAnswer',data)
        },
        downvote(id){
            let data = {
                answerId : id,
                questionId : this.question._id
            }
            console.log('kok trigerred');
            console.log('ini id',id);      
            this.$store.dispatch('downvoteAnswer',data)
        },
        deleteAnswer(id,userId){
            if(userId === localStorage.userId){
                let data = {
                    questionId : this.question._id,
                    answerId : id
                }
                this.show = false
                this.$store.dispatch('deleteAnswer',data)
            }else{
                this.$swal(`youre not authorized!`,'','error')
            }
        }
    },
    created(){
        //  this.$store.dispatch('getQuestionAnswer')        
        // console.log('ini answerrrrrrr',this.answer);
        
    }
}
</script>

<style>
.clickable{
    cursor : pointer
}
</style>

