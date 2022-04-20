<script setup>
  import { ref } from 'vue'

  const username = ref('')
  const repos = ref([])
  const message = ref('')

  const resetUsername = () => {
    username.value = ''
  }

  const resetRepos = () => {
    repos.value = []
  }

  const resetMessage = () => {
    message.value = ''
  }

  const queryReposByUsername = async (username) => {
    return await fetch(`https://api.github.com/users/${username}/repos`).then(
      (resp) =>
        resp.json().then((repositories) => {
          if (repositories.length > 0 && repositories[0].id) {
            repos.value = repositories
            console.log(repositories)
          } else {
            message.value = 'El usuario no existe o no tiene repositorios'
          }
        })
    )
  }
</script>

<template>
  <div class="text-h3 q-pa-md col-12">
    <span>C13</span>
    <q-icon size="xl" name="las la-rocket" class="q-pa-xs" />
  </div>
  <div class="row justify-center">
    <q-input
      bottom-slots
      v-model="username"
      label="Ingresa el usuario de github"
      class="username-input"
    >
      <template v-slot:before>
        <q-icon name="lab la-github" />
      </template>

      <template v-slot:append>
        <q-icon
          v-if="username !== ''"
          name="close"
          @click="resetUsername(), resetRepos(), resetMessage()"
          class="cursor-pointer"
        />
        <q-icon
          name="search"
          @click="resetRepos(), resetMessage(), queryReposByUsername(username)"
        />
      </template>
    </q-input>
  </div>
  <div class="row justify-center">
    <template v-if="message != ''">
      <span class="text-h6">{{ message }}</span>
    </template>

    <template v-else>
      <ul style="text-align: left">
        <li v-for="repo in repos">
          {{ repo.name }}
        </li>
      </ul>
    </template>
  </div>
</template>

<style lang="scss" scoped>
  .username-input {
    min-width: 320px;
    text-align: center;
    margin: 0;
    padding: 0;
  }
</style>
