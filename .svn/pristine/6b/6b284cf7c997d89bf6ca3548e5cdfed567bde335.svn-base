<template scoped>
  <div>
    <el-row :gutter="20" class="bottom20">
      <el-col :span="4"><bc-statbox title="人员总数" part1="123" back="#58B7FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="手机用户" part1="123" back="#20A0FF"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="邮箱用户" part1="123" back="#1D8CE0"></bc-statbox></el-col>
      <el-col :span="4"><bc-statbox title="新增人数" part1="123" back="#13CE66"></bc-statbox></el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="4">
        <bc-panel title="查询充值记录" icon="el-icon-search">
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
      </el-col>
      <el-col :span="20">
      <bc-panel title="查询结果" icon="icon-grid icons">
        <el-table :data="table.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="name" label="姓名" width="150">
          </el-table-column>
          <el-table-column prop="sex" label="性别" width="100">
          </el-table-column>
          <el-table-column prop="mobilePhone" label="手机" width="150">
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
          { id: '1', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '2', name: '欧阳小行', sex: '男', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '3', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '4', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '5', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '6', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '7', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '8', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '9', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '10', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
          { id: '11', name: 'haha', sex: '未知', disable: '正常', mobilePhone: '18000000000', note: 'abcdefg' },
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
     this.$http.get(baseUrl+"admin/customer/many")
          .then2(function(response){
                 me.table.items=response.data.result.data;
                 me.table.total=response.data.result.total;
      }).catch(function(err){
       console.log(err);
      });
    },
  },mounted:function(){

  }
}
</script>