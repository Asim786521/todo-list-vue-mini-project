<script setup>
 
 import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
 

const todos_asc = computed(() => todos.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})

const addTodo = () => {
	if (input_content.value.trim() === ''  ) {
		return
	}

 todos.value.push({
		content: input_content.value,
 
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})

}

const removeTodo = (todo) => { 
  
  console.log("key",todo.editable)
  if (confirm(`are you want deleted the task ${todo.content}?`)) {
   
   
   todos.value = todos.value.filter((t) => t !== todo)
 } else {
   txt = "You pressed Cancel!";
 }
}

onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
 
</script>



 <template>
	<main class="app">
		
		<section class="greeting">
			<h2 class="title">
				What's up, <input type="text" id="name" placeholder="Name here" v-model="name">
			</h2>
		</section>

		<section class="create-todo">
			<h3> TODO DIARY</h3>

			<form id="new-todo-form" @submit.prevent="addTodo">
				<h4>make your  todo list?</h4>
				<input 
					type="text" 
					name="content" 
					id="content" 
					placeholder="type your thoughts..."
					v-model="input_content" />
				
			 

				<input type="submit" value="Add todo" />
			</form>
		</section>

		<section class="todo-list">
			<h3>TODO LIST</h3>
			<div class="list" id="todo-list">

				<div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
					<label>
						<input type="checkbox" v-model="todo.done" />
						 
					</label>

					<div class="todo-content">
						<h1 v-if="edit"><input type="text" style="color: red;" v-model="todo.content" /></h1>
	                 <h1 v-else> <input type="text"  v-model="todo.content"  readonly="{{ todo.content }}"/></h1>
						
					</div>
          <div class="actions">		
						<button class="edit" @click="edit=!edit">edit</button>
						
				
					</div>
					<div class="actions">
						<button class="delete" @click="removeTodo(todo)">Delete</button>
					</div>
				</div>

			</div>
		</section>

	</main>
</template>


<script>
export default {
  data() {
  	return {
	    edit: false
  	}
	}
}
</script>