<template>
    <div class="container">
        <table class="table table-hover">
            <thead class="thead-light">
            <tr>
                <th scope="col">#</th>
                <th scope="col">Title</th>
                <th scope="col">Content</th>
                <th scope="col">Person In Charge</th>
                <th scope="col">Show</th>
                <th scope="col">Edit</th>
                <th scope="col">Delete</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
            
                <th scope="row">{{ task.id }}</th>
                <td>{{ task.title }}</td>
                <td>{{ task.content }}</td>
                <td>{{ task.person_in_charge }}</td>
                <td>
                    <router-link v-bind:to="{name: 'task.show', params: {taskId: task.id }}">
                        <button class="btn  btn-primary">show</button>
                    </router-link>
                </td>
                <td>
                    <router-link v-bind:to="{name: 'task.edit', params: {taskId: task.id }}">
                        <button class="btn btn-success">edit</button>
                    </router-link>
                </td>
                <td>
                    <button class="btn btn-danger" v-on:click="deleteTask(task.id)">delete</button>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        
        data: function () {
            return {
                tasks: []
            }
        },
        
        methods: {
            
            getTasks() {





                axios.get('/api/tasks')
                    .then((res) => {
                        this.tasks = res.data;
                    });


                    // ほとんどのエラーをキャッチ
                    Vue.config.errorHandler = function (err, vm, info) {
                        // 2.2.0 以降で使用可
                        alert(err);
                    }
                    // 残りのエラーをキャッチ
                    window.addEventListener("error", event => {
                        alert(event.error);
                    });
                    window.addEventListener("unhandledrejection", event => {
                        alert(event.reason);
                    });

                    /*
                    .catch(function (error){
                        if(error.reponse){
                            console.log(error.reponse.data);
                            console.log(error.reponse.status);
                            console.log(error.reponse.header);
                        } else if (error.request) {
                            // 要求がなされたが、応答が受信されなかった
                            console.log(error.request);
                        } else  {
                            // トリガーしたリクエストの設定に何かしらのエラーがある
                            console.log('Error', error.message);
                        }
                        console.log(error.config);
                    });
                    */



            },
            deleteTask(id) {
                axios.delete('/api/tasks/' + id)
                    .then((res) => {
                        this.getTasks();
                    });
            }
        },

        mounted() {
            window.intercepted.$on('response', data => {
                console.log(data); // { status: 404, code: 'Not found', body: null }
            });
            this.getTasks();
        }
    }
</script>