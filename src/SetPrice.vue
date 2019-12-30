<template scoped>
  <div>
  	<!--查询弹出框-->
	    <el-dialog title="查询定价表" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
			    <el-col :span="12">
						<el-input placeholder="请输入产品名称" v-model="search.data.product_name" :disabled="!search.useProductName" class="bottom20 input-size">
            	<el-checkbox label="产品名称" slot="prepend" v-model="search.useProductName"></el-checkbox>
          	</el-input>
	          <el-input placeholder="请输入供应商名称" v-model="search.data.supplier_name" :disabled="!search.useSupplierName" class="bottom20 input-size">
	            <el-checkbox label="供应商名称" slot="prepend" v-model="search.useSupplierName"></el-checkbox>
	          </el-input>
		      </el-col>
			   	<el-col :span="12">
				    	<el-row class="bottom20">
		            <el-col :span="5" style="text-align: center">
		              <el-checkbox label="发布状态" v-model="search.usePublishStatus" style="margin-top: 5px"></el-checkbox>
		            </el-col>
		            <el-col :span="16">
		              <el-select style="width: 100%" v-model="search.data.publish_status" placeholder="请选择发布状态" :disabled="!search.usePublishStatus">
		                <el-option label="" value=""></el-option>
		                <el-option label="创建" value="create"></el-option>
		                <el-option label="修改" value="update"></el-option>
		              </el-select>
		            </el-col>
		          </el-row>
		          <el-row class="bottom20">
		            <el-col :span="5" style="text-align: center">
		              <el-checkbox label="审核状态" v-model="search.useStatus" style="margin-top: 5px"></el-checkbox>
		            </el-col>
		            <el-col :span="16">
		              <el-select  style="width: 100%" v-model="search.data.status" placeholder="请选择审核状态" :disabled="!search.useStatus">
		                <el-option label="" value=""></el-option>
		                <el-option label="未审核" value="normal"></el-option>
		                <el-option label="通过" value="pass"></el-option>
		                <el-option label="不通过" value="refuse"></el-option>
		              </el-select>
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
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商" width="200">
          </el-table-column>
          <el-table-column prop="product_name" label="产品名称" width="200">
          </el-table-column>
          <el-table-column prop="property" label="规格" width="200">
          </el-table-column>
          <el-table-column prop="oldSuggestedRetailUnitPrice" label="原促销价" width="100">
          </el-table-column>
          <el-table-column prop="newSuggestedRetailUnitPrice" label="新促销价" width="100">
          </el-table-column>
          <el-table-column prop="oldOriginUnitPrice" label="原零售价" width="100">
          </el-table-column>
          <el-table-column prop="newOriginUnitPrice" label="新零售价" width="100">
          </el-table-column>
          <el-table-column prop="oldUnitCost" label="原成本价" width="100">
          </el-table-column>
          <el-table-column prop="newUnitCost" label="新成本价" width="100">
          </el-table-column>  
          <el-table-column prop="publish_status" label="发布状态" width="100">
          </el-table-column> 
          <el-table-column prop="status" label="审核状态" width="100">
          </el-table-column> 
          <el-table-column prop="created_at" label="创建时间" width="150">
          </el-table-column>
          <el-table-column prop="publisher" label="创建人" width="150">
          </el-table-column>
          <el-table-column prop="last_audit_time" label="最后审核时间" width="150">
          </el-table-column>
          <el-table-column label="审核" width="150">
            <template slot-scope="scope">
              <el-button size="small" @click="handleEdit(scope.$index, scope.row)">审核</el-button>
              <el-button size="small" @click="handleDetail(scope.$index, scope.row)">详情</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="info" size="small" @click="handleExport()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
              <el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
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
        <!--审核表单-->
        <el-dialog :title="auditForm.title" :visible.sync="auditForm.visible" size='large'>
          <el-table :data="auditForm.data">
            <el-table-column property="product_name" label="产品名称" width="150"></el-table-column>
            <el-table-column property="property" label="规格" width="200"></el-table-column>
            <el-table-column property="oldSuggestedRetailUnitPrice" label="原促销价"></el-table-column>
            <el-table-column property="newSuggestedRetailUnitPrice" label="新促销价"></el-table-column>
            <el-table-column property="oldOriginUnitPrice" label="原零售价"></el-table-column>
            <el-table-column property="newOriginUnitPrice" label="新零售价"></el-table-column>
            <el-table-column property="oldUnitCost" label="原成本价"></el-table-column>
            <el-table-column property="newUnitCost" label="新成本价"></el-table-column>
          </el-table>
          <el-row class="bottom20" style='margin-top:20px'>
            <el-col :span="1">
              <span style='padding-top: 7px;display: inline-block;'>审核</span>
            </el-col>
            <el-col :span="5">
              <el-select v-model="auditForm.status" placeholder="请选择审核状态">
                <el-option label="未审核" value="normal"></el-option>
                <el-option label="不通过" value="refuse"></el-option>
                <el-option label="通过" value="pass"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <div slot="footer" class="dialog-footer">
            <el-button @click="auditForm.visible=false">取 消</el-button>
            <el-button type="primary" @click="handleSubmit()">确 定</el-button>
          </div>
        </el-dialog>
        <!--详情-->
        <el-dialog :title="detailDialog.title" v-model="detailDialog.visible" size='small'>
          <el-table :data="detailDialog.data" border @selection-change="handleSelectionChange1" style="width: 100%">
            <el-table-column prop="operator" label="审核人">
            </el-table-column>
            <el-table-column prop="status" label="审核状态">
            </el-table-column>
            <el-table-column prop="audit_time" label="审核时间">
            </el-table-column>
          </el-table>
        </el-dialog>
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
          {id:'',supplier_name:'', product_name: '', property: '', newSuggestedRetailUnitPrice: '', oldSuggestedRetailUnitPrice: '', newOriginUnitPrice: '',oldOriginUnitPrice:'',newUnitCost:'',oldUnitCost:'',publish_status:'',status:'',created_at:''},
        ],
        selections: [],
      },
      auditForm: {
        visible: false,
        title: '',
        status: '',
        data: [{
          theSameNum: '',
          product_name: '', 
          property: '', 
          newSuggestedRetailUnitPrice: '', 
          oldSuggestedRetailUnitPrice: '', 
          newOriginUnitPrice: '',
          oldOriginUnitPrice: '',
          newUnitCost: '',
          oldUnitCost: '',
          created_at:'',
          publish_status:'',
          status,
        }]
      },
      detailDialog: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        visible: false,
        title: '',
        data: [{
          operator: '',
          status: '', 
          audit_time: '', 
        }]
      },
      search: {
      	visible: false,
        useSupplierName: true,
        useProductName: true,
        usePublishStatus: true,
        useStatus: true,
        data: {
          supplier_name: '',
          product_name: '',
          publish_status: '',
          status: ''
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
      this.detailDialog.pageCount = val;
      this.handleSearch();
    },
    handleCurrentChange(val) {
      this.table.pageIndex = val;
      this.detailDialog.pageIndex = val;
      this.handleSearch();
    },
    handleEdit(id, row) {
      var me = this;
      me.auditForm.title = "定价审核";
      me.auditForm.visible = true;

      me.$get('admin/auditprice/get', {theSameNum:row.theSameNum}).then2(function(response) {
        me.auditForm.data = response.data.data;
        me.auditForm.status = response.data.status;
      }),function(response) {
        me.$notify.error({
          title: '错误',
          message: '查看失败',
        });
      };
    },
    handleDetail(id, row) {
      var me = this;
      var data = {};
      me.detailDialog.title = "审核详情";
      me.detailDialog.visible = true;
      data.length = me.detailDialog.pageCount ;
      data.offset = (me.detailDialog.pageIndex-1)*me.detailDialog.pageCount;
      data.theSameNum = row.theSameNum;
      
      me.$get('admin/auditprice/details', data).then2(function(response) {
        var list = response.data.data;
        for (var i = 0; i < list.length; i++) {
          list[i].status = me.getStatusName(list[i].status);
          list[i].publish_status = me.getStatusName(list[i].publish_status);
        }
        me.detailDialog.data = list;
      }),function(response) {
        me.$notify.error({
          title: '错误',
          message: '查看失败',
        });
      };
    },
    getStatusName(status) {
      if (status == 'normal')
        status = '未审核';
      else if (status == 'pass')
        status = '通过';
      else if (status == 'refuse')
        status = '不通过';
      else
        status = '未知';
      return status;
    },
    getPublishStatusName(publish_status) {
      if (publish_status == 'create')
        publish_status = '创建';
      else if (publish_status == 'update')
        publish_status = '修改';
      else
        publish_status = '未知';
      return publish_status;
    },
    handleSubmit() {
      var me = this;
      var data = {};
      data.theSameNum = me.auditForm.data[0].theSameNum;
      data.status = me.auditForm.status;

      if (!data.status) {
        me.$notify({
          title: '警告',
          message: '请选择审核状态！',
          type: 'warning'
        });
        return false;
      }

      me.$post('admin/auditprice/audit', data).then2(function(response){
        if (response.data.error == 0) {
          me.$notify({
            title: '成功',
            message: '审核成功',
            type: 'success'
          });
        } else if (response.data.error == 2) {
          me.$notify.error({
            title: '错误',
            message: '审核状态不能为未审核',
          });
        } else {
          me.$notify.error({
            title: '错误',
            message: '审核失败',
          });
        } 
        me.auditForm.visible=false;
        me.handleSearch();
      },function(response){
        me.$notify.error({
          title: '错误',
          message: '审核失败',
        });
        me.auditForm.visible=false;
      });
    },
    handleSearch() {
      var key={};
      var me=this;
      me.searchStr = '搜索';
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      if(this.search.useProductName&&this.search.data.product_name){
        key.product_name=this.search.data.product_name;
					me.searchStr += ' 产品名称： ' + key.product_name;
					if(key.product_name != '') {
						me.searchStr += '；';
					}
      }
      if(this.search.useSupplierName&&this.search.data.supplier_name){
        key.supplier_name=this.search.data.supplier_name;
        me.searchStr += ' 供应商名称： ' + key.supplier_name;
					if(key.supplier_name != '') {
						me.searchStr += '；';
					}
      }
      if(this.search.usePublishStatus&&this.search.data.publish_status){
        key.publish_status=this.search.data.publish_status;
        me.searchStr += ' 发布状态： ' + (key.publish_status=="create"?"创建":"修改") ;
					if(key.publish_status != '') {
						me.searchStr += '；';
					}
      }
      if(this.search.useStatus&&this.search.data.status){
        key.status=this.search.data.status;
        me.searchStr += ' 审核状态： ' + (key.status == "normal"?"未审核":'')+(key.status == "pass"?"通过":'')+(key.status == "refuse"?"未通过":'');
					if(key.status != '') {
						me.searchStr += '；';
					}
      }

      me.$get("admin/auditprice/many",key).then2(function(response){
        var data = response.data.data;
        for (var i = 0; i < data.length; i++) {
          var publish_status = data[i].publish_status;
          if (publish_status == 'create')
            publish_status = '创建';
          else if (publish_status == 'update')
            publish_status = '修改';

          var status = data[i].status;
          if (status == 'normal')
            status = '未审核';
          else if (status == 'pass')
            status = '通过';
          else if (status == 'refuse')
            status = '不通过';

          data[i].publish_status = publish_status;
          data[i].status = status;
        }
        
        me.table.items=data;
        me.table.total=response.data.totalRow;
        me.search.visible = false;
      }).catch(function(err){
        console.log(err);
      });
    },
     handleExport(){
     this.$get("admin/auditprice/exportAuditPriceInfo")
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