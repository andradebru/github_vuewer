<template>
    <div>
      <v-row>
        <v-col cols="12">
            <v-simple-table>
              <template v-slot:default>
                <thead>
                    <th class="text-left">{{ repo.name.toUpperCase() }}</th>
                    <!-- <th class="text-left">{{ breadcrumb.toUpperCase() }}</th> -->
                </thead>
                <tbody>
                  <tr v-for="content in contents" :key="content.number">
                    <td>
                      {{ content.name }}
                      <v-btn x-small plain @click="open_directory(content)" color="primary"
                      v-if="content.type == 'dir'">|
                        abrir
                      </v-btn>
                    </td>
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
          <v-btn v-if="files.length > 0" @click="back_to_dir">Voltar</v-btn>
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
        currentPage: 1,
        files: [],
        current_content: ''
      }),
      methods: {
        async lista_content(){
          this.loading = true
          const maiscontents = await api.lista_content(this.repo.owner.login, this.repo.name, this.currentPage)
          this.contents = this.contents.concat(maiscontents)
          this.currentPage++
          this.loading = false
          this.temmais = maiscontents.length > 0
        },
        async open_directory(content) {
          this.loading = true
          let path = content.path
          let cont = this.repo.owner.login
          let repo_name = this.repo.name
          let files = this.files
          this.contents = await api.open_directory(cont, repo_name, path)
          files.push(path)
          this.current_content = path
          this.loading = false
        },
        async back_to_dir() {
          let files = this.files
          this.loading = true
          files.pop()
          let previous_content = files.length == 1 ? files[0] : files[-1]
          if (previous_content == undefined) previous_content = '';
          this.contents = await api.open_directory(this.repo.owner.login, this.repo.name, previous_content)
          this.current_content = previous_content
          this.loading = false
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
  