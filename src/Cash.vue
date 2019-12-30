<template scoped>
  <div>
<!--    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="人员总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="手机用户" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="邮箱用户" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="新增人数" part1="123" back="#13CE66"></bc-statbox></el-col>
   </el-row>
-->
<!--查询弹出框-->
	    <el-dialog title="查询优惠券" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
			    <el-col :span="12">
			    	
			    	
				   		<el-row class="bottom20">
		            <el-col :span="5" style="text-align: left">
		              <el-checkbox label="适用范围" v-model="search.useAppliedTo" style="margin-top: 5px"></el-checkbox>
		            </el-col>
		            <el-col :span="16">
		              <el-select style="width: 100%;" v-model="search.data.appliedTo" placeholder="请选择适用范围" :disabled="!search.useAppliedTo">
		                <el-option label="请选择适用范围" value=""></el-option>
		                <el-option label="订单" value="1"></el-option>
		                <el-option label="所有产品购买" value="2"></el-option>
		                <el-option label="所有服务购买" value="3"></el-option>
		                <el-option label="特定产品购买" value="4"></el-option>
		                <el-option label="特定服务购买" value="5"></el-option>
		              </el-select>
		            </el-col>
			        </el-row> 
			        
		            <el-row class="bottom20">
		            <el-col :span="5" style="text-align: left">
		              <el-checkbox label="条件" v-model="search.useConditions" style="margin-top: 5px"></el-checkbox>
		            </el-col>
		            <el-col :span="16">
		              <el-select style="width: 100%;" v-model="search.data.conditions" placeholder="请选择条件" :disabled="!search.useConditions">
		                <el-option label="请选择条件" value=""></el-option>
		                <el-option label="无条件折扣" value="1"></el-option>
		                <el-option label="达到最低额时折扣" value="2"></el-option>
		              </el-select>
		            </el-col>
			         </el-row>   
			         
				    	 <el-row class="bottom20">
		            <el-col :span="5" style="text-align: left">
		              <el-checkbox label="时间类型" v-model="search.useTimeType" style="margin-top: 5px"></el-checkbox>
		            </el-col>
		            <el-col :span="16">
		              <el-select style="width: 100%;" v-model="search.data.timeType" placeholder="" :disabled="!search.useTimeType">
		                <el-option label="存入时间" value="10"></el-option>
		                <el-option label="失效时间" value="11"></el-option>
		              </el-select>
		            </el-col>
		          </el-row>
		          
		          <el-row class="bottom20">
		            <el-col :span="5" style="text-align: left">
		              <el-checkbox label="开始" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
		            </el-col>
		            <el-col :span="16">
		              <el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
		            </el-col>
		          </el-row>
		          <el-row class="bottom20">
		            <el-col :span="5" style="text-align: left">
		              <el-checkbox label="结束" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
		            </el-col>
		            <el-col :span="16">
		              <el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
		            </el-col>
		          </el-row> 
		          
		      </el-col>
			   	<el-col :span="12">
			   		
				    	<el-input placeholder="请输入折扣关键字" v-model="search.data.cashDiscount" :disabled="!search.useCashDiscount" class="bottom20 input-size">
	            	<el-checkbox label="折扣" slot="prepend" v-model="search.useCashDiscount"></el-checkbox>
         			</el-input>
						  <el-input placeholder="请输入名称关键字" v-model="search.data.cashTitle" :disabled="!search.useCashTitle" class="bottom20 input-size">
		            <el-checkbox label="优惠券名称" slot="prepend" v-model="search.useCashTitle"></el-checkbox>
		          </el-input>
		          
			        <el-input placeholder="请输入最低购买金额大于" v-model="search.data.minPurchaseAmountMoreThan" :disabled="!search.useMinPurchaseAmountMoreThan" class="bottom20 input-size">
	            		<el-checkbox label="最低购买金额大于" slot="prepend" v-model="search.useMinPurchaseAmountMoreThan"></el-checkbox>
	          	</el-input>
	          	
			         <el-input placeholder="请输入最低购买金额小于" v-model="search.data.minPurchaseAmountLessThan" :disabled="!search.useMinPurchaseAmountLessThan" class="bottom20 input-size">
			            <el-checkbox label="最低购买金额小于" slot="prepend" v-model="search.useMinPurchaseAmountLessThan"></el-checkbox>
			        </el-input>
		        </el-col>
			</el-form>
			<div slot="footer" class="dialog-footer">
			     <el-button @click="search.visible = false">取 消</el-button>
			     <el-button type="primary" @click=" handleSearch()">查询</el-button>
			</div>
	    </el-dialog>
    <el-row :gutter="20">
     <!-- <el-col :span="4">
        <bc-panel title="查询优惠券" icon="el-icon-search">
          <div slot="footer" style="text-align: right">
            <el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
          </div>
          <el-input placeholder="请输入名称关键字" v-model="search.data.cashTitle" :disabled="!search.useCashTitle" class="bottom20">
            <el-checkbox label="优惠券名称" slot="prepend" v-model="search.useCashTitle"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="适用范围" v-model="search.useAppliedTo" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.appliedTo" placeholder="请选择适用范围" :disabled="!search.useAppliedTo">
                <el-option label="请选择适用范围" value=""></el-option>
                <el-option label="订单" value="1"></el-option>
                <el-option label="所有产品购买" value="2"></el-option>
                <el-option label="所有服务购买" value="3"></el-option>
                <el-option label="特定产品购买" value="4"></el-option>
                <el-option label="特定服务购买" value="5"></el-option>
              </el-select>
            </el-col>
          </el-row> 
            <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="条件" v-model="search.useConditions" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.conditions" placeholder="请选择条件" :disabled="!search.useConditions">
                <el-option label="请选择条件" value=""></el-option>
                <el-option label="无条件折扣" value="1"></el-option>
                <el-option label="达到最低额时折扣" value="2"></el-option>
              </el-select>
            </el-col>
          </el-row>   
         <el-input placeholder="请输入最低购买金额大于" v-model="search.data.minPurchaseAmountMoreThan" :disabled="!search.useMinPurchaseAmountMoreThan" class="bottom20">
            <el-checkbox label="最低购买金额大于" slot="prepend" v-model="search.useMinPurchaseAmountMoreThan"></el-checkbox>
          </el-input>
         <el-input placeholder="请输入最低购买金额小于" v-model="search.data.minPurchaseAmountLessThan" :disabled="!search.useMinPurchaseAmountLessThan" class="bottom20">
            <el-checkbox label="最低购买金额小于" slot="prepend" v-model="search.useMinPurchaseAmountLessThan"></el-checkbox>
          </el-input>
           <el-input placeholder="请输入折扣关键字" v-model="search.data.cashDiscount" :disabled="!search.useCashDiscount" class="bottom20">
            <el-checkbox label="折扣" slot="prepend" v-model="search.useCashDiscount"></el-checkbox>
          </el-input>
           <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="时间类型" v-model="search.useTimeType" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.timeType" placeholder="" :disabled="!search.useTimeType">
                <el-option label="存入时间" value="10"></el-option>
                <el-option label="失效时间" value="11"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="开始" v-model="search.useStartTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-date-picker v-model="search.data.startTime" :disabled="!search.useStartTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="结束" v-model="search.useEndTime" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-date-picker v-model="search.data.endTime" :disabled="!search.useEndTime || !search.useTimeType" style="width:100%" type="date" placeholder="选择日期" :picker-options="pickerOptions"></el-date-picker>
            </el-col>
          </el-row> 
        </bc-panel>
      </el-col>-->
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="cashTitle" label="优惠券名称" width="150">
          </el-table-column>
          <el-table-column prop="appliedTo" label="适用范围" width="100">
          </el-table-column>
          <el-table-column prop="conditions" label="条件" width="150">
          </el-table-column> 
          <el-table-column prop="minPurchaseAmount" label="最低购买金额" width="100">
          </el-table-column>  
          <el-table-column prop="cashDiscount" label="折扣" width="100">
          </el-table-column>           
          <el-table-column prop="depositTime" label="存入时间" width="150">
          </el-table-column> 
          <el-table-column prop="expirationTime" label="失效时间" width="150">
          </el-table-column>  
          <el-table-column label="商品" width="100">
            <template slot-scope="scope">
              <el-button size="small">活动商品</el-button>
            </template>
          </el-table-column>     
          <el-table-column label="操作" width="150">
            <template slot-scope="scope">
              <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
                <i class="icon-minus icons"></i> 禁用
              </el-button> 
              <el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
              <el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
              <el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>
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
        </el-dialog>
      </el-col>
    </el-row>
  </div>
