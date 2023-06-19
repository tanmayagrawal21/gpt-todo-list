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
                <v-btn variant="outlined" v-if = "subTasksSizeZero()" :key="x" @click="generateSubtasks"> Generate subtasks</v-btn>
                <div v-else>
                    <li v-for="(subtask, index) in subTasksModifiable" :key="index">
                        {{ subtask }}
                    </li>
                </div>
            </ol>
        </div>
    </div>
</template>

<script>
import { OpenAI } from "langchain/llms/openai";
import { PromptTemplate } from "langchain/prompts";
import { LLMChain } from "langchain/chains";
console.log(process.env.VITE_OPENAI_KEY);
export default {
    name: 'ListItem',
    props: {
        task: String,
        subtasks: String
    },
    data () {
        this.subTasksModifiable = eval(this.subtasks);
        this.x = 0;
    },
    methods: {
        async generateSubtasks() {
            // model 
            const model = new OpenAI({
                openAIApiKey: process.env.VITE_OPENAI_KEY,
                temperature: 0.5
            });
            const template = `Generate a list of actions to ensure the proper completion of the following task: "{task}". The answer should be provided a list of comma separated values, without any numeric or alphabetical ordering`;

            const prompt = new PromptTemplate({
                template: template,
                inputVariables: ["task"],
            });

            const chain = new LLMChain({ llm: model, prompt:prompt });
            const data = await chain.call ({
                task: this.task
            });

            console.log(data);
            // split the data into a list of values
            const all_vals = data.text.split(",");
            // remove all values from subtasks
            for (let i = 0; i < this.subTasksModifiable.length; i++) {
                this.subTasksModifiable.pop();
            }
            for (let i = 0; i < all_vals.length; i++) {
                this.subTasksModifiable.push(all_vals[i]);
            }
            // a copy of tasks, and recache
            const tasks_cur = JSON.parse(localStorage.getItem('tasks')) || [];
            // remove the old task
            let i;
            for (i = 0; i < tasks_cur.length; i++) {
                if (tasks_cur[i].task == this.task) {
                    tasks_cur.splice(i, 1);
                    break;
                }
            }
            // add the new task at old index
            tasks_cur.splice(i, 0, {
                task: this.task,
                subtasks: this.subTasksModifiable
            });
            
            localStorage.setItem('tasks', JSON.stringify(tasks_cur));
        },
        subTasksSizeZero() {
            if (this.subTasksModifiable.length != 0) {
                this.x = +1;
            }
            return this.subTasksModifiable.length == 0;
        }
    }
}
</script>

<style scoped>
/* make the ol numbers appear next to the task */
.list-item__subtitle {
    display: inline-block;
    width: 100%;
    margin-left: 10vw;
    padding-left: 0;
}
</style>