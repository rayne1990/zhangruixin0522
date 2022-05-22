<template>
  <div class="dashboard-container">
    <TestLayout>
      <template v-slot:header>
        <el-radio-group v-model="perms">
          <el-radio-button label="admin"></el-radio-button>
          <el-radio-button label="editor"></el-radio-button>
        </el-radio-group>
        <el-form
          :model="form"
          ref="form"
          label-width="80px"
          :inline="false"
          size="normal"
          style="margin-top: 20px"
          v-if="perms == 'admin'"
        >
          <el-form-item label="↑点击切换">
            <el-input
              v-model="form.name"
              style="width: 200px"
              placeholder="↑点击切换↑"
            ></el-input>
          </el-form-item>
          <el-form-item label="活动名称">
            <el-select v-model="form.region" placeholder="请选择活动区域">
              <el-option label="区域一" value="shanghai"></el-option>
              <el-option label="区域二" value="beijing"></el-option>
            </el-select>
          </el-form-item>
        </el-form>
        <AlTable
          :column="columns1"
          :data="testList.items"
          :pagination="true"
        ></AlTable>
      </template>

      <template v-slot:main>
        <RenderlessForm />
      </template>

      <template v-slot:footer>
        <el-table :data="testList.items" border stripe>
          <el-table-column
            v-for="(col, index) in columns2"
            :prop="col.prop"
            :key="index"
            :label="col.label"
            :width="col.width"
            :type="col.type"
          >
            <template v-if="index == 4">
              <el-button type="warning" size="mini" icon="el-icon-edit"
                >编辑</el-button
              >
              <el-button type="danger" size="mini" icon="el-icon-delete"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>
      </template>
    </TestLayout>
  </div>
</template>

<script>
import { getTestList } from "@/api/test";
import TestLayout from "@/components/TestLayout";
import AlTable from "@/components/AlTable";
import RenderlessForm from "./RenderlessForm";
export default {
  name: "Dashboard",
  components: { TestLayout, AlTable, RenderlessForm },
  data() {
    return {
      perms: "admin",
      testList: [],
      form: { name: "", region: "" },
      columns1: [
        { label: "名字", prop: "name" },
        { label: "电话", prop: "phone" },
        { label: "年龄", prop: "age" },
        {
          label: "操作",
          render(_, scope) {
            return (
              <div>
                <a style="color:blue">查看</a>
                <span> |</span> <a style="color:blue">编辑</a>
              </div>
            );
          },
        },
      ],
      columns2: [
        { label: "序号", type: "index" },
        { label: "名字", prop: "name" },
        { label: "电话", prop: "phone" },
        { label: "年龄", prop: "age" },
        { label: "操作" },
      ],
    };
  },
  methods: {
    async getList() {
      let res = await getTestList();
      if (res.code === 20000) {
        this.testList = res.data;
      } else {
        Promise.reject(res.message);
      }
    },
  },
  mounted() {
    this.getList();
  },
};
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
