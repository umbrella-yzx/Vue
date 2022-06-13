<template>
    <div>
        <div class="ef-node-form">
            <div class="ef-node-form-header">
                编辑
            </div>
            <div class="ef-node-form-body">
                <el-form :model="node" ref="dataForm" label-width="80px" v-show="type === 'node'">
                    <!-- 数据源 -->
                    <div v-if="node.type=='dataResource'">
                        <el-form-item label="数据源类型">
                            <el-select v-model="node.state" placeholder="请选择">
                                <el-option
                                    v-for="item in DataSourseList"
                                    :key="item.state"
                                    :label="item.label"
                                    :value="item.state">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <div v-if="node.state=='JDBC'">
                            <el-form-item label="数据库url">
                                <el-input v-model="node.jdbc_DBUrl" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="用户名">
                                <el-input v-model="node.jdbc_userName" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="密码">
                              <el-input v-model="node.jdbc_password" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="连接的表名">
                                <el-input v-model="node.jdbc_tableName" placeholder="请输入"></el-input>
                            </el-form-item>

                          <el-form-item label="数据结构定义">
                            <el-input v-model="node.dataResourceDingyi" placeholder="请输入"></el-input>
                          </el-form-item>
                          <el-form-item label="其余配置">
                          <el-input v-model="node.jdbc_exConfig" placeholder="请输入"></el-input>
                        </el-form-item>
                          <el-form-item label="数据源周期选择">
                            <el-select v-model="node.dataResourceZhouqiState" placeholder="请选择">
                              <el-option
                                v-for="item in DataResourseZhouqi"
                                :key="item.state"
                                :label="item.label"
                                :value="item.state">
                              </el-option>
                            </el-select>
                          </el-form-item>
                            <el-form-item>
                              <el-button   @click="SAVEnode">保存</el-button>
                              <el-button type="primary"  @click="saveJdbc">保存配置</el-button>
                            </el-form-item>
                            <el-form-item>
                              <el-button  @click="showJdbc">配置信息</el-button>
                              <el-button  @click="daoruNode">导入</el-button>
                            </el-form-item>
                          <el-form-item>
                            导入目标-{{SN.type}}:{{SN.id}}
                          </el-form-item>

                        </div>
                        <div v-if="node.state=='CSV'">
                            <el-form-item label="文件路径">
                                <el-input v-model="node.csv_path" placeholder="请输入"></el-input>
                            </el-form-item>

                          <el-form-item label="数据结构定义">
                            <el-input v-model="node.dataResourceDingyi" placeholder="请输入"></el-input>
                          </el-form-item>
                          <el-form-item label="其余配置">
                          <el-input v-model="node.csv_exConfig" placeholder="请输入"></el-input>
                        </el-form-item>
                          <el-form-item label="数据源周期选择">
                            <el-select v-model="node.dataResourceZhouqiState" placeholder="请选择">
                              <el-option
                                v-for="item in DataResourseZhouqi"
                                :key="item.state"
                                :label="item.label"
                                :value="item.state">
                              </el-option>
                            </el-select>
                          </el-form-item>
                          <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
                            <el-button type="primary"  @click="saveCSV">保存配置</el-button>
                          </el-form-item>
                          <el-form-item>
                            <el-button  @click="showCSV">配置信息</el-button>
                            <el-button  @click="daoruNode">导入</el-button>
                          </el-form-item>
                          <el-form-item>
                            导入目标-{{SN.type}}:{{SN.id}}
                          </el-form-item>

                        </div>
                        <div v-if="node.state=='Redis'">
                            <el-form-item label="主机">
                                <el-input v-model="node.redis_host" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="端口">
                                <el-input type="number" v-model="node.redis_port" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="密码">
                                <el-input  v-model="node.redis_password" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="command">
                            <el-select v-model="node.redis_command" placeholder="请选择">
                                <el-option
                                    v-for="item in RedisCommandlist"
                                    :key="item.state"
                                    :label="item.label"
                                    :value="item.state">
                                </el-option>
                            </el-select>
                            </el-form-item>
                            <el-form-item label="key">
                                <el-input v-model="node.redis_key" placeholder="请输入"></el-input>
                            </el-form-item>

                          <el-form-item label="数据结构定义">
                            <el-input v-model="node.dataResourceDingyi" placeholder="请输入"></el-input>
                          </el-form-item>
                          <el-form-item label="其余配置">
                            <el-input v-model="node.redis_exConfig" placeholder="请输入"></el-input>
                          </el-form-item>
                          <el-form-item label="数据源周期选择">
                            <el-select v-model="node.dataResourceZhouqiState" placeholder="请选择">
                              <el-option
                                v-for="item in DataResourseZhouqi"
                                :key="item.state"
                                :label="item.label"
                                :value="item.state">
                              </el-option>
                            </el-select>
                          </el-form-item>
                          <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
                            <el-button type="primary"  @click="saveRedis">保存配置</el-button>
                          </el-form-item>
                          <el-form-item>
                            <el-button  @click="showRedis">配置信息</el-button>
                            <el-button  @click="daoruNode">导入</el-button>
                          </el-form-item>
                          <el-form-item>
                            导入目标-{{SN.type}}:{{SN.id}}
                          </el-form-item>

                        </div>
                        <div v-if="node.state=='HDFS'">
                            <el-form-item label="主机">
                                <el-input v-model="node.hdfs_host" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="端口">
                                <el-input type="number" v-model="node.hdfs_port" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="文件地址">
                                <el-input  v-model="node.hdfs_filePath" placeholder="请输入"></el-input>
                            </el-form-item>

                          <el-form-item label="数据结构定义">
                            <el-input v-model="node.dataResourceDingyi" placeholder="请输入"></el-input>
                          </el-form-item>
                          <el-form-item label="数据源周期选择">
                            <el-select v-model="node.dataResourceZhouqiState" placeholder="请选择">
                              <el-option
                                v-for="item in DataResourseZhouqi"
                                :key="item.state"
                                :label="item.label"
                                :value="item.state">
                              </el-option>
                            </el-select>
                          </el-form-item>
                          <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
                            <el-button type="primary"  @click="saveHDFS">保存配置</el-button>
                          </el-form-item>
                          <el-form-item>
                            <el-button  @click="showHDFS">配置信息</el-button>
                            <el-button  @click="daoruNode">导入</el-button>
                          </el-form-item>
                          <el-form-item>
                            导入目标-{{SN.type}}:{{SN.id}}
                          </el-form-item>

                        </div>
                        <div v-if="node.state=='Kafka'">
                            <el-form-item label="kafka_topic">
                                <el-input v-model="node.kafka_topic" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="kafka_bootstrapServers">
                                <el-input v-model="node.kafka_bootstrapServers" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="kafka_groupId">
                                <el-input  v-model="node.kafka_groupId" placeholder="请输入"></el-input>
                            </el-form-item>

                          <el-form-item label="数据结构定义">
                            <el-input v-model="node.dataResourceDingyi" placeholder="请输入"></el-input>
                          </el-form-item>
                          <el-form-item label="数据源周期选择">
                            <el-select v-model="node.dataResourceZhouqiState" placeholder="请选择">
                              <el-option
                                v-for="item in DataResourseZhouqi"
                                :key="item.state"
                                :label="item.label"
                                :value="item.state">
                              </el-option>
                            </el-select>
                          </el-form-item>
                          <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
                            <el-button type="primary"  @click="saveKafka">保存配置</el-button>
                          </el-form-item>
                          <el-form-item>
                            <el-button  @click="showKafka">配置信息</el-button>
                            <el-button  @click="daoruNode">导入</el-button>
                          </el-form-item>
                          <el-form-item>
                            导入目标-{{SN.type}}:{{SN.id}}
                          </el-form-item>

                        </div>
                    </div>

                    <!-- 滚动聚合 -->
                    <div v-if="node.type=='Aggregation'">
                        <el-form-item label="聚合条件">
