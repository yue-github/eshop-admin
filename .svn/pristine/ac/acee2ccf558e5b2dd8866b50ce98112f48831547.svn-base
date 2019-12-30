<template scoped>
  <div>
  	<!--高级查询弹出框-->
    <el-dialog title="查询供应商对账明细" v-model="pointSearch.visible" size="small">
	    <el-form :inline="true" :model="pointSearch" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
          <el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="产品 " v-model="pointSearch.useProductId" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select filterable style="width: 100%;" v-model="pointSearch.data.product_id" placeholder="请选择产品" :disabled="!pointSearch.useProductId">
                <el-option v-for="item in pointSearch.product_options"
                  :key="item.id"
                  :label="item.name"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-col>
          </el-row>
		    </el-col>
		    <el-col :span="12">
		    	<el-row class="bottom20">
            <el-col :span="5" style="text-align: center">
              <el-checkbox label="是否上架" v-model="pointSearch.useIsShow" style="margin-top: 5px"></el-checkbox>
            </el-col>
            <el-col :span="16">
              <el-select style="width: 100%;" v-model="pointSearch.data.is_show" placeholder="请选择" :disabled="!pointSearch.useIsShow">
                <el-option label="是" value="是"></el-option>
                <el-option label="否" value="否"></el-option>
              </el-select>
            </el-col>
          </el-row>
		    </el-col>
	    </el-form>
	    <div slot="footer" class="dialog-footer">
		    <el-button @click="pointSearch.visible = false">取 消</el-button>
		    <el-button type="primary" @click="handleSearch()">查询</el-button>
	    </div>
  	</el-dialog>
  	
    <!-- 积分列表 -->
    <el-row :gutter="20">
      <el-col :span="24">
        <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
        	<span style="padding-bottom:20px;display: inline-block;"> {{pointSearch.searchStr}} </span>
        	<el-button type="primary" @click="pointSearch.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
          <el-table :data="pointTable.items" border @selection-change="handleSelectionChange" style="width: 100%">
            <el-table-column type="selection" width="55">
            </el-table-column>
            <el-table-column prop="product_name" label="产品">
            </el-table-column>
            <el-table-column prop="need_point" label="所需积分">
            </el-table-column> 
            <el-table-column prop="is_show" label="是否上架">
            </el-table-column> 
            <el-table-column prop="start_time" label="开始时间">
            </el-table-column>  
            <el-table-column prop="end_time" label="结束时间">
            </el-table-column>       
            <el-table-column prop="created_at" label="创建时间">
            </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button size="small" @click="editPoint(scope.row)">编辑</el-button>
                <el-button size="small" type="danger" @click="deletePoint(scope.row)">删除</el-button>
              </template>
            </el-table-column>  
          </el-table>
          <el-row slot="footer">
            <el-col :span="8">
              <el-button-group>
                <el-button type="info" size="small" @click="createPoint()"><i class="icon-arrow-down-circle icons"></i> 创建</el-button>
                <!-- <el-button type="info" size="small" @click="showProductTable()"><i class="icon-printer icons"></i> 产品</el-button> -->
              </el-button-group>
            </el-col>
            <el-col :span="16" style="text-align: right">
              <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="pointTable.pageIndex"
                :page-sizes="[25, 50, 75, 100]"
                :page-size="25"
                layout="total, sizes, prev, pager, next, jumper"
                :total="pointTable.total">
              </el-pagination>
            </el-col>
          </el-row>
        </bc-panel>
      </el-col>
    </el-row>

    <!-- 积分表单 -->
    <el-dialog
      :title="pointForm.title"
      v-model="pointForm.visible">
      <el-form ref="pointForm.data" :model="pointForm.data" :rules='pointForm.rules' label-width="100px">
        <el-form-item label="产品" prop='product_id'>
          <template>
            <el-select v-model="pointForm.data.product_id" filterable placeholder="请选择产品">
              <el-option
                v-for="item in product_options"
                :key="item.id"
                :label="item.name"
                :value="item.id">
              </el-option>
            </el-select>
          </template>
        </el-form-item>
        <el-form-item label="所需积分" prop='need_point'>
          <el-input v-model="pointForm.data.need_point"></el-input>
        </el-form-item>
        <el-form-item label="是否上架" prop='is_show'>
          <template>
            <el-select v-model="pointForm.data.is_show" filterable placeholder="请选择">
              <el-option
                v-for="item in pointForm.is_show_options"
                :key="item.value"
                :label="item.name"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
        </el-form-item>
        <el-form-item label="排序" prop='sort'>
          <el-input v-model="pointForm.data.sort"></el-input>
        </el-form-item>
        <el-form-item label="开始时间" prop='start_time'>
          <el-date-picker
            v-model="pointForm.data.start_time"
            type="datetime"
            placeholder="选择日期时间">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="结束时间" prop='end_time'>
          <el-date-picker
            v-model="pointForm.data.end_time"
            type="datetime"
            placeholder="选择日期时间">
          </el-date-picker>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitPoint()">提交</el-button>
          <el-button @click='pointForm.visible=false'>取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!-- 产品列表弹窗框 -->
    <el-dialog 
      :title="productTable.title" 
      v-model="productTable.visible" 
      size='full'
      :before-close='closeProductTable'
      class='s_dialog'>
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
        <el-table v-loading="loading" :data="productTable.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="name" label="标题">
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商">
          </el-table-column>
          <el-table-column prop="storeAmount" label="库存">
          </el-table-column>
          <el-table-column prop="suggestedRetailUnitPrice" label="售价">
          </el-table-column>
          <el-table-column prop="unitCost" label="含税成本">
          </el-table-column>
          <el-table-column prop="unitCostNoTax" label="未税成本">
          </el-table-column>
          <el-table-column prop="taxRate" label="税率(%)">
          </el-table-column>
          <el-table-column prop="invoiceType" label="发票类型">
          </el-table-column>
          <el-table-column prop="mainPic" label="主图">
            <template slot-scope="props">
              <img v-bind:src="props.row.mainPic" width="50px" height="50px" style="padding-top:5px;padding-bottom:5px;">
            </template>
          </el-table-column>
          <el-table-column label="操作" width="200">
            <template slot-scope="scope">
              <el-button size="small" @click="editProduct(scope.row)">编辑</el-button>
              <el-button size="small" type="danger" @click="deleteProduct(scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="info" size="small" @click="createProduct()"><i class="icon-arrow-down-circle icons"></i> 创建</el-button>
            </el-button-group>
          </el-col>
          <el-col :span="16" style="text-align: right">
            <el-pagination
              @size-change="handleSizeChangeProduct"
              @current-change="handleCurrentChangeProduct"
              :current-page="productTable.pageIndex"
              :page-sizes="[25, 50, 75, 100]"
              :page-size="25"
              layout="total, sizes, prev, pager, next, jumper"
              :total="productTable.total">
            </el-pagination>
          </el-col>
        </el-row>
      </bc-panel>
    </el-dialog>

    <!-- 产品表单 -->
    <el-dialog
      :title="productForm.title"
      v-model="productForm.visible">
      <el-form ref="productForm.data" :rules='productForm.rules' :model="productForm.data" label-width="100px">
        <el-form-item label="产品标题" prop='name'>
          <el-input v-model="productForm.data.name"></el-input>
        </el-form-item>
        <el-form-item label="供应商" prop='supplier_id'>
          <template>
            <el-select v-model="productForm.data.supplier_id" filterable placeholder="请选择供应商">
              <el-option
                v-for="item in supplier_options"
                :key="item.id"
                :label="item.name"
                :value="item.id">
              </el-option>
            </el-select>
          </template>
        </el-form-item>
        <el-form-item label="库存" prop='storeAmount'>
          <el-input v-model="productForm.data.storeAmount" type='number'></el-input>
        </el-form-item>
        <el-form-item label="售价" prop='suggestedRetailUnitPrice'>
          <el-input v-model="productForm.data.suggestedRetailUnitPrice" type='number'></el-input>
        </el-form-item>
        <el-form-item label="含税成本" prop='unitCost'>
          <el-input v-model="productForm.data.unitCost" @change="changeUnitCost" type='number'></el-input>
        </el-form-item>
        <el-form-item label="税率(%)" prop='taxRate'>
          <el-input v-model="productForm.data.taxRate" @change="changeTaxRate" type='number'></el-input>
        </el-form-item>
        <el-form-item label="发票类型" prop='invoiceType'>
          <template>
            <el-select v-model="productForm.data.invoiceType" placeholder="请选择发票类型" @change="changeInvoiceType">
              <el-option
                v-for="item in productForm.invoice_type_options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
        </el-form-item>
        <el-form-item label="未税成本" prop='unitCostNoTax'>
          <el-input v-model="productForm.data.unitCostNoTax" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="主图">
          <el-upload ref="upload" class="upload-demo" 
            v-bind:action="uploadUrl" 
            :on-preview="handlePreview" 
            :on-remove="handleRemove" 
            :before-remove="beforeRemove" 
            multiple 
            :limit="1" 
            :file-list="fileList" 
            :on-success="handleAvatarSuccessProduct" 
            :before-upload="beforeAvatarUpload"
            :name="file">
            <el-button size="small" type="primary">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
          </el-upload>
          <img :src="productForm.show_image" style='width:100px;'>
        </el-form-item>
        <el-form-item label="详情">
          <WangeditorUtil v-model="productForm.data.note"></WangeditorUtil>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitProduct()">提交</el-button>
          <el-button @click='productForm.visible=false'>取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
