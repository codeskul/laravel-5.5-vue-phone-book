<template>
    <div class="modal" :class='openModel'>
  <div class="modal-background"></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Update {{list.name}}'s details</p>
      <button class="delete" aria-label="close" @click="close"></button>
    </header>
    <section class="modal-card-body">
      <div class="field">
        <label class="label">Name</label>
        <div class="control">
          <input class="input" :class="{'is-danger':errors.name}" type="text" v-model="list.name" placeholder="Name">
        </div>
        <small v-if='errors.name' class="has-text-danger">{{errors.name[0]}}</small>
      </div>

      <div class="field">
        <label class="label">Phone</label>
        <div class="control">
          <input class="input" :class="{'is-danger':errors.phone}" type="number" v-model="list.phone" placeholder="Phone">
        </div>
        <small v-if='errors.phone' class="has-text-danger">{{errors.phone[0]}}</small>
      </div>

      <div class="field">
        <label class="label">Email</label>
        <div class="control">
          <input class="input" :class="{'is-danger':errors.email}" type="email" v-model="list.email" placeholder="Email">
        </div>
        <small v-if='errors.email' class="has-text-danger">{{errors.email[0]}}</small>
      </div>
    </section>
    <footer class="modal-card-foot">
      <button class="button is-success" @click="update">Save changes</button>
      <button class="button" @click="close">Cancel</button>
    </footer>
  </div>
</div>
</template>

<script>
export default {
  props:[
    'openModel'
  ],
  data(){
    return{
      list:{},
      errors:{}
    }
  },
  methods:{
    close(){
      this.$emit('closeRequest')
    },
    update(){
      axios.patch(`/phonebook/${this.list.id}`,this.$data.list).then((response) => this.close())
      .catch((error) => this.errors = error.response.data.errors);
    }
  }
}
</script>