<!--                             <el-input v-model="node.Aggregation_tiaojian" placeholder="请输入"></el-input> -->
                            <textarea style="width:200px;height:100px;" v-model="node.Aggregation_tiaojian" placeholder="请输入"></textarea>
                        </el-form-item>
                        <el-form-item label="聚合类型">
                            <el-select v-model="node.Aggregation_leixing" placeholder="请选择">
                                <el-option
                                    v-for="item in AggregationLeixing"
                                    :key="item.state"
                                    :label="item.label"
                                    :value="item.state">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
<!--                            <el-button type="primary"  @click="save">保存配置</el-button>-->
                        </el-form-item>
<!--                        <el-form-item>-->
<!--                            <el-button  @click="show">配置信息</el-button>-->
<!--                            <el-button  @click="daoruNode">导入</el-button>-->
<!--                        </el-form-item>-->
<!--                        <el-form-item>-->
<!--                            导入目标-{{SN.type}}:{{SN.id}}-->
<!--                        </el-form-item>-->
                    </div>
                    <!-- 聚合 -->
                    <div v-if="node.type=='Reduce'">
<!--                        <el-form-item label="输入类">-->
<!--                            <el-input v-model="node.Reduce_inLei" placeholder="请输入"></el-input>-->
<!--                        </el-form-item>-->
                        <el-form-item label="聚合条件">
