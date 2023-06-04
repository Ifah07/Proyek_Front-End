<style>
#side-bar {
  background-color: red;
}

/* Ganti warna teks dan ikon di dalam sidebar */
.text-gray-300 {
  color: white;
}

</style>

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