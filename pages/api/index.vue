<template>
  <div class="container">
    <div class="mt-5 p-5">
      <button class="btn btn-secondary" @click="posts = []">Очистить posts</button>
      <button class="btn btn-secondary ml-2"  @click="loadPosts">Загрузить заново</button>

      <b-table
        class="mt-5"
        empty-text="Нет записей для вывода"
        :show-empty="true"
        :items="posts"
        :fields="fields"
        :current-page="page"
        :per-page="limit">
        <template v-slot:row-details="row">
          <pre>{{row.item.body}}</pre>
        </template>
        <template v-slot:cell(actions)="row">
          <button class="btn btn-secondary btn-sm" @click="row.toggleDetails">
              {{ row.detailsShowing ? 'Скрыть текст' : 'Показать текст' }} 
          </button>
        </template>
      </b-table>
    
      <b-pagination
        v-if="total > 0"
        v-model="page"
        :per-page="limit"
        :total-rows="total"
      />
    </div>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                posts: [],
                fields: [
                    {key: 'id', title: 'Id', sortable: true},
                    {key: 'title', title: 'Название', sortable: true},
                    {key: 'actions', title: 'Действия'},
                ],
                page: 1,
                limit: 15,
            }
        },
        computed: {
            total() {
                return this.posts.length
            }
        },
        methods: {
          loadPosts() {
              this.$axios.get('https://jsonplaceholder.typicode.com/posts')
                  .then(res => {
                      this.posts = res.data
                  })
          }
        },
        created() {
            this.loadPosts()
        }
    }
</script>
</template>
