<template>
    <el-dialog
            title="Jobs"
            :visible.sync="dialogVisible"
            width="70%"
    >

    <div class="ef-node-form">
            刷新<el-button style="margin: auto" @click="duqu" class="el-icon-refresh-right"></el-button>
            <div class="ef-node-form-header">
                Running Job
            </div>
            <div class="ef-node-form-body">
                <table  border="5">
                <tr>
                <th v-for="th in title1">{{th}} </th>
                    </tr>
                    <tr v-for = "i in RJ">
                    <td>{{i.JobName}}</td>
                    <td>{{i.StartTime}}</td>
                    <td>{{i.Duration}}</td>
                    <td>{{i.EndTime}}</td>
                    <td>{{i.Tasks}}</td>
                    <td>{{i.Status}}</td>
                    <button @click="quxiao($event,i.Jid)">取消</button>
                </tr>
                </table>
            </div>
            <div class="ef-node-form-header">
                Completed Job
            </div>
            <div class="ef-node-form-body">
                <table  border="5">
                <tr>
                <th v-for="th in title2">{{th}} </th>
                    </tr>
                    <tr v-for = "i in CJ">
                    <td>{{i.JobName}}</td>
                    <td>{{i.StartTime}}</td>
                    <td>{{i.Duration}}</td>
                    <td>{{i.EndTime}}</td>
                    <td>{{i.Tasks}}</td>
                    <td>{{i.Status}}</td>
                </tr>
                </table>
            </div>

        </div>
    </el-dialog>

</template>

<script>
    import 'codemirror/lib/codemirror.css'
    import { codemirror } from 'vue-codemirror'
    import request from "../../utils/request";

    require("codemirror/mode/javascript/javascript.js")

    export default {
        props: {
            data: Object,
        },
        data() {
            return {
                dialogVisible: false,
                title1:['JobName','StartTime','Duration','EndTime','Tasks','Status','编辑'],
                title2:['JobName','StartTime','Duration','EndTime','Tasks','Status'],
                //running job 列表
                RJ:[
                    //测试数据，实际使用注释，对象属性相同
                    {
                        Jid:'',
                        JobName:'test1',
                        StartTime:'11',
                        Duration:'12',
                        EndTime:'13',
                        Tasks:'14',
                        Status:'15',
                    },
                ],
                //complete job列表
                CJ:[
                    //测试数据，实际使用注释，对象属性相同
                    {
                        Jid:'',
                        JobName:'test2',
                        StartTime:'21',
                        Duration:'22',
                        EndTime:'23',
                        Tasks:'24',
                        Status:'25',
                    },
                ],
            }
        },
        components: {
            codemirror
        },
        methods: {
            init() {
                this.dialogVisible = true;
                this.duqu();
            },
            //取消任务
            quxiao(e,jid){
                //删除前端样式
                this.RJ=this.RJ.filter((i)=>{
                    return i.Jid!==jid;
                });
                //在这里写往后端发送消息删除
                request.post("/cancelJob",JSON.stringify(jid))
                  .then((res)=> {

                })
            },
            //读取数据(在这里写从后端读取数据)
            duqu(){
              this.RJ = [];
              this.CJ = [];
              request.get("/jobs")
                .then((res)=> {
                  for (let i = 0; i < res.length; i++) {
                    var tmp = {};
                    var job = res[i];
                    tmp.Jid = job.jid;
                    tmp.JobName = job.jobName;
                    tmp.StartTime = job.startTime;
                    tmp.EndTime = job.endTime;
                    tmp.Duration = job.duration;
                    tmp.Status = job.status;
                    if(job.status==="RUNNING"){
                      this.RJ.push(tmp);
                    }else{
                      this.CJ.push(tmp);
                    }
                  }
                  // console.log(res);
                  // console.log(this.RJ);
                })
            }
        },
        mounted:function(){
            this.duqu();
        }
    }
</script>
