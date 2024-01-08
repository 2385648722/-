<template>
  <div>
    <el-card style="margin: 10px 15px;">
      <div slot="header" class="clearfix">
        <!-- <span>名称</span> -->
        <el-button class="button-text" type="primary">新增</el-button>
        <el-button class="button-text" type="primary" @click="selectAllRows">全选</el-button>
        <el-button class="button-text" type="danger" @click="handleDelete">删除</el-button>
        <el-upload class="button-textx" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview"
          :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3" :on-exceed="handleExceed">
          <el-button type="primary">文件上传</el-button>
        </el-upload>
      </div>

      <!-- 添加隐藏的文件选择器 -->
      <!-- <input type="file" id="fileInput" style="display: none" accept=".pdf" @change="handleFileUpload" /> -->
      <!-- 表格内容 -->
      <div class="tables">
        <el-table :data="tableData" border ref="table" style="width: 100%" height="970" :select-all="selectAll">
          <el-table-column prop="date" type="selection" label="日期" align="center">
          </el-table-column>
          <el-table-column prop="name" label="公司名称" align="center">
          </el-table-column>
          <el-table-column prop="Supplier" label="供应商" align="center">
          </el-table-column>
          <el-table-column prop="SerialNumber" label="供应商编号" align="center">
          </el-table-column>
          <el-table-column prop="IncomingMaterial" label="来料说明" align="center">
          </el-table-column>
          <el-table-column prop="Project" label="检测项目" align="center">
          </el-table-column>
          <el-table-column prop="Result" label="结论" align="center">
          </el-table-column>
          <el-table-column prop="TestingPersonnel" label="检测人" align="center">
          </el-table-column>
        </el-table>
      </div>
    </el-card>

  </div>
</template>

<script>
import VuePdf from 'vue-pdf'
// import pdfjsLib from 'pdfjs-dist' 
// import pdfjsLib from 'pdfjs-dist/legacy/build/pdf.js'

export default {
  components: {
    VuePdf
  },
  data() {
    return {
      selectAll: false,
      fileList: [{ name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100' }, { name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100' }],
      extractedData: '',
      tableData: [{
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },

      {
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },
      {
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },
      {
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },
      {
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },
      {
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },
      {
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },
      {
        date: '2024-01-01',
        name: '众客恒泰',
        Supplier: '供应商1',
        SerialNumber: 'QX123456',
        IncomingMaterial: '穿山甲',
        Project: '磺胺',
        Result: '合格',
        TestingPersonnel: '邱'
      },

      ]
    }

  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
    handleExceed(files, fileList) {
      this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
    },
    beforeRemove(file, fileList) {
      return this.$confirm(`确定移除 ${file.name}？`);
    },

    // 删除按钮
    handleDelete() {
      const selectedRows = this.$refs.table.selection;

      if (selectedRows.length === 0) {
        this.$message.warning('请选择要删除的数据');
        return;
      }

      this.$confirm('确定删除选中的数据吗？', '提示', {
        type: 'warning',
      })
        .then(() => {
          // 清除选中的行
          this.$refs.table.clearSelection();

          // 从tableData数组中移除选中的行
          this.tableData = this.tableData.filter(
            (row) => !selectedRows.includes(row)
          );

          this.$message.success('删除成功');
        })
        .catch(() => {
          // 如果用户取消删除操作，则不执行任何操作
        });
    },
    // 全选
    selectAllRows() {
      this.$refs.table.toggleAllSelection();
    }
  }
}
</script>

<style lang='scss' scoped>
.clearfix {
  display: flex;

  .button-text {
    // float: right;
    padding: 10px 20px;
    margin: 0 10px;
  }

  .button-textx {
    margin-left: 10px;
  }
}

::v-deep .has-gutter {
  color: #606266;
  font-size: 20px;
  // margin: 5px 0;
}
</style>
