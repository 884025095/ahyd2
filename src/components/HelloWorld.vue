<template>
  <div>
    <el-button type="primary" icon="el-icon-search" @click="selectRouteCable">搜 索</el-button>
    <el-button type="primary" icon="el-icon-plus" @click="dialogFormVisible = true">添 加</el-button>
    <el-button type="primary" icon="el-icon-plus" @click="caxun">查 询</el-button>
    <el-table
        :data="routeCableList"
        border
        style="width: 100%">
      <el-table-column
          prop="cable_seg_name"
          label="光缆段"
          width="250">
      </el-table-column>
      <el-table-column
          prop="station_a"
          label="A端站点"
          width="120">
      </el-table-column>
      <el-table-column
          prop="station_z"
          label="Z端站点"
          width="120">
      </el-table-column>
      <el-table-column
          prop="cable_name"
          label="光缆信息"
          width="200">
      </el-table-column>
      <el-table-column
          prop="is_main_backup"
          label="主备标识"
          width="300">
      </el-table-column>
      <el-table-column
          label="操作"
          width="100">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
          <el-button type="text" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
        @next-click="next"
        @prev-click="previous"
        @current-change="handleCurrentChange"
        :current-page="this.currentPage"
        :page-sizes="[100, 200, 300, 400]"
        :page-size="100"
        layout="total, sizes, prev, pager, next, jumper"
        :total="400">
    </el-pagination>

    <el-dialog title="新增规划设计评估记录" :visible.sync="dialogFormVisible" width="1300px">
      <el-form :model="form">
        <el-form-item label="规划工单编号:" :label-width="formLabelWidth">
          <el-input v-model="form.plan_bill_no" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
        <el-form-item label="规划设计名称:" :label-width="formLabelWidth">
          <el-input v-model="form.plan_design_name" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
        <el-form-item label="设计单位:" :label-width="formLabelWidth">
          <el-input v-model="form.design_company" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
        <el-form-item label="业务类型:" :label-width="formLabelWidth">
          <el-input v-model="form.spec_id" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
        <el-form-item label="项目总负责人:" :label-width="formLabelWidth">
          <el-input v-model="form.project_director" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
        <el-form-item label="专业负责人:" :label-width="formLabelWidth">
          <el-input v-model="form.spec_leader" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
        <el-form-item label="设计人:" :label-width="formLabelWidth">
          <el-input v-model="form.designer" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
        <el-form-item label="校审人:" :label-width="formLabelWidth">
          <el-input v-model="form.reviewer" autocomplete="off" :style="{width:'450px'}"></el-input>
        </el-form-item>
      </el-form>
      <div style="margin: 0px; border-bottom: 10px">
        <div class="sangcuan" style="margin: 10px;">
          <span slot="tip" class="el-upload__tip" style=" margin: 0px 10px 0px 0px;">系统规划CAD图纸:   </span>
          <el-upload
              class="upload-demo"
              action="http://localhost:8080/upload.do"
              :on-remove="handleRemove"
              :before-remove="beforeRemove"
              :on-exceed="exceedTips"
              :on-success="handleDwgSuccess"
              :limit="1">

            <el-button size="small" type="primary" style="float: left">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">只能上传dwg文件，不超过10MB,上传数量限制一个。</div>
          </el-upload>
        </div>
        <div class="sangcuan" style="margin: 10px;">
          <span slot="tip" class="el-upload__tip" style=" margin: 0px 10px 0px 0px;">系统规划Excel图纸:   </span>
          <el-upload
              class="upload-demo"
              action="http://localhost:8080/upload.do"
              :on-remove="handleRemove"
              :before-remove="beforeRemove"
              :on-exceed="exceedTips"
              :on-success="handleExcelSuccess"
              :limit="1">

            <el-button size="small" type="primary" style="float: left">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">只能上传Excel文件，不超过10MB,上传数量限制一个。</div>
          </el-upload>
        </div>
        <div class="sangcuan" style="margin: 10px;">
          <span slot="tip" class="el-upload__tip" style=" margin: 0px 10px 0px 0px;">系统规划Excel图纸:   </span>
          <el-upload
              class="upload-demo"
              action="http://localhost:8080/upload.do"
              :on-remove="handleRemove"
              :before-remove="beforeRemove"
              :on-exceed="exceedTips"
              :on-success="handleExcel2Success"
              :limit="1">

            <el-button size="small" type="primary" style="float: left">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">只能上传Excel文件，不超过10MB,上传数量限制一个。</div>
          </el-upload>
        </div>
