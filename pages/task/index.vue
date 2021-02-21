<template>
    <div>
        <h2>Task List</h2>
        <TaskList :complete-flag="myCompleteFlag" />
    </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import TaskList from '~/components/TaskList.vue'

@Component({
    components: {
        TaskList,
    },
})
export default class IndexVue extends Vue {
    myCompleteFlag: boolean = false
}
</script>
