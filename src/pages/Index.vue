<template>
  <Layout>
    <h1 class="text-2xl font-bold mb-4 text-center">List of Github Users</h1>
    <loading v-if="loadingUsers" />
    <div class="list-of-users" v-else>
      <div
        class="grid grid lg:grid-cols-5 grid-cols-2 lg:gap-4 gap-2 text-white"
      >
        <div
          class="user-card bg-gradient-to-r from-gray-700 to-gray-900 rounded-lg puff-in-center "
          v-for="user in users.data"
          :key="user.id"
        >
          <div class="ml-2 font-semibold text-sm">{{ user.id }}</div>
          <g-link :to="'/' + user.login"
            ><div
              class="text-base text-center cursor-pointer hover:text-blue-400"
            >
              {{ user.login }}
            </div></g-link
          >
        </div>
      </div>

      <div class="home-links text-center mt-4">
        <span
          class="mx-4 cursor-pointer hover:text-blue-400"
          @click="fetchUsers(users.pagination.first['since}'])"
          >{{ users.pagination && users.pagination.first.rel }}
        </span>

        <span
          class="mx-4 cursor-pointer hover:text-blue-400"
          @click="fetchUsers(users.pagination.next.since)"
          >{{ users.pagination && users.pagination.next.rel }}
        </span>
      </div>
    </div>
  </Layout>
</template>

<script>
import axios from "axios"
import Loading from '../components/Loading'

export default {
  metaInfo: {
    title: "Users"
  },
  components: {
    Loading
  },
  data() {
    return {
      users: [],
      loadingUsers: false
    }
  },
  methods: {
    async fetchUsers(since = 0) {
      try {
        this.loadingUsers = true
        const { data } = await axios.get(
          `https://anuar-githubapi-proxy.herokuapp.com/api/users?since=${since}`
        )
        this.users = data ? data : []
      } catch (error) {
        console.error(error)
      } finally {
        this.loadingUsers = false
      }
    }
  },
  async mounted() {
    this.fetchUsers()
  }
}
</script>

<style>
.puff-in-center {
  -webkit-animation: puff-in-center 0.3s cubic-bezier(0.47, 0, 0.745, 0.715)
    both;
  animation: puff-in-center 0.3s cubic-bezier(0.47, 0, 0.745, 0.715) both;
}

@-webkit-keyframes puff-in-center {
  0% {
    -webkit-transform: scale(1.2);
    transform: scale(1.2);
    -webkit-filter: blur(4px);
    filter: blur(4px);
    opacity: 0;
  }
  100% {
    -webkit-transform: scale(1);
    transform: scale(1);
    -webkit-filter: blur(0px);
    filter: blur(0px);
    opacity: 1;
  }
}
@keyframes puff-in-center {
  0% {
    -webkit-transform: scale(1.2);
    transform: scale(1.2);
    -webkit-filter: blur(4px);
    filter: blur(4px);
    opacity: 0;
  }
  100% {
    -webkit-transform: scale(1);
    transform: scale(1);
    -webkit-filter: blur(0px);
    filter: blur(0px);
    opacity: 1;
  }
}
</style>