</template>

<script>
module.exports = {
  data: function () {
    return {
      table: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [
          {cashTitle:'满100减30',appliedTo:'订单',conditions:'达到最低额时折扣',minPurchaseAmount:'100',cashDiscount:'10%',depositTime:'2018-01-31',expirationTime:'2018-02-07',},
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
      	 visible: false,
         useCashTitle:true,
         useAppliedTo:true,
         useConditions:true,
         useMinPurchaseAmountMoreThan:true,
         useMinPurchaseAmountLessThan:true,
         useCashDiscount:true,
         useTimeType:true,
         useStartTime:true,
         useEndTime:true,
        data: {
         cashTitle:'',
         appliedTo:'',
         conditions:'',
         minPurchaseAmountMoreThan:'',
         minPurchaseAmountLessThan:'',
         cashDiscount:'',
         timeType:'10',
         startTime:'',
         endTime:'',
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
      this.handleSearch();
    },
    handleCurrentChange(val) {
      this.table.pageIndex = val;
      this.handleSearch();
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
    },
    handleEdit(id, row) {
      this.form.title = "修改用户";
      this.form.disabled = true;
      this.form.data.name = row.name;
      this.form.data.sex = row.sex;
      this.form.data.role = row.role;
      this.form.data.mobilePhone = row.mobilePhone;
      this.form.data.address = row.address;
      this.form.visible = true;
    },
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
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      if(this.search.useCashTitle&&this.search.data.cashTitle){
        key.cashTitle=this.search.data.cashTitle;
      }
      if(this.search.useAppliedTo&&this.search.data.appliedTo){
        key.appliedTo=this.search.data.appliedTo;
      }
      if(this.search.useConditions&&this.search.data.conditions){
        key.conditions=this.search.data.conditions;
      }
      if(this.search.useMinPurchaseAmountMoreThan&&this.search.data.minPurchaseAmountMoreThan){
        key.minPurchaseAmountMoreThan=this.search.data.minPurchaseAmountMoreThan;
      }
      if(this.search.useMinPurchaseAmountLessThan&&this.search.data.minPurchaseAmountLessThan){
        key.minPurchaseAmountLessThan=this.search.data.minPurchaseAmountLessThan;
      }
      if(this.search.useCashDiscount&&this.search.data.cashDiscount){
        key.cashDiscount=this.search.data.cashDiscount;
      }
      if(this.search.useTimeType&&this.search.data.timeType){
        key.timeType=this.search.data.timeType;
      }
      if(this.search.useStartTime&&this.search.data.startTime&&this.search.useTimeType&&this.search.data.timeType){
         key.startTime= this.$filter(this.search.data.startTime,'yyyy-mm-dd','0');
      }
       if(this.search.useEndTime&&this.search.data.endTime&&this.search.useTimeType&&this.search.data.timeType){
        key.endTime=this.$filter(this.search.data.endTime,'yyyy-mm-dd','0');
      }
      var me=this;
     this.$get("admin/cash/many",key)
          .then2(function(response){
                 me.table.items=response.data.data;
                 me.table.total=response.data.totalRow;
                 me.search.visible = false;
      }).catch(function(err){
       console.log(err);
      });
    },

  },mounted:function(){
      this.handleSearch();
  }
}
</script>