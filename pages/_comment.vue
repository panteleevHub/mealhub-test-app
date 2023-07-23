<template>
  <section>
    <h1>Комментарий</h1>
    <dl>
      <div v-for="(value, key) in comment" :key="key">
        <dt>{{ key }}</dt>
        <dd>{{ value }}</dd>
      </div>
    </dl>
    <nuxt-link to="/">Вернуться на главную</nuxt-link>
  </section>
</template>

<script>
export default {
  async asyncData({$axios, params}) {
    const comment = await $axios.$get(`https://jsonplaceholder.typicode.com/comments/${params.comment}`);
    return {comment};
  },

  validate({params}) {
    return !isNaN((parseInt(params.comment)));
  },

  data: () => ({
    comment: {}
  })
}
</script>

<style scoped>
  dl {
    margin: 0;
    margin-bottom: 35px;
  }

  dl div {
    display: grid;
    grid-template-columns: repeat(2, 1fr );
    padding: 10px 0;
    border-bottom: 1px solid black;
  }

  dt {
    font-size: 22px;
  }

  dd {
    margin: 0;
  }
</style>