import WangeditorUtil from './WangeditorUtil.vue'
export default {
  components: {WangeditorUtil},
  data: function () {
    return {
      uploadUrl: this.baseUrl + "admin/file/upload",
      supplier_options: [],
      product_options: [],
      pointTable: {
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [{id: 1, product_id: 1, supplier_name: '天农', need_point: 20, is_show: '是', start_time: '2018-12-10', end_time: '2019-12-12', created_at: '2018-12-12'}],
        list: [], 
        selections: [],
      },
      pointForm: {
        title: '',
        visible: false,
        data: {
          sort: '0',
        },
        is_show_options: [
          {name: '是', value: '是'},
          {name: '否', value: '否'},
        ],
        rules: {
          product_id: [
            { type: 'number', required: true, message: '请选择产品', trigger: 'blur' },
          ],
          need_point: [
            { required: true, message: '请输入所需积分', trigger: 'blur' }
          ],
          is_show: [
            { required: true, message: '请选择是否上架', trigger: 'blur' }
          ],
          sort: [
            { required: true, message: '请输入排序', trigger: 'blur' }
          ],
          start_time: [
            { type: 'date', required: true, message: '请选择开始时间', trigger: 'blur' }
          ],
          end_time: [
            { type: 'date', required: true, message: '请选择结束时间', trigger: 'blur' }
          ],
        },
      },
      pointSearch: {
        searchStr: '',
      	visible: false,
        useProductId: true,
        useIsShow: true,
        data: {
          product_id: '',
          is_show: '',
        },
        product_options:[],
      },
      productTable: {
        title: '产品管理',
        visible: false,
        total: 11,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        list: [], 
        selections: [],
      },
      productForm: {
        title: '',
        visible: false,
        show_image: '',
        data: {},
        invoice_type_options: [
          {value: 'general', label: '增值税普通发票'},
          {value: 'value_add', label: '增值税专用发票'},
        ],
        rules: {
          name: [
            { required: true, message: '请输入标题', trigger: 'blur' },
          ],
          supplier_id: [
            { type: 'number', required: true, message: '请选择供应商', trigger: 'blur' }
          ],
          storeAmount: [
            { required: true, message: '请输入库存', trigger: 'blur' }
          ],
          suggestedRetailUnitPrice: [
            { required: true, message: '请输入售价', trigger: 'blur' }
          ],
          unitCost: [
            { required: true, message: '请输入含税成本', trigger: 'blur' }
          ],
          unitCostNoTax: [
            { required: true, message: '请输入未税成本', trigger: 'blur' }
          ],
          taxRate: [
            { required: true, message: '请输入税率', trigger: 'blur' }
          ],
          invoiceType: [
            { required: true, message: '请选择发票类型', trigger: 'blur' }
          ],
        },
      }
    }
  },
  methods: {
    handleAvatarSuccessProduct(val) {
      this.productForm.data.mainPic = val.path;
      this.productForm.show_image = this.$getAbsolutePath(val.path);
    },
    handleAvatarSuccessSetMeal(val) {
      this.setMealForm.data.image = val.path;
      this.setMealForm.show_image = this.$getAbsolutePath(val.path);
    },
    beforeAvatarUpload(file) {
      const isJPG = file.type === 'image/jpeg';
      const isPNG = file.type === 'image/png';
      if (!isJPG && !isPNG) {
        this.$message.error('上传图片只能是 JPG、PNG 格式!');
      }
      return isJPG || isPNG;
    },
    handlewangeditorSetContent(content){
      WangeditorUtil.methods.setContent(content);
    },
    handlewangeditorGetContent(){
      return WangeditorUtil.methods.getContent();
    },
    handleSelectionChange(val) {
      this.table.selections = val;
    },
    handleSizeChange(val) {
      this.pointTable.pageCount = val;
      this.handleSearch();
    },
    handleCurrentChange(val) {
      this.pointTable.pageIndex = val;
      this.handleSearch();
    },
    // 积分管理
    editPoint(row) {
      var me = this;
      me.pointForm.visible = true;
      me.pointForm.title = '修改积分产品';
      me.$post("admin/point/get", {id: row.id}).then2(function(response){
        var data = response.data.data;
        data.need_point += '';
        data.sort += '';
        me.pointForm.data = data;
      }).catch(function(err){
        console.log(err);
      });
    },
    createPoint() {
      var me = this;
      me.pointForm.visible = true;
      me.pointForm.title = '创建积分产品';
      me.pointForm.data = {};
      me.pointForm.data.sort = '0';
    },
    deletePoint(row) {
      var me = this;
      me.$confirm('是否要删除该记录?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        me.$post("admin/point/delete",{id: row.id}).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('删除成功');
            me.handleSearch();
            me.pointForm.visible = false;
          } else {
            me.notifyError('删除失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      }).catch(() => {
                
      });
    },
    submitPoint() {
      var me = this;
      var is_pass = false;
      if (!me.empty(me.pointForm.data.start_time) && !(me.pointForm.data.start_time instanceof Date)) {
        me.pointForm.data.start_time = new Date(me.pointForm.data.start_time);
      }
      if (!me.empty(me.pointForm.data.end_time) && !(me.pointForm.data.end_time instanceof Date)) {
        me.pointForm.data.end_time = new Date(me.pointForm.data.end_time);
      }

      me.$refs['pointForm.data'].validate((valid) => {
        is_pass = valid;
      });
      if (!is_pass) {
        return false;
      }

      var data = me.pointForm.data;
      data.start_time = me.$formatDateTime(data.start_time);
      data.end_time = me.$formatDateTime(data.end_time);
      
      if (data.id) {
        me.$post("admin/point/update",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
            me.handleSearch();
          } else {
            me.notifyError('提交失败');
          }
        }).catch(function(err){
          console.log(err);
        });
        me.pointForm.visible = false;
      } else {
        me.$post("admin/point/create",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
            me.handleSearch();
          } else {
            me.notifyError('提交失败');
          }
          me.pointForm.visible = false;
        }).catch(function(err){
          console.log(err);
        });
      }
    },
    // 产品管理
    changeUnitCost(event) {
      this.calculateUnitCostNoTax();
    },
    changeTaxRate(event) {
      this.calculateUnitCostNoTax();
    },
    changeInvoiceType(value) {
      this.calculateUnitCostNoTax();
    },
    calculateUnitCostNoTax() {
      var unitCost = this.productForm.data.unitCost;
      var taxRate = this.productForm.data.taxRate;
      var invoiceType = this.productForm.data.invoiceType;
      var rate = 1;
      if (this.empty(unitCost) || this.empty(taxRate) || this.empty(invoiceType)) {
        this.productForm.unitCostNoTax = '';
        return false;
      }
      if (invoiceType === 'value_add') {
        rate += parseFloat(taxRate) * 0.01;
      }
      var unitCostNoTax = (unitCost / rate).toFixed(2);
      this.productForm.data.unitCostNoTax = unitCostNoTax;
    },
    showProductTable() {
      this.productTable.visible = true;
      this.handleSearchProduct();
    },
    closeProductTable() {
      this.productTable.visible = false;
    },
    createProduct() {
      this.productForm.title = '创建产品';
      this.productForm.visible = true;
      this.productForm.show_image = '';
      this.productForm.data = {};
      this.handlewangeditorSetContent('');
    },
    editProduct(row) {
      var me = this;
      this.productForm.title = '修改产品';
      this.productForm.visible = true;
      me.$post("admin/point_product/get",{id: row.id}).then2(function(response){
        var data = response.data.data;
        data.suggestedRetailUnitPrice += '';
        data.unitCost += '';
        data.taxRate += '';
        data.unitCostNoTax += '';
        data.storeAmount += '';
        me.productForm.data = data;
        me.productForm.show_image = me.$getAbsolutePath(data.mainPic);
        me.handlewangeditorSetContent(me.productForm.data.note);
      }).catch(function(err){
        console.log(err);
      });
    },
    deleteProduct(row) {
      var me = this;
      me.$confirm('是否要删除该产品?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        me.$post("admin/point_product/delete",{id: row.id}).then2(function(response){
          if (response.data.error == 0) {
            me.notifySuccess('删除成功');
            me.handleSearchProduct();
          } else {
            me.notifyError('删除失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      }).catch(() => {
                
      });
    },
    submitProduct() {
      var me = this;
      var is_pass = false;
      me.$refs['productForm.data'].validate((valid) => {
        is_pass = valid;
      });
      if (!is_pass) {
        return false;
      }
      var data = me.productForm.data;
      data.note = me.handlewangeditorGetContent();
      if (data.id) {
        me.$post("admin/point_product/update",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
            me.handleSearchProduct();
          } else {
            me.notifyError('提交失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      } else {
        me.$post("admin/point_product/create",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
            me.handleSearchProduct();
          } else {
            me.notifyError('提交失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      }
      me.productForm.visible = false;
    },
    handleSearchProduct() {
      var me = this;
      var key = {};
      key.offset = (me.productTable.pageIndex - 1) * me.productTable.pageCount;
      key.length = me.productTable.pageCount;
      me.$get("admin/point_product/list",key).then2(function(response){
        var items = response.data.data;
        for (var i = 0; i < items.length; i++) {
          var item = items[i];
          item.invoiceType = me.$productInvoiceType(item.invoiceType);
          item.mainPic = me.$getAbsolutePath(item.mainPic);
        }
        me.productTable.items = items;   
        me.productTable.total = response.data.total;
      }).catch(function(err){
        console.log(err);
      });
    },
    getSearchKey(key) {
      var me = this;
      if(this.pointSearch.useIsShow && this.pointSearch.data.is_show){
        key.is_show = this.pointSearch.data.is_show;
        me.pointSearch.searchStr += '是否上架： ' + key.is_show;
      }
      return key;
    },
    handleSearch() {
        var me=this;
        var key={};
        me.pointSearch.searchStr = '搜索';
        key.length=this.pointTable.pageCount ;
        key.offset= (this.pointTable.pageIndex-1)*this.pointTable.pageCount;
        key = this.getSearchKey(key);

        this.$get("admin/point/list",key).then2(function(response){
          me.pointTable.items = response.data.data;
        }).catch(function(err){
          console.log(err);
        });
    },
    getAllSupplier(){
      var me = this;
      me.$get("admin/supplier/allSuppliers",{}).then2(function(response){
        me.supplier_options = response.data.data;
      }).catch(function(err){
        console.log(err);
      });
    },
    getAllProduct(){
      var me = this;
      var key = {};
      key.offset = 0;
      key.length = 1000;
      me.$get("admin/point/allProduct",key).then2(function(response){
        me.product_options = response.data.data;
      }).catch(function(err){
        console.log(err);
      });
    },
  },mounted:function(){
    this.handleSearch();
    this.getAllSupplier();
    this.getAllProduct();
  }
}
</script>
<style>
	.input-size{
		width: 87%	
	}
  .s_dialog .el-dialog__title {
    color: #fff;
  }
  .s_dialog .el-dialog__header {
    background-color: #50BFFF;
    padding: 20px;
  }
</style>