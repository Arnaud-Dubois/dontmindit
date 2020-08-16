<template>
    <div class="absolute bg-white rounded-lg top-0 right-0 p-8 shadow-xl">
        <button class="absolute text-gray-500 top-0 right-0 m-3" @click="$emit('close-form')">
            <font-awesome-icon :icon="['fas', 'times']"/>
        </button>
        <div class="container mx-auto">
            <div>
                
                <!-- Color picker -->
                <v-swatches
                    v-model="color"
                    :swatches="swatches"
                    shapes="circles"
                    popover-x="left"
                    row-length="5"
                    class="inline-block mr-3 align-middle"
                >
                </v-swatches>
                <!-- / Color picker -->

                <!-- Date picker -->
                <date-pick
                    v-model="date"
                    class="inline-block border-2 mr-0 md:mr-3 rounded-full w-32 px-4 py-2"
                >
                </date-pick>
                <!-- / Date picker -->
                
                <!-- Task title -->
                <input
                    class="bg-gray-200 rounded-full w-full md:w-auto px-4 py-2 mr-0 md:mr-3 mt-6 md:mt-0"
                    type="text"
                    placeholder="Type something here.."
                    v-model="form.name"
                >
                <!-- / Task title -->

                <button
                    class="bg-color-prime w-full md:w-auto text-white rounded-full px-4 py-2 mt-6 md:mt-0"
                    @click="createItem"
                >
                    Add task
                </button>

            </div>
        </div>
    </div>
</template>

<script>
import DatePick from 'vue-date-pick';

export default {
    name: 'add-form',
    components: {
        DatePick
    },
    data() {
        return {
            color: '#f64272',
            swatches: [
                ['#F64272', '#F6648B', '#F493A7', '#F891A6', '#FFCCD5' ],
                ['#8b5aff', '#a27bff', '#b99cff', '#d0bdff', '#e8deff' ],
                ['#51e5db', '#74ebe3', '#96f0ea', '#b9f5f1', '#dcfaf8' ],
                ['#ffa51a', '#ffb748', '#ffc976', '#ffdba3', '#ffedd1' ]
            ],
            date: '2020-08-01',
            form: {
                name: ''
            },            
            error: false
        }
    },
    methods: {
        createItem() {
            if(this.form.name !== '') {
                this.error = false
                let value = {
                    color: this.color,
                    date: this.date,
                    checked: false,
                    editable: false,
                    name: ''
                }
                value.name = this.form.name
                this.$emit('addItem', value)
                
                this.form.name = ''
            } else {
                this.error = true
            }
        },
    }
}
</script>

<style lang="scss">
    $vdpColor: rgb(31,28,227);
    @import 'vue-date-pick/src/vueDatePick.scss';

    /* DatePick custom style */
    .vdpComponent {
        font-size: 1rem;
    }
    .vdpComponent input {
        width: 100%;
    }

</style>