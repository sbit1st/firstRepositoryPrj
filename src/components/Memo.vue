<template>
    <div class="memo">
        <div class="act">
            <button @click="add()" class="btn btn-primary"> + 추가 </button>
        </div>
        <ul>
            <li v-for="(item) in state.data" :key="item.id" @click="edit(item.id)"> {{item.content}} </li>
        </ul>
    </div>
</template>

<script>
import {reactive} from "vue";
import axios from 'axios';

export default {
    setup() {
        const state = reactive({
            data : []
        });
        
        const add = () => {
            const content = prompt("내용을 입력해주세요.");
            
            if(!content) {
                alert('내용을 입력해주세요');
                return add();
            }

            axios.post("/api/memos", {content:content}).then((res) => {
                state.data = res.data;
            })
        };

        const edit = (id) => {
            const content = prompt("내용을 입력해주세요", state.data.find(item=>item.id === id).content);
            axios.put('/api/memos/' + id, {content:content}).then((res) => {
                state.data = res.data;
            });
        };

        

        axios.get("/api/memos").then((res) => {
            state.data = res.data;
        });

        return {state, add, edit};
    },
};
</script>

<style lang="scss" scoped>
.memo {
    .act {
        padding: 10px 10px 5px 5px;
        text-align: right;
    }

    ul {
        border-top: 1px solid #eee;
        list-style: none;
        padding: 15px;
        margin: 0;

        li {
            padding: 15px;
            margin: 10px;
            border: 1px solid #eee;
        }
    }
}


</style>