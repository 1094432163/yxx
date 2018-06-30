<template>

<b-container fluid>
      <b-row>
      
        <b-col class="todo-content" sm="9">
          <todo></todo>
        </b-col>
		  <b-col class="menus" sm="3">
          <menus></menus>
        </b-col>
      </b-row>
  </b-container>

</template>

<script>
import todo from './todo'
import menus from './menus'

export default {

	data() {

	},
	components: {
		todo,
		menus
	}

}


</script>

<style>

body {
  background: #00FF00 url(../assets/bg.jpg) repeat fixed top;
}

</style>
