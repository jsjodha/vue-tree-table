---
title: Pagination
---

# Pagination example

If you need to add pagination to your table here's an example how you can achieve this.
<br />
<br />

<PaginationExampleComponent />


What you've basically need to do is to create a separate pagination component which is going to modify table table-data.

```
<template>
  <div>
    <tree-table
            class="table"
            :columns="columns"
            :table-data="paginatedData"
    />
    <div class="pagination">
      <div class="pagination--button pagination--button__active" v-if="!isFirstPage()" v-on:click="prevPage()">&lt;</div>
      <div class="pagination--button" v-if="isFirstPage()">-</div>
      <div>{{ currentPage + 1 }} / {{ numberOfPages() }}</div>
      <div class="pagination--button pagination--button__active" v-if="!isLastPage()" v-on:click="nextPage()">></div>
      <div class="pagination--button" v-if="isLastPage()">-</div>
    </div>
  </div>
</template>

<script>
  import TreeTable from '../../../../tree-table/src/package'
  import {tableData, columns} from '../resources/data'

  export default {
    name: 'PaginationExample',
    components: {TreeTable},
    data: function(){
      return {
        tableData,
        columns,
        currentPage: 0,
        recordsPerPage: 3
      }
    },
    computed: {
      paginatedData: function () {
        const firstIndexOnPage = this.currentPage*this.recordsPerPage
        const firstIndexOnNextPage = (this.currentPage+1)*this.recordsPerPage
        return this.tableData.slice(firstIndexOnPage, firstIndexOnNextPage)
      }
    },
    methods: {
      nextPage(){
        this.currentPage = this.currentPage+1
      },
      prevPage(){
        this.currentPage = this.currentPage-1
      },
      numberOfPages(){
        return Math.ceil(this.tableData.length / this.recordsPerPage)
      },
      isFirstPage(){
        return this.currentPage == 0
      },
      isLastPage(){
        const firstIndexOnNextPage = (this.currentPage+1)*this.recordsPerPage
        return firstIndexOnNextPage > this.tableData.length;
      }
    }
  }
</script>

<style scoped>
  .table{
    width: 50%;
    margin: auto;
  }

  .pagination{
    display: flex;
    width: 40%;
    margin: auto;
    justify-content: center;
    padding: 1rem;
  }

  .pagination--button{
    margin-left: 0.5rem;
    margin-right: 0.5rem;
  }

  .pagination--button__active:hover{
    color: #acacac;
  }

</style>
```
