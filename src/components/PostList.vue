<template>
  <div class="p-4">
    <h2 class="text-xl font-bold mb-4">Posts</h2>
    <ul class="space-y-2">
      <li v-for="post in posts" :key="post.id" class="border p-4 rounded-md shadow-sm">
        <h3 class="text-lg font-semibold">{{ post.title }}</h3>
        <p class="text-gray-700">{{ post.content }}</p>
        <div class="mt-2">
          <button @click="editPost(post)" class="bg-blue-500 text-white px-4 py-2 rounded mr-2">Edit</button>
          <button @click="deletePost(post.id)" class="bg-red-500 text-white px-4 py-2 rounded">Delete</button>
        </div>
      </li>
    </ul>
    <button @click="createNewPost" class="bg-green-500 text-white px-4 py-2 rounded mt-4">Create New Post</button>
  </div>
</template>

<script>
import apiClient from '../services/api';

export default {
  data() {
    return {
      posts: [],
    };
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await apiClient.get('/posts');
        this.posts = response.data;
      } catch (error) {
        console.error('Error fetching posts:', error);
      }
    },
    async deletePost(id) {
      try {
        await apiClient.delete(`/posts/${id}`);
        this.fetchPosts();
      } catch (error) {
        console.error('Error deleting post:', error);
      }
    },
    createNewPost() {
      this.$emit('create-new-post');
    },
    editPost(post) {
      this.$emit('edit-post', post);
    },
  },
  mounted() {
    this.fetchPosts();
  },
};
</script>
