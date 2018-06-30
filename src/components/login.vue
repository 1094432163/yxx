<template>

<div class="app-location">

  <b-modal ref="myModalRef" hide-footer title="输入信息">
    <div class="d-block text-center">
      信息输入有误！
    </div>
    <b-btn class="mt-3" variant="outline-danger" block @click="hideModal">确定</b-btn>
  </b-modal>

  <b-form-input v-model="username"
                  type="text"
                  placeholder="Enter your name"></b-form-input>

  <b-form-input v-model="password"
                  type="password"
                  placeholder="Enter your password"></b-form-input>

  <b-button @click="checkLogin" :size="size" :variant="variant">
                登陆
            </b-button>

</div>

</template>

<script>

export default{
  data() {
    return {
      username: '',
      password: '',
      isLogin: false
    }
  },
  computed: {
    users() {
      return this.$store.getters.users;
    }
  },
  methods: {
    showModal () {
      this.$refs.myModalRef.show()
    },
    hideModal () {
      this.$refs.myModalRef.hide()
    },
    checkLogin() {
        if ( this.username === 'yxx' && this.password === '123456') {
          this.$router.push('/todo');
        }else {
          this.showModal();
        }
      }
  }
}

</script>

<style>
.app-location {
  width: 500px;
  margin: 200px auto;
}

body {
  background: #00FF00 url(../assets/bg.jpg) repeat fixed top;
}



</style>