<!--                             <el-input v-model="node.Reduce_tiaojian" placeholder="请输入"></el-input> -->
                          <textarea style="width:200px;height:100px;" v-model="node.Reduce_tiaojian" placeholder="请输入"></textarea>
                        </el-form-item>
                        <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
<!--                            <el-button type="primary"  @click="save">保存配置</el-button>-->
                        </el-form-item>
<!--                        <el-form-item>-->
<!--                            <el-button  @click="show">配置信息</el-button>-->
<!--                            <el-button  @click="daoruNode">导入</el-button>-->
<!--                        </el-form-item>-->
<!--                        <el-form-item>-->
<!--                            导入目标-{{SN.type}}:{{SN.id}}-->
<!--                        </el-form-item>-->
                    </div>
                    <!-- 过滤 -->
                    <div v-if="node.type=='Filter'">
<!--                        <el-form-item label="输入类">-->
<!--                            <el-input v-model="node.Filter_inLei" placeholder="请输入"></el-input>-->
<!--                        </el-form-item>-->
                        <el-form-item label="过滤条件">
<!--                             <el-input v-model="node.Filter_tiaojian" placeholder="请输入"></el-input> -->
                          <textarea style="width:200px;height:100px;" v-model="node.Filter_tiaojian" placeholder="请输入"></textarea>
                        </el-form-item>
                        <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
<!--                            <el-button type="primary"  @click="save">保存配置</el-button>-->
                        </el-form-item>
<!--                        <el-form-item>-->
<!--                            <el-button  @click="show">配置信息</el-button>-->
<!--                            <el-button  @click="daoruNode">导入</el-button>-->
<!--                        </el-form-item>-->
<!--                        <el-form-item>-->
<!--                            导入目标-{{SN.type}}:{{SN.id}}-->
<!--                        </el-form-item>-->
                    </div>
                    <!-- 映射 -->
                    <div v-if="node.type=='Map'">
<!--                        <el-form-item label="输入类">-->
<!--                            <el-input v-model="node.Map_inLei" placeholder="请输入"></el-input>-->
<!--                        </el-form-item>-->
                        <el-form-item label="输出类">
                            <el-input v-model="node.Map_outLei" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item label="选择条件方式">
                            用户输入<input type="radio" v-model="node.Map_bool" name="node.Map_bool" value="0"></input>
                            字段对添加<input type="radio" v-model="node.Map_bool" name="node.Map_bool" value="1"></input>
                        </el-form-item>

                        <el-form-item label="映射条件" v-if="node.Map_bool==='0'">
<!--                             <el-input v-model="node.Map_tiaojian" placeholder="请输入"></el-input> -->
                            <textarea style="width:200px;height:100px;" v-model="node.Map_tiaojian" placeholder="请输入"></textarea>
                        </el-form-item>
                        <div v-if="node.Map_bool==='1'">
                          <div class="ef-node-form-header">
                            已定义映射对
                          </div>
                          <div v-for="item in node.Map_TypeFieldList">
                                <el-button @click="DelMapTypeField($event,item)">删除</el-button>
                                {{item.map_OldTypeField}}={{item.map_NewTypeField}}
                          </div>
                          <div class="ef-node-form-header">
                            新定义映射对
                          </div>
                          <div>
                            <article style="text-align: center">旧字段=新字段</article>
                            <el-form-item :inline="true">
                            <el-input v-model="linshiMapTypeField.map_OldTypeField" placeholder="请输入" style="width: 80px"></el-input>=
                            <el-input v-model="linshiMapTypeField.map_NewTypeField" placeholder="请输入" style="width: 80px"></el-input>
                            </el-form-item>
                            <el-form-item>
                              <el-button @click="AddMapTypeField">添加字段对</el-button>
                            </el-form-item>
                          </div>
                        </div>
                        <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
