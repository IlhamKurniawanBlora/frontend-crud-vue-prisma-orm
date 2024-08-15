<template>
    <div class="p-4">
      <h2 class="text-xl font-bold mb-4">{{ isEditing ? 'Edit Post' : 'Create Post' }}</h2>
      <form @submit.prevent="submitForm" class="space-y-4">
        <div>
          <label for="title" class="block text-sm font-medium text-gray-700">Title</label>
          <input id="title" v-model="post.title" required class="mt-1 p-2 border border-gray-300 rounded w-full" />
        </div>
        <div>
          <label for="content" class="block text-sm font-medium text-gray-700">Content</label>
          <textarea id="content" v-model="post.content" class="mt-1 p-2 border border-gray-300 rounded w-full"></textarea>
        </div>
        <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
      </form>
    </div>
  </template>
  
  <script>
  import apiClient from '../services/api';
  
  export default {
    props: {
      postToEdit: Object,
    },
    data() {
      return {
        post: this.postToEdit ? { ...this.postToEdit } : { title: '', content: '' },
      };
    },
    computed: {
      isEditing() {
        return !!this.postToEdit;
      },
    },
    methods: {
    async submitForm() {
      try {
        if (this.isEditing) {
          await apiClient.put(`/posts/${this.post.id}`, this.post);
        } else {
          await apiClient.post('/posts', this.post);
        }
        this.$emit('form-submitted');
        await this.fetchPosts(); // Refresh the list of posts
      } catch (error) {
        console.error('Error submitting form:', error);
      }
    },
    async fetchPosts() {
      try {
        const response = await apiClient.get('/posts');
        this.$emit('posts-updated', response.data); // Emit an event with updated posts
      } catch (error) {
        console.error('Error fetching posts:', error);
      }
    },
  },
  mounted() {
    this.fetchPosts(); // Fetch posts when the component is mounted
  },
  };
  </script>
  