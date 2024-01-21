<template>
 
 
  <div class="dual-view-container">
<!-- Bottom - Page A Component -->
    <div class="page-a">
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
              <td>{{ book.InventoryId }}</td>
              <td>{{ book.Name }}</td>
              <td>{{ book.Author }}</td>
              <td>{{ book.Introduction }}</td>
              <td><button @click="borrowBook(index)">Borrow</button></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
    <!-- Top - Page B Component -->
    <div class="page-b">
      <div class="header-container">
        <div class="header-left">
          <h2>BorrowList</h2>
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
              <th>borrowingTime</th>
             
              <th>Return</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(book, index) in borrowList" :key="index">
              <td>{{ book.InventoryId }}</td>
              <td>{{ book.Name }}</td>
              <td>{{ book.BorrowingTime }}</td>
              
              <td><button @click="returnBook(index)">Return</button></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div>
        <button @click="logout" class="logout-button">Logout</button>
      </div>
  <footer class="footer-container">
      <span>玉山技術測驗考卷</span>
  </footer>
  
</template>


<script>
import axios from 'axios';
import { mapState } from 'vuex';
export default {
  name: 'BookListPage',
  data() {
    return {
      bookList: [],
      borrowList: [],
      loading: false, // 添加 loading 状态
    };
  },
  computed: {
    ...mapState(['userId']),
  },
  methods: {
    async packageGetData() {
      this.loading = true; 
      const url = 'http://localhost:8005/library/booklist';
      try {
        const response = await axios.get(url);
        this.bookList = response.data;
        console.log('Fetched data:', response.data);
      } catch (error) {
        console.error('Error fetching data:', error.response || error);
      } finally {
        this.loading = false; // 数据加载完成或出错时设置 loading 为 false
      }
    },

    async borrowBook(index) {
      const userId = this.$route.query.userId;
      console.log(userId);
      //console.log(`Borrowed book: ${this.bookList[index].InventoryId}`);
      const id = this.bookList[index].InventoryId;
      try {
        await axios.put(`http://localhost:8005/library/borrowBook/${userId}/${id}`);
              this.borrowData();
        this.packageGetData();
        this.$forceUpdate();
      } catch (error) {
        console.error('Error borrowing book:', error.response || error);
      }
    },
    async returnBook(index) {
      const userId = this.$route.query.userId;
      console.log(userId);
      //console.log(`Borrowed book: ${this.bookList[index].InventoryId}`);
      const id = this.borrowList[index].InventoryId;
      try {
        await axios.put(`http://localhost:8005/library/returnBook/${userId}/${id}`);
        this.borrowData();
        this.packageGetData();
        this.$forceUpdate();
      } catch (error) {
        console.error('Error borrowing book:', error.response || error);
      }
    },

    async borrowData() {
      this.loading = true; 
      const userId = this.$route.query.userId;
      console.log("borrowData"+userId);
      try {
      const response = await axios.get(`http://localhost:8005/library/recorder/${userId}`);
      this.borrowList = response.data;
        console.log('Fetched data:', response.data);
      } catch (error) {
        console.error('Error fetching data:', error.response || error);
      } finally {
        this.loading = false; // 数据加载完成或出错时设置 loading 为 false
      }
    },


    async logout() {
       
      try {
        await axios.get(`http://localhost:8005/library/logout`);
        this.$router.push('/');
        
      } catch (error) {
        console.error('Error fetching data:', error.response || error);
      } finally {
        this.loading = false; // 数据加载完成或出错时设置 loading 为 false
      }
    },
  },
  
  // 在组件创建时调用 packageGetData 方法
  created() {
    this.packageGetData();
    this.borrowData();
  },
};
</script>




<style scoped>

 /* Add styles for the dual-view container and pages */
 /* Add styles for the dual-view container and pages */
 .dual-view-container {
    display: flex;
    flex-direction: column;
  }

  .page-a,
  .page-b {
    box-sizing: border-box;
    padding: 20px;
  
  margin-bottom: 100px; /* 距離底部 50px */
}


  .logout-button {
margin-bottom:100px;
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
  .footer-container {
  background-color: #f5f5f5;
  padding: 10px;
  text-align: center;
  position: fixed;
  bottom: 0;
  width: 100%;
  color: #333;
  font-size: 14px;
}
</style>