<!--                            <el-button type="primary"  @click="save">保存配置</el-button>-->
                        </el-form-item>
<!--                        <el-form-item>-->
<!--                            <el-button  @click="show">配置信息</el-button>-->
<!--                            <el-button  @click="daoruNode">导入</el-button>-->
<!--                        </el-form-item>-->
<!--                        <el-form-item>-->
<!--                            导入目标-{{SN.type}}:{{SN.id}}-->
<!--                        </el-form-item>-->
                    </div>
                    <!-- 扁平映射 -->
                    <div v-if="node.type=='FlatMap'">
<!--                        <el-form-item label="输入类">-->
<!--                            <el-input v-model="node.FlatMap_inLei" placeholder="请输入"></el-input>-->
<!--                        </el-form-item>-->
                        <el-form-item label="输出类">
                            <el-input v-model="node.FlatMap_outLei" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item label="映射条件">
<!--                             <el-input v-model="node.FlatMap_tiaojian" placeholder="请输入"></el-input> -->
                            <textarea style="width:200px;height:100px;" v-model="node.FlatMap_tiaojian" placeholder="请输入"></textarea>
                        </el-form-item>
                        <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
<!--                            <el-button type="primary"  @click="save">保存配置</el-button>-->
                        </el-form-item>
<!--                        <el-form-item>-->
<!--                            <el-button  @click="show">配置信息</el-button>-->
<!--                            <el-button  @click="daoruNode">导入</el-button>-->
<!--                        </el-form-item>-->
<!--                        <el-form-item>-->
<!--                            导入目标-{{SN.type}}:{{SN.id}}-->
<!--                        </el-form-item>-->
                    </div>
                    <!-- 分组 -->
                    <div v-if="node.type=='KeyBy'">
<!--                        <el-form-item label="输入类">-->
<!--                            <el-input v-model="node.KeyBy_inLei" placeholder="请输入"></el-input>-->
<!--                        </el-form-item>-->
                        <el-form-item label="输出类">
                            <el-input v-model="node.KeyBy_outLei" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item label="分组条件">
<!--                             <el-input v-model="node.KeyBy_tiaojian" placeholder="请输入"></el-input> -->
                            <textarea style="width:200px;height:100px;" v-model="node.KeyBy_tiaojian" placeholder="请输入"></textarea>
                        </el-form-item>
                        <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
<!--                            <el-button type="primary"  @click="save">保存配置</el-button>-->
                        </el-form-item>
<!--                        <el-form-item>-->
<!--                            <el-button  @click="show">配置信息</el-button>-->
<!--                            <el-button  @click="daoruNode">导入</el-button>-->
<!--                        </el-form-item>-->
<!--                        <el-form-item>-->
<!--                            导入目标-{{SN.type}}:{{SN.id}}-->
<!--                        </el-form-item>-->
                    </div>
                    <!-- 数据结果 -->
                    <div v-if="node.type=='dataResult'">
                        <el-form-item label="数据结果输入类型">
                            <el-select v-model="node.dataResultType" placeholder="请选择">
                                <el-option
                                    v-for="item in DataResultTypelist"
                                    :key="item.state"
                                    :label="item.label"
                                    :value="item.state">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <div v-if="node.dataResultType==='window'">
                            {{node.dataResultWindow_data}}
                            <el-form-item >
                              <el-button  @click="ShowDataResultWindow" >显示结果</el-button>
<!--                              <el-button  @click="ShowDataResultWindow">显示结果</el-button>-->
                            </el-form-item>
                        </div>
                        <div v-if="node.dataResultType==='MySql'">
                            <el-form-item label="数据库url">
                                <el-input v-model="node.dataResultMySql_url" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="用户名">
                                <el-input v-model="node.dataResultMySql_userName" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="连接的表名">
                                <el-input v-model="node.dataResultMySql_tableName" placeholder="请输入"></el-input>
                            </el-form-item>
                            <el-form-item label="密码">
                                <el-input v-model="node.dataResultMySql_password" placeholder="请输入"></el-input>
                            </el-form-item>
                          <el-form-item label="数据配置项">
                            <el-input v-model="node.dataResultMySql_dataPeizhi" placeholder="请输入"></el-input>
                          </el-form-item>
                            <el-form-item label="其余配置">
                                <el-input v-model="node.dataResultMySql_exConfig" placeholder="请输入"></el-input>
                            </el-form-item>
