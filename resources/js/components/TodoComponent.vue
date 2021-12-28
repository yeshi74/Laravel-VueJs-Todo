<template>
    <div class="w-25">

        <form @submit.prevent="saveData">
            <div class="input-group mb-3 w-100">
                <input v-model="form.title" :class="{ 'is-invalid' : form.errors.has('title') }" type="text" class="form-control form-control-lg" @keydown="form.errors.clear('title')"
                aria-label="Recipient's username" aria-describedby="button-addon2">
                    <button class="btn btn-success" type="submit" id="button-addon2">Add this to your list</button>
            </div>
            <span class="text-danger pt-3 pb-3" style="font-size:20px;" v-if="form.errors.has('title')" v-text="form.errors.get('title')">

            </span>
        </form>

        <div class="w-25">
            <div v-for="todo in todos" :key="todo.id" class="w-100" style="font-size:18px;">
                {{ todo.title }}
            </div>
        </div>


    </div>
</template>

<script>
    export default {
        data(){
            return {
                todos:'',
                form: new Form({
                    title: '',
                })
            }
        },
        methods: {
            getTodos(){
                axios.get('/api/todo').then((res) => {
                    this.todos = res.data
                }).catch((error) => {
                    console.log(error)
                })
            },

            saveData(){
                let data = new FormData();
                data.append('title', this.form.title)
                axios.post('/api/todo', data).then((res) => {
                    this.form.reset()
                    this.getTodos()
                }).catch((error) => {
                    this.form.errors.record((error.response.data.errors))
                })
            }
        },
        mounted() {
            this.getTodos()
        }
    }
</script>
