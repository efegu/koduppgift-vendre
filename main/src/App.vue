<script>
export default {
  data() {
    return {
      baseURL: 'https://reqres.in/api/users?page=',
      currentPage: 1,
      users: null,
    };
  },
  computed: {
    fetchUsers() {
      fetch('https://reqres.in/api/users?page=' + this.currentPage, {
        method: 'GET',
      })
        .then((res) => res.json())
        .then((res) => {
          this.users = res.data.map((user) => {
            user.show = false;
            return user;
          });
        });
    },
  },
  methods: {
    showEmail(user) {
      user.show = !user.show;
    },
    updatePage(newPage) {
      this.currentPage = newPage;
      this.fetchUsers();
    },
  },
  mounted() {
    this.fetchUsers();
  },
};
</script>

<template>
  <div class="header">
    <h1>Vendre koduppgift</h1>
  </div>
  <main class="container">
    <div class="user-boxes" v-if="users != null">
      <div v-for="user in users" :key="user.id" class="user-box">
        <img class="image" :src="user.avatar" />
        <h3>{{ user.first_name }} {{ user.last_name }}</h3>
        <div id="contact" @click="showEmail(user)">Contact</div>
        <span id="email" v-if="user.show">
          <a :href="'mailto:' + user.email">{{ user.email }}</a>
        </span>
      </div>
    </div>
  </main>
  <div class="pagination-section">
    <div class="pagination">
      <input type="button" v-for="i in 2" :value="i" @click="updatePage(i)" />
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
* {
  font-family: 'Poppins', sans-serif;
  list-style: none;
  text-decoration: none;
  color: inherit;
}
.image {
  border-radius: 50%;
  width: 128px;
  height: 128px;
  object-fit: cover;
}
.container {
  margin: auto;
  display: flex;
  justify-content: center;
}
.user-boxes {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10vh 5vh 0 5vh;
  flex-wrap: wrap;
}
.pagination-section {
  padding: 3rem;
  display: flex;
  justify-content: center;
}
.pagination {
  display: flex;
  width: 5%;
  justify-content: space-around;
}

.pagination > input[type='button'] {
  background: none;
  border: none;
  font-size: larger;
  font-weight: 600;
  cursor: pointer;
}

#contact {
  cursor: pointer;
  display: flex;
  justify-content: space-around;
}

#email {
  font-size: small;
  display: flex;
  justify-content: space-around;
}

.user-box {
  padding: 40px;
  text-align: center;
  box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
  margin: 20px;
  border-radius: 5%;
  transition: 0.5s;
  width: 220px;
  height: 300px;
}

.user-box:hover {
  transform: scale(105%);
}

.header {
  background-color: rgb(255, 255, 255);
  box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
  padding: 15px;
  width: 100%;
}
</style>
