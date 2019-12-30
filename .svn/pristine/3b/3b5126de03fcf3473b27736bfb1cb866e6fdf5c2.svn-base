<template scoped>
  <div>
  	<!--查询弹出框-->
    <el-dialog title="查询销售明细" v-model="activitySearch.visible" size="small" top="5vh">
	    <el-form :inline="true" :model="activitySearch" class="demo-form-inline" label-width="100px">
		    <el-col :span="12">
          <el-input v-model="activitySearch.data.title" :disabled="!activitySearch.useTitle" class="bottom20 input-size">
            <el-checkbox label="标题" slot="prepend" v-model="activitySearch.useTitle"></el-checkbox>
          </el-input>
	    	</el-col>
	    	<el-col :span="12">
		    </el-col>
	    </el-form>
	    <div slot="footer" class="dialog-footer">
  	    <el-button @click="activitySearch.visible = false">取 消</el-button>
  	    <el-button type="primary" @click="handleActivitySearch()">查询</el-button>
	    </div>
	  </el-dialog>

    <!-- 活动列表 -->
    <el-row :gutter="10">
      <el-col :span="24">
        <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
          <span style="padding-bottom:20px;display: inline-block;"> {{activitySearch.searchStr}} </span>
          <el-button type="primary" @click="activitySearch.visible = true" style="position: absolute; right: 38px; top: 18px;">查询</el-button>
          <el-table v-loading="loading" :data="activityTable.items" border @selection-change="handleSelectionChange" style="width: 100%">
            <el-table-column type="selection" width="55">
            </el-table-column>
            <el-table-column prop="title" label="标题">
            </el-table-column>
            <el-table-column prop="total_money" label="总金额">
            </el-table-column>
            <el-table-column prop="discount" label="团购卡金额">
            </el-table-column>
            <el-table-column prop="start_time" label="开始时间">
            </el-table-column>
            <el-table-column prop="end_time" label="结束时间">
            </el-table-column>
            <el-table-column prop="created_at" label="创建时间">
            </el-table-column>
            <el-table-column label="操作" width="250">
              <template slot-scope="scope">
                <el-button size="small" @click="editActivity(scope.row)">编辑</el-button>
                <el-button size="small" @click="showLink(scope.row)">链接</el-button>
                <el-button size="small" type="danger" @click="deleteActivity(scope.row)">删除</el-button>
              </template>
            </el-table-column>  
          </el-table>
          <el-row slot="footer">
            <el-col :span="8">
              <el-button-group>
                <el-button type="info" size="small"  @click="createActivity()"><i class="icon-arrow-down-circle icons"></i> 创建</el-button>
              </el-button-group>
            </el-col>
            <el-col :span="16" style="text-align: right">
              <el-pagination
                @size-change="handleSizeChangeActivity"
                @current-change="handleCurrentChangeActivity"
                :current-page="activityTable.pageIndex"
                :page-sizes="[25, 50, 75, 100]"
                :page-size="25"
                layout="total, sizes, prev, pager, next, jumper"
                :total="activityTable.total">
              </el-pagination>
            </el-col>
          </el-row>
        </bc-panel>
      </el-col>
    </el-row>

    <!-- 活动链接 -->
    <el-dialog
      :title="linkForm.title"
      v-model="linkForm.visible">
      <el-form ref="linkForm.data" :model="linkForm.data" label-width="100px">
        <el-form-item label="PC端链接">
          <el-input v-model="linkForm.data.pc_url" :disabled='true'>
            <el-button slot="append" 
              v-clipboard:copy="linkForm.data.pc_url"
              v-clipboard:success="onCopy"
              v-clipboard:error="onError">复制</el-button>
          </el-input>
        </el-form-item>
        <el-form-item label="微信端链接">
          <el-input v-model="linkForm.data.mobile_url" :disabled='true'>
            <!-- <el-button slot="append" @click='copyUrl(linkForm.data.mobile_url)'>复制</el-button> -->
            <el-button slot="append" 
              v-clipboard:copy="linkForm.data.mobile_url"
              v-clipboard:success="onCopy"
              v-clipboard:error="onError">复制</el-button>
          </el-input>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!-- 活动弹窗框 -->
    <el-dialog 
      :title="contentForm.title" 
      v-model="contentForm.visible" 
      size='full'
      :before-close='closeContentForm'
      class='s_dialog'>
      <el-tabs v-model="activeTab" type="card" @tab-click="handleTabClick">
        <el-tab-pane label="基本信息" name="first">
          <el-form ref="contentForm.data" :rules="contentForm.rules" :model="contentForm.data" label-width="100px">
            <el-form-item label="标题" prop='title'>
              <el-input v-model="contentForm.data.title"></el-input>
            </el-form-item>
            <el-form-item label="开始时间" prop='start_time'>
              <el-date-picker
                v-model="contentForm.data.start_time"
                type="datetime"
                placeholder="选择日期时间">
              </el-date-picker>
            </el-form-item>
            <el-form-item label="结束时间" prop='end_time'>
              <el-date-picker
                v-model="contentForm.data.end_time"
                type="datetime"
                placeholder="选择日期时间">
              </el-date-picker>
            </el-form-item>
            <el-form-item label="活动总金额" prop='total_money'>
              <el-input v-model="contentForm.data.total_money" @change="changeActTotalMoney"></el-input>
            </el-form-item>
            <el-form-item label="团购卡金额" prop='discount'>
              <el-input v-model="contentForm.data.discount" @change="changeActDiscount"></el-input>
            </el-form-item>
            <el-form-item label="卡号数量">
              <el-input v-model="contentForm.data.total_amount" :disabled='true'></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitActivity()">提交</el-button>
              <el-button @click='closeContentForm()'>取消</el-button>
            </el-form-item>
          </el-form>
        </el-tab-pane>
        <el-tab-pane label="产品" name="second" :disabled='isDisabledTab'>
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
                  :current-page="1"
                  :page-sizes="[25, 50, 75, 100]"
                  :page-size="25"
                  layout="total, sizes, prev, pager, next, jumper"
                  :total="productTable.total">
                </el-pagination>
              </el-col>
            </el-row>
          </bc-panel>
        </el-tab-pane>
        <el-tab-pane label="套餐" name="third" :disabled='isDisabledTab'>
          <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
            <el-table v-loading="loading" :data="setMealTable.items" border @selection-change="handleSelectionChange" style="width: 100%">
              <el-table-column type="selection" width="55">
              </el-table-column>
              <el-table-column prop="title" label="标题">
              </el-table-column>
              <el-table-column prop="discount" label="套餐金额">
              </el-table-column>
              <el-table-column prop="store_amount" label="库存">
              </el-table-column>
              <el-table-column prop="is_sale" label="是否上架">
              </el-table-column>
              <el-table-column prop="image" label="图片">
                <template slot-scope="props">
                  <img v-bind:src="props.row.image" width="50px" height="50px" style="padding-top:5px;padding-bottom:5px;">
                </template>
              </el-table-column>
              <el-table-column label="操作" width="200">
                <template slot-scope="scope">
                  <el-button size="small" @click="editSetMeal(scope.row)">编辑</el-button>
                  <el-button size="small" @click="showSetMealProductTable(scope.row)">产品</el-button>
                  <el-button size="small" type="danger" @click="deleteSetMeal(scope.row)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <el-row slot="footer">
              <el-col :span="8">
                <el-button-group>
                  <el-button type="info" size="small"  @click="createSetMeal()"><i class="icon-arrow-down-circle icons"></i> 创建</el-button>
                </el-button-group>
              </el-col>
              <el-col :span="16" style="text-align: right">
                <el-pagination
                  @size-change="handleSizeChangeSetMeal"
                  @current-change="handleCurrentChangeSetMeal"
                  :current-page="1"
                  :page-sizes="[25, 50, 75, 100]"
                  :page-size="25"
                  layout="total, sizes, prev, pager, next, jumper"
                  :total="activityTable.total">
                </el-pagination>
              </el-col>
            </el-row>
          </bc-panel>
        </el-tab-pane>
        <el-tab-pane label="卡号" name="fourth" :disabled='isDisabledTab'>
          <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
            <el-table v-loading="loading" :data="cardTable.items" border @selection-change="handleSelectionChange" style="width: 100%">
              <el-table-column type="selection" width="55">
              </el-table-column>
              <el-table-column prop="code" label="卡号">
              </el-table-column>
              <el-table-column prop="password" label="密码">
              </el-table-column>
              <el-table-column prop="discount" label="金额">
              </el-table-column>
              <el-table-column prop="is_used" label="是否已使用">
              </el-table-column>
              <el-table-column prop="member_name" label="使用人">
              </el-table-column>
              <el-table-column prop="use_time" label="使用时间">
              </el-table-column>
              <el-table-column prop="created_at" label="创建时间">
              </el-table-column>
            </el-table>
            <el-row slot="footer">
              <el-col :span="8">
                <el-button-group>
                  <el-button type="info" size="small" @click="cardForm.visible=true"><i class="icon-arrow-down-circle icons"></i> 生成卡号</el-button>
                  <el-button type="success" size="small" @click="exportCard()"><i class="icon-arrow-down-circle icons"></i> 导出</el-button>
                </el-button-group>
              </el-col>
              <el-col :span="16" style="text-align: right">
                <el-pagination
                  @size-change="handleSizeChangeCard"
                  @current-change="handleCurrentChangeCard"
                  :current-page="1"
                  :page-sizes="[25, 50, 75, 100]"
                  :page-size="25"
                  layout="total, sizes, prev, pager, next, jumper"
                  :total="cardTable.total">
                </el-pagination>
              </el-col>
            </el-row>
          </bc-panel>
        </el-tab-pane>
      </el-tabs>
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
                v-for="item in productForm.supplier_options"
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
        <el-form-item>
          <el-button type="primary" @click="submitProduct()">提交</el-button>
          <el-button @click='productForm.visible=false'>取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!-- 套餐表单 -->
    <el-dialog
      :title="setMealForm.title"
      v-model="setMealForm.visible">
      <el-form ref="setMealForm.data" :rules='setMealForm.rules' :model="setMealForm.data" label-width="100px">
        <el-form-item label="标题" prop='title'>
          <el-input v-model="setMealForm.data.title"></el-input>
        </el-form-item>
        <el-form-item label="库存" prop='store_amount'>
          <el-input v-model="setMealForm.data.store_amount"></el-input>
        </el-form-item>
        <el-form-item label="是否上架" prop='is_sale'>
          <template>
            <el-select v-model="setMealForm.data.is_sale" placeholder="请选择">
              <el-option
                v-for="item in setMealForm.options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
        </el-form-item>
        <el-form-item label="图片">
          <el-upload ref="upload" class="upload-demo" 
            v-bind:action="uploadUrl" 
            :on-preview="handlePreview" 
            :on-remove="handleRemove" 
            :before-remove="beforeRemove" 
            multiple 
            :limit="1" 
            :file-list="fileList" 
            :on-success="handleAvatarSuccessSetMeal" 
            :before-upload="beforeAvatarUpload"
            :name="file">
            <el-button size="small" type="primary">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
          </el-upload>
          <img :src="setMealForm.show_image" style='width:100px;'>
        </el-form-item>
        <el-form-item label="详情">
          <WangeditorUtil v-model="setMealForm.data.desc"></WangeditorUtil>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitSetMeal()">提交</el-button>
          <el-button @click='setMealForm.visible=false'>取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!-- 套餐产品列表 -->
    <el-dialog
      title="套餐产品列表"
      v-model="setMealProductTable.visible"
      size='large'>
      <bc-panel title="查询结果" icon="icon-grid icons" style="position: relative;">
        <el-table v-loading="loading" :data="setMealProductTable.items" border @selection-change="handleSelectionChange" style="width: 100%">
          <el-table-column type="selection" width="55">
          </el-table-column>
          <el-table-column prop="name" label="产品标题">
          </el-table-column>
          <el-table-column prop="supplier_name" label="供应商">
          </el-table-column>
          <el-table-column prop="amount" label="数量">
          </el-table-column>
          <el-table-column prop="suggestedRetailUnitPrice" label="售价">
          </el-table-column>
          <el-table-column prop="unitCost" label="含税成本">
          </el-table-column>
          <el-table-column prop="unitCostNoTax" label="未税成本">
          </el-table-column>
          <el-table-column label="操作" width="200">
            <template slot-scope="scope">
              <el-button size="small" @click="editSetMealProduct(scope.row)">编辑</el-button>
              <el-button size="small" type="danger" @click="deleteSetMealProduct(scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-row slot="footer">
          <el-col :span="8">
            <el-button-group>
              <el-button type="info" size="small" @click="createSetMealProduct()"><i class="icon-arrow-down-circle icons"></i> 创建</el-button>
            </el-button-group>
          </el-col>
          <el-col :span="16" style="text-align: right">
            <el-pagination
              @size-change="handleSizeChangeSetMealProduct"
              @current-change="handleCurrentChangeSetMealProduct"
              :current-page="1"
              :page-sizes="[25, 50, 75, 100]"
              :page-size="25"
              layout="total, sizes, prev, pager, next, jumper"
              :total="setMealProductTable.total">
            </el-pagination>
          </el-col>
        </el-row>
      </bc-panel>
    </el-dialog>

    <!-- 套餐产品表单 -->
    <el-dialog
      :title="setMealProductForm.title"
      v-model="setMealProductForm.visible"
      size='small'>
      <el-form ref="setMealProductForm.data" :model="setMealProductForm.data" :rules='setMealProductForm.rules' label-width="100px">
        <el-form-item label="产品" prop='product_id'>
          <template>
            <el-select v-model="setMealProductForm.data.product_id" filterable placeholder="请选择产品" @change='changeSetMealProduct'>
              <el-option
                v-for="item in setMealProductForm.product_options"
                :key="item.id"
                :label="item.name"
                :value="item.id">
              </el-option>
            </el-select>
          </template>
        </el-form-item>
        <el-form-item label="数量" prop='amount'>
          <el-input v-model="setMealProductForm.data.amount"></el-input>
        </el-form-item>
        <el-form-item label="供应商">
          <el-input v-model="setMealProductForm.data.supplier_name" :disabled='true'></el-input>
        </el-form-item>
        <el-form-item label="售价">
          <el-input v-model="setMealProductForm.data.suggestedRetailUnitPrice" :disabled='true'></el-input>
        </el-form-item>
        <el-form-item label="含税成本">
          <el-input v-model="setMealProductForm.data.unitCost" :disabled='true'></el-input>
        </el-form-item>
        <el-form-item label="未税成本">
          <el-input v-model="setMealProductForm.data.unitCostNoTax" :disabled='true'></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitSetMealProduct()">提交</el-button>
          <el-button @click='setMealProductForm.visible=false'>取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!-- 卡号表单 -->
    <el-dialog
      title="卡号"
      v-model="cardForm.visible">
      <el-form ref="cardForm" :model="contentForm" label-width="100px">
        <el-form-item label="活动总金额">
          <el-input v-model="contentForm.data.total_money" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="套餐金额">
          <el-input v-model="contentForm.data.discount" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="卡号数量">
          <el-input v-model="contentForm.data.total_amount" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitCard()">生成</el-button>
          <el-button @click='cardForm.visible=false'>取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

  </div>
