<template scoped>
  <div>
  		<!--查询弹出框-->
    <el-dialog title="查询记录" v-model="search.visible" size="small">
	    <el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
		    	<el-row class="bottom20">
            <el-col :span="6" style="text-align: center">
              <el-checkbox label="开始支付时间" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="15">
              <el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
            </el-col>
          </el-row>
          <el-input placeholder="请输入商户订单号关键字" v-model="search.data.tradeCode" :disabled="!search.useTradeCode" class="bottom20 input-size">
            <el-checkbox label="订单号" slot="prepend" v-model="search.useTradeCode"></el-checkbox>
          </el-input>    
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="类型" v-model="search.useTradeType" style="margin-top: 5px;"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" v-model="search.data.tradeType" placeholder="请选择交易类型" :disabled="!search.useTradeType">
                <el-option label="请选择交易类型" value=""></el-option>
                <el-option label="消费" value="1"></el-option>
                <el-option label="退款" value="2"></el-option>
              </el-select>
            </el-col>
          </el-row>    
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="状态" v-model="search.useStatus" style="margin-top: 5px;"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" v-model="search.data.status" placeholder="请选择对比状态" :disabled="!search.useStatus">
                <el-option label="请选择数据状态" value=""></el-option>
                <el-option label="正常" value="1"></el-option>
                <el-option label="支付平台缺失" value="2"></el-option>
                <el-option label="系统数据缺失" value="3"></el-option>
              </el-select>
            </el-col>
          </el-row>
		    </el-col>
		    <el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="6" style="text-align: center">
              <el-checkbox label="结束支付时间" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="15">
              <el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
            </el-col>
          </el-row>   
		    	
          <el-input placeholder="请输入流水号" v-model="search.data.tradeNo" :disabled="!search.useTradeNo" class="bottom20 input-size">
            <el-checkbox label="流水号" slot="prepend" v-model="search.useTradeNo"></el-checkbox>
          </el-input>   
            <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="对比结果" v-model="search.useDiffer" style="margin-top: 5px;margin-left: 10px;"></el-checkbox>
            </el-col>
            <el-col :span="15">
              <el-select style="width: 100%;margin-left: 20px;" v-model="search.data.differ" placeholder="请输入对比结果" :disabled="!search.useDiffer">
                <el-option label="请输入对比结果" value=""></el-option>
                <el-option label="等于0" value="1"></el-option>
                <el-option label="大于0" value="2"></el-option>
                <el-option label="小于0" value="3"></el-option>
              </el-select>
            </el-col>
          </el-row>
		    </el-col>
	    </el-form>
	    <div slot="footer" class="dialog-footer">
		    <el-button @click="search.visible = false">取 消</el-button>
		    <el-button type="primary" @click="handleSearch()">查询</el-button>
	    </div>
  	</el-dialog>
    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="商户交易总金额" v-bind:part1="totalPayable" back="#409EFF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="平台交易总金额" v-bind:part1="totalThirdPayable" back="#67C23A"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="对比结果总金额" v-bind:part1="totalDiffer" back="#F56C6C"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="差异结果总金额" v-bind:part1="totalResult" back="#F56C6C"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-tabs v-model="payType" @tab-click="handleClick">
          <el-tab-pane label="微信支付自动对账记录" name="1">
               <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
                  <el-table-column prop="tradeCode" label="商户订单号">
                  </el-table-column>   
                  <el-table-column prop="tradeNo" label="流水号">
                  </el-table-column> 
                  <el-table-column prop="tradeType" label="交易类型">
                  </el-table-column>
                  <el-table-column prop="payable" label="商户交易金额">
                  </el-table-column>  
                  <el-table-column prop="thirdPayable" label="平台交易金额">
                  </el-table-column>           
                  <el-table-column prop="tradeTime" label="交易时间">
                  </el-table-column>   
                  <el-table-column prop="payType" label="支付方式">
                  </el-table-column>    
                  <el-table-column prop="differ" label="对比结果">
                  </el-table-column>        
                  <el-table-column prop="status" label="数据状态">
                  </el-table-column>    
                </el-table>
          </el-tab-pane>
          <el-tab-pane label="支付宝支付自动对账记录" name="2">
               <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
                  <el-table-column prop="tradeCode" label="商户订单号">
                  </el-table-column>   
                  <el-table-column prop="tradeNo" label="流水号">
                  </el-table-column>
                  <el-table-column prop="tradeType" label="交易类型">
                  </el-table-column>
                  <el-table-column prop="payable" label="商户交易金额">
                  </el-table-column>  
                  <el-table-column prop="thirdPayable" label="平台交易金额">
                  </el-table-column>           
                  <el-table-column prop="tradeTime" label="交易时间">
                  </el-table-column>   
                  <el-table-column prop="payType" label="支付方式">
                  </el-table-column>    
                  <el-table-column prop="differ" label="对比结果">
                  </el-table-column>        
                  <el-table-column prop="status" label="数据状态">
                  </el-table-column>    
                </el-table>
          </el-tab-pane>
          <el-tab-pane label="银联支付自动对账记录" name="3">
              <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
                <el-table-column prop="tradeCode" label="商户订单号">
                </el-table-column>   
                <el-table-column prop="tradeNo" label="流水号">
                </el-table-column>
                <el-table-column prop="tradeType" label="交易类型">
                </el-table-column>
                <el-table-column prop="payable" label="商户交易金额">
                </el-table-column>  
                <el-table-column prop="thirdPayable" label="平台交易金额">
                </el-table-column>           
                <el-table-column prop="tradeTime" label="交易时间">
                </el-table-column>   
                <el-table-column prop="payType" label="支付方式">
                </el-table-column>    
                <el-table-column prop="differ" label="对比结果">
                </el-table-column>         
                <el-table-column prop="status" label="数据状态">
                </el-table-column>    
              </el-table>
          </el-tab-pane>
                <el-tab-pane label="微信APP支付自动对账记录" name="4">
              <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
                <el-table-column prop="tradeCode" label="商户订单号">
                </el-table-column>   
                <el-table-column prop="tradeNo" label="流水号">
                </el-table-column>
                <el-table-column prop="tradeType" label="交易类型">
                </el-table-column>
                <el-table-column prop="payable" label="商户交易金额">
                </el-table-column>  
                <el-table-column prop="thirdPayable" label="平台交易金额">
                </el-table-column>           
                <el-table-column prop="tradeTime" label="交易时间">
                </el-table-column>   
                <el-table-column prop="payType" label="支付方式">
                </el-table-column>    
                <el-table-column prop="differ" label="对比结果">
                </el-table-column>         
                <el-table-column prop="status" label="数据状态">
                </el-table-column>    
              </el-table>
          </el-tab-pane>
        </el-tabs>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="info" size="small" @click="handExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
              <el-button type="info" size="small" @click="printtf()"><i class="icon-printer icons"></i> 打印</el-button>
               <el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 自动对账</el-button>
            </el-button-group>
          </el-col>
          <el-col :span="16" style="text-align: right">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="table.pageIndex"
              :page-sizes="[25, 50, 75, 100]"
              :page-size="25"
              layout="total, sizes, prev, pager, next, jumper"
              :total="table.total">
            </el-pagination>
          </el-col>
        </el-row>
        </bc-panel>
       <el-dialog :title="form.title" v-model="form.visible" size="tiny">
          <el-form ref="form" :model="form.data" label-width="80px">
            <el-form-item label="开始时间:">
             <el-date-picker
              v-model="form.data.startTime"
              type="date"
              placeholder="选择日期">
            </el-date-picker>
            <span style="color: #E6A23C">要与平台对账表开始时间一致！</span>
            </el-form-item>      
            <el-form-item label="结束时间:">
             <el-date-picker
              v-model="form.data.endTime"
              type="date"
              placeholder="选择日期">
            </el-date-picker>
            <span style="color: #E6A23C">要与平台对账表结束时间一致！</span>
            </el-form-item>
            <el-form-item label="支付账单">
              <el-upload
                class="upload-demo"
                v-bind:action="uploadUrl"
                :on-success="handleAvatarSuccess" :name="file">
                <el-button size="small" type="primary">点击上传</el-button>
                <div slot="tip" class="el-upload__tip">只能上传2M的文件</div>
              </el-upload>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="handleSubmit()">开始对账</el-button>
              <el-button @click="form.visible=false">取消</el-button>
            </el-form-item>
          </el-form>
        </el-dialog>
      </el-col>
    </el-row>

    <div id="pdf-wrap" style="display:none;">
          <el-row :gutter="20" style="margin-bottom: 20px">
             <el-col :span='24' v-if="payType==1" >
                  <div  style="width: 100%;text-align: center;">微信网页支付自动对账记录</div>
             </el-col>
              <el-col :span='24' v-if="payType==2" >
                  <div  style="width: 100%;text-align: center;">支付宝支付自动对账记录</div>
             </el-col>
              <el-col :span='24' v-if="payType==3" >
                  <div  style="width: 100%;text-align: center;">银联支付自动对账记录</div>
             </el-col>
             <el-col :span='24' v-if="payType==4" >
                  <div  style="width: 100%;text-align: center;">微信APP支付自动对账记录</div>
             </el-col>
          </el-row>
              <el-row :gutter="20" >
            <el-col :span='24'>
                <table width="100%" border="1" cellspacing="0" cellpadding="0">
                <tr width="100%">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">商户订单号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">流水号</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">交易类型</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">商户交易金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">平台交易金额</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">交易时间</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">支付方式</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">对比结果</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">数据状态</td>
                </tr>
                <tr width="100%" v-for="(item,index) in table.items">
                   <td style="padding: 5px;width:15%;font-size:12px;text-align:center;">{{item.tradeCode}}</td>
                    <td style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeNo}}</td>
                    <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeType}}</td>
                   <td  style="padding: 5px;font-size:12px;text-align:center;">{{item.payable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.thirdPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.tradeTime}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.payType}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.differ}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;">{{item.status}}</td>
                </tr>
                <tr width="100%">
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">合计</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalThirdPayable}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 150px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 100px">&nbsp;</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">{{totalDiffer}}</td>
                   <td style="padding: 5px;font-size:12px;text-align:center;width: 80px">&nbsp;</td>
                </tr>
                <tr width="100%">
                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
                   <td colspan="6"  style="padding: 5px;font-size:12px;text-align:left;">日期：</td>
                </tr>   
                <tr width="100%">
                   <td colspan="3" style="padding: 5px;font-size:12px;text-align:left;">制表人：</td>
                   <td colspan="6"  style="padding: 5px;font-size:12px;text-align:left;">审核人：</td>
                </tr>
              </table>
            </el-col>
          </el-row>
      </div>
  </div>
