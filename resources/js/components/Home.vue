<template>
<div>
    <nav class="panel column is-offset-2 is-8">
        <p class="panel-heading">
            Vue Phonebook 
            <button @click="launchModel" class="button is-link is-outlined">
                Add New
            </button>
            
        </p>
        <div class="panel-block">
            <p class="control has-icons-left">
            <input class="input is-small" type="text" placeholder="search">
            <span class="icon is-small is-left">
                <i class="fa fa-search" aria-hidden="true"></i>
            </span>
            </p>
        </div>
        
        <a class="panel-block is-active" v-for="item,key in lists">
            <div class="column is-9">
               {{key}} - {{item.name}}
            </div>
            
            <span class="panel-icon column is-1">
                <i class="has-text-danger fa fa-trash" aria-hidden="true"></i>
            </span>
            <span class="panel-icon column is-1">
                <i class="has-text-info fa fa-edit" aria-hidden="true"></i>
            </span>
            <span class="panel-icon column is-1">
                <i class="has-text-primary fa fa-eye" @click="openShow(key)" aria-hidden="true"></i>
            </span>
            
        </a>
    </nav>

    <Add :openModel='modelClass' @closeRequest='closeModel'></Add>
    <Show :openModel='showActive' @closeRequest='closeModel'></Show>
</div>
</template>

<script>
let Add = require('./Add.vue');
let Show = require('./Show.vue');
export default {
    components:{
        Add,Show
    },
    data(){
        return{
            modelClass : '',
            showActive : '',
            lists:{},
            errors:{}
        }
    },
    mounted(){
        axios.post('/getData')
            .then((response) => this.lists = response.data)
            .catch((error) => this.errors = error.response.data.errors);
    },
    methods:{
        launchModel(){
            console.log('model');
            this.modelClass = 'is-active';
        },
        closeModel(){
            this.modelClass = this.showActive = '';
        },
        openShow(key){
            this.$children[1].list = this.lists[key];
            this.showActive = 'is-active';
        }
    }
}
</script>
