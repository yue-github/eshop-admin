<template scoped>

  <div>
  	<!--查询弹出框-->
    <el-dialog title="查询发票记录" v-model="search.visible" size="small">
	    <el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
		    	<el-input placeholder="请输入发票编号" v-model="search.data.invoiceCode" :disabled="!search.useInvoiceCode" class="bottom20" style="width: 87%;">
            <el-checkbox label="发票编号" slot="prepend" v-model="search.useInvoiceCode"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入发票内容关键字" v-model="search.data.invoiceContent" :disabled="!search.useInvoiceContent" class="bottom20" style="width: 87%;">
            <el-checkbox label="发票内容" slot="prepend" v-model="search.useInvoiceContent"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="5" style="text-align:left;">
              <el-checkbox label="发票类型 " v-model="search.useType" style="margin-top: 5px;"></el-checkbox>
            </el-col>
            <el-col :span="16" >
              <el-select style="width:180px;margin-left: 28px;"  v-model="search.data.type" placeholder="请选择" :disabled="!search.useType" >
                <el-option label="请选择发票类型" value=""></el-option>
                <el-option label="普通发票" value="1"></el-option>
                <el-option label="电子发票" value="2"></el-option>
                <el-option label="增值发票" value="3"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="5" style="text-align:left;">
              <el-checkbox label="结束开票时间 " v-model="search.useEndTime" style="margin-top: 5px; margin-left: 10px;"></el-checkbox>
            </el-col>
            <el-col :span="16" >
              <el-date-picker  v-model="search.data.endTime"  type="date" placeholder="选择日期" :disabled="!search.useEndTime" style="width:180px;margin-left: 28px;"></el-date-picker>
            </el-col>
          </el-row>
		    </el-col>
		    <el-col :span="12">
  			  <el-input placeholder="请输入订单号" v-model="search.data.orderCode" :disabled="!search.useOrderCode" class="bottom20" style="width: 87%;">
  				<el-checkbox label="订单号" slot="prepend" v-model="search.useOrderCode"></el-checkbox>
  			  </el-input>
    			<el-input placeholder="请输入发票抬头关键字" v-model="search.data.invoiceHead" :disabled="!search.useInvoiceHead" class="bottom20" style="width: 87%;">
            <el-checkbox label="发票抬头" slot="prepend" v-model="search.useInvoiceHead"></el-checkbox>
          </el-input>  
          <el-row class="bottom20">
            <el-col :span="5" style="text-align:left;">
              <el-checkbox  label="开始开票时间" v-model="search.useStartTime" style="margin-top: 5px; margin-left: 10px; "></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-date-picker v-model="search.data.startTime" type="date" placeholder="选择日期" :disabled="!search.useStartTime" style="width:180px;margin-left: 28px;"></el-date-picker>
            </el-col>
          </el-row>
		    </el-col>
	    </el-form>
	    <div slot="footer" class="dialog-footer">
		    <el-button @click="search.visible = false">取 消</el-button>
		    <el-button type="primary" @click=" handleSearch()">查询</el-button>
	    </div>
  	</el-dialog>
    <el-row :gutter="20">
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%;">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="orderCode" label="订单号">
          </el-table-column> 
          <el-table-column prop="type" label="发票类型">
          </el-table-column> 
          <el-table-column prop="invoiceHead" label="发票抬头" >
          </el-table-column>  
          <el-table-column prop="invoiceContent" label="发票内容" >
          </el-table-column>  
          <el-table-column prop="invoiceCode" label="发票编号" >
          </el-table-column>
          <el-table-column prop="money" label="发票金额" >
          </el-table-column> 
          <el-table-column prop="created_at" label="开票时间">
          </el-table-column>    
         <!-- <el-table-column label="操作" width="150">
            <template slot-scope="scope">
              <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button>
            </template>
          </el-table-column>-->
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <!--<el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
                <i class="icon-minus icons"></i> 禁用
              </el-button> -->
              <!-- <el-button type="info" size="small" @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
              <el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button> -->
              <!--<el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>-->
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
       <!-- <el-dialog :title="form.title" v-model="form.visible" size="tiny">
          <el-form ref="form" :model="form.data" label-width="80px">
            <el-form-item label="用户姓名">
              <el-input v-model="form.data.name" :disabled="form.disabled"></el-input>
            </el-form-item>
            <el-form-item label="用户性别">
              <el-select v-model="form.data.sex" placeholder="请选择性别" style="width: 100%">
                <el-option label="未知" value="unkown"></el-option>
                <el-option label="男" value="male"></el-option>
                <el-option label="女" value="female"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="用户状态">
              <el-select v-model="form.data.disable" placeholder="是否需要禁用用户" style="width: 100%">
                <el-option label="正常" value="0"></el-option>
                <el-option label="禁用" value="1"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="手机号码">
              <el-input v-model="form.data.mobilePhone" placeholder="请输入手机号码" ></el-input>
            </el-form-item>
            <el-form-item label="用户简介">
              <el-input v-model="form.data.note" placeholder="请输入用户简介" ></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="handleSubmit()">立即更新</el-button>
              <el-button @click="form.visible=false">取消</el-button>
            </el-form-item>
          </el-form>
        </el-dialog>-->
      </el-col>
    </el-row>
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
          { invoiceCode: '1', invoiceContent: 'haha', invoiceHead: '未知', money: '正常', orderCode: '18000000000', type: 'abcdefg' ,created_at:'未知'},
        ],
        selections: [],
      },
      form: {
        visible: false,
        title: '',
        disabled: false,
        data: {
          name: '',
          sex: 'unkown',
          role: 'query',
          mobilePhone: '',
          address: ''
        }
      },
      search: {
		visible:false,//
        useInvoiceCode:true,
        useInvoiceContent:true,
        useInvoiceHead:true,
        useMoneyMoreThan:true,
        useMoneyLessThan:true,
        useOrderCode:true,
        useType:true,
        useStartTime: true,
        useEndTime: true,
        data: {
        invoiceCode:'',
        invoiceContent:'',
        invoiceHead:'',
        moneyMoreThan:'',
        moneyLessThan:'',
        orderCode:'',
        type:'',
        startTime: '',
        endTime: '',
        }
      }
    }
  },
  methods: {
    handleSelectionChange(val) {
      this.table.selections = val;
    },
    handleSizeChange(val) {
      this.table.pageCount = val;
    },
    handleCurrentChange(val) {
      this.table.pageIndex = val;
    },
    handleCreate() {
      this.form.title = "创建用户";
      this.form.disabled = false;
      this.form.data.name = '';
      this.form.data.sex = 'unkown';
      this.form.data.role = 'query';
      this.form.data.mobilePhone = '';
      this.form.data.address = '';
      this.form.visible = true;
    }/*,
    handleEdit(id, row) {
      this.form.title = "修改用户";
      this.form.disabled = true;
      this.form.data.name = row.name;
      this.form.data.sex = row.sex;
      this.form.data.role = row.role;
      this.form.data.mobilePhone = row.mobilePhone;
      this.form.data.address = row.address;
      this.form.visible = true;
    }*/,
    handleDelete(id, row) {

    },
    handleDeleteSelections() {

    },
    handleSubmit() {

    },
    handleSearch() {
      //var data=this.search.data;
      var key={};
      var me=this;
      me.searchStr = '搜索';
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      if(this.search.useInvoiceCode&&this.search.data.invoiceCode){
        key.invoiceCode=this.search.data.invoiceCode;
        me.searchStr += ' 发票编号： ' + key.invoiceCode;
					if(key.invoiceCode != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useInvoiceContent&&this.search.data.invoiceContent){
        key.invoiceContent=this.search.data.invoiceContent;
        me.searchStr += ' 发票内容： ' + key.invoiceContent;
					if(key.invoiceContent != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useInvoiceHead&&this.search.data.invoiceHead){
        key.invoiceHead=this.search.data.invoiceHead;
        me.searchStr += ' 发票抬头： ' + key.invoiceHead;
					if(key.invoiceHead != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useMoneyMoreThan&&this.search.data.moneyMoreThan){
        key.moneyMoreThan=this.search.data.moneyMoreThan;
        me.searchStr += ' 发票金额小于： ' + key.moneyMoreThan;
					if(key.moneyMoreThan != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useMoneyLessThan&&this.search.data.moneyLessThan){
        key.moneyLessThan=this.search.data.moneyLessThan;
        me.searchStr += ' 发票金额大于： ' + key.moneyLessThan;
					if(key.moneyLessThan != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useOrderCode&&this.search.data.orderCode){
        key.orderCode=this.search.data.orderCode;
        me.searchStr += ' 订单编号： ' + key.orderCode;
					if(key.orderCode != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useType&&this.search.data.type){
        key.type=this.search.data.type;
        me.searchStr += ' 发票类型： ' + (key.type=="1"?"普通纸质发票":"")+(key.type=="2"?"电子发票":"")+(key.type=="3"?"增值发票":"");
					if(key.type != '') {
						me.searchStr += '；';
					}
      }
      if(this.search.useStartTime&&this.search.data.startTime){
         key.startTime= this.$filter(this.search.data.startTime,'yyyy-mm-dd','0');
         me.searchStr += ' 开始时间： ' + key.startTime;
					if(key.startTime != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$filter(this.search.data.endTime,'yyyy-mm-dd','0');
        me.searchStr += ' 结束时间： ' + key.endTime;
					if(key.endTime != '') {
						me.searchStr += '；';
					}
      }
      var me=this;
      // alert(JSON.stringify(key));
     this.$post	("admin/invoice/many",key)
          .then2(function(response){
          			for(var i=0; i<response.data.data.length; i++){
          				response.data.data[i].type = me.$getInvoiceType(response.data.data[i].type);
          			}
                 me.table.items=response.data.data;
                 me.table.total=response.data.totalRow;
                 me.search.visible = false;
      }).catch(function(err){
       console.log(err);
      });
    },

     handleExport(){
      var key={};
      var me=this;
      if(this.search.useInvoiceCode&&this.search.data.invoiceCode){
        key.invoiceCode=this.search.data.invoiceCode;
      }
       if(this.search.useInvoiceContent&&this.search.data.invoiceContent){
        key.invoiceContent=this.search.data.invoiceContent;
      }
       if(this.search.useInvoiceHead&&this.search.data.invoiceHead){
        key.invoiceHead=this.search.data.invoiceHead;
      }
       if(this.search.useMoneyMoreThan&&this.search.data.moneyMoreThan){
        key.moneyMoreThan=this.search.data.moneyMoreThan;
      }
       if(this.search.useMoneyLessThan&&this.search.data.moneyLessThan){
        key.moneyLessThan=this.search.data.moneyLessThan;
      }
       if(this.search.useOrderCode&&this.search.data.orderCode){
        key.orderCode=this.search.data.orderCode;
      }
       if(this.search.useInvoiceCode&&this.search.data.invoiceCode){
        key.endTime=this.search.data.invoiceCode;
      }
       if(this.search.useType&&this.search.data.type){
        key.type=this.search.data.type;
      }
      if(this.search.useStartTime&&this.search.data.startTime){
         key.startTime= this.$filter(this.search.data.startTime,'yyyy-mm-dd','0');
      }
       if(this.search.useEndTime&&this.search.data.endTime){
        key.endTime=this.$filter(this.search.data.endTime,'yyyy-mm-dd','0');
      }
      this.$get("admin/excel/exportInvoice",key)
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

  },mounted:function(){
      this.handleSearch();
  }
}
</script>