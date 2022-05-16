<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <custom-input
        v-model="searchQuery"
        placeholder="Поиск по заголовку"
    />
    <div class="app__btns">
    <custom-button
        @click="showModal"
    >Создать новый пост
    </custom-button>
      <custom-select
          v-model="selectedSort"
          :options="sortOptions"
      ></custom-select>
    </div>
    <custom-modal
        v-model:show="modalVisible">
      <post-form @create="createPost"/>
    </custom-modal>
    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!loading"
    />
    <div v-else>Loading...</div>
    <div class="page__wrapper">
      <div
          v-for="numberPage in totalPages"
          :key="numberPage"
          class="page"
          @click="changePage(numberPage)"
          :class="{
            'current-page': page === numberPage
          }"
      >
        {{numberPage}}
      </div>
    </div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import axios from "axios";

export default {
  components: {
    PostForm, PostList
  },
  data() {
    return {
      posts: [],
      modalVisible: false,
      loading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'По заголовку'},
        {value: 'body', name: 'По содержимому'}
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.modalVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id);
    },
    showModal() {
      this.modalVisible = true;
    },
    changePage(numberPage) {
      this.page = numberPage;
    },
    async fetchPosts() {
      try {
        this.loading = true;
        const response = await axios.get(
            'https://jsonplaceholder.typicode.com/posts', {
              params: {
                _limit: this.limit,
                _page: this.page,
              }
            });
        this.totalPages = Math.ceil(response.headers['x-total-count']/this.limit);
        this.posts = response.data;
      } catch (e) {
        console.log(e);
      }
      finally {
        this.loading = false;
      }
    }
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    sortedPosts() {
      return [...this.posts.sort((post1, post2) =>
          post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      )]
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
    watch: {
      page() {
        this.fetchPosts();
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.app__btns {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 20px 0;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
  justify-content: center;
}

.page {
  border: 1px solid black;
  padding: 4px 6px;
  margin-right: 6px;
}

.current-page {
 border: 1px solid #00bd7e;
}

</style>
