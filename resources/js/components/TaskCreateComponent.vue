<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-sm-6">
                <ValidationObserver ref="observer" v-slot="{ invalid }">
                <form v-on:submit.prevent="submit">
                    <!-- 
                    <div class="form-group row">
                        <label for="name" class="col-sm-3 col-form-label">name</label>
                        <ValidationProvider 
                            name="name" 
                            rules="required|min:6|max:20" 
                            v-slot="{ errors }"
                        >
                            <input type="text" name="name" class="col-sm-9 form-control" v-model="task.name">
                            <div class="text-danger">{{ errors[0] }}</div>
                        </ValidationProvider>
                    </div>
                    -->

                    <div class="form-group row">
                        <label for="title" class="col-sm-3 col-form-label">Title</label>
                        <ValidationProvider 
                            name="title" 
                            rules="required|min:5|max:30" 
                            v-slot="{ errors }"
                        >
                            <input type="text" name="title" class="col-sm-9 form-control" v-model="task.title">
                            <div class="text-danger">{{ errors[0] }}</div>
                        </ValidationProvider>
                    </div>

                    <div class="form-group row">
                        <label for="content" class="col-sm-3 col-form-label">Content</label>
                        <ValidationProvider 
                            name="content" 
                            rules="required|min:10|max:50" 
                            v-slot="{ errors }"
                        >
                            <input type="text" class="col-sm-9 form-control" name="content" v-model="task.content">
                            <div class="text-danger">{{ errors[0] }}</div>
                        </ValidationProvider>
                    </div>

                    <div class="form-group row">
                        <label for="person-in-charge" class="col-sm-3 col-form-label">Person In Charge</label>
                        <input type="text" id="person-in-charge" class="col-sm-9 form-control" v-model="task.person_in_charge">
                    </div>

                    <div>
                        <button type="submit" :disabled="invalid" class="btn btn-primary">Submit</button>
                    </div>
                </form>
                </ValidationObserver>
            </div>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'
    import {
        ValidationObserver,
        ValidationProvider,
        localize,
        extend,
    } from 'vee-validate'

  // ルール設定
    import * as rules from 'vee-validate/dist/rules'
    for (let rule in rules) {
        extend(rule, rules[rule])
    }
    
  // 個別にextendするなら
  // import { required, min, max, email } from 'vee-validate/dist/rules'
  // extend('required', required)
  // extend('min', min)
  // extend('max', max)
  // extend('email', email)

  // 日本語化
    import ja from 'vee-validate/dist/locale/ja'
    localize('ja', ja)

    // コンポーネント設定
    Vue.component('ValidationProvider', ValidationProvider)
    Vue.component('ValidationObserver', ValidationObserver) 




    export default {
        data: function () {
            return {
                task: {}
            }
        },
        methods: {
            submit() {

                this.$refs.observer.validate().then((result) => {
                    if(result) {
                        // OK
                        axios.post('/api/tasks', this.task)
                        .then((res) => {
                            this.$router.push({name: 'task.list'});
                        });
                    }
                })

            }
        }
    }
</script>