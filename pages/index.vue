<template>
  <div>
    <TheHeader/>
    <div class="container mx-auto bg-white p-4 rounded relative -mt-8 shadow-lg">
      <div class="flex justify-between items-center pb-3 border-b-2 border-gray-200">

        <!-- Left options -->
        <div>
          <div class="text-gray-500">
          
            <!-- Check all tasks -->
            <button @click="checkAllTodos" class="mr-6">
              <font-awesome-icon class="text-sm" :icon="['fas', 'check']"/>
              <span class="ml-1 align-middle">Check all</span>
            </button>

            <!-- Delete all tasks -->
            <button @click="deleteAllTodos" class="mr-6">
              <font-awesome-icon class="text-sm" :icon="['fas', 'trash']"/>
              <span class="ml-1 align-middle">Delete all</span>
            </button>

          </div>
        </div>
        <!-- / Left options -->
        
        <!-- Right options -->
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
        <!-- / Right options -->

      </div>

      <div v-if="tasks.length > 0">
        <draggable v-model="tasks" :animation="150" ghost-class="ghost">
          <transition-group>
            <div
              :style="`border-left: 6px solid ${task.color}`"
              :key="task.name" v-for="(task, index) in tasks"
              class=" cursor-pointer bg-white flex flex-col md:flex-row flex-wrap md:flex-no-wrap content-between items-center justify-between p-4 rounded my-2 transition duration-300 ease-in-out shadow hover:shadow-md"
            >
              
              <div class="flex w-full">
                <TaskCheckbox
                  @click="toggleCheck(index)"
                  :isChecked="task.checked"
                >
                  <input
                    :class="[task.checked ? 'line-through text-gray-500 italic' : 'no-underline']"
                    class="bg-transparent ml-3 truncate"
                    :disabled="true"
                    type="text"
                    :value="task.name"
                  >
                </TaskCheckbox>
              </div>
              
              <div class="flex justify-end mt-3 md:mt-0 w-full">

                <!-- Deadline -->
                <span class="text-gray-500 mr-3">
                  <span :class="{'text-red-500' : getRemainingDays(task.date) < 2}">
                    {{ getRemainingDays(task.date) }} day{{ 2 > getRemainingDays(task.date) ? '' : 's'}} remaining
                  </span>
                   - {{ task.date }}
                </span>
                <!-- / Deadline -->
                
                <!-- Delete item -->
                <button class="text-gray-500 text-sm" @click="deleteItem(index)">
                  <font-awesome-icon :icon="['fas', 'trash']"/>
                </button>
                <!-- / Delete item -->

              </div>
              
            </div>
          </transition-group>
        </draggable>
      </div>

      <div class="text-center py-4 text-gray-500 italic">
        <span v-if="tasks.length > 0">{{ tasks.length }} task{{ tasks.length > 1 ? 's': ''}} remaining on the list.</span>
        <span v-else>No more tasks to do</span>
      </div>

    </div>
    
    <TheFooter/>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'app',
  components: {
    draggable
  },
  data() {
    return {
      isAddForm: false,
      tasks: [
        { date: '2020-09-02', color: '#F6648B', checked: false, name: 'Say Hello to the World' },
        { date: '2020-08-08', color: '#a27bff', checked: false, name: 'Learn something new' },
        { date: '2020-12-12', color: '#ffb748', checked: false, name: 'Do some sport' },
      ],
    }
  },
  methods: {

    getRemainingDays(dueDate) {
      const oneDay = 24 * 60 * 60 * 1000
      const today = new Date()
      const secondDate = new Date(dueDate)

      let daysRemaining = Math.round((secondDate - today) / oneDay)
      if (daysRemaining < 0) {
        daysRemaining = 0
      }
      return daysRemaining
    },

    addItem(value) {
      this.tasks.push(value)
      this.isAddForm = false
    },

    checkAllTodos() {
      this.tasks.map(x => {
        x.checked = true
      }) 
    },

    deleteAllTodos() {
      this.tasks = []
    },

    toggleAddForm() {
      this.isAddForm = !this.isAddForm
    },

    toggleCheck(index) {
      this.tasks[index].checked = !this.tasks[index].checked
    },

    deleteItem(index) {
      this.tasks.splice(index, 1)
    }

  }
}
</script>

<style lang="scss">

/* DontMindIT theme */
$primaryColor: rgb(31,28,227);
$textColor: #97c3e8;

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

.text-color {
  color: $textColor;
}

/* Animations */
.fade-enter-active, .fade-leave-active {
  transition: all .2s ease-in-out;
}
.fade-enter, .fade-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}

</style>
