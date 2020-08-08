<template>
  <div>
    <div class="header text-white text-center pt-16 pb-32">

        <!-- Version -->
        <div class="absolute top-0 right-0 m-3 text-color">
          v2.0.1
        </div>
        <!-- / Version -->
        
        <Logo/>
        <h1 class="text-5xl leading-none">DontMindIT</h1>
        <h2 class="text-2xl text-color font-light">Clear your mind</h2>
        <!-- <div>{{ new Date() | moment('LL') }}</div> -->

        <!-- Date -->
        <div class="w-40 mx-auto mt-8">

          <div class="grid grid-cols-2 grid-rows-2">
            <div class="row-span-2 mt-1 text-6xl text-right mr-2 leading-10">{{ new Date().getDate() }}</div>
            <div class="text-2xl font-light text-left leading-none">{{ new Date() | moment('MMMM') }}</div>
            <div class="text-xl mt-1 font-bold text-left leading-none">{{ new Date().getFullYear() }}</div>  
          </div>

        </div>
        <!-- / Date -->
    </div>
    <div class="container mx-auto bg-white p-4 rounded relative -mt-8 shadow-lg">
      
      <div class="flex justify-between items-center pb-3 border-b-2 border-gray-200">

        <!-- Left -->
        <div>
          
          <div class="text-gray-500">
          
            <!-- Check all todos -->
            <button @click="checkAllTodos" class="mr-6">
              <font-awesome-icon class="text-sm" :icon="['fas', 'check']"/>
              <span class="ml-1 align-middle">Check all</span>
            </button>

            <!-- Delete all todos -->
            <button @click="deleteAllTodos" class="mr-6">
              <font-awesome-icon class="text-sm" :icon="['fas', 'trash']"/>
              <span class="ml-1 align-middle">Delete all</span>
            </button>

          </div>

        </div>
        
        <!-- Right -->
        <div>
          <transition name="fade">
            <AddForm
              v-if="isAddForm"
              @addItem="addItem"
              @close-form="toggleAddForm"
            />
          </transition>
        
          <button class="bg-color-prime text-white shadow-lg rounded-full w-10 h-10" @click="toggleAddForm">
            <font-awesome-icon :icon="['fas', 'plus']"/>
          </button>
        </div>
        

      </div>

      <div v-if="todos.length > 0">
        <draggable v-model="todos" :animation="150" ghost-class="ghost">
          <transition-group>
            <div
              :style="`border-left: 6px solid ${todo.color}`"
              :key="todo.name" v-for="(todo, index) in todos"
              class="bg-white flex flex-col md:flex-row flex-wrap md:flex-no-wrap content-between items-center justify-between p-4 rounded my-2 transition duration-300 ease-in-out shadow hover:shadow-md"
            >
              
              <div class="flex w-full">
                <!-- <span class="border-2 border-indigo-200 rounded p-2">{{ index + 1}}</span> -->
                
                <ItemCheckbox
                  @click="toggleCheck(index)"
                  :isChecked="todo.checked"
                >
                  <input
                    :class="[todo.checked ? 'line-through text-gray-500 italic' : 'no-underline']"
                    class="bg-transparent ml-3 truncate"
                    :disabled="!todo.editable"
                    @input="saveText(index)"
                    type="text"
                    :value="todo.name"
                  >
                </ItemCheckbox>
                
              </div>
              
              <div class="flex justify-end mt-3 md:mt-0 w-full">
                <!-- Deadline -->
                <span class="text-gray-500 mr-3">
                  <span :class="{'text-red-500' : getRemainingDays(todo.date) < 2}">
                    {{ getRemainingDays(todo.date) }} day{{ 2 > getRemainingDays(todo.date) ? '' : 's'}} remaining
                  </span>
                   - {{ todo.date }}
                </span>
                
                <!-- Delete item -->
                <button class="text-gray-500 text-sm" @click="deleteItem(index)">
                  <font-awesome-icon :icon="['fas', 'trash']"/>
                </button>
              </div>
              
            </div>
          </transition-group>
        </draggable>
      </div>

      <div class="text-center py-4 text-gray-500 italic">
        <span v-if="todos.length > 0">{{ todos.length }} task{{ todos.length > 1 ? 's': ''}} remaining on the list.</span>
        <span v-else>No more tasks to do</span>
      </div>

    </div>
    
    <TheFooter/>
  </div>
</template>

<script>
import Logo from './../components/Logo'
import ItemCheckbox from './../components/ItemCheckbox'
import TheFooter from './../components/TheFooter'
import AddForm from './../components/AddForm'
import draggable from 'vuedraggable'
import moment from 'moment'


export default {
  name: 'app',
  components: {
    Logo,
    ItemCheckbox,
    TheFooter,
    AddForm,
    draggable
  },
  data() {
    return {
      isAddForm: false,
      todos: [
        { date: '2020-01-01', color: '#F6648B', checked: false, editable: false, name: 'Say Hello to the World' },
        { date: '2020-08-08', color: '#a27bff', checked: false, editable: false, name: 'Learn something new' },
        { date: '2020-12-12', color: '#ffb748', checked: false, editable: false, name: 'Do some sport' },
      ],
    }
  },
  filters: {
    moment: function (date, format) {
      return moment(date).format(format)
    }
  },
  methods: {
    getRemainingDays(dueDate) {
      const oneDay = 24 * 60 * 60 * 1000
      const today = new Date()
      const secondDate = new Date(dueDate)

      const diffDays = Math.round(Math.abs((today - secondDate) / oneDay))
      return diffDays
    },
    addItem(value) {
      this.todos.push(value)
      this.isAddForm = false
    },
    checkAllTodos() {
      this.todos.map(x => {
        x.checked = true
      }) 
    },
    deleteAllTodos() {
      this.todos = []
    },
    saveText(index) {
      const value = event.target.value
      console.log(value)
      // this.todos[index].name = value
    },
    toggleAddForm() {
      this.isAddForm = !this.isAddForm
    },
    toggleCheck(index) {
      this.todos[index].checked = !this.todos[index].checked
    },
    editItem(index) {
      this.todos[index].editable = !this.todos[index].editable
    },
    deleteItem(index) {
      this.todos.splice(index, 1)
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/

body {
  background-color: aliceblue;
  font-family:
    'Prompt',
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
}

.header {
  background: rgb(31,28,227);
  background: linear-gradient(58deg, rgba(31,28,227,1) 0%, rgba(54,9,251,1) 42%, rgba(29,132,255,1) 100%);
}


.title {
  font-family:
    'Prompt',
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.text-color {
  color:#97c3e8;
}




/* Animations */
.fade-enter-active, .fade-leave-active {
  transition: all .2s ease-in-out;
  /* animation: from-top .5s ease-in-out; */
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translateY(-20px);
  opacity: 0;
}

</style>
