<template>
  <div class="post">
    <div>
    <div><strong>Название:</strong> {{ post.title }}</div>
    <div><strong>О чем:</strong> {{ post.body }}</div>
  </div>
    <div class="post__btns">
      <custom-button
          style="margin-top: 0"
          @click="$emit('remove', post)"
      >Удалить
      </custom-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  },
}
</script>

<style scoped>
.post {
  padding: 15px;
  border: 2px solid #00bd7e;
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>