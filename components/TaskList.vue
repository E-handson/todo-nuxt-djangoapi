<template>
    <div>
        <template v-if="!completeFlag">
            <FormVue @init-todo-list="getTodoList()" />
        </template>
        <v-container fluid>
            <v-simple-table>
                <thead>
                    <tr>
                        <th class="text-left">ID</th>
                        <th class="text-left">やること</th>
                        <th class="text-left">優先度</th>
                        <th class="text-center">ステータス更新</th>
                        <!-- 未完了画面のみ -->
                        <th v-if="!completeFlag" class="text-center">編集</th>
                        <th class="text-center">削除</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="task in taskList" :key="task.id">
                        <td>{{ task.id }}</td>
                        <td>{{ task.todo }}</td>
                        <td>{{ stringPriority(task.priority) }}</td>
                        <td v-if="!completeFlag" class="text-center">
                            <v-btn
                                class="ma-2"
                                color="primary"
                                dark
                                @click="completeClick(task.id)"
                            >
                                Complete
                                <v-icon dark right>
                                    mdi-checkbox-marked-circle
                                </v-icon>
                            </v-btn>
                        </td>
                        <td v-else class="text-center">
                            <v-btn
                                class="ma-2"
                                color="secondary"
                                dark
                                @click="completeClick(task.id)"
                            >
                                Incomplete
                                <v-icon dark right>
                                    mdi-checkbox-marked-circle
                                </v-icon>
                            </v-btn>
                        </td>
                        <!-- 未完了画面のみ -->
                        <td v-if="!completeFlag" class="text-center">
                            <v-btn
                                :to="'/task/' + task.id"
                                class="ma-2"
                                color="green"
                                dark
                            >
                                Edit
                                <v-icon dark right>
                                    mdi-file-document-edit
                                </v-icon>
                            </v-btn>
                        </td>
                        <td class="text-center">
                            <v-btn
                                class="ma-2"
                                color="red"
                                dark
                                @click="deleteClick(task.id)"
                            >
                                Delete
                                <v-icon dark right>mdi-cancel</v-icon>
                            </v-btn>
                        </td>
                    </tr>
                </tbody>
            </v-simple-table>
        </v-container>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'
import axios from 'axios'
import FormVue from '~/components/Form.vue'

interface Task {
    id: number
    todo: string
    priority: number
    completeFlag: boolean
}
interface Priority {
    [key: number]: string
}

axios.defaults.xsrfCookieName = 'csrftoken'
axios.defaults.xsrfHeaderName = 'X-CSRFTOKEN'

@Component({
    components: {
        FormVue,
    },
})
export default class TaskList extends Vue {
    taskList: Task[] = []
    priority: Priority = { 0: '小', 1: '中', 2: '大' }
    @Prop({ type: Boolean, required: true })
    completeFlag!: boolean

    created() {
        this.getTodoList()
    }

    getTodoList() {
        try {
            axios
                .get(process.env.API + '/tasks/', {
                    responseType: 'json',
                    params: { complete_flag: this.completeFlag },
                })
                .then((response) => {
                    this.taskList = response.data
                })
                .catch((error) => {
                    return error
                })
        } catch (e) {
            return e
        }
    }

    completeClick(id: string) {
        try {
            axios
                .patch(process.env.API + '/tasks/' + id + '/', {
                    complete_flag: !this.completeFlag,
                })
                .then(() => {
                    // リストを初期化
                    this.getTodoList()
                })
        } catch (e) {
            return e
        }
    }

    deleteClick(id: string) {
        try {
            axios.delete(process.env.API + '/tasks/' + id + '/').then(() => {
                // リストを初期化
                this.getTodoList()
            })
        } catch (e) {
            return e
        }
    }

    stringPriority(priority: number): string {
        return this.priority[priority]
    }
}
</script>
