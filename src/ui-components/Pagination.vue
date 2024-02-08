<template>
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item" :class="{disabled: page === 1}">
          <a class="page-link" href="#" @click.prevent="changePage(page - 1)" aria-label="Previous">
            <span aria-hidden="true">&laquo;</span>
          </a>
        </li>
  
        <li v-for="pageNumber in totalPages" :key="pageNumber" :class="{active: pageNumber === page}" class="page-item">
          <span class="page-link" @click.prevent="changePage(pageNumber)">{{ pageNumber }}</span>
        </li>
  
        <li class="page-item" :class="{disabled: page === totalPages}">
          <a class="page-link" href="#" @click.prevent="changePage(page + 1)" aria-label="Next">
            <span aria-hidden="true">&raquo;</span>
          </a>
        </li>
      </ul>
    </nav>
  </template>
  
  <script>
  export default {
    name: 'Pagination',
    props: {
      totalPages: Number,
      page: Number
    },
    methods: {
      changePage(newPage) {
        if (newPage >= 1 && newPage <= this.totalPages) {
          this.$emit('page-changed', newPage);
        }
      }
    }
  }
  </script>
  