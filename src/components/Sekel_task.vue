<template>
  <div id="app">
    <h1 class="text-center">Sekel dummy Table</h1>
    <div class="table-container">
      <table class="table table-bordered tableFixHead">
        <thead class="thead-default">
          <tr>
            <th>ID</th>
            <th>Title</th>
            <th>Body</th>
            <th>User ID</th>
            <th>Comments</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in postsArr" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.title }}</td>
            <td>{{ item.body }}</td>
            <td>{{ item.userId }}</td>
            <td>
              <button @click="loadComments(item.id)" class="btn btn-success">
                Comments
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-if="selectedUser" class="comment-container mt-4">
      <h2 class="text-center">Comments for Post {{ selectedUser.postId }}</h2>
      <ul class="list-unstyled">
        <li
          v-for="comment in selectedUser.comments"
          :key="comment.id"
          class="comment-item"
        >
          <div><strong>Comment ID:</strong> {{ comment.id }}</div>
          <div><strong>Body:</strong> {{ comment.body }}</div>
          <div><strong>User ID:</strong> {{ comment.user.id }}</div>
          <div><strong>Username:</strong> {{ comment.user.username }}</div>
          <hr class="my-2" />
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      postsArr: [],
      selectedUser: null,
    };
  },
  mounted() {
    this.getPostsData();
  },
  methods: {
    getPostsData() {
      axios
        .get("https://dummyjson.com/posts")
        .then((response) => {
          this.postsArr = response.data.posts;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    loadComments(postId) {
      axios
        .get(`https://dummyjson.com/posts/${postId}/comments`)
        .then((response) => {
          this.selectedUser = {
            postId: postId,
            comments: response.data.comments,
          };
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style>
#app {
  margin: 20px;
}

.table-container {
  max-height: 300px;
  overflow-y: auto;
  margin: 0;
  border: 2px solid #282a2c;
  border-radius: 8px;
  background-color: #f8f9fa;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  border: 2px solid #222020;
  text-align: center;
  padding: 8px;
  background-color: #ffffff;
}

th {
  background-color: #007bff;
  position: sticky;
  top: 0;
  z-index: 1;
  color: #fff;
}

button {
  cursor: pointer;
  background-color: #28a745;
  color: #fff;
  border: 1px solid #28a745;
  border-radius: 4px;
}

button:hover {
  background-color: #218838;
}

.comment-container {
  border: 2px solid #28a745;
  max-width: 800px;
  padding: 10px;
  margin: 10px auto;
  background-color: #d4edda;
}

.text-center {
  text-align: center;
  color: #007bff;
}

.mt-4 {
  margin-top: 1.5rem;
}

.comment-item {
  margin-bottom: 8px;
  background-color: #fff;
  padding: 10px;
  border: 2px solid #007bff;
  border-radius: 4px;
}

hr {
  border-color: #007bff;
}
</style>