</template>

<script>
module.exports = {
  data: function () {
    return {
    	searchStr: '',
      table: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [
           {tradeCode: '213123123', tradeNo: '12312312',tradeType: '消费', payable: '120', thirdPayable: '120', tradeTime: '2017-12-28', payType: '支付宝支付', differ: '0', status: '正常' }
        ],
        selections: [],
      },
      payType:"1",
      totalPayable:0,
      totalThirdPayable:0,
      totalDiffer:0,
      totalResult:0,
      uploadUrl:this.baseUrl+"admin/file/upload"+'&file=file',
      form: {
        visible: false,
        title: '',
        disabled: false,
        data: {
          startTime: '',
          endTime: '',
          file: '',
        }
      },
      statusMap:{1:"正常",2:"支付平台缺失",3:"系统数据缺失"},
      tradeTypeMap:{
            1 : '消费',
            2 : '退款'
        },
      payTypeMap:{
            1 : '微信支付',
            2 : '支付宝支付',
            3 : '银联支付',
            4 : 'APP微信支付'
        },
      search: {
      	visible: false,
        useTradeCode: true,
        useTradeNo: true,
        useTradeType: true,
        useDiffer: true,
        useStatus: true,
        useStartTime: true,
        useEndTime: true,
        data: {
          tradeCode: '',
          tradeNo: '',
          tradeType: '',
          status: '',
          startTime: '',
          endTime: '',
          differ: ''
        }
      }
    }
  },
  methods: {
    getTradeType(val) {
      if (val == 1) {
        return '消费';
      } else if (val == 2) {
        return '退款';
      } else {
        return '';
      }
    },
    getStatus(val) {
      if (val == 1) {
        return '正常';
      } else if (val == 2) {
        return '支付平台缺失';
      } else if (val == 3) {
        return '系统数据缺失';
      } else {
        return '';
      }
    },
    getDiffer(val) {
      if (val == 1) {
        return '等于0';
      } else if (val == 2) {
        return '大于0';
      } else if (val == 3) {
        return '小于0';
      } else {
        return '';
      }
    },
    handleSelectionChange(val) {
      this.table.selections = val;
    },
    handleSizeChange(val) {
      this.table.pageCount = val;
      this.handleSearch();
    },
    handleCurrentChange(val) {
      this.table.pageIndex = val;
      this.handleSearch();
    },
    handleDelete(id, row) {

    },
    handleDeleteSelections() {

    },
    handleSubmit() {
      var me=this;
      key={};
        if(!this.form.data.startTime){
          this.$message({
            type: 'warning',
            message: '开始时间不可为空！'
          });   
          return;
        }  
        if(!this.form.data.endTime){
          this.$message({
            type: 'warning',
            message: '结束时间不可为空！'
          });   
          return;
        }  
        if(!this.form.data.file){
          this.$message({
            type: 'warning',
            message: '支付对账单不可为空！'
          });   
          return;
        }
        
        key.startTime=this.$filter(this.form.data.startTime,'yyyy-mm-dd','0');
        key.endTime=this.$filter(this.form.data.endTime,'yyyy-mm-dd','0');
        key.file=this.form.data.file;
        key.payType=this.payType;
        this.$post("admin/order/checkBalance",key)
          .then2(function(response){
            if(response.data.error>0){
              me.$notify({
                title: '警告',
                message: response.data.errmsg,
                type: 'warning'
              });
            }else{
              me.$notify({
                  title: '成功',
                  message: '',
                  type: 'success'
              });
              me.handleSearch();
            }
     
      }).catch(function(err){
       console.log(err);
      });
    },
    handleCreate() {
      this.form.title = "上传对账单";
      this.form.disabled = false;
      this.form.data.startTime = '';
      this.form.data.endTime = '';
      this.form.data.file = '';
      this.form.visible = true;
    },
    handleClick(tab, event){
      this.payType=tab.$options.propsData.name;
      this.table.pageIndex=1;
      this.table.pageCount=25;
      this.handleSearch();
    },
    handleSearch() {
      var key={};
      var me=this;
      me.searchStr = '搜索';
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      if(this.search.useTradeCode&&this.search.data.tradeCode){
        key.tradeCode=this.search.data.tradeCode;
        me.searchStr += ' 订单号: ' + key.tradeCode;
        if(key.tradeCode != '') {
						me.searchStr += '；';
				}
      }
      if(this.search.useTradeNo&&this.search.data.tradeNo){
        key.tradeNo=this.search.data.tradeNo;
        me.searchStr += ' 流水号: ' + key.tradeNo;
        if(key.searchStr != '') {
						me.searchStr += '；';
				}
      }
      if(this.search.useTradeType&&this.search.data.tradeType){
        key.tradeType=this.search.data.tradeType;
        me.searchStr += ' 交易类型: ' + this.getTradeType(key.tradeType);
        if(key.tradeType != '') {
						me.searchStr += '；';
				}
      }
      if(this.search.useStatus&&this.search.data.status){
        key.status=this.search.data.status;
        me.searchStr += ' 数据状态: ' + this.getStatus(key.status);
        if(key.status != '') {
						me.searchStr += '；';
				}
      } 
      if(this.search.useDiffer&&this.search.data.differ){
        key.differ=this.search.data.differ;
        me.searchStr += ' 对比结果: ' + key.differ+this.getDiffer(key.differ);
        if(key.differ != '') {
						me.searchStr += '；';
				}
      }
      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime=this.$formatDate(this.search.data.startTime);
        me.searchStr += ' 开始支付时间: ' + key.startTime ;
        if(key.startTime != '') {
						me.searchStr += '；';
				}
      }
       if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$formatDate(this.search.data.endTime);
        me.searchStr += ' 结束支付时间: ' + key.endTime;
        if(key.endTime != '') {
						me.searchStr += '；';
				}
      }
      key.payType=this.payType;
      var me=this;
      this.$get("admin/order/auditRecords",key)
          .then2(function(response){
                for(var i=0;i<response.data.data.length;i++){
                   response.data.data[i].status=me.statusMap[response.data.data[i].status];
                   response.data.data[i].payType=me.payTypeMap[response.data.data[i].payType];
                   response.data.data[i].tradeType=me.tradeTypeMap[response.data.data[i].tradeType];
                }
                 me.table.items=response.data.data;
                 me.table.total=response.data.totalRow;
                 me.totalPayable=response.data.totalPayable;
                 me.totalThirdPayable=response.data.totalThirdPayable;
                 me.totalDiffer=response.data.totalDiffer;
                 me.totalResult=(parseFloat(response.data.totalPayable)-parseFloat(response.data.totalThirdPayable)).toFixed(2);
                 me.search.visible=false;
      }).catch(function(err){
       console.log(err);
      });
    },
    //导出Excel表
    handExport(){
      var key={};
      if(this.search.useTradeCode&&this.search.data.tradeCode){
        key.tradeCode=this.search.data.tradeCode;
      }
       if(this.search.useTradeNo&&this.search.data.tradeNo){
        key.tradeNo=this.search.data.tradeNo;
      }
       if(this.search.useTradeType&&this.search.data.tradeType){
        key.tradeType=this.search.data.tradeType;
      }
       if(this.search.useStatus&&this.search.data.status){
        key.status=this.search.data.status;
      } 
       if(this.search.useDiffer&&this.search.data.differ){
        key.differ=this.search.data.differ;
      }
      if(this.search.useStartTime&&this.search.data.startTime){
        key.startTime=this.$filter(this.search.data.startTime,'yyyy-mm-dd','1');
      }
       if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$filter(this.search.data.endTime,'yyyy-mm-dd','1');
      }
      key.payType=this.payType;
     
       this.$get("admin/order/exportAuditRecords", key)
          .then2(function(response){
             if (response.error > 0) {
                    return false;
                } else {
                     window.open(response.data.path);
                }        
      }).catch(function(err){
       console.log(err);
      });
    },
    handleAvatarSuccess(res) {
         this.form.data.file=res.result.file;
      },
        //打印功能
    printtf:function(){
        this.bundprint('pdf-wrap');
    },
  },
  mounted:function(){
   this.handleSearch();
  }
}
</script>
<style>
	.input-size{
		width: 87%	
	}
	
</style>