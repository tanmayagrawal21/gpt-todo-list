<template>
    <div class="list-item">
        <!-- Task -->
        <div class="list-item__title">
            {{ task }}
        </div>
        <!-- subtasks : user editable or GPT generatable-->
        <div class="list-item__subtitle">
            <!-- iterate through subtasks-->
            <ol>
                <v-btn variant="outlined" v-if = "subtasks.length == 0">Generate subtasks</v-btn>
                <div v-else>
                    <li v-for="(subtask, index) in subtasks" :key="index">
                        {{ subtask }}
                    </li>
                </div>
            </ol>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ListItem',
    props: {
        task: String,
        subtasks: String
    }
}
</script>

<style scoped>
/* make the ol numbers appear next to the task */
.list-item__subtitle {
    display: inline-block;
    width: 100%;
    margin-left: 0;
    padding-left: 0;
}
</style>