</template>
<style>
  .el-date-editor.el-input{
    width: 100%;
  }
</style>
<script>
import WangeditorUtil from './WangeditorUtil.vue'
export default {
  components: {WangeditorUtil},
  data: function () {
    return {
      config: {
        initialFrameWidth: null,
        initialFrameHeight: 350
      },
      loading:false,
      uploadUrl: this.baseUrl + "admin/file/upload",
      fileList: [],
      activeTab: 'first',
      isDisabledTab: false,
      activitySearch: {
        searchStr: '',
        visible: false,
        disabled: false,
        useTitle: false,
        data: {
          title: '',
        },
      },
      activityTable: {
        total: 0,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        selections: [],
      },
      linkForm: {
        title: '前端链接',
        visible: false,
        data: {},
      },
      contentForm: {
        title: '',
        visible: false,
        rules: {
          title: [
            { required: true, message: '请输入标题', trigger: 'blur' },
          ],
          start_time: [
            { type: 'date', required: true, message: '请选择开始时间', trigger: 'blur' }
          ],
          end_time: [
            { type: 'date', required: true, message: '请选择结束时间', trigger: 'blur' }
          ],
          total_money: [
            { required: true, message: '请输入活动总金额', trigger: 'blur' }
          ],
          discount: [
            { required: true, message: '请输入套餐金额', trigger: 'blur' }
          ],
        },
        data: {},
      },
      productTable: {
        total: 0,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        selections: [],
      },
      productForm: {
        visible: false,
        title: '',
        show_image: '',
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
        data: {},
        supplier_options: [],
        invoice_type_options: [
          {value: 'general', label: '增值税普通发票'},
          {value: 'value_add', label: '增值税专用发票'},
        ],
      },
      setMealTable: {
        total: 0,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        selections: [],
      },
      setMealForm: {
        visible: false,
        title: '',
        show_image: '',
        rules: {
          title: [
            { required: true, message: '请输入标题', trigger: 'blur' },
          ],
          store_amount: [
            { required: true, message: '请输入库存', trigger: 'blur' }
          ],
          is_sale: [
            { required: true, message: '请选择是否上架', trigger: 'blur' }
          ],
        },
        data: {},
        options: [{label: '是', value: '是'}, {label: '否', value: '否'}],
      },
      setMealProductTable: {
        visible: false,
        group_set_meal_id: 0,
        total: 0,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        selections: [],
      },
      setMealProductForm: {
        visible: false,
        title: '',
        rules: {
          product_id: [
            { type: 'number', required: true, message: '请选择产品', trigger: 'blur' },
          ],
          amount: [
            { required: true, message: '请输入数量', trigger: 'blur' }
          ],
        },
        data: {},
        product_options: [],
      },
      cardTable: {
        visible: false,
        total: 0,
        pageCount: 25,
        pageIndex: 1,
        items: [],
        selections: [],
      },
      cardForm: {
        visible: false,
        title: '',
        data: {},
      },
    }
  },

  methods: {
    handlewangeditorSetContent(content){
      WangeditorUtil.methods.setContent(content);
    },
    handlewangeditorGetContent(){
      return WangeditorUtil.methods.getContent();
    },
    setUEContent(name, content) {
      this.$refs[name][0].setUEContent(content);
    },
    getUEContent(name) {
      console.log(this.$refs);
      return this.$refs[name].getUEContent();
    },
    handleRemove(file, fileList) {
    },
    handlePreview(file) {
    },
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
    // 点击选项卡
    handleTabClick(tab, event) {
      var tabIndex = tab.index;
      if (tabIndex == 1) {
        this.getAllSupplier();
        this.handleSearchProduct();
      } else if (tabIndex == 2) {
        this.handleSearchSetMeal();
      } else if (tabIndex == 3) {
        this.handleSearchCard();
      }
    },
    handleSelectionChange(val) {
      this.table.selections = val;
    },
    handleSizeChangeActivity(val) {
      this.activityTable.pageCount = val;
      this.handleActivitySearch();
    },
    handleCurrentChangeActivity(val) {
      this.activityTable.pageIndex = val;
      this.handleActivitySearch();
    },

    handleSizeChangeProduct(val) {
      this.productTable.pageCount = val;
      this.handleSearchProduct();
    },
    handleCurrentChangeProduct(val) {
      this.productTable.pageIndex = val;
      this.handleSearchProduct();
    },

    handleSizeChangeSetMeal(val) {
      this.setMealTable.pageCount = val;
      this.handleSearchSetMeal();
    },
    handleCurrentChangeSetMeal(val) {
      this.setMealTable.pageIndex = val;
      this.handleSearchSetMeal();
    },

    handleSizeChangeCard(val) {
      this.cardTable.pageCount = val;
      this.handleSearchCard();
    },
    handleCurrentChangeCard(val) {
      this.cardTable.pageIndex = val;
      this.handleSearchCard();
    },

    handleSizeChangeSetMealProduct(val) {
      this.setMealProductTable.pageCount = val;
      this.handleSearchSetMealProduct();
    },
    handleCurrentChangeSetMealProduct(val) {
      this.setMealProductTable.pageIndex = val;
      this.handleSearchSetMealProduct();
    },
    // 团购卡活动
    showLink(row) {
      this.linkForm.visible = true;
      this.linkForm.data.pc_url = this.pcUrl + '#/groupActivity?id=' + row.id;
      this.linkForm.data.mobile_url = this.mobileUrl + 'index.html?page=./dist/setMealList.js?activityId=' + row.id;
    },
    onCopy: function (e) {
      this.messageSuccess('复制成功');
    },
    onError: function (e) {
      this.messageError('无法复制文本！');
    },
    changeActTotalMoney(event) {
      this.calculateCardAmount();
    },
    changeActDiscount(event) {
      this.calculateCardAmount();
    },
    calculateCardAmount() {
      var total_money = this.contentForm.data.total_money;
      var discount = this.contentForm.data.discount;
      if (!total_money || !discount || total_money <= 0 || discount <= 0) {
        this.contentForm.data.total_amount = '';
        return;
      }
      var total_amount = total_money / discount;
      this.contentForm.data.total_amount = total_amount.toFixed(2);
    },
    closeContentForm() {
      this.contentForm.visible = false;
      this.activeTab = 'first';
      this.handleActivitySearch();
    },
    createActivity() {
      this.activeTab = 'first';
      this.isDisabledTab = true;
      this.contentForm.title = '创建团购活动';
      this.contentForm.visible = true;
      this.contentForm.data = {};
    },
    editActivity(row) {
      var me = this;
      this.isDisabledTab = false;
      this.contentForm.title = '修改团购活动';
      this.contentForm.visible = true;
      me.$post("admin/group_activity/get",{id: row.id}).then2(function(response){
        var data = response.data.data;
        data.total_money += '';
        data.discount += '';
        me.contentForm.data = data;
        me.calculateCardAmount();
      }).catch(function(err){
        console.log(err);
      });
    },
    deleteActivity(row) {
      var me = this;
      me.$confirm('是否要删除该活动?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        me.$post("admin/group_activity/delete",{id: row.id}).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('删除成功');
            me.handleActivitySearch();
          } else if (error == -2) {
            me.notifyWarning('该活动已启用，不能删除');
          } else {
            me.notifyError('删除失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      }).catch(() => {
                
      });
    },
    submitActivity() {
      var me = this;
      var is_pass = false;
      if (!me.empty(me.contentForm.data.start_time) && !(me.contentForm.data.start_time instanceof Date)) {
        me.contentForm.data.start_time = new Date(me.contentForm.data.start_time);
      }
      if (!me.empty(me.contentForm.data.end_time) && !(me.contentForm.data.end_time instanceof Date)) {
        me.contentForm.data.end_time = new Date(me.contentForm.data.end_time);
      }
      me.$refs['contentForm.data'].validate((valid) => {
        is_pass = valid;
      });
      if (!is_pass) {
        return false;
      }
      var data = me.contentForm.data;
      data.start_time = me.$formatDateTime(data.start_time);
      data.end_time = me.$formatDateTime(data.end_time);
      
      if (data.id) {
        me.$post("admin/group_activity/update",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
          } else if (error == -1) {
            me.notifyWarning('该活动已启用，不能修改');
          }  else if (error == -3) {
            me.notifyWarning('卡号数量不是整数');
          } else {
            me.notifyError('提交失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      } else {
        me.$post("admin/group_activity/create",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.contentForm.data.id = response.data.id;
            me.isDisabledTab = false;
            me.notifySuccess('提交成功');
          } else if (error == -3) {
            me.notifyWarning('卡号数量不是整数');
          } else {
            me.notifyError('提交失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      }
    },
    // 产品
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
    createProduct() {
      this.productForm.title = '创建产品';
      this.productForm.visible = true;
      this.productForm.show_image = '';
      this.productForm.data = {};
    },
    editProduct(row) {
      var me = this;
      this.productForm.title = '修改产品';
      this.productForm.visible = true;
      me.$post("admin/group_product/get",{id: row.id}).then2(function(response){
        var data = response.data.data;
        data.suggestedRetailUnitPrice += '';
        data.unitCost += '';
        data.taxRate += '';
        data.unitCostNoTax += '';
        data.storeAmount += '';
        me.productForm.data = data;
        me.productForm.show_image = me.$getAbsolutePath(data.mainPic);
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
        me.$post("admin/group_product/delete",{id: row.id}).then2(function(response){
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
      data.relate_id = me.contentForm.data.id;
      if (data.id) {
        me.$post("admin/group_product/update",data).then2(function(response){
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
        me.$post("admin/group_product/create",data).then2(function(response){
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
    // 套餐
    createSetMeal() {
      this.setMealForm.title = '创建套餐';
      this.setMealForm.visible = true;
      this.setMealForm.show_image = '';   
      this.setMealForm.data = {};
      this.handlewangeditorSetContent('');
    },
    editSetMeal(row) {
      var me = this;
      this.setMealForm.title = '修改套餐';
      this.setMealForm.visible = true;
      me.$post("admin/group_set_meal/get",{id: row.id}).then2(function(response){
        var data = response.data.data;
        data.store_amount += '';
        me.setMealForm.data = data;
        me.setMealForm.show_image = me.$getAbsolutePath(data.image);
        me.handlewangeditorSetContent(data.desc);
      }).catch(function(err){
        console.log(err);
      });
    },
    deleteSetMeal(row) {
      var me = this;
      me.$confirm('是否要删除该套餐?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        me.$post("admin/group_set_meal/delete",{id: row.id}).then2(function(response){
          if (response.data.error == 0) {
            me.notifySuccess('删除成功');
            me.handleSearchSetMeal();
          } else {
            me.notifyError('删除失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      }).catch(() => {
                
      });
      
    },
    submitSetMeal() {
      var me = this;
      var is_pass = false;
      me.$refs['setMealForm.data'].validate((valid) => {
        is_pass = valid;
      });
      if (!is_pass) {
        return false;
      }
      var data = me.setMealForm.data;
      data.desc = me.handlewangeditorGetContent();
      data.group_activity_id = me.contentForm.data.id;
      if (data.id) {
        me.$post("admin/group_set_meal/update",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
          } else {
            me.notifyError('提交失败');
          }
          me.handleSearchSetMeal();
          me.setMealForm.visible = false;
        }).catch(function(err){
          console.log(err);
        });
      } else {
        me.$post("admin/group_set_meal/create",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
          } else {
            me.notifyError('提交失败');
          }
          me.handleSearchSetMeal();
          me.setMealForm.visible = false;
        }).catch(function(err){
          console.log(err);
        });
      }
    },
    // 套餐产品
    changeSetMealProduct(val) {
      var me = this;
      me.$get("admin/group_product/get",{id:val}).then2(function(response){
        var data = response.data.data;
        me.setMealProductForm.data.supplier_name = data.supplier_name;
        me.setMealProductForm.data.suggestedRetailUnitPrice = data.suggestedRetailUnitPrice;
        me.setMealProductForm.data.unitCost = data.unitCost;
        me.setMealProductForm.data.unitCostNoTax = data.unitCostNoTax;
      }).catch(function(err){
        console.log(err);
      });
    },
    showSetMealProductTable(row) {
      var me = this;
      var key = {};
      me.setMealProductTable.visible = true;
      me.setMealProductTable.group_set_meal_id = row.id;

      me.handleSearchSetMealProduct();
      me.getAllGroupProduct();
    },
    createSetMealProduct() {
      this.setMealProductForm.title = '创建套餐产品';
      this.setMealProductForm.visible = true;
      this.setMealProductForm.data = {};
    },
    editSetMealProduct(row) {
      var me = this;
      this.setMealProductForm.title = '修改套餐产品';
      this.setMealProductForm.visible = true;
      me.$post("admin/group_set_meal_product/get",{id: row.id}).then2(function(response){
        var data = response.data.data;
        data.amount += '';
        me.setMealProductForm.data = data;
      }).catch(function(err){
        console.log(err);
      });
    },
    deleteSetMealProduct(row) {
      var me = this;
      me.$confirm('是否要删除该套餐产品?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        me.$post("admin/group_set_meal_product/delete",{id: row.id}).then2(function(response){
          if (response.data.error == 0) {
            me.notifySuccess('删除成功');
            me.handleSearchSetMealProduct();
          } else {
            me.notifyError('删除失败');
          }
        }).catch(function(err){
          console.log(err);
        });
      }).catch(() => {
                
      });
    },
    submitSetMealProduct() {
      var me = this;
      var is_pass = false;
      me.$refs['setMealProductForm.data'].validate((valid) => {
        is_pass = valid;
      });
      if (!is_pass) {
        return false;
      }
      var data = me.setMealProductForm.data;
      if (data.id) {
        me.$post("admin/group_set_meal_product/update",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
          } else {
            me.notifyError('提交失败');
          }
          me.handleSearchSetMealProduct();
          me.setMealProductForm.visible = false;
        }).catch(function(err){
          console.log(err);
        });
      } else {
        data.group_activity_id = me.contentForm.data.id;
        data.group_set_meal_id = me.setMealProductTable.group_set_meal_id;
        me.$post("admin/group_set_meal_product/create",data).then2(function(response){
          var error = response.data.error;
          if (error == 0) {
            me.notifySuccess('提交成功');
          } else {
            me.notifyError('提交失败');
          }
          me.handleSearchSetMealProduct();
          me.setMealProductForm.visible = false;
        }).catch(function(err){
          console.log(err);
        });
      }
    },
    // 卡号
    submitCard() {
      var me = this;
      var group_activity_id = me.contentForm.data.id;
      me.$get("admin/group_activity_card/create",{group_activity_id:group_activity_id}).then2(function(response){
        var error = response.data.error;
        if (error == 0) {
          me.notifySuccess('生成成功');
        } else if (error == -1) {
          me.notifyWarning('已生成了卡号，不能重复生成');
        } else if (error == -2) {
          me.notifyWarning('卡号数量不是整数');
        } else {
          me.notifyError('生成失败');
        }
        me.handleSearchCard();
        me.cardForm.visible = false;
      }).catch(function(err){
        console.log(err);
      });
    },
    // 导出卡号
    exportCard() {
      var me = this;
      var group_activity_id = me.contentForm.data.id;
      me.$get("admin/group_activity_card/export",{group_activity_id:group_activity_id}).then2(function(response){
        var error = response.data.error;
        if (error == 0) {
          window.open(response.data.path);
        } else {
          me.notifyError('导出失败');
        }
      }).catch(function(err){
        console.log(err);
      });
    },
    // 活动查询
    getActivitySearchKey(key) {
      if(this.activitySearch.useTitle && this.activitySearch.data.title){
        key.title = this.activitySearch.data.title;
        me.activitySearch.searchStr += '标题： ' + key.title;
      }
      return key;
    },
    handleActivitySearch() {
      var me = this;
      var key = {};
      key.offset = (me.activityTable.pageIndex - 1) * me.activityTable.pageCount;
      key.length = me.activityTable.pageCount;
      key = me.getActivitySearchKey(key);
      me.$get("admin/group_activity/list",key).then2(function(response){
        me.activityTable.items = response.data.data;   
        me.activityTable.total = response.data.total;
      }).catch(function(err){
        console.log(err);
      });
    },
    // 活动产品查询
    handleSearchProduct() {
      var me = this;
      var key = {};
      key.relate_id = me.contentForm.data.id;
      key.offset = (me.productTable.pageIndex - 1) * me.productTable.pageCount;
      key.length = me.productTable.pageCount;
      me.$get("admin/group_product/list",key).then2(function(response){
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
    // 套餐查询
    handleSearchSetMeal() {
      var me = this;
      var key = {};
      key.group_activity_id = me.contentForm.data.id;
      key.offset = (me.setMealTable.pageIndex - 1) * me.setMealTable.pageCount;
      key.length = me.setMealTable.pageCount;
      me.$get("admin/group_set_meal/list",key).then2(function(response){
        var items = response.data.data;
        for (var i = 0; i < items.length; i++) {
          var item = items[i];
          item.image = me.$getAbsolutePath(item.image);
        }
        me.setMealTable.items = items;   
        me.setMealTable.total = response.data.total;
      }).catch(function(err){
        console.log(err);
      });
    },
    // 套餐产品查询
    handleSearchSetMealProduct() {
      var me = this;
      var key = {};
      key.group_set_meal_id = me.setMealProductTable.group_set_meal_id;
      key.offset = (me.setMealProductTable.pageIndex - 1) * me.setMealProductTable.pageCount;
      key.length = me.setMealProductTable.pageCount;
      me.$get("admin/group_set_meal_product/list",key).then2(function(response){
        var items = response.data.data;
        me.setMealProductTable.items = items;   
        me.setMealProductTable.total = response.data.total;
      }).catch(function(err){
        console.log(err);
      });
    },
    // 卡号列表查询
    handleSearchCard() {
      var me = this;
      var key = {};
      key.group_activity_id = me.contentForm.data.id;
      key.offset = (me.cardTable.pageIndex - 1) * me.cardTable.pageCount;
      key.length = me.cardTable.pageCount;
      me.$get("admin/group_activity_card/list",key).then2(function(response){
        var items = response.data.data;
        me.cardTable.items = items;   
        me.cardTable.total = response.data.total;
      }).catch(function(err){
        console.log(err);
      });
    },
    getAllSupplier(){
      var me = this;
      me.$get("admin/supplier/allSuppliers",{}).then2(function(response){
        me.productForm.supplier_options = response.data.data;
      }).catch(function(err){
        console.log(err);
      });
    },
    getAllGroupProduct(){
      var me = this;
      var id = me.contentForm.data.id;
      var key = {};
      key.relate_id = id;
      key.offset = 0;
      key.length = 1000;
      me.$get("admin/group_product/list",key).then2(function(response){
        me.setMealProductForm.product_options = response.data.data;
      }).catch(function(err){
        console.log(err);
      });
    },
  },mounted:function(){
      this.handleActivitySearch();
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