<template>
    <el-dialog
            title="配置信息"
            :visible.sync="dialogVisible"
            width="70%"
    >

        <div v-for="item in ST" class="border_style" style="border: 1px solid #000;">
          <article style="display:inline;">结点id:{{item.field_id}}&nbsp;&nbsp;&nbsp;</article>
          <article style="display:inline;">类型:{{item.type}}&nbsp;&nbsp;&nbsp;</article>
          <!-- 数据源 -->
          <article style="display:inline;" v-if="item.type==='dataResource'">数据源类型:{{item.state}}&nbsp;&nbsp;&nbsp;</article>
<!--          <article style="display:inline;" v-if="item.type==='dataResource'">数据结构定义:{{item.dataResourceDingyi}}&nbsp;&nbsp;&nbsp;</article>-->
<!--          <article style="display:inline;" v-if="item.type==='dataResource'">数据源周期:{{item.dataResourceZhouqiState}}&nbsp;&nbsp;&nbsp;</article>-->
          <div v-if="item.type==='dataResource'">
            <div v-if="item.state==='JDBC'">
              <article style="display:inline;">数据库url:{{item.jdbc_DBUrl}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">用户名:{{item.jdbc_userName}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">连接的表名:{{item.jdbc_tableName}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">密码:{{item.jdbc_password}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">其余配置:{{item.jdbc_exConfig}}&nbsp;&nbsp;&nbsp;</article>
            </div>
            <div v-if="item.state==='CSV'">
              <article style="display:inline;">文件路径:{{item.csv_path}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">其余配置:{{item.csv_exConfig}}&nbsp;&nbsp;&nbsp;</article>
            </div>
            <div v-if="item.state==='Redis'">
              <article style="display:inline;">主机:{{item.redis_host}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">端口:{{item.redis_port}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">密码:{{item.redis_password}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">command:{{item.redis_command}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">key:{{item.redis_key}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">其余配置:{{item.redis_exConfig}}&nbsp;&nbsp;&nbsp;</article>
            </div>
            <div v-if="item.state==='HDFS'">
              <article style="display:inline;">主机:{{item.hdfs_host}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">端口:{{item.hdfs_port}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">文件地址:{{item.hdfs_filePath}}&nbsp;&nbsp;&nbsp;</article>
            </div>
            <div v-if="item.state==='Kafka'">
              <article style="display:inline;">kafka_topic:{{item.kafka_topic}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">kafka_bootstrapServers:{{item.kafka_bootstrapServers}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">kafka_groupId:{{item.kafka_groupId}}&nbsp;&nbsp;&nbsp;</article>
            </div>
            <div v-if="item.state==='Any'">
              <article style="display:inline;">数据源名称:{{item.udf_source_jar}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">入口类:{{item.udf_source_entry_class}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">数据源解释类:{{item.udf_source_outClass}}&nbsp;&nbsp;&nbsp;</article>
            </div>
          </div>
          <!-- 滚动聚合 -->
          <div v-if="item.type==='Aggregation'">
            <article style="display:inline;">聚合条件:{{item.Aggregation_tiaojian}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">聚合类型:{{item.Aggregation_leixing}}&nbsp;&nbsp;&nbsp;</article>
          </div>
          <!-- 聚合 -->
          <div v-if="item.type==='Reduce'">
            <article style="display:inline;">输入类:{{item.Reduce_inLei}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">聚合条件:{{item.Reduce_tiaojian}}&nbsp;&nbsp;&nbsp;</article>
          </div>
          <!-- 过滤 -->
          <div v-if="item.type==='Filter'">
            <article style="display:inline;">输入类:{{item.Filter_inLei}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">过滤条件:{{item.Filter_tiaojian}}&nbsp;&nbsp;&nbsp;</article>
          </div>
          <!-- 映射 -->
          <div v-if="item.type==='Map'">
            <article style="display:inline;">输入类:{{item.Map_inLei}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">输出类:{{item.Map_outLei}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">映射条件:{{item.Map_tiaojian}}&nbsp;&nbsp;&nbsp;</article>
          </div>
          <!-- 扁平映射 -->
          <div v-if="item.type==='KeyBy'">
            <article style="display:inline;">输入类:{{item.KeyBy_inLei}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">输出类:{{item.KeyBy_outLei}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">映射条件:{{item.KeyBy_tiaojian}}&nbsp;&nbsp;&nbsp;</article>
          </div>
          <!-- 用户自定义数据处理 -->
          <div v-if="item.type==='UserDefineFunction'">
            <article style="display:inline;">数据源名称:{{item.udf_function_jar}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">入口类:{{item.udf_function_entry_class}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">处理类型:{{item.udf_function_type}}&nbsp;&nbsp;&nbsp;</article>
            <article style="display:inline;">数据源解释类:{{item.udf_function_outClass}}&nbsp;&nbsp;&nbsp;</article>
          </div>
          <!-- 数据结果 -->
          <div v-if="item.type==='dataResult'">
            <!-- MySql -->
            <div v-if="item.dataResultType==='MySql'">
              <article style="display:inline;">数据库url:{{item.dataResultMySql_url}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">用户名:{{item.dataResultMySql_userName}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">连接的表名:{{item.dataResultMySql_tableName}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">密码:{{item.dataResultMySql_password}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">其余配置:{{item.dataResultMySql_exConfig}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">数据配置项:{{item.dataResultMySql_dataPeizhi}}&nbsp;&nbsp;&nbsp;</article>
            </div>
            <div v-if="item.dataResultType==='CSV'">
              <article style="display:inline;">文件路径:{{item.dataResultCSV_path}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">输入类:{{item.dataResultCSV_outLei}}&nbsp;&nbsp;&nbsp;</article>
              <article style="display:inline;">其余配置:{{item.dataResultCSV_exConfig}}&nbsp;&nbsp;&nbsp;</article>
            </div>
          </div>
          <div>
            <button @click="SetSN($event,item.field_id)">选择</button>
            <button v-on:click="DelST($event,item.filed_id)">删除</button>
          </div>

          <div><br></div>
        </div>
    </el-dialog>
</template>

<!-- 引入组件库 -->
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
                ST: [],
                SN:{},
            }
        },
        components: {
            codemirror
        },
        methods: {
            init(type) {
                this.dialogVisible = true;
                switch (type) {
                  case "jdbc":this.initJdbc();break;
                  case "csv":this.initCSV();break;
                  case "redis":this.initRedis();break;
                  case "hdfs":this.initHDFS();break;
                  case "kafka":this.initKafka();break;
                  case "jdbcResult":this.initResultJdbc();break;
                  case "csvResult":this.initResultCSV();break;
                  default:break;
                }
                // this.ST = JSON.parse(localStorage.getItem("store"));
            },

            initJdbc(){
              //获取后端数据
              request.get("/config/jdbc").then((res)=>{
                // console.log(res);
                var tmpST = [];
                for (let i = 0; i < res.length; i++) {
                  var node = {};
                  node.type ='dataResource';
                  node.state ='JDBC';
                  node.field_id = res[i].id;
                  node.id = this.getUUID();
                  node.jdbc_DBUrl = res[i].dburl;
                  node.jdbc_userName = res[i].userName;
                  node.jdbc_tableName = res[i].tableName;
                  node.jdbc_password = res[i].password;
                  node.jdbc_exConfig = res[i].exConfig;
                  tmpST.push(node);
                }
                // console.log(JSON.stringify(this.ST));
                this.ST = tmpST;
              });
            } ,

          initResultJdbc(){
            //获取后端数据
            request.get("/config/jdbc").then((res)=>{
              // console.log(res);
              var tmpST = [];
              for (let i = 0; i < res.length; i++) {
                var node = {};
                node.type ='dataResult';
                node.dataResultType = 'MySql';
                node.field_id = res[i].id;
                node.id = this.getUUID();
                node.dataResultMySql_url = res[i].dburl;
                node.dataResultMySql_userName = res[i].userName;
                node.dataResultMySql_tableName = res[i].tableName;
                node.dataResultMySql_password = res[i].password;
                node.dataResultMySql_exConfig = res[i].exConfig;
                tmpST.push(node);
              }
              this.ST = tmpST;
              // console.log(JSON.stringify(this.ST));
            });
          } ,

          initCSV(){
            //获取后端数据
            request.get("/config/csv").then((res)=>{
              // console.log(res);
              var tmpST = [];
              for (let i = 0; i < res.length; i++) {
                var node = {};
                node.type ='dataResource';
                node.state ='CSV';
                node.field_id = res[i].id;
                node.id = this.getUUID();
                node.csv_path = res[i].path;
                node.jdbc_exConfig = res[i].exConfig;
                tmpST.push(node);
              }
              // console.log(JSON.stringify(this.ST));
              this.ST = tmpST;
            });
          } ,

          initResultCSV(){
            //获取后端数据
            request.get("/config/csv").then((res)=>{
              console.log(res);
              var tmpST = [];
              for (let i = 0; i < res.length; i++) {
                var node = {};
                node.type ='dataResult';
                node.dataResultType = 'CSV';
                node.field_id = res[i].id;
                node.id = this.getUUID();
                node.dataResultCSV_path = res[i].path;
                node.dataResultCSV_exConfig = res[i].exConfig;
                tmpST.push(node);
              }
              // console.log(JSON.stringify(this.ST));
              this.ST = tmpST;
            });
          } ,

          initRedis(){
            //获取后端数据
            request.get("/config/redis").then((res)=>{
              // console.log(res);
              var tmpST = [];
              for (let i = 0; i < res.length; i++) {
                var node = {};
                node.type ='dataResource';
                node.state ='Redis';
                node.field_id = res[i].id;
                node.id = this.getUUID();
                node.redis_host = res[i].host;
                node.redis_port = res[i].port;
                node.redis_password = res[i].password;
                node.redis_exConfig = res[i].exConfig;
                node.redis_command = res[i].command;
                node.redis_key = res[i].redisKey;
                tmpST.push(node);
              }
              // console.log(JSON.stringify(this.ST));
              this.ST = tmpST;
            });
          } ,

          initKafka(){
            //获取后端数据
            request.get("/config/kafka").then((res)=>{
              // console.log(res);
              var tmpST = [];
              for (let i = 0; i < res.length; i++) {
                var node = {};
                node.type ='dataResource';
                node.state ='Kafka';
                node.field_id = res[i].id;
                node.id = this.getUUID();
                node.kafka_topic = res[i].topic;
                node.kafka_bootstrapServers = res[i].bootstrapServers;
                node.kafka_groupId = res[i].groupId;
                tmpST.push(node);
              }
              // console.log(JSON.stringify(this.ST));
              this.ST = tmpST;
            });
          } ,

          initHDFS(){
            //获取后端数据
            request.get("/config/hdfs").then((res)=>{
              // console.log(res);
              var tmpST = [];
              for (let i = 0; i < res.length; i++) {
                var node = {};
                node.type ='dataResource';
                node.state ='HDFS';
                node.field_id = res[i].id;
                node.id = this.getUUID();
                node.hdfs_host = res[i].host;
                node.hdfs_port = res[i].port;
                node.hdfs_filePath = res[i].filePath;
                tmpST.push(node);
              }
              // console.log(JSON.stringify(this.ST));
              this.ST = tmpST;
            });
          } ,

            // 返回唯一标识
            getUUID() {
              return 'N'+Math.random().toString(36).substr(3, 10)
            },

            //删除配置+-
            DelST(e,id){
              this.ST=this.ST.filter((i)=>{
                return i.id!=id
              });
              localStorage.setItem("store", JSON.stringify(this.ST));
            },
            //导入配置
            SetSN(e,id){
              this.ST.forEach((i)=>{
                if(i.field_id===id){
                  this.SN=i;
                  this.SN.name=i.name
                  this.SN.field_id = i.field_id
                  i.ischecked=1;
                  // console.log(i);
                }
                else{
                  i.ischecked=0;
                }
              })
              localStorage.setItem("storeNode", JSON.stringify(this.SN));
              this.$parent.setNode();
              this.dialogVisible = false;
            }
        }
    }
</script>
