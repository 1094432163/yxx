<template>
<div>
	<div class="form-group">
	
    <input v-model=searchinfo type="text" @keyup.enter="searchbytext" class="form-control"  placeholder="搜索">
		<b-button v-model=searchinfo @click="searchbytext">搜索</b-button>
  
	</div>




	<b-modal ref="myModalRef" hide-footer title="新建事项">
    <div class="d-block text-center">
      <b-form-input v-model="newText"
                  type="text"
                  placeholder="输入事项名称" @keyup.enter="addItem"></b-form-input>

	
	 <label for="myDate">选择您的日</label>
    <date-picker field="myDate"
                 placeholder="选择日期"
								 v-model="newdate"></date-picker>
      <b-form-select v-model="selected" class="mb-3">
	      <option v-for="list in lists" :value="list.title">{{ list.title }}</option>
	    </b-form-select>

    </div>
    <b-btn class="mt-3" variant="outline-danger" block @click="addItem">确定</b-btn>
  </b-modal>

  <table class="table table-striped">
	    <thead>
	    <tr>
	    	<th>check</th>
        <th>things</th>
        <th>date</th>
        <th>edit</th>
        <th>delete</th>
	    </tr>
	    </thead>
	    <tbody>
	    <tr v-for="(item,index) in items">
	        <td>
	        	<input  v-model="item.isfinished" class="form-check-input" type="checkbox" value="option1">
	        </td>
	        <td>
	        	<span v-if="!isupdate&&item.isfinished" style="text-decoration:line-through" >{{item.text}}</span>
						<span v-if="!isupdate&&!item.isfinished">{{item.text}}</span>
	        	<span v-if="isupdate">
	        		<input @keyup.enter="isupdate = false" type="text" name="" value="item.text" v-model="item.text">
	        	</span>
	        </td>
	        	<span v-if="!isupdate&&item.isfinished" style="text-decoration:line-through" >{{item.date}}</span>
						<span v-if="!isupdate&&!item.isfinished">{{item.date}}</span>
	        <td>
	        	<a href="#" @click="isupdate='ture'">编辑</a>
	        </td>
	        <td>
	        	<a href="#" @click="deleteItem(item.text)">删除</a>
	        </td>
	    </tr>
	    </tbody>
	</table>

    <b-button @click="showModal" class="btn btn-primary">
           新建待办事项
     </b-button>
</div>

</template>

<script>
import bus from '../assets/eventBus'
import myDatepicker from 'vue-datepicker-simple/datepicker-2.vue'; 
const toLower = text => {
  return text.toString().toLowerCase()
}

const searchByName = (items, term) => {
  if (term) {
    return items.filter(item => toLower(item.text).includes(toLower(term)))
  }
  return items
}

export default {

data() {
		return {
			title: 'all',
			newText: '',
			newdate: '',
			selected: '请选择清单',
			items: [],
			searchinfo: '',
			isupdate: false
		}
	},
	created() {
    this.init();
  },
	computed: {
		lists() {
			return this.$store.getters.lists;
		}
	},
 components:{
        'date-picker': myDatepicker
    },
	methods: {
		init() {
			if (this.title === 'all') {
				this.items = this.$store.getters.items
			}else {
				this.items = this.$store.getters.getItemsbytitle(this.title);
			}
		},
		showModal () {
      this.$refs.myModalRef.show()
    },
    hideModal () {
      this.$refs.myModalRef.hide()
    },
		addItem() {
			let newItem = {
				text: this.newText,
				date: this.newdate,
				belongto: this.selected,
				isfinished: false,
				isupdate:false
			}
			console.log(newItem.belongto)
			this.$store.commit('addItem', newItem);
			this.newText  ="";
			this.hideModal();
		},
		deleteItem(text) {
      this.$store.commit('deleteItem', text);
      this.init()
    },
    searchbytext() {
    	this.items = searchByName(this.$store.getters.items, this.searchinfo);
    }
	},
	mounted() {
    var self = this;
    bus.$on('LIstTitle', function(msg) {
        self.title = msg;
        self.init()
    });
  }

}

</script>

<style>


</style>