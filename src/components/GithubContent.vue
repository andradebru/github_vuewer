<template>
    <div>
      <v-row>
        <v-col cols="12">
            <v-simple-table>
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">Number</th>
                    <!-- <th class="text-left">Title</th> -->
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="content in contents" :key="content.number">
                    <td>{{ content.name }}</td>
                    <!-- <td>{{ content.title }}</td> -->
                  </tr>
                </tbody>
              </template>
          </v-simple-table>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <v-progress-circular indeterminate color="secondary" v-if="loading"></v-progress-circular>
          <v-btn color="primary" v-if="temmais" @click="lista_content">MAIS</v-btn>
        </v-col>
      </v-row>
    </div>
  </template>
  
  <script>
  
    import {api} from '~api'
  
    export default {
      props: ['repo'],
      data: () => ({
        contents: [],
        loading: false,
        temmais: false,
        currentPage: 1
      }),
      methods: {
        async lista_content(){
          this.loading = true
          const maiscontents = await api.lista_content(this.repo.owner.login, this.repo.name, this.currentPage)
          this.contents = this.contents.concat(maiscontents)
          this.currentPage++
          this.loading = false
          this.temmais = maiscontents.length > 0
        }
      },
      watch: {
        repo(){
          this.contents = []
          if (this.repo) {
            this.temmais = false
            this.currentPage = 1
            this.lista_content()
          } else {
            this.contents = []
            this.temmais = false
            this.currentPage = 1
          }
        }
      }
    }
  </script>
  