<template>
    <el-dialog
            title="数据结果"
            :visible.sync="dialogVisible"
            width="70%"
    >
        <div>
            {{jieguo}}
        </div>
        
    </el-dialog>
</template>

<script>
    import 'codemirror/lib/codemirror.css'
    import { codemirror } from 'vue-codemirror'

    require("codemirror/mode/javascript/javascript.js")

    export default {
        props: {
            data: Object,
        },
        data() {
            return {
                dialogVisible: false,
                jieguo:'没有结果',//存放数据结果，从后端读取后存到这里就行
            }
        },
        components: {
            codemirror
        },
        methods: {
            init() {
                this.dialogVisible = true;
            }
        }
    }
</script>
