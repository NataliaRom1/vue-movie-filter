<template>
  <div>
    <h1>Movie List</h1>
    <form @submit.prevent="fetchMovies">
      <label for="search">Search:</label>
      <input type="text" v-model="searchQuery" id="search" />
      <button type="submit">Search</button>
    </form>
    <table v-if="movies.length">
      <thead>
        <tr>
          <th @click="sortMovies('title')">Title</th>
          <th @click="sortMovies('release_date')">Release Date</th>
          <th @click="sortMovies('popularity')">Popularity</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="movie in movies" :key="movie.id">
          <td>{{ movie.title }}</td>
          <td>{{ movie.release_date }}</td>
          <td>{{ movie.popularity }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else>No movies found</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      movies: [],
      sortField: 'title',
      sortOrder: 'asc',
    };
  },
  methods: {
    async fetchMovies() {
      const response = await fetch('http://185.185.69.80:8082/list', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          page: 0,
          page_size: 10,
          search: this.searchQuery,
          sort_field: this.sortField,
          sort_order: this.sortOrder,
        }),
      });
      const data = await response.json();
      this.movies = data.data;
    },
    sortMovies(field) {
      this.sortField = field;
      this.sortOrder = this.sortOrder === 'asc' ? 'desc' : 'asc';
      this.fetchMovies();
    },
  },
  mounted() {
    this.fetchMovies();
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  border: 1px solid black;
  padding: 8px;
  text-align: left;
}
th {
  cursor: pointer;
}
</style>
