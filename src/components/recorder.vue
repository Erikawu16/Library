<template>
  <div class="book-list-page">
    <div class="header-container">
      <div class="header-left">
        <h2>BookList</h2>
      </div>
    </div>
    <div class="borrow-list-container">
      <!-- 添加 loading 状态 -->
      <div v-if="loading">Loading...</div>
      <table class="book-table" v-if="!loading">
        <thead>
          <tr>
            <th>InventoryId</th>
            <th>Name</th>
            <th>Author</th>
            <th>Introduction</th>
            <th>Borrow</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(book, index) in bookList" :key="index">
            <td>{{ book.inventoryId }}</td>
            <td>{{ book.userId }}</td>
            <td>{{ book.borrowingTime }}</td>
            <td>{{ book.returnTime }}</td>
            <td><button @click="borrowBook(index)">Borrow</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'BookListPage',
  data() {
    return {
      bookList: [],
      loading: false, // 添加 loading 状态
    };
  },

  methods: {
    async packageGetData() {
      this.loading = true; 
      const userId = this.$route.query.userId;
      console.log(userId);
      try {
      const response = await axios.get(`http://localhost:8005/library/recorder/${userId}`);
      this.bookList = response.data;
        console.log('Fetched data:', response.data);
      } catch (error) {
        console.error('Error fetching data:', error.response || error);
      } finally {
        this.loading = false; // 数据加载完成或出错时设置 loading 为 false
      }
    },

    async returnBook(index) {
      const userId = this.$route.query.userId;
      console.log(`Borrowed book: ${this.bookList[index].InventoryId}`);
      const id = this.bookList[index].InventoryId;
      try {
        await axios.put(`http://localhost:8005/library/returnBook/${userId}/${id}`);
        this.packageGetData();
      } catch (error) {
        console.error('Error borrowing book:', error.response || error);
      }
    },
  },
  
  // 在组件创建时调用 packageGetData 方法
  created() {
    this.packageGetData();
  },
};
</script>




<style scoped>
  .book-list-page {
    max-width: 800px;
    margin: auto;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  .header-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }

  .header-left h2 {
    margin-right: 20px;
  }


  .book-table {
    width: 100%;
    border-collapse: collapse;
  }

  th, td {
    border: 1px solid #ddd;
    padding: 10px;
    text-align: left;
  }

  th {
    background-color: #f2f2f2;
  }

  button:hover {
    background-color: #368b7d;
  }
</style>
