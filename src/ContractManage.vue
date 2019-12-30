<template scoped>
  <div>
  	<!--查询弹出框-->
    <el-dialog title="高级查询" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
		    	
					 <el-input placeholder="请输入供应商名称" v-model="search.data.supplier_name" :disabled="!search.useSupplierName" class="bottom20" style="width: 87%;">
            <el-checkbox label="供应商名称" slot="prepend" v-model="search.useSupplierName"></el-checkbox>
          </el-input>
			
	      </el-col>
	          
		   	<el-col :span="12">
			    	
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
          <el-table-column prop="supplierName" label="供应商">
          </el-table-column>
          <el-table-column prop="account_period" label="账期">
          </el-table-column>
          <el-table-column prop="start_at" label="合同生效时间">
          </el-table-column> 
          <el-table-column prop="end_at" label="合同到期日期">
          </el-table-column>  
          <el-table-column prop="contract_file" label="合同文件">
            <template slot-scope="scope">
              <el-button type="primary" size="small" @click="handleDownload(scope.$index, scope.row)">下载</el-button>
            </template>
          </el-table-column>   
          <!-- <el-table-column label="编辑" width="150">
            <template slot-scope="scope">
              <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">禁用</el-button>
            </template>
          </el-table-column> -->
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <!-- <el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
                <i class="icon-minus icons"></i> 禁用
              </el-button> 
              <el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
              <el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
               <el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button> -->
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
            <el-form-item label="供应商">
              <el-input v-model="form.data.supplierName" :disabled="form.disabled"></el-input>
            </el-form-item>
            <el-form-item label="账期">
              <el-select v-model="form.data.account_period" placeholder="合同账期" style="width: 100%">
                <el-option label="1个月" value="1"></el-option>
                <el-option label="2个月" value="2"></el-option>
                <el-option label="3个月" value="3"></el-option>
                <el-option label="4个月" value="4"></el-option>
                <el-option label="5个月" value="5"></el-option>
                <el-option label="6个月" value="6"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="合同生效时间">
              <el-input v-model="form.data.start_at" placeholder="请输入合同生效时间" ></el-input>
            </el-form-item>
            <el-form-item label="合同到期日期">
              <el-input v-model="form.data.end_at" placeholder="请输入合同到期日期" ></el-input>
            </el-form-item>
            <el-form-item label="合同文件">
							<el-upload class="upload-demo" v-bind:action="uploadUrl" :on-preview="handlePreview" :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed" :file-list="fileList" :on-success="handleAvatarSuccess" :name="file">
								<el-button size="small" type="primary">点击上传</el-button>
							</el-upload>
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
    	searchStr: '',
    	uploadUrl: this.baseUrl + "/admin/file/upload/",
      table: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        selections: [],
      },
      form: {
        visible: false,
        title: '',
        disabled: false,
        data: {
          id: '',
          supplierName: '',
          account_period: '',
          start_at: '2012-09-22 12:23:21',
          end_at: '2018-09-22 12:23:21',
          contract_file: ''
        }
      },
      supplier: [{
        id: '',
        name: ''
      }],
      search: {
      	visible: false,
        useSupplierName: true,
        data: {
          supplier_name: '',
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
      this.form.title = "创建供应商合同信息表";
      this.form.supplierName = '',
      this.form.data.account_period = '';
      this.form.visible = true;
    },
    handleEdit(id, row) {
    	var me = this;
      this.form.title = "修改供应商合同信息表";
      this.form.visible = true;
      this.$post('admin/supplierContract/get', {id: row.id}).then2(function(response) {
				me.form.data = response.data.data;
				console.log(response.data.data)
			})
    },
    handleDownload(id, row) {
      var url = this.baseUrl + row.contract_file;
      window.open(url);
    },
    handleDelete(id, row) {

    },
    handleDeleteSelections() {

    },
    handleSubmit() {
    		var me = this;
				var data = me.form.data;
				if(!data.supplierName) {
					me.$notify({
						title: '警告',
						message: '请输入供应商名称！',
						type: 'warning'
					});
					return false;
				}
				if(data.id) {
					this.$post('admin/supplierContract/update', data).then2(function(response) {
						if(response.data.error > 0) {
							me.$notify({
								title: '警告',
								message: response.data.errmsg,
								type: 'warning'
							});
						} else {
							me.$notify({
								title: '成功',
								message: '保存成功',
								type: 'success'
							});
						}
						
						me.form.visible = false;
						me.handleSearch();
					}, function(response) {
						me.$notify.error({
							title: '错误',
							message: '保存失败',
						});
						me.form.visible = false;
					});
				} else {
					this.$post('admin/supplierContract/create', data).then2(function(response) {
						if(response.data.error > 0) {
							me.$notify({
								title: '警告',
								message: response.data.errmsg,
								type: 'warning'
							});
						} else {
							me.$notify({
								title: '成功',
								message: '保存成功',
								type: 'success'
							});
						}
						me.handleSearch();
						me.form.visible = false;
					}, function(response) {
						me.$notify.error({
							title: '错误',
							message: '保存失败',
						});
						me.form.visible = false;
					});
				}
    },
    changeAccountPeriod(account_period) {
      var arr = account_period.split(",");
      var type = arr[0];
      var days = arr[1];
      var str = '';

      if (type == 'day') {
        str += "日结";
      } else if (type == 'month') {
        str += '月结';
      }

      str += days + '天';

      if (type == 'month' && days == 0) {
        str = '当月结';
      }

      return str;
    },
    handleSearch() {
      var me = this;
      me.searchStr = '搜索';
      if(this.$route.query.supplierId) {
      	this.$post('admin/supplierContract/getContract', {supplierId: this.$route.query.supplierId}).then2(function(response) {
					if(response.error > 0) {
						alert(response.errmsg);
					} else {
						me.table.items = response.data.data;
					}
				}).catch(function(err) {
					console.log(err)
				});
      } else {
	      var key = {};
      	key.length = this.table.pageCount ;
	      key.offset = (this.table.pageIndex - 1) * this.table.pageCount;
	      if (this.search.useSupplierName && this.search.data.supplier_name) {
	        key.supplierName = this.search.data.supplier_name;
	        me.searchStr += ' 供应商名称： ' + key.supplierName;
					if(key.supplierName != '') {
						me.searchStr += '；';
					}
	      }
	      if (this.search.useSex&&this.search.data.sex) {
	        key.sex = this.search.data.sex;
	        me.searchStr += '"' + key.sex + '"';
					if(key.sex != '') {
						me.searchStr += ',';
					}
	      }
	      if (this.search.useNickName && this.search.data.nickName) {
	        key.nickName = this.search.data.nickName;
	        me.searchStr += '"' + key.nickName + '"';
					if(key.nickName != '') {
						me.searchStr += ',';
					}
	      }
	      if (this.search.usePhone && this.search.data.mobilePhone) {
	        key.mobilePhone = this.search.data.mobilePhone;
	        me.searchStr += '"' + key.mobilePhone + '"';
					if(key.mobilePhone != '') {
						me.searchStr += ',';
					}
	      }
	
	      this.$get("admin/supplierContract/many", key).then2(function(response){
          var data = response.data.data;
          for (var i = 0; i < data.length; i++) {
            data[i].account_period = me.changeAccountPeriod(data[i].account_period);
          }
          me.table.items = data;
          me.search.visible=false;
	      }).catch(function(err){
	        console.log(err);
	      });
      }
    },
    handleAvatarSuccess(val) {
    	this.form.data.contract_file = val.path;
    }
  },mounted:function(){
      this.handleSearch();
  }
}
</script>