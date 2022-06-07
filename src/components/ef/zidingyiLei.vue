<template>
    <el-dialog
            title="自定义类"
            :visible.sync="dialogVisible"
            width="70%"
            @close="entityPost"
    >

        <div>
            <div class="ef-node-form">
            <div class="ef-node-form-header">
                已定义类
            </div>
            <div class="ef-node-form-body">
                <el-form  ref="dataForm" label-width="80px" >
                    <div v-for="item in Leilist">
                        <el-button @click="ShowL($event,item.name)">编辑</el-button>
                        <el-button @click="DelL($event,item.name)">删除</el-button>
                        类:{{item.name}}-----
                        <article style="display:inline;" v-for="iz in item.ziduan">{{iz.name}}:{{iz.type}}&nbsp;&nbsp;&nbsp;</article>
                    </div>
                </el-form>
            </div>
            <div class="ef-node-form-header">
                类定义
            </div>
            <div class="ef-node-form-body">
                <el-form  ref="dataForm" label-width="80px" >
                    <el-form-item label="类名">
                        <el-input v-model="L.name" placeholder="请输入"></el-input>
                    </el-form-item>
                    <el-form-item label="字段">
                        <div v-for="item in L.ziduan">
                        <el-button @click="ShowZD($event,item.name)">编辑</el-button>
                        <el-button @click="DelZD($event,item.name)">删除</el-button>
                        字段:{{item.name}}--{{item.type}}
                    </div>
                    </el-form-item>
                    <div style="text-align:center">
                        <el-button @click="AddLei">保存类</el-button>
                        <el-button @click="LClear">清空</el-button>
                    </div>
                </el-form>
            </div>
            <div class="ef-node-form-header">
                字段定义
            </div>
            <div class="ef-node-form-body">
                <el-form  ref="dataForm" label-width="80px" >
                    <el-form-item label="字段名">
                        <el-input v-model="ZD.name" placeholder="请输入"></el-input>
                    </el-form-item>
                    <el-form-item label="字段类型">
                    <el-select v-model="ZD.type" placeholder="请选择">
                                <el-option
                                    v-for="item in typeList"
                                    :key="item.state"
                                    :label="item.label"
                                    :value="item.state">
                                </el-option>
                    </el-select>
                    </el-form-item>
                    <div style="text-align:center">
                        <el-button @click="AddZD">保存字段</el-button>
                        <el-button @click="ZDClear">清空</el-button>
                    </div>
                </el-form>
            </div>
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
                //类列表
                Leilist:[],
                //类
                L:{
                    name:'',
                    ziduan:[],
                },
                //字段
                ZD:{
                    name:'',
                    type:'',
                },
                typeList:[
                    {
                        state:'Byte',
                        label:'Byte',
                    },
                    {
                        state:'Short',
                        label:'Short',
                    },
                    {
                        state:'Int',
                        label:'Int',
                    },
                    {
                        state:'Long',
                        label:'Long',
                    },
                    {
                        state:'Boolean',
                        label:'Boolean',
                    },
                    {
                        state:'Float',
                        label:'Float',
                    },
                    {
                        state:'Double',
                        label:'Double',
                    },
                    {
                        state:'String',
                        label:'String',
                    },
                    {
                        state:'ByteArray',
                        label:'ByteArray',
                    },
                    {
                        state:'Date',
                        label:'Date',
                    },
                    {
                        state:'List',
                        label:'List',
                    },
                ]
            }
        },
        components: {
            codemirror
        },
        methods: {
            init() {
                this.dialogVisible = true;
                this.Load();
            },
            //清空类
            LClear(){
                this.L.name='';
                this.L.ziduan=[];
            },
            //清空字段
            ZDClear(){
                this.ZD.name='';
                this.ZD.type='';
            },
            //将类加入列表
            AddLei(){
                if(this.L.name!=''&&this.L.ziduan.length!=0){
                    var temp=JSON.parse(JSON.stringify(this.L));
                    var s=1;
                    if(this.Leilist){
                    this.Leilist.forEach((i)=>{
                        if(i.name===this.L.name){
                            s=0;
                            i.ziduan=this.L.ziduan;
                            alert(this.L.name+"类已经修改为最新值")
                            this.LClear();
                        }
                    });
                    }
                    if(s===1){
                    if(!this.Leilist) this.Leilist=[];
                    this.Leilist.push(temp);
                    this.LClear();
                    }
                }
                else{
                    alert("类名和字段定义不能为空");
                };
                this.Store();
            },
            //将字段加入列表
            AddZD(){
                if(this.ZD.name!=''&&this.ZD.type!=''){
                    var temp=JSON.parse(JSON.stringify(this.ZD));
                    var s=1;
                    if(this.L.ziduan){
                    this.L.ziduan.forEach((i)=>{
                        if(i.name===this.ZD.name)
                        {
                            s=0;
                            i.type=this.ZD.type;
                            alert(this.ZD.name+"字段已修改为最新值");
                            this.ZDClear();
                        }
                    })}
                    if(s===1){
                    this.L.ziduan.push(temp);
                    this.ZDClear();
                    }
                }
                else{
                    alert("字段名称和类型不能为空");
                }
            },
            //展示字段
            ShowZD(e,name){
                this.L.ziduan.forEach((i)=>{
                    if(i.name===name){
                    this.ZD.name=i.name;
                    this.ZD.type=i.type;
                    }
                })
            },
            //删除字段
            DelZD(e,name){
                this.L.ziduan=this.L.ziduan.filter((i)=>{
                    return i.name!=name
                })
            },
            //展示类
            ShowL(e,name){
                this.Load();
                this.Leilist.forEach((i)=>{
                    if(i.name===name){
                        this.L.name=i.name;
                        this.L.ziduan=i.ziduan;
                    }
                })
            },
            //删除类
            DelL(e,name){
                this.Leilist=this.Leilist.filter((i)=>{
                    return i.name!=name
                });
                this.store();
            },
            //存储类
            Store(){
                localStorage.setItem("storeLei", JSON.stringify(this.Leilist));
            },
            //载入类
            Load(){
                this.Leilist=JSON.parse(localStorage.getItem("storeLei"));
            },

            //将entity数据上传到后端
            entityPost(){
                request.post("/entityJson",JSON.stringify(this.Leilist)).then((res)=>{
                    console.log(res)
                });
            }
        }
    }
</script>
