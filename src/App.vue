<template>
  <div id="app" class="max-w-2xl mx-auto p-4">
    <PostList @create-new-post="showForm" @edit-post="showForm" />
    <PostForm v-if="showingForm" :postToEdit="postToEdit" @form-submitted="hideForm" />
  </div>
</template>

<script>
import PostList from './components/PostList.vue';
import PostForm from './components/PostForm.vue';

export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      showingForm: false,
      postToEdit: null,
    };
  },
  methods: {
    showForm(post = null) {
      this.postToEdit = post;
      this.showingForm = true;
    },
    hideForm() {
      this.showingForm = false;
      this.postToEdit = null;
    },
  },
};
</script>
