<template>
    <div>
      <v-row>
        <v-col cols="12">
            <v-simple-table>
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">Number</th>
                    <th class="text-left">Title</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="issue in directories" :key="issue.number">
                    <td>{{ issue.number }}</td>
                    <td>{{ issue.title }}</td>
                  </tr>
                </tbody>
              </template>
          </v-simple-table>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <v-progress-circular indeterminate color="primary" v-if="loading"></v-progress-circular>
          <v-btn color="primary" v-if="temmais" @click="lista_directories">MAIS</v-btn>
        </v-col>
      </v-row>
    </div>
  </template>
  
  <script>
  
    import {api} from '~api'
  
    export default {
      props: ['repo'],
      data: () => ({
        directories: [],
        loading: false,
        temmais: false,
        currentPage: 1
      }),
      methods: {
        async lista_directories(){
          this.loading = true
          const mais_directories = await api.lista_directories(this.repo.owner.login, this.repo.name, this.currentPage)
          this.directories = this.directories.concat(mais_directories)
          this.currentPage++
          this.loading = false
          this.temmais = mais_directories.length > 0
        }
      },
      watch: {
        repo(){
          this.directories = []
          if (this.repo) {
            this.temmais = false
            this.currentPage = 1
            this.lista_directories()
          } else {
            this.directories = []
            this.temmais = false
            this.currentPage = 1
          }
        }
      }
    }
  </script>
  