<!--                             <el-form-item >
                            <el-button type="primary" @click="ShowDataResultMySql">显示结果</el-button>
                            </el-form-item> -->
                            <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
                            <el-button type="primary"  @click="saveResultJdbc">保存配置</el-button>
                        </el-form-item>
                        <el-form-item>
                            <el-button  @click="showResultJdbc">配置信息</el-button>
                            <el-button  @click="daoruNode">导入</el-button>
                        </el-form-item>
                        <el-form-item>
                            导入目标-{{SN.type}}:{{SN.id}}
                        </el-form-item>
                        </div>
                        <div v-if="node.dataResultType==='CSV'">
                            <el-form-item label="文件路径">
                                <el-input v-model="node.dataResultCSV_path" placeholder="请输入"></el-input>
                            </el-form-item>
<!--                            <el-form-item label="输入类">-->
<!--                              <el-input v-model="node.dataResultCSV_outLei" placeholder="请输入"></el-input>-->
<!--                            </el-form-item>-->
                            <el-form-item label="其余配置">
                                <el-input v-model="node.dataResultCSV_exConfig" placeholder="请输入"></el-input>
                            </el-form-item>
<!--                             <el-form-item>
                                <el-button type="primary" @click="ShowDataResultCsv">显示结果</el-button>
                            </el-form-item> -->
                            <el-form-item>
                            <el-button   @click="SAVEnode">保存</el-button>
                            <el-button type="primary"  @click="saveResultCsv">保存配置</el-button>
                        </el-form-item>
                        <el-form-item>
                            <el-button  @click="showResultCsv">配置信息</el-button>
                            <el-button  @click="daoruNode">导入</el-button>
                        </el-form-item>
                        <el-form-item>
                            导入目标-{{SN.type}}:{{SN.id}}
                        </el-form-item>
                        </div>
                    </div>
                </el-form>
            </div>
        </div>
        <peizhi v-if="peizhiVisiable" ref="peizhi" :data="data"></peizhi>
        <datasesultwindow v-if="windowDataVisible" ref="datasesultwindow" ></datasesultwindow>
    </div>

</template>

