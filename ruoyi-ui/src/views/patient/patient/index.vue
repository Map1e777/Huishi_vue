<template>
  <div class="app-container">
    <el-form :model="queryParams" ref="queryForm" size="small" :inline="true" v-show="showSearch" label-width="68px">
     
      <el-form-item label="患者姓名" prop="name">
        <el-input
          v-model="queryParams.name"
          placeholder="请输入患者姓名"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="性别" prop="sex">
        <el-select v-model="queryParams.sex" placeholder="请选择性别" clearable>
          <el-option
            v-for="dict in dict.type.sys_user_sex"
            :key="dict.value"
            :label="dict.label"
            :value="dict.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="年龄" prop="age">
        <el-input
          v-model="queryParams.age"
          placeholder="请输入年龄"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="手机号" prop="phoneNumber">
        <el-input
          v-model="queryParams.phoneNumber"
          placeholder="请输入手机号"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="病历号" prop="medicalRecordId">
        <el-input
          v-model="queryParams.medicalRecordId"
          placeholder="请输入病历号"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="左眼球镜" prop="leftSph">
        <el-input
          v-model="queryParams.leftSph"
          placeholder="请输入左眼球镜"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="右眼球镜" prop="rightSph">
        <el-input
          v-model="queryParams.rightSph"
          placeholder="请输入右眼球镜"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="左眼柱镜" prop="leftCyl">
        <el-input
          v-model="queryParams.leftCyl"
          placeholder="请输入左眼柱镜"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="右眼柱镜" prop="rightCyl">
        <el-input
          v-model="queryParams.rightCyl"
          placeholder="请输入右眼柱镜"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="左眼轴位" prop="leftAxis">
        <el-input
          v-model="queryParams.leftAxis"
          placeholder="请输入左眼轴位"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="右眼轴位" prop="rightAxis">
        <el-input
          v-model="queryParams.rightAxis"
          placeholder="请输入右眼轴位"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="手术负责人" prop="operationDoctor">
        <el-input
          v-model="queryParams.operationDoctor"
          placeholder="请输入手术负责人"
          clearable
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="手术时间" prop="operationTime">
        <el-date-picker clearable
          v-model="queryParams.operationTime"
          type="date"
          value-format="yyyy-MM-dd"
          placeholder="请选择手术时间">
        </el-date-picker>
      </el-form-item>
     
      <el-form-item label="复诊时间" prop="reviewTime">
        <el-date-picker clearable
          v-model="queryParams.reviewTime"
          type="date"
          value-format="yyyy-MM-dd"
          placeholder="请选择复诊时间">
        </el-date-picker>
      </el-form-item>
     
      <!-- <el-form-item label="创建时间" prop="createdTime">
        <el-date-picker clearable
          v-model="queryParams.createdTime"
          type="date"
          value-format="yyyy-MM-dd"
          placeholder="请选择创建时间">
        </el-date-picker>
      </el-form-item>
      <el-form-item label="更新时间" prop="updatedTime">
        <el-date-picker clearable
          v-model="queryParams.updatedTime"
          type="date"
          value-format="yyyy-MM-dd"
          placeholder="请选择更新时间">
        </el-date-picker>
      </el-form-item> -->
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini" @click="handleQuery">搜索</el-button>
        <el-button icon="el-icon-refresh" size="mini" @click="resetQuery">重置</el-button>
      </el-form-item>
    </el-form>

    <el-row :gutter="10" class="mb8">
      <el-col :span="1.5">
        <el-button
          type="primary"
          plain
          icon="el-icon-plus"
          size="mini"
          @click="handleAdd"
          v-hasPermi="['patient:patient:add']"
        >新增</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
          type="success"
          plain
          icon="el-icon-edit"
          size="mini"
          :disabled="single"
          @click="handleUpdate"
          v-hasPermi="['patient:patient:edit']"
        >修改</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
          type="danger"
          plain
          icon="el-icon-delete"
          size="mini"
          :disabled="multiple"
          @click="handleDelete"
          v-hasPermi="['patient:patient:remove']"
        >删除</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
          type="warning"
          plain
          icon="el-icon-download"
          size="mini"
          @click="handleExport"
          v-hasPermi="['patient:patient:export']"
        >导出</el-button>
      </el-col>
      <right-toolbar :showSearch.sync="showSearch" @queryTable="getList"></right-toolbar>
    </el-row>

     <!-- 表格部分，表头 -->
    <el-table v-loading="loading" :data="patientList" @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55" align="center" />
      <el-table-column label="序号" align="center" prop="id" />
      <el-table-column label="患者姓名" align="center" prop="name" />
      <el-table-column label="性别" align="center" prop="sex">
        <template slot-scope="scope">
          <dict-tag :options="dict.type.sys_user_sex" :value="scope.row.sex"/>
        </template>
      </el-table-column>
      <el-table-column label="年龄" align="center" prop="age" />
      <el-table-column label="联系方式" align="center" prop="phoneNumber" />
      <el-table-column label="病历号" align="center" prop="medicalRecordId" />
      <el-table-column label="操作" align="center" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="text"
            icon="el-icon-view"
            @click="handleDetail(scope.row)"
          >详情</el-button>
          <el-button
            size="mini"
            type="text"
            icon="el-icon-edit"
            @click="handleUpdate(scope.row)"
            v-hasPermi="['patient:patient:edit']"
          >修改</el-button>
          <el-button
            size="mini"
            type="text"
            icon="el-icon-delete"
            @click="handleDelete(scope.row)"
            v-hasPermi="['patient:patient:remove']"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    
    <pagination
      v-show="total>0"
      :total="total"
      :page.sync="queryParams.pageNum"
      :limit.sync="queryParams.pageSize"
      @pagination="getList"
    />

    <!-- 添加或修改患者对话框 -->
    <el-dialog :title="title" :visible.sync="open" width="500px" append-to-body>
      <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      
        <el-form-item label="患者姓名" prop="name">
          <el-input v-model="form.name" placeholder="请输入患者姓名" />
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-radio-group v-model="form.sex">
            <el-radio
              v-for="dict in dict.type.sys_user_sex"
              :key="dict.value"
              :label="dict.value"
            >{{dict.label}}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="年龄" prop="age">
          <el-input v-model="form.age" placeholder="请输入年龄" />
        </el-form-item>
        <el-form-item label="手机号" prop="phoneNumber">
          <el-input v-model="form.phoneNumber" placeholder="请输入手机号" />
        </el-form-item>
        <el-form-item label="病历号" prop="medicalRecordId">
          <el-input v-model="form.medicalRecordId" placeholder="请输入病历号" />
        </el-form-item>
        <el-form-item label="手术时间" prop="operationTime">
          <el-date-picker clearable
            v-model="form.operationTime"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="请选择手术时间">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="手术负责人" prop="operationDoctor">
          <el-input v-model="form.operationDoctor" placeholder="请输入手术负责人" />
        </el-form-item>
        <el-form-item label="复诊时间" prop="reviewTime">
          <el-date-picker clearable
            v-model="form.reviewTime"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="请选择复诊时间">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="左眼球镜" prop="leftSph">
          <el-input v-model="form.leftSph" placeholder="请输入左眼球镜" />
        </el-form-item>
        <el-form-item label="右眼球镜" prop="rightSph">
          <el-input v-model="form.rightSph" placeholder="请输入右眼球镜" />
        </el-form-item>
        <el-form-item label="左眼柱镜" prop="leftCyl">
          <el-input v-model="form.leftCyl" placeholder="请输入左眼柱镜" />
        </el-form-item>
        <el-form-item label="右眼柱镜" prop="rightCyl">
          <el-input v-model="form.rightCyl" placeholder="请输入右眼柱镜" />
        </el-form-item>
        <el-form-item label="左眼轴位" prop="leftAxis">
          <el-input v-model="form.leftAxis" placeholder="请输入左眼轴位" />
        </el-form-item>
        <el-form-item label="右眼轴位" prop="rightAxis">
          <el-input v-model="form.rightAxis" placeholder="请输入右眼轴位" />
        </el-form-item>
        <el-form-item label="创建时间" prop="createdTime">
          <el-date-picker clearable
            v-model="form.createdTime"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="请选择创建时间">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="更新时间" prop="updatedTime">
          <el-date-picker clearable
            v-model="form.updatedTime"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="请选择更新时间">
          </el-date-picker>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm">确 定</el-button>
        <el-button @click="cancel">取 消</el-button>
      </div>
    </el-dialog>
    
    <!-- 患者详情弹窗 -->
    <el-dialog :title="form.name + '的详细信息'" :visible.sync="detailOpen" width="600px" append-to-body>
      <el-form ref="detailForm" :model="form" label-width="100px" :disabled="true">
        <el-form-item label="病历号" prop="medicalRecordId">
          <el-input v-model="form.medicalRecordId" />
        </el-form-item>
        <el-form-item label="患者姓名" prop="name">
          <el-input v-model="form.name" />
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-radio-group v-model="form.sex">
            <el-radio
              v-for="dict in dict.type.sys_user_sex"
              :key="dict.value"
              :label="dict.value"
            >{{dict.label}}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="年龄" >
          <el-input v-model="form.age" />
        </el-form-item>
        <!-- 其他字段按照相同模式补充 -->
        <el-form-item label="复诊时间">
          <el-date-picker v-model="form.reviewTime" type="date" format="yyyy-MM-dd" readonly />
        </el-form-item>
        <el-form-item label="手术负责人">
          <el-input v-model="form.operationDoctor" disabled />
        </el-form-item>
        <el-form-item label="手术时间">
          <el-date-picker v-model="form.operationTime" type="date" format="yyyy-MM-dd" readonly />
        </el-form-item>
        <el-form-item label="左眼球镜">
          <el-input v-model="form.leftSph" disabled />
        </el-form-item>
        <el-form-item label="右眼球镜">
          <el-input v-model="form.rightSph" disabled />
        </el-form-item>
        <el-form-item label="左眼轴位">
          <el-input v-model="form.leftAxis" disabled />
        </el-form-item>
        <el-form-item label="右眼轴位">
          <el-input v-model="form.rightAxis" disabled />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="detailOpen = false">关 闭</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { listPatient, getPatient, delPatient, addPatient, updatePatient } from "@/api/patient/patient";

