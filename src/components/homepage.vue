<template>
  <div class="login-page">
    <div class="form-container">
      <h1>{{ loginTitle }}</h1>
      <form @submit.prevent="login">
        <label for="login-username">Username:</label>
        <input type="text" id="login-username" v-model="loginData.loginPhoneNumber" required>

        <label for="login-password">Password:</label>
        <input type="password" id="login-password" v-model="loginData.loginPassword" required>

        <button type="submit">Login</button>
      </form>
    </div>


    <div class="form-container">
      <h1>{{ registerTitle }}</h1>
      <form @submit.prevent="registerMember">
      <label for="username">Username:</label>
      <input type="text" id="username" v-model="formData.UserName" required>

      <label for="phone">Phone Number:</label>
      <input type="tel" id="phone" v-model="formData.PhoneNumber" required>

      <label for="password">Password:</label>
      <input type="password" id="password" v-model="formData.Password" required>

      <button type="submit">Register</button>
    </form>
     
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'MyHomePage',
  data() {
    return {

      loginData: {
         loginPhoneNumber: '',
         loginPassword: '',
      },

      formData: {
        UserName: '',
        PhoneNumber: '',
        Password: '',
      },
    };
  },
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
},
methods: {
  async login() {
    try {
      const response = await axios.post('http://localhost:8005/library/login', this.loginData, {
        headers: {
          'Content-Type': 'application/json',
        },
      });

      console.log('登入資料:', response.data);

      if (response.data.status === 'login ok') {
        localStorage.setItem('sessionId', response.data.sessionId);
        //this.user = response.data.user;
        this.userId=response.data.userId;
        alert('登入成功');
        this.$router.push('/booklist');
    
        console.log(this.userId);

        this.$router.push({path: '/booklist',
            query: { userId: this.userId }
          });

      } else {
        alert('帳號密碼有誤，請重試。');
      }
    } catch (error) {
      console.error('登入失敗:', error.response || error);
      alert('登入失敗，請重試。');
    }
  },

  

    async registerMember() {
      console.log('注册資料:', JSON.stringify(this.formData)); // 使用 JSON.stringify

      try {
        await axios.post('http://localhost:8005/library/regist', this.formData, {
            headers: {
                'Content-Type': 'application/json',
            },
        });
        // 处理成功响应，你可以根据实际需求进行相应的处理
        alert('注册成功，請重新登入系統。');
        window.location.reload();
      } catch (error) {
        // 处理注册错误
        console.error('註冊失敗:', error.response || error);
        alert('註冊失敗，請重试。');
      }
    },



  },
};
</script>


<style scoped>
  .login-page {
    display: flex;
    flex-direction: column;
    align-items: center;
    max-width: 600px;
    margin: auto;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: center;
  }

  .form-container {
    width: 50%;
    margin-bottom: 20px;
  }

  h1 {
    color: #333;
  }

  form {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  label {
    margin-bottom: 8px;
  }

  input {
    padding: 8px;
    margin-bottom: 16px;
    width: 80%;
    box-sizing: border-box;
  }

  button {
    background-color: #42b983;
    color: #fff;
    padding: 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #368b7d;
  }
</style>
