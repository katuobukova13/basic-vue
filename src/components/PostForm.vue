<template>
    <form @submit.prevent>
      <h3>Создать пост</h3>
      <custom-input v-model="post.title"
             placeholder="Название"/>
      <custom-input v-model="post.body"
             placeholder="O чем"/>
      <custom-button
          style="align-self: flex-end; margin-top: 10px"
          @click="createPost">Создать</custom-button>
    </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: '',
        body: '',
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit('create', this.post);
      this.post =  {
        title: '',
        body: '',
      }
    },
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}

</style>