export default {
  name: "Patient",
  dicts: ['sys_user_sex'],
  data() {
    return {
      // 新增详情弹窗控制
      detailOpen: false,
      // 遮罩层
      loading: true,
      // 选中数组
      ids: [],
      // 非单个禁用
      single: true,
      // 非多个禁用
      multiple: true,
      // 显示搜索条件
      showSearch: true,
      // 总条数
      total: 0,
      // 患者表格数据
      patientList: [],
      // 弹出层标题
      title: "",
      // 是否显示弹出层
      open: false,
      // 查询参数
      queryParams: {
        pageNum: 1,
        pageSize: 10,
        medicalRecordId: null,
        name: null,
        sex: null,
        age: null,
        phoneNumber: null,
        operationType: null,
        operationTime: null,
        operationDoctor: null,
        reviewTime: null,
        leftSph: null,
        rightSph: null,
        leftCyl: null,
        rightCyl: null,
        leftAxis: null,
        rightAxis: null,
        createdTime: null,
        updatedTime: null
      },
      // 表单参数
      form: {},
      // 表单校验
      rules: {
        medicalRecordId: [
          { required: true, message: "病历号不能为空", trigger: "blur" }
        ],
        name: [
          { required: true, message: "患者姓名不能为空", trigger: "blur" }
        ],
        sex: [
          { required: true, message: "性别不能为空", trigger: "change" }
        ],
        age: [
          { required: true, message: "年龄不能为空", trigger: "blur" }
        ],
        phoneNumber: [
          { required: true, message: "手机号不能为空", trigger: "blur" }
        ],
        leftSph: [
          { required: true, message: "左眼球镜不能为空", trigger: "blur" }
        ],
        rightSph: [
          { required: true, message: "右眼球镜不能为空", trigger: "blur" }
        ],
        leftCyl: [
          { required: true, message: "左眼柱镜不能为空", trigger: "blur" }
        ],
        rightCyl: [
          { required: true, message: "右眼柱镜不能为空", trigger: "blur" }
        ],
        leftAxis: [
          { required: true, message: "左眼轴位不能为空", trigger: "blur" }
        ],
        rightAxis: [
          { required: true, message: "右眼轴位不能为空", trigger: "blur" }
        ],
      }
    };
  },
  created() {
    this.getList();
  },
  methods: {
    /** 查询患者列表 */
    getList() {
      this.loading = true;
      listPatient(this.queryParams).then(response => {
        this.patientList = response.rows;
        this.total = response.total;
        this.loading = false;
      });
    },
    // 取消按钮
    cancel() {
      this.open = false;
      this.reset();
    },
    // 表单重置
    reset() {
      this.form = {
        id: null,
        medicalRecordId: null,
        name: null,
        sex: null,
        age: null,
        phoneNumber: null,
        operationType: null,
        operationTime: null,
        operationDoctor: null,
        reviewTime: null,
        leftSph: null,
        rightSph: null,
        leftCyl: null,
        rightCyl: null,
        leftAxis: null,
        rightAxis: null,
        createdTime: null,
        updatedTime: null
      };
      this.resetForm("form");
    },
    /** 搜索按钮操作 */
    handleQuery() {
      this.queryParams.pageNum = 1;
      this.getList();
    },
    /** 重置按钮操作 */
    resetQuery() {
      this.resetForm("queryForm");
      this.handleQuery();
    },
    // 多选框选中数据
    handleSelectionChange(selection) {
      this.ids = selection.map(item => item.id)
      this.single = selection.length!==1
      this.multiple = !selection.length
    },
    /** 新增按钮操作 */
    handleAdd() {
      this.reset();
      this.open = true;
      this.title = "添加患者";
    },
    
    handleDetail(row) {
      this.reset();
      getPatient(row.id).then(response => {
        this.form = response.data;
        this.detailOpen = true;
      });
    },
    /** 修改按钮操作 */
    handleUpdate(row) {
      this.reset();
      const id = row.id || this.ids
      getPatient(id).then(response => {
        this.form = response.data;
        this.open = true;
        this.title = "修改患者";
      });
    },
    /** 提交按钮 */
    submitForm() {
      this.$refs["form"].validate(valid => {
        if (valid) {
          if (this.form.id != null) {
            updatePatient(this.form).then(response => {
              this.$modal.msgSuccess("修改成功");
              this.open = false;
              this.getList();
            });
          } else {
            addPatient(this.form).then(response => {
              this.$modal.msgSuccess("新增成功");
              this.open = false;
              this.getList();
            });
          }
        }
      });
    },
    /** 删除按钮操作 */
    handleDelete(row) {
      const ids = row.id || this.ids;
      this.$modal.confirm('是否确认删除患者编号为"' + ids + '"的数据项？').then(function() {
        return delPatient(ids);
      }).then(() => {
        this.getList();
        this.$modal.msgSuccess("删除成功");
      }).catch(() => {});
    },
    /** 导出按钮操作 */
    handleExport() {
      this.download('patient/patient/export', {
        ...this.queryParams
      }, `patient_${new Date().getTime()}.xlsx`)
    }
  }
};
</script>
