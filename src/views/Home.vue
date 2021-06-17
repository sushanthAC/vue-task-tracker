<template>
    <div v-show="showTaskForm">
        <AddTask  @add-newTask="addNewTask"/>
    </div>
    <Tasks @toggle-reminder="toggleReminder" @delete-task ="deleteTask" :tasks="tasks"/>
</template>

<script>
    import Tasks from '../components/Tasks.vue';
    import AddTask from '../components/AddTask.vue';
    export default {
        name:"Home",
        props: {
            showTaskForm:Boolean
        },
        components:{
            Tasks,
            AddTask
        }, 
        data() {
            return {
                tasks:[]
            }
        },
         methods:{
            async deleteTask(id) {
                if (confirm('Are you sure')) {
                    const res = await fetch(`/api/tasks/${id}`, {
                        method: 'DELETE'
                    });
                    res.status == 200 ? this.tasks = this.tasks.filter(task => task.id !== id):alert('Somthing went wrong');
                    
                }
            },

            async toggleReminder(id){
                const taskToggle = await this.fetchSingleTask(id);
                const updatedTask = { ...taskToggle, reminder:!taskToggle.reminder};

                await fetch(`/api/tasks/${id}`, {
                    method: 'PUT',
                    headers:{
                    'Content-type': 'application/json'
                    },
                    body: JSON.stringify(updatedTask)
                });

                this.tasks = this.tasks.map(task => task.id === id ? { ...task, reminder: !task.reminder}:task)
            }, 

            async addNewTask(newTask){
                const res = await fetch('/api/tasks', {
                    method:"POST",
                    headers: {
                        'Content-type':"application/json"
                    },
                    body: JSON.stringify(newTask)
                });
                const data = await res.json();
                this.task = this.tasks.push(data);
            },

            async fetchTask() {
                const res = await fetch('/api/tasks');
                const data =  await res.json();
                return data;
            },

            async fetchSingleTask(id) {
                const res = await fetch(`/api/tasks/${id}`);
                const data =  await res.json();
                return data;
            }
            
        },
        async created() {
            this.tasks = await this.fetchTask();
        }
    }
</script>

<style scoped>

</style>