<script>
    import { cloneDeep } from 'lodash'
    import peizhi from '@/components/ef/peizhi'
    import request from "../../utils/request";
    import panel from "./panel";
    import datasesultwindow from '@/components/ef/dataresultwindow';
    import FlowInfo from '@/components/ef/info';
    export default {
        components:{
            peizhi,FlowInfo,datasesultwindow
        },
        data() {
            return {
                visible: true,
                // node 或 line
                type: 'node',
                //用于存放配置结点
                ST:[],
                //用于存放导入结点
                SN:{},
                //用于存放结果显示
                DR:{
                    dataResultWindow_data:'窗口',
                    dataResultMySql_url:'11',
                    dataResultMySql_userName:'12',
                    dataResultMySql_password:'13',
                    dataResultMySql_tableName:'14',
                    dataResultMySql_exConfig:'15',
                    dataResultCSV_path:'21',
                    dataResultCSV_exConfig:'22',
                },
                SN_bool:0,
                //临时映射字段对
                linshiMapTypeField:{
                  map_NewTypeField:'',
                  map_OldTypeField:'',
                },
                //控制配置框的显示与隐藏
                peizhiVisiable:false,
                //控制窗口结果显示
                windowDataVisible:false,
                node: {},
                line: {},
                data: {},
                DataSourseList: [{
                    state: 'JDBC',
                    label: 'JDBC数据源'
                }, {
                    state: 'CSV',
                    label: 'CSV数据源'
                }, {
                    state: 'Kafka',
                    label: 'Kafka数据源'
                }, {
                    state: 'HDFS',
                    label: 'HDFS数据源'
                },{
                    state: 'Restful+JSON',
                    label: 'Restful+JSON数据源'
                },{
                    state: 'Redis',
                    label: 'Redis数据源'
                },{
                    state: 'Any',
                    label: '任意'
                }
                ],
                //周期
                DataResourseZhouqi:[{
                    state:'1',
                    label:'直接完成'
                },{
                    state:'2',
                    label:'给定周期完成'
                },{
                    state:'3',
                    label:'循环完成'
                }
                ],
                AggregationLeixing:[{
                    state:'sum',
                    label:'sum',
                },
                {
                    state:'min',
                    label:'min',
                },
                {
                    state:'max',
                    label:'max',
                },
                {
                    state:'minBy',
                    label:'minBy',
                },
                {
                    state:'maxBy',
                    label:'maxBy'
                },
                ],
                DataResultTypelist:[
                    {
                    state:'window',
                    label:'窗口',
                },
                {
                    state:'MySql',
                    label:'MySql',
                },
                {
                    state:'CSV',
                    label:'CSV',
                },
                ],
                RedisCommandlist:[
                    {
                    state:'GET',
                    label:'GET',
                },
                {
                    state:'HGET',
                    label:'HGET',
                },
                {
                    state:'HGETALL',
                    label:'HGETALL',
                },
                {
                    state:'LRANGE',
                    label:'LRANGE',
                },
                {
                    state:'SMEMBERS',
                    label:'SMEMBERS',
                },
                {
                    state:'ZRANGE',
                    label:'ZRANGE',
                },
                {
                    state:'ZRANGEWITHSCORES',
                    label:'ZRANGEWITHSCORES',
                },
                {
                    state:'PFCOUNT',
                    label:'PFCOUNT',
                },
                ]
            }
        },
        methods: {
            /**
             * 表单修改，这里可以根据传入的ID进行业务信息获取
             * @param data
             * @param id
             */
            nodeInit(data, id) {
                this.type = 'node'
                this.data = data
                data.nodeList.filter((node) => {
                    if (node.id === id) {
                        this.node = cloneDeep(node)
                    }
                })
            },
            lineInit(line) {
                this.type = 'line'
                this.line = line
            },
            // 修改连线
            saveLine() {
                this.$emit('setLineLabel', this.line.from, this.line.to, this.line.label)
            },
            //保存结点
            SAVEnode(){
                this.data.nodeList.filter((node) => {
                    if (node.id === this.node.id) {
                        var temp=JSON.parse(JSON.stringify(this.node));
                        this.xieru(node,temp)
                        this.$emit('repaintEverything')
                    }
                })
            },
          //保存jdbc配置
          saveJdbc(){
            // console.log(JSON.stringify(this.node));
            request.post("/config/jdbc/json",JSON.stringify(this.node))
              .then((res)=> {

              })
          },

          //显示jdbc配置
          showJdbc(){
            this.peizhiVisiable = true
            this.$nextTick(function () {
              this.$refs.peizhi.init("jdbc")
            })
          },

          saveCSV(){
            // console.log(JSON.stringify(this.node));
            request.post("/config/csv/json",JSON.stringify(this.node))
              .then((res)=> {

              })
          },

          showCSV(){
            this.peizhiVisiable = true
            this.$nextTick(function () {
              this.$refs.peizhi.init("csv")
            })
          },

          saveRedis(){
            // console.log(JSON.stringify(this.node));
            request.post("/config/redis/json",JSON.stringify(this.node))
              .then((res)=> {

              })
          },

          showRedis(){
            this.peizhiVisiable = true
            this.$nextTick(function () {
              this.$refs.peizhi.init("redis")
            })
          },

          saveHDFS(){
            // console.log(JSON.stringify(this.node));
            request.post("/config/hdfs/json",JSON.stringify(this.node))
              .then((res)=> {

              })
          },

          showHDFS(){
            this.peizhiVisiable = true
            this.$nextTick(function () {
              this.$refs.peizhi.init("hdfs")
            })
          },

          saveKafka(){
            // console.log(JSON.stringify(this.node));
            request.post("/config/kafka/json",JSON.stringify(this.node))
              .then((res)=> {

              })
          },

          showKafka(){
            this.peizhiVisiable = true
            this.$nextTick(function () {
              this.$refs.peizhi.init("kafka")
            })
          },

          saveResultJdbc(){
            // console.log(JSON.stringify(this.node));
            request.post("/config/jdbcResult/json",JSON.stringify(this.node))
              .then((res)=> {

              })
          },

          showResultJdbc(){
            this.peizhiVisiable = true
            this.$nextTick(function () {
              this.$refs.peizhi.init("jdbcResult")
            })
          },

          saveResultCsv(){
            // console.log(JSON.stringify(this.node));
            request.post("/config/csvResult/json",JSON.stringify(this.node))
              .then((res)=> {

              })
          },

          showResultCsv(){
            this.peizhiVisiable = true
            this.$nextTick(function () {
              this.$refs.peizhi.init("csvResult")
            })
          },

            //保存配置
            save() {
                this.ST = JSON.parse(localStorage.getItem("store"));
                this.data.nodeList.filter((node) => {
                    if (node.id === this.node.id) {
                        var temp=JSON.parse(JSON.stringify(this.node));
                        this.xieru(node,temp)
                        if(this.ST.length===0){
                            this.ST.push(node)
                        }
                        else{
                            var s=1;
                            this.ST.forEach((i)=>{
                            if(i.id===node.id)
                            {
                                s=0;
                            }
                        })
                            if(s===1){
                                this.ST.push(node)
                            }
                        }
                        this.$emit('repaintEverything')
                    }
                })
                localStorage.setItem("store", JSON.stringify(this.ST));
            },

            //打开配置界面
            show(){
                // // this.ST = JSON.parse(localStorage.getItem("store"));
                // if(this.ST==null){
                //   this.ST=[];
                // }
                // //获取后端数据
                // request.get("/config/jdbc").then((res)=>{
                //   // console.log(res);
                //   for (let i = 0; i < res.length; i++) {
                //     var node = {};
                //     node.type ='dataResource';
                //     node.state ='JDBC';
                //     node.id = "123";
                //     node.jdbc_DBUrl = res[i].dburl;
                //     node.jdbc_userName = res[i].userName;
                //     node.jdbc_tableName = res[i].tableName;
                //     node.jdbc_password = res[i].password;
                //     node.jdbc_exConfig = res[i].exConfig;
                //     node.field_id = res[i].field_id;
                //     this.ST.push(node);
                //   }
                //   console.log(JSON.stringify(this.ST));
                // });
                // localStorage.store = JSON.stringify(this.ST);
                // this.peizhiVisiable = true
                // this.$nextTick(function () {
                //     this.$refs.peizhi.init()
                // })
            },
            //获取this.SN
            setNode(){
                this.SN = JSON.parse(localStorage.getItem("storeNode"));
                this.SN_bool=1;
            },
            //导入配置
            daoruNode(){
                if(this.SN_bool===0){
                    alert("未选择要导入的配置")
                }
                else{
                this.data.nodeList.filter((node)=>{
                    if(node.id===this.node.id){
                      if(node.type===this.SN.type){
                        // console.log(this.SN);
                        this.xieru(node,this.SN);
                        this.node=node;
                      }
                      else{
                          alert("请选择相同类型的配置再导入");
                      }
                    }
                })}
            },
            //将结点2写入结点1
            xieru(node1,node2){
                    node1.state=node2.state
                    node1.dataResourceDingyi=node2.dataResourceDingyi
                    node1.dataResourceZhouqiState=node2.dataResourceZhouqiState
                    //jdbc
                    node1.jdbc_DBUrl=node2.jdbc_DBUrl
                    node1.jdbc_userName=node2.jdbc_userName
                    node1.jdbc_tableName=node2.jdbc_tableName
                    node1.jdbc_password=node2.jdbc_password
                    node1.jdbc_exConfig=node2.jdbc_exConfig
                    //csv
                    node1.csv_path=node2.csv_path
                    node1.csv_exConfig=node2.csv_exConfig
                    //redis
                    node1.redis_host=node2.redis_host
                    node1.redis_port=node2.redis_port
                    node1.redis_password=node2.redis_password
                    node1.redis_exConfig=node2.redis_exConfig
                    node1.redis_command=node2.redis_command
                    node1.redis_key=node2.redis_key
                    //hdfs
                    node1.hdfs_host=node2.hdfs_host
                    node1.hdfs_port=node2.hdfs_port
                    node1.hdfs_filePath =node2.hdfs_filePath
                    //kafka
                    node1.kafka_topic =node2.kafka_topic
                    node1.kafka_bootstrapServers =node2.kafka_bootstrapServers
                    node1.kafka_groupId =node2.kafka_groupId
                    //滚动聚合
                    node1.Aggregation_tiaojian=node2.Aggregation_tiaojian
                    node1.Aggregation_leixing=node2.Aggregation_leixing
                    //聚合
                    node1.Reduce_inLei=node2.Reduce_inLei
                    node1.Reduce_tiaojian=node2.Reduce_tiaojian
                    //过滤
                    node1.Filter_inLei=node2.Filter_inLei
                    node1.Filter_tiaojian=node2.Filter_tiaojian
                    //映射
                    node1.Map_inLei=node2.Map_inLei
                    node1.Map_outLei=node2.Map_outLei
                    node1.Map_tiaojian=node2.Map_tiaojian
                    node1.Map_bool=node2.Map_bool
                    node1.Map_TypeFieldList=node2.Map_TypeFieldList
                    //扁平映射
                    node1.FlatMap_inLei=node2.FlatMap_inLei
                    node1.FlatMap_outLei=node2.FlatMap_outLei
                    node1.FlatMap_tiaojian=node2.FlatMap_tiaojian
                    //分组
                    node1.KeyBy_inLei=node2.KeyBy_inLei
                    node1.KeyBy_outLei=node2.KeyBy_outLei
                    node1.KeyBy_tiaojian=node2.KeyBy_tiaojian
                    //数据结果
                    node1.dataResultType=node2.dataResultType
                    //数据结果Windows
                    node1.dataResultWindow_data=node2.dataResultWindow_data
                    //数据结果mysql
                    node1.dataResultMySql_url=node2.dataResultMySql_url
                    node1.dataResultMySql_userName=node2.dataResultMySql_userName
                    node1.dataResultMySql_password=node2.dataResultMySql_password
                    node1.dataResultMySql_tableName=node2.dataResultMySql_tableName
                    node1.dataResultMySql_exConfig=node2.dataResultMySql_exConfig
                    node1.dataResultMySql_dataPeizhi=node2.dataResultMySql_dataPeizhi
                    //数据结果csv
                    node1.dataResultCSV_path=node2.dataResultCSV_path
                    node1.dataResultCSV_exConfig=node2.dataResultCSV_exConfig
                    node1.dataResultCSV_outLei=node2.dataResultCSV_outLei
                    //字段
                    node1.field_id=node2.field_id
            },
            //数据结果窗口显示

            ShowDataResultWindow(){
              request.get("/Data").then((res)=> {
                console.log(res);
                this.DR.dataResultWindow_data = res;
              });
              this.windowDataVisible = true
              this.$nextTick(function () {
                this.$refs.datasesultwindow.init()
              })


                // // this.DR = JSON.parse(localStorage.getItem("storeResult"));
                // this.data.nodeList.filter((node)=>{
                //     if(node.id===this.node.id){
                //         node.dataResultType='window';
                //         var temp=JSON.parse(JSON.stringify(this.DR));
                //         node.dataResultWindow_data=temp.dataResultWindow_data;
                //         this.node=node;
                //     }
                // });
                // this.DR={};
            },
            //数据结果Mysql显示(未绑定事件)
            ShowDataResultMySql(){

            },
            //数据结果Csv显示(未绑定事件)
            ShowDataResultCsv(){

            },
            //存放数据结果
            StoreDataResult(){
                localStorage.setItem("storeResult",JSON.stringify(this.DR));
            },

            //删除字段对
            DelMapTypeField(e,item)
            {
              this.node.Map_TypeFieldList=this.node.Map_TypeFieldList.filter((i)=>{
                    return i.map_OldTypeField!=item.map_OldTypeField||i.map_NewTypeField!=item.map_NewTypeField
                });
            },

            //添加字段对到列表
            AddMapTypeField()
            {
              if(this.linshiMapTypeField.map_OldTypeField!=''&&this.linshiMapTypeField.map_NewTypeField!='')
              {
                var temp=JSON.parse(JSON.stringify(this.linshiMapTypeField));
                this.node.Map_TypeFieldList.push(temp);
                this.ClearMapTypeField();
              }
              else{
                alert("字段对不能为空");
              }
            },

            //清空临时字段对
            ClearMapTypeField()
            {
              this.linshiMapTypeField.map_OldTypeField="";
              this.linshiMapTypeField.map_NewTypeField="";
            }

        },
        mounted:function(){
          this.ClearMapTypeField();
        },

    }
</script>

<style>
    .el-node-form-tag {
        position: absolute;
        top: 50%;
        margin-left: -15px;
        height: 40px;
        width: 15px;
        background-color: #fbfbfb;
        border: 1px solid rgb(220, 227, 232);
        border-right: none;
        z-index: 0;
    }
    /* .el-node-container{
        height: 100px;
    } */
</style>
