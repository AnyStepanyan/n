<template>
  <v-sheet class="mx-auto ma-10 text-center" height="auto" max-width="700">
    <h1 class="text-capitalize font-weight-black">
      {{ currentPost.title }}
    </h1>

    <h5 class="ma-7">Post Number {{ currentPost.id }}</h5>

    <v-img
      height="400px"
      src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
      cover
    ></v-img>

    <v-textarea
      v-if="edit === false"
      class="ma-8"
      maxlength="auto"
      v-model="editedText"
      :value="editedText === null ? currentPost.body : editedText"
    >
    </v-textarea>

    <p v-if="edit === true" class="ma-8">
      {{ editedText === null ? currentPost.body : editedText }}
    </p>

    <div class="d-flex flex-end">
      <GreyButtons
      button-name="Edit" 
      @eventHandler="edit = false" 
      />
      <GreyButtons
        v-if="edit === false"
        button-name="Done"
        @eventHandler="updatePost(currentPost.id)"
      />
      <GreyButtons
        button-name="Delete"
        @eventHandler="deletePost(currentPost.id), $router.push({ name: 'home' })"
      />
    </div>
  </v-sheet>
</template>

<script setup>
import { computed } from "vue";
import { useStore } from "vuex";
import { ref } from "vue";
import { useRoute } from "vue-router";
import GreyButtons from "./GreyButtons.vue";import { defineProps } from 'vue';

defineProps({
currentPost: Object,
edit:{
  type: Boolean,
  default: true
},
editedText: String
})

const store = useStore();
const router = useRoute();
const edit = ref(true);
const editedText = ref(null);
store.commit(
  "setCurrentPostFromServer",
  store
    .dispatch("getCurrentPostFromServer", router.params.id)
    .then((res) => res)
);
const currentPost = computed(() => store.getters.currentPostFromServer);

const updatePost = (id) => {
  edit.value = true;
  fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: "PATCH",
    body: JSON.stringify({
      body: editedText.value,
    }),
    headers: {
      "Content-type": "application/json; charset=UTF-8",
    },
  }).catch((error) => {
    console.error(error);
  });
};

const deletePost = (id) => {
  fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: "DELETE",
  }).catch((error) => {
    console.error(error);
  });
};
</script>
