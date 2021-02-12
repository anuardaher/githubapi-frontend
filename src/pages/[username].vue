<template>
  <Layout>
    <div
      class="grid grid-rows-3 lg:grid-flow-col gap-10 lg:p-8 items-center justify-items-center"
    >
      <div class="row-span-3 col-span-3">
        <div class="user-profile text-center">
          <div
            class="rounded-full h-40 w-40 flex items-center justify-center spang-gray-300 mx-auto border-2"
          >
            <g-image
              class="rounded-full"
              alt="Example image"
              :src="user.avatar_url"
              width="100"
            />
          </div>
          <div class="user-id mt-4">
            <span class="font-semibold">ID</span>: {{ user.id }}
          </div>
          <div class="user-login mt-2">
            <span class="font-semibold">Login</span>:
            {{ user.login }}
          </div>
          <div class="user-profile mt-2">
            <span class="font-semibold">Profile</span>:
            <a
              class="hover:text-blue-500"
              target="_blank"
              :href="user.html_url"
              >{{ user.login }}</a
            >
          </div>
          <div class="user-created-at mt-2">
            <span class="font-semibold">Created at</span>:
            {{ userCreatedDateFormated }}
          </div>
        </div>
      </div>
      <div class="row-span-3 lg:col-span-2 col-span-3 text-center">
        <table class="table-fixed mx-auto w-full">
          <caption class="text-2xl mb-4">
            Repositories
          </caption>
          <thead class="flex w-full border">
            <tr class="flex w-full mb-2">
              <th class="w-1/3">ID</th>
              <th class="w-1/3">Name</th>
              <th class="w-1/3">URL</th>
            </tr>
          </thead>
          <tbody
            class="flex flex-col items-center overflow-y-scroll w-full border"
            style="height: 50vh;"
          >
            <tr
              class="flex w-full mb-2"
              v-for="repo in repositories"
              :key="repo.id"
            >
              <td class="w-1/3">{{ repo.id }}</td>
              <td class="w-1/3">{{ repo.name }}</td>
              <td class="w-1/3">
                <a
                  class="hover:text-blue-500"
                  target="_blank"
                  :href="repo.html_url"
                  >Repository</a
                >
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </Layout>
</template>

<script>
import axios from "axios"

export default {
  metaInfo() {
    return {
      title: this.user.name
    }
  },
  data() {
    return {
      user: {},
      repositories: []
    }
  },
  computed: {
    userCreatedDateFormated() {
      return this.user.created_at
        ? new Date(this.user.created_at).toLocaleDateString()
        : new Date().toLocaleDateString()
    }
  },
  methods: {
    async fetchUserDetails(username) {
      try {
        const { data } = await axios.get(
          `https://anuar-githubapi-proxy.herokuapp.com/api/users/${username}/details`
        )
        this.user = data ? data : {}
      } catch (error) {
        console.error(error)
      }
    },
    async fetchUserRepositories(username) {
      try {
        const { data } = await axios.get(
          `https://anuar-githubapi-proxy.herokuapp.com/api/users/${username}/repos`
        )
        this.repositories = data ? data : []
      } catch (error) {
        console.error(error)
      }
    }
  },
  async mounted() {
    const { username } = this.$route.params
    this.fetchUserDetails(username)
    this.fetchUserRepositories(username)
  }
}
</script>

<style></style>
