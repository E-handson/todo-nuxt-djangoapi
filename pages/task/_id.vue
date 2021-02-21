<template>
    <div>
        <h2>Task Edit - ID {{ $route.params.id }}</h2>
        <FormVue
            :init-priority="task.priority"
            :init-todo="task.todo"
            :param-id="$route.params.id"
            @init-todo-list="redirectTodo()"
        />
    </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from 'axios'
import FormVue from '~/components/Form.vue'

axios.defaults.xsrfCookieName = 'csrftoken'
axios.defaults.xsrfHeaderName = 'X-CSRFTOKEN'

interface Task {
    id: Number | null
    todo: String | ''
    priority: Number | null
    completeFlag: Boolean | null
}

@Component({
    components: {
        FormVue,
    },
    validate({ params }) {
        // 数値のカテゴリIDじゃないと404にする
        return /^\d+$/.test(params.id)
    },
})
export default class IdVue extends Vue {
    task?: Task = {
        id: null,
        todo: '',
        priority: null,
        completeFlag: null,
    }

    created() {
        this.getTodo()
    }

    getTodo() {
        try {
            axios
                .get(
                    process.env.API + '/tasks/' + this.$route.params.id + '/',
                    { responseType: 'json' }
                )
                .then((response) => {
                    this.task = response.data
                })
                .catch((error) => {
                    return error
                })
        } catch (e) {
            return e
        }
    }

    redirectTodo() {
        this.$router.push('/task')
    }
}
</script>
