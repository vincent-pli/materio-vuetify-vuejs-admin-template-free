<script>
import axios from 'axios';

export default {
    data () {
      return {
        name : "",
        model: "",
        select: { id: 'text-generation', title: 'Text Generation' },
        items: [
          { id: 'automatic-speech-recognition', title: 'Automatic Speech Recognition' },
          { id: 'text-generation', title: 'Text Generation' },
          { id: 'image-to-text', title: 'Image to Text' },
        ],
      }
    },

    methods: {
      async getData() {
          try {
            const response = await this.$http.get(
              "http://jsonplaceholder.typicode.com/posts"
            );
            // JSON responses are automatically parsed.
            this.posts = response.data;
          } catch (error) {
            console.log(error);
          }
      },

      async submit() {
        axios.post('http://127.0.0.1:8000/launch', {
          name: this.name,
          task: this.select["id"],
          model: this.model
        })
        .then(function (response) {
          console.log(response);
          this.$router.push("/dashboard");
        })
        .catch(function (error) {
          console.log(error);
        });
        
      },
    },
  }
</script>

<template>
  <VForm @submit.prevent="() => {}">
    <VRow>
      <VCol cols="12">
        <VRow no-gutters>
          <!-- 👉 First Name -->
          <VCol
            cols="12"
            md="3"
          >
            <label for="name">Name</label>
          </VCol>

          <VCol
            cols="12"
            md="9"
          >
            <VTextField
              id="name"
              v-model="name"
              placeholder="Name of your deployment"
              persistent-placeholder
            />
          </VCol>
        </VRow>
      </VCol>

      <VCol cols="12">
        <VRow no-gutters>
          <VCol
            cols="12"
            md="3"
          >
            <label for="task">Task</label>
          </VCol>

          <VCol
            cols="12"
            md="9"
          >
            <v-select
              v-model="select"
              :items="items"
              item-text="text"
              item-value="id"
              label="Select"
              persistent-hint
              return-object
              single-line
            ></v-select>
          </VCol>
        </VRow>
      </VCol>

      <VCol cols="12">
        <VRow no-gutters>
          <!-- 👉 Mobile -->
          <VCol
            cols="12"
            md="3"
          >
            <label for="model">Model Path</label>
          </VCol>

          <VCol
            cols="12"
            md="9"
          >
            <VTextField
              id="model"
              v-model="model"
              placeholder="Model path for example: gpt2"
              persistent-placeholder
            />
          </VCol>
        </VRow>
      </VCol>

      <VCol
        offset-md="3"
        cols="12"
        md="9"
        class="d-flex gap-4"
      >
        <VBtn @click="submit()" type="submit">
          Submit
        </VBtn>

      </VCol>
    </VRow>
  </VForm>
</template>
