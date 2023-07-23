<template>
  <section class="comments">
    <h1>Комментарии</h1>
    <div class="sort">
      <h2 class="sort__caption">Сортировка по id:</h2>
      <select @change="onSortChange">
        <option value="asc">По возрастанию</option>
        <option value="desc">По убыванию</option>
      </select>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>id</th>
          <th>name</th>
          <th>email</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="comment of comments"
          :key="comment.id"
          @click="onCommentClick(comment)"
          class="table__link"
        >
          <td>{{ comment.id }}</td>
          <td>{{ comment.name }}</td>
          <td>{{ comment.email }}</td>
        </tr>
      </tbody>
    </table>
    <div class="pagination">
      <button
        @click="onPrevButtonClick"
        :disabled="isPrevButtonDisabled"
        type="button"
        class="pagination__button"
      >
        Предыдущая страница
      </button>
      <button
        @click="onNextButtonClick"
        :disabled="isNextButtonDisabled"
        type="button"
        class="pagination__button"
      >
        Следующая страница
      </button>
    </div>
  </section>
</template>

<script>
export default {
  async fetch() {
    const {data, headers} = await this.$axios.get(
      `https://jsonplaceholder.typicode.com/comments?_sort=id&_order=${this.sortDirection}&_page=${this.currentPage}&_limit=${this.limit}`
    );
    this.comments = data;
    this.totalPages = Math.ceil(parseInt(headers['x-total-count']) / 10);
  },

  data: () => ({
    comments: [],
    sortDirection: 'asc',
    currentPage: 1,
    totalPages: 1,
    limit: 10
  }),

  computed: {
    isPrevButtonDisabled() {
      return this.currentPage === 1;
    },

    isNextButtonDisabled() {
      return this.currentPage === this.totalPages;
    }
  },

  methods: {
    onCommentClick(comment) {
      this.$router.push(`/${comment.id}`);
    },

    onPrevButtonClick() {
      this.currentPage--;
      this.$fetch();
    },

    onNextButtonClick() {
      this.currentPage++;
      this.$fetch();
    },

    onSortChange({target}) {
      this.sortDirection = target.value;
      this.currentPage = 1;
      this.$fetch();
    }
  }
}
</script>

<style scoped>
.comments {
  position: relative;
}
.sort {
  display: flex;
  column-gap: 15px;
  margin-bottom: 35px;
}

.sort__caption {
  font-size: 22px;
  margin: 0;
}

.sort select {
  font-size: inherit;
}

.table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid black;
  margin-bottom: 35px;
}

th,
td {
  padding: 7px;
  border: 1px solid black;
  text-align: center;
}

.table__link:hover {
  background-color: #ECECEC;
  cursor: pointer;
}

.pagination {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
}

.pagination__button {
  display: block;
  font-family: inherit;
  font-size: inherit;
  text-align: center;
  text-decoration: none;
  padding: 7px 10px;
  color: inherit;
  background-color: #ECECEC;
  border: 1px solid black;
  cursor: pointer;
}

.pagination__button:hover {
  background-color: #DBDBDB;
}

.pagination__button:disabled {
  background-color: inherit;
  opacity: 0.3;
  cursor: default;
}
</style>
