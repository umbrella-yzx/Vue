<template>
  <div>
    <el-dialog
            title="Jobs"
            :visible.sync="dialogVisible"
            width="70%"
    >

    <div class="ef-node-form">
            刷新<el-button style="margin: auto" @click="duqu" class="el-icon-refresh-right"></el-button>
            <div class="ef-node-form-header">
              Jars
            </div>
            <div class="ef-node-form-body">
              <table  border="5">
                <tr>
                  <th v-for="th in title3">{{th}} </th>
                </tr>
                <tr v-for = "i in JARS">
                  <td>{{i.id}}</td>
                  <td>{{i.name}}</td>
                  <td>{{i.uploaded}}</td>
                  <button @click="zhixingJars($event,i.id)">执行</button>
                  <button @click="delJars($event,i.id)">删除</button>
                </tr>
              </table>
            </div>

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

                    <td><input type="number" v-model="i.cycle_day" style="width:50px">day
                    <input type="number" v-model="i.cycle_hour" style="width:50px">hour
                    <input type="number" v-model="i.cycle_minute" style="width:50px">minute
                    <input type="number" v-model="i.cycle_second" style="width:50px">second
                    </td>

                    <button @click="quxiao($event,i.Jid)">取消</button>
                    <button @click="showMessage($event,i.JobName)">显示结果</button>

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
                    <td><input type="number" v-model="i.cycle_day" style="width:50px">day
                    <input type="number" v-model="i.cycle_hour" style="width:50px">hour
                    <input type="number" v-model="i.cycle_minute" style="width:50px">minute
                    <input type="number" v-model="i.cycle_second" style="width:50px">second
                    </td>
                    <button @click="qidong($event,i.Jid)">启动</button>
                </tr>
                </table>
            </div>
        </div>
    </el-dialog>
    <datasesultwindow v-if="windowDataVisible" ref="datasesultwindow" :data="data"></datasesultwindow>
  </div>
</template>

<script>
    import 'codemirror/lib/codemirror.css'
    import { codemirror } from 'vue-codemirror'
    import request from "../../utils/request";
    import datasesultwindow from '@/components/ef/dataresultwindow';

    require("codemirror/mode/javascript/javascript.js")

    export default {
        props: {
            data: Object,
        },
        data() {
            return {
                //控制窗口结果显示
                windowDataVisible:false,
                //信息数据
                data:{},
                dialogVisible: false,
                title1:['JobName','StartTime','Duration','EndTime','Tasks','Status','Cycle','操作',],
                title2:['JobName','StartTime','Duration','EndTime','Tasks','Status','Cycle','操作',],
                title3:['id','name','uploaded','操作',],
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
                        //周期时间
                        cycle_day:'5',
                        cycle_hour:'2',
                        cycle_minute:'59',
                        cycle_second:'59',
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
                        //周期时间
                        cycle_day:'5',
                        cycle_hour:'2',
                        cycle_minute:'59',
                        cycle_second:'59',
                    },
                ],
                JARS:[
                    //测试数据，实际使用注释，对象属性相同
                    {
                        id:'31',
                        name:'32',
                        uploaded:'33'
                    },
                ]
            }
        },
        components: {
            codemirror,datasesultwindow
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

            showMessage(e,jobName){
              request.post("/getMessage",JSON.stringify(jobName))
                .then((res)=> {
                  console.log(res);
                  this.data = res;
                })
              this.windowDataVisible = true
              this.$nextTick(function () {
                this.$refs.datasesultwindow.init()
              })
            },
            //启动任务
            qidong(e,jid){
                //在这里写往后端发送消息启动********************************








            },
            //删除jars
            delJars(e,id){
                this.JARS=this.JARS.filter((i)=>{
                    return i.id!==id;
                });
                //在这里写往后端发送消息删除****************************************
              request.post("/deleteJar",JSON.stringify(id).toString()).then((res)=>{
                console.log(res)
              });
            },
            //执行jars
            zhixingJars(e,id){
                //在这里写往后端发送消息执行*******************************************
              request.post("/runJar",JSON.stringify(id).toString()).then((res)=>{
                console.log(res)
              });
            },
            //读取数据(在这里写从后端读取数据)
            duqu(){
              this.RJ = [];
              this.CJ = [];
              this.JARS=[];
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
                })
              request.get("/getJars")
                .then((res)=> {
                  for (let i = 0; i < res.length; i++) {
                    var tmp = {};
                    var jar = res[i];
                    tmp.id = jar.id;
                    tmp.name = jar.name;
                    tmp.uploaded = jar.uploaded;
                    this.JARS.push(jar);
                  }
                })
            }
        },
        mounted:function(){
            this.duqu();
        },
        watch:{
            //监听修改Running job
            RJ:{
                deep:true,
                handler(newValue,oldValue){
                    if(newValue.length>0){
                        newValue.forEach((i)=> {
                            if(i.cycle_day<=0) i.cycle_day=0;
                            if(i.cycle_hour<=0) i.cycle_hour=0;
                            if(i.cycle_minute<=0) i.cycle_minute=0;
                            if(i.cycle_second<=0) i.cycle_second=0;
                            if(i.cycle_hour>=23) {
                                i.cycle_hour=23;
                            };
                            if(i.cycle_minute>=59){
                                i.cycle_minute=59;
                            };
                            if(i.cycle_second>=59){
                                i.cycle_second=59;
                            }
                        });
                    }
                    //周期时间已修改（请在下面写返回周期时间到后端）************************************





                    }
            },
            //监听修改complete job
            CJ:{
                deep:true,
                handler(newValue,oldValue){
                    if(newValue.length>0){
                        newValue.forEach((i)=> {
                            if(i.cycle_day<=0) i.cycle_day=0;
                            if(i.cycle_hour<=0) i.cycle_hour=0;
                            if(i.cycle_minute<=0) i.cycle_minute=0;
                            if(i.cycle_second<=0) i.cycle_second=0;
                            if(i.cycle_hour>=23) {
                                i.cycle_hour=23;
                            };
                            if(i.cycle_minute>=59){
                                i.cycle_minute=59;
                            };
                            if(i.cycle_second>=59){
                                i.cycle_second=59;
                            }
                        });
                    }
                    //周期时间已修改（请在下面写返回周期时间到后端）********************************






                    }
            }
        },
    }
</script>
