<template>
  <div class="client-edit">
    <h1>Edit Client</h1>
    <div class="client">
      <div class="info">
        <div class="personal">
          <label for="name">Name</label>
          <input type="text" name="name" v-model="name" />
          <br />
          <label for="email">Email</label>
          <input type="text" name="email" v-model="email" />
        </div>
        <TagsEdit :tags="tags" />
      </div>
    </div>

    <a id="saveBtn" @click="saveClient" name="saveBtn" href="#">save</a>
  </div>
</template>

<script>
import TagsEdit from "./TagsEdit.vue";
import axios from "axios";

export default {
  name: "EditClient",
  props: {
    id: String,
  },
  data() {
    return {
      name: "Fulaninho",
      email: "chupaqui@gmail.com",
      tags: [{ id: 1, name: "ouro", color: "red" }],
    };
  },
  created() {
    console.log(this.$route.params);
    this.fetchClient();
  },
  methods: {
    fetchClient() {
      axios
        .get(`${process.env.VUE_APP_SERVER_URL}/client/${this.$route.params.id}`)
        .then((response) => {
          // handle success
          console.log(response);
          this.name = response.data.name;
          this.email = response.data.email;
          this.tags = response.data.tags;
        })
        .catch(function (error) {
          // handle error
          console.log(error);
        })
        .then(function () {
          // always executed
        });
    },
    saveClient() {
      const clientId = this.$route.params.id;
      const data = {
        name: this.name,
        email: this.email,
      };

      // update
      if (clientId)
        axios
          .patch(`${process.env.VUE_APP_SERVER_URL}/client/${this.$route.params.id}`, data)
          .then((response) => {
            console.log(response);
            this.$router.push("/");
          });
      // create new
      else
        axios.post(`${process.env.VUE_APP_SERVER_URL}/client`, data).then((response) => {
          console.log(response);
          this.$router.push("/");
        });
    },
  },
  components: { TagsEdit },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.client-edit {
  margin: 30px;
}

h1 {
  font-size: 23px;
  margin-bottom: 10px;
}

a {
  color: #42b983;
}

.client {
  position: relative;
  background-color: rgb(221, 221, 221);
  padding: 15px;
  cursor: pointer;
  border-bottom: 1px solid rgb(221, 221, 221);
  height: 120px;
}

.client-list-container {
  width: 100%;
  height: 100%;
  border: solid 1px rgb(206, 205, 205);
}

.info {
  height: 60px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.info > div {
}

.tags {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  height: 60px;
  width: 300px;
  justify-content: flex-start;
}

.tag {
  background-color: green;
  color: white;
  margin: 3px;
  padding: 4px;
  height: 15px;
  border-radius: 3px;
}
.personal {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  justify-content: flex-start;
}
#saveBtn {
  float: right;
}
</style>