<!--        <div >-->
<!--          <el-form :model="form">-->
<!--            <el-form-item label="左上:" :label-width="formLabelWidth">-->
<!--              <el-input v-model="form.zuoshang" autocomplete="off" style="{margin:10px auto;border-top:10px; float: left}"></el-input>-->
<!--            </el-form-item>-->
<!--            <el-form-item label="右下:" :label-width="formLabelWidth">-->
<!--              <el-input v-model="form.youxia" autocomplete="off" style="{left:50px;float: left}"></el-input>-->
<!--            </el-form-item>-->
<!--          </el-form>-->
<!--        </div>-->
      </div>

      <div class=" savebutton" slot="footer">
        <el-button type="primary" @click="tianjia">保 存</el-button>
        <el-button @click="dialogFormVisible = false">保存并分析</el-button>
      </div>

    </el-dialog>

  </div>

</template>

<script>
import axios from 'axios'
import {planDesignType} from "@/enum";

export default {
  name: 'HelloWord',
  data() {
    return {
      planDesignType,
      routeCableList: [],


      formLabelWidth: '120px',
      form: {
        plan_bill_no: '123',
        plan_design_name: '12345',
      },
      dialogFormVisible: false,
      dwgFileUrl: '',
    }
  },
  methods: {
    previous(){
      console.log("向前翻页")
    },
    next(){
      console.log("向下翻页")
    },
    handleCurrentChange(val){
      console.log(`当前页: ${val}`);
    },

    selectRouteCable() {
      let _this = this;
      axios.get('http://localhost:8080/selectRouteCableList.do')
          .then(function (response) {
            // 处理成功情况
            console.log(response);
            _this.routeCableList = response.data.data;
          })
          .catch(function (error) {
            // 处理错误情况
            console.log(error);
          })
          .then(function () {
            // 总是会执行
          });

    },
    tianjia() {
      let _this = this;
      _this.dialogFormVisible = false;
      axios.post('http://localhost:8080/tianjia.do', {
        "plan_bill_no": _this.form.plan_bill_no,
        "plan_design_name": _this.form.plan_design_name,
        "design_company": _this.form.design_company,
        "spec_id": _this.form.spec_id,
        "project_director": _this.form.project_director,
        "spec_leader": _this.form.spec_leader,
        "designer": _this.form.designer,
        "reviewer": _this.form.reviewer,
        "zuoshang": _this.form.zuoshang,
        "youxia": _this.form.youxia,
      })
          .then(function (response) {
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    caxun() {
      let _this = this;
      axios.get('http://localhost:8080/caxun.do')
          .then(function (response) {
            // 处理成功情况
            console.log(response);
            _this.routeCableList = response.data.data;
          })
          .catch(function (error) {
            // 处理错误情况
            console.log(error);
          })
          .then(function () {
            // 总是会执行
          });

    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    beforeRemove(file, fileList) {
      return this.$confirm(`确定移除 ${file.name}？`);
    },
    handleDwgSuccess(res) {
      let _this = this;
      _this.dwgFileUrl = res.data[0];
      console.log("上传成功URL地址：", _this.dwgFileUrl);
    },
    handleExcelSuccess(res) {
      let _this = this;
      _this.dwgFileUrl = res.data[0];
      console.log("上传成功URL地址：", _this.dwgFileUrl);
    },
    handleExcel2Success(res) {
      let _this = this;
      _this.dwgFileUrl = res.data[0];
      console.log("上传成功URL地址：", _this.dwgFileUrl);
    },
    exceedTips() {
      this.$message.error("只能上传一个文件");
    }
  }

}
</script>

<style scoped>
.el-form-item {
  display: inline-block !important;
}

.el-form-item__label-warp {
  float: left;
  margin-left: 0px !important;
}

.sangcuan {
  display: inline-block !important;
  width: 400px;
  float: left;
}

.savebutton {
  display: inline-block;
  height: 40px;
  margin: 10px 10px auto;

}

</style>


