<template scoped>
  <div>
  	<!--查询弹出框-->
    <el-dialog title="查询消息记录" v-model="search.visible" size="small">
			<el-form :inline="true" :model="search" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
					 <el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
            <el-checkbox label="名称" slot="prepend" v-model="search.useName"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入手机号码" v-model="search.data.mobilePhone" :disabled="!search.usePhone" class="bottom20">
            <el-checkbox label="手机" slot="prepend" v-model="search.usePhone"></el-checkbox>
          </el-input>
			
	      </el-col>
	          
		   	<el-col :span="12">
		   		
			    	<el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="性别" v-model="search.useSex" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select v-model="search.data.sex" placeholder="请选择性别" :disabled="!search.useSex">
                <el-option label="未知" value="unkown"></el-option>
                <el-option label="男" value="male"></el-option>
                <el-option label="女" value="female"></el-option>
              </el-select>
            </el-col>
          </el-row>
          
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="状态" v-model="search.useDisable" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select v-model="search.data.disable" placeholder="请选择是否禁用" :disabled="!search.useDisable">
                <el-option label="禁用" value="1"></el-option>
                <el-option label="正常" value="0"></el-option>
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
    
    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="人员总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="手机用户" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="邮箱用户" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="新增人数" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
     <!-- <el-col :span="4">
        <bc-panel title="查询消息记录" icon="el-icon-search">
          <div slot="footer" style="text-align: right">
            <el-button type="primary" size="small" icon="search" @click="handleSearch()">查询</el-button>
          </div>
          <el-input placeholder="请输入名称关键字" v-model="search.data.name" :disabled="!search.useName" class="bottom20">
            <el-checkbox label="名称" slot="prepend" v-model="search.useName"></el-checkbox>
          </el-input>
          <el-input placeholder="请输入手机号码" v-model="search.data.phone" :disabled="!search.usePhone" class="bottom20">
            <el-checkbox label="手机" slot="prepend" v-model="search.usePhone"></el-checkbox>
          </el-input>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="性别" v-model="search.useSex" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.sex" placeholder="请选择性别" :disabled="!search.useSex">
                <el-option label="未知" value="unkown"></el-option>
                <el-option label="男" value="male"></el-option>
                <el-option label="女" value="female"></el-option>
              </el-select>
            </el-col>
          </el-row>
          <el-row class="bottom20">
            <el-col :span="7" style="text-align: center">
              <el-checkbox label="状态" v-model="search.useDisable" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="17">
              <el-select v-model="search.data.disable" placeholder="请选择是否禁用" :disabled="!search.useDisable">
                <el-option label="禁用" value="1"></el-option>
                <el-option label="正常" value="0"></el-option>
              </el-select>
            </el-col>
          </el-row>
        </bc-panel>
      </el-col>-->
      <el-col :span="24">
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
      	<span style="padding-bottom:20px;display: inline-block;"> {{searchStr}} </span>
      	<el-button type="primary" @click="search.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="name" label="姓名" width="150">
          </el-table-column>
          <el-table-column prop="sex" label="性别" width="100">
          </el-table-column>
          <el-table-column prop="mobilePhone" label="手机" width="150">
          </el-table-column> 
          <el-table-column prop="email" label="邮箱" width="250">
          </el-table-column>  
          <el-table-column prop="disable" label="状态" width="100">
          </el-table-column>           
          <el-table-column prop="note" label="简介">
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
             <!-- <el-button type="danger" size="small" :disabled="table.selections.length == 0" @click="handleDeleteSelections()">
                <i class="icon-minus icons"></i> 禁用
              </el-button> 
              <el-button type="info" size="small"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
              <el-button type="info" size="small"><i class="icon-printer icons"></i> 打印</el-button>
              <el-button type="success" size="small" @click="handleCreate()"><i class="icon-plus icons"></i> 添加</el-button>-->
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
    	searchStr: '',
      table: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [
          { id: '1', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' }
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
        useName: true,
        useSex: true,
        useDisable: true,
        usePhone: true,
        data: {
          name: '',
          sex: 'unkown',
          disable: '0',
          mobilePhone: ''
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
      me.searchStr = '搜索';
      key.length=this.table.pageCount ;
      key.offset= (this.table.pageIndex-1)*this.table.pageCount;
      if(this.search.useName&&this.search.data.name){
        key.name=this.search.data.name;
        me.searchStr += ' 名称： ' + key.name;
					if(key.name != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useSex&&this.search.data.sex){
        key.sex=this.search.data.sex;
        me.searchStr += ' 性别： '+(key.sex =="male"?"男":"")+(key.sex==="unkown"?"未选择":"")+(key.sex =="female"?"女":"");
					if(key.sex != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.useDisable&&this.search.data.disable){
        key.disable=this.search.data.disable;
        me.searchStr += ' 状态： ' + (key.disable == "0"?"正常":"禁用");
					if(key.disable != '') {
						me.searchStr += '；';
					}
      }
       if(this.search.usePhone&&this.search.data.mobilePhone){
        key.mobilePhone=this.search.data.mobilePhone;
        me.searchStr += ' 手机： ' + key.mobilePhone;
					if(key.mobilePhone != '') {
						me.searchStr += '；';
					}
      }
      var me=this;
     this.$get("admin/customer/many",key)
          .then2(function(response){
          			for(var i = 0; i < response.data.data.length; i++) {
								if(response.data.data[i].disable == 1) {
										response.data.data[i].disable = '是';
									} else if(response.data.data[i].disable == 0) {
										response.data.data[i].disable = '否';
									}
								}
                 me.table.items=response.data.data;
                 me.table.total=response.data.totalRow;
                 me.search.visible=false;
      }).catch(function(err){
       console.log(err);
      });
    },
  },mounted:function(){
      this.handleSearch();
  }
}
</script>