<template>
	<div>
		<b-list-group>

			<b-list-group-item href="#" @click="toLogin">
		  	注销
		  </b-list-group-item>

			<b-list-group-item href="#" @click='goList("all")'>
		  	All
		  </b-list-group-item>

		  <b-list-group-item href="#" v-for="list in lists" @click="goList(list.title)">
		  	<span v-if="!isupdate">{{ list.title }}</span>
      	<span v-if="isupdate">
      		<b-form-input v-model="list.title"
            type="text"
            placeholder="title"></b-form-input>
           <b-button style="float: right" @click="isupdate = false" :size="size" :variant="variant">
                确定
            </b-button>
      	</span>
      	<b-button style="float: right" @click="isupdate = true" :size="size" :variant="variant">
                编辑
            </b-button>
		  </b-list-group-item>

		  <b-list-group-item href="#">
		  	<input type="text" v-model="newtitle" name="" v-if="isAdd" @keyup.enter="addList">
		  </b-list-group-item>

		  <b-list-group-item href="#" @click="isAdd = true">
		  	+新建清单
		  </b-list-group-item>

		</b-list-group>
	</div>
</template>

<script>
import bus from '../assets/eventBus'

export default{
	data() {
		return {
			name: 'lj',
			newtitle: '',
			isAdd: false,
			isupdate: false
		}
	},
	computed: {
		lists() {
			return this.$store.getters.lists;
		}
	},
	methods: {
		addList() {
			let newList = {
				title: this.newtitle,
				count: 0
			}
			this.$store.commit('addList', newList)
			this.newtitle = "";
			this.isAdd = false
		},
		goList(title) {
      bus.$emit('LIstTitle', title)
    },
    toLogin() {
      this.$router.push('/');
    }
	}
}

</script>

<style>

</style>
