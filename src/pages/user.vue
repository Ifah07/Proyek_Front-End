<template>
  <div class="px-5">
    <div class="font-bold text-4xl py-3">Data Pengguna</div>

    <div class="py-5">
      <table class="border-collapse border-2 w-full">
        <thead>
          <tr>
            <th class="border-2 p-3">NO</th>
            <th class="border-2 p-3">Tanggal</th>
            <th class="border-2 p-3">Nama</th>
            <th class="border-2 p-3">No.Wa</th>
            <th class="border-2 p-3">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="data in posts" :key="data.id">
            <td class="border-2 p-3">{{ data.id }}</td>
            <td class="border-2 p-3">{{ data.name }}</td>
            <td class="border-2 p-3">{{ data.description }}</td>
            <td class="border-2 p-3"></td>
            <td class="border-2 p-3">
              <button class="px-4 py-2 border rounded bg-blue-500 text-white hover:bg-blue-900"
                @click="edit(data)">Edit</button>
              <button class="px-4 py-2 border rounded bg-red-500 text-white hover:bg-red-900"
                @click="destroy(data.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="flex justify-center items-center space-x-2">
      <button class="px-4 py-2 border rounded bg-gray-500 text-white hover:bg-gray-400" @click="modelAdd = true">Add New
        Post</button>
      <button class="px-4 py-2 border rounded bg-blue-500 text-white hover:bg-blue-400" @click="modelUpdate = true">Update
        Post</button>
    </div>

    <!-- Pop up model Update  -->
  </div>
</template>


<script>
import axios from 'axios';
import { mapGetters } from 'vuex';
export default {
  data() {
    return {
      selectedFile: null,
      modelAdd: false,
      modelUpdate: false,
      data: {
        id: "",
        name: "",
        description: ""
      }
    }
  },
  computed: {
    ...mapGetters({
      posts: 'post/posts'
    })
  },
  methods: {
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0];
    },
    uploadImage() {
      const formData = new FormData();
      formData.append('image', this.selectedFile);

      axios.post('http://localhost:3001/api/upload', formData)
        .then(response => {
          console.log(response.data);
          console.log(response.data.imageUrl);
          // Handle the response as needed
        })
        .catch(error => {
          console.error(error);
          // Handle the error as needed
        });
    },


    addnew() {
      this.$store.dispatch('post/ADD_NEW', this.data).then(respone => {
        if (respone) {
          this.modelAdd = false;
          console.log("Data Berhasil Disimpan")
          console.log(this.data)
          // reset data 
          this.data = {
            id: "",
            name: "",
            description: ""
          }
        }
      })
    },
    edit(id) {
      this.modelUpdate = true;
      this.$store.dispatch('post/GET_POST', id).then(respone => {
        //set data model
        this.data.id = respone[0].id;
        this.data.name = respone[0].name;
        this.data.description = respone[0].description;
      })
    },
    updatePost() {
      this.$store.dispatch('post/UPDATE_POST', this.data).then(respone => {
        if (respone) {
          this.modelUpdate = false;
        }
      })
    },
    destory(id) {
      this.$store.dispatch('post/DELETE_POST', id).then(respone => {
        if (respone) {
          alert("ok");
        }
      })
    }

  }
}


</script>

<style></style>



<!-- <template>
  <div>
      <input type="file" @change="handleFileUpload" accept="image/*">
      <button @click="uploadImage">Upload</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
      return {
          selectedFile: null
      };
  },
  methods: {
      handleFileUpload(event) {
          this.selectedFile = event.target.files[0];
      },
      uploadImage() {
          const formData = new FormData();
          formData.append('image', this.selectedFile);

          axios.post('http://localhost:3001/api/upload', formData)
              .then(response => {
                  console.log(response.data);
                  console.log(response.data.imageUrl);
                  // Handle the response as needed
              })
              .catch(error => {
                  console.error(error);
                  // Handle the error as needed
              });
      }
  }
};
</script>
 -->