<template>
  <div>
    <el-row>
      <el-col :span="24">
        <div class="grid-content">
          <el-button
            type="primary"
            icon="el-icon-close"
            size="mini"
            @click="toggleSelection([tableData[1], tableData[2]])"
          >选择第一、三行</el-button>
          <el-button type="primary" icon="el-icon-close" size="mini" @click="toggleSelection()">取消选择</el-button>
          <el-button
            type="primary"
            icon="el-icon-finished"
            size="mini"
            @click="getSelection()"
          >获取选择行</el-button>
        </div>
      </el-col>
    </el-row>

    <el-table
      :data="tableData"
      ref="multipleTable"
      style="width: 100%"
      @selection-change="handleSelectionChange"
      @select="selectHandle"
      @select-all="selectAllHandle"
      @row-click="rowClickHandle"
    >
      <el-table-column type="selection" width="55"></el-table-column>
      <el-table-column prop="date" label="日期" width="180">
        <template slot-scope="scope">
          <i class="el-icon-time"></i>
          <span style="margin-left: 10px">{{ scope.row.date }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="180">
        <template slot-scope="scope">
          <el-popover trigger="hover" placement="top">
            <p>姓名: {{ scope.row.name }}</p>
            <p>住址: {{ scope.row.address }}</p>
            <div slot="reference" class="name-wrapper">
              <el-tag size="medium">{{ scope.row.name }}</el-tag>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <el-table-column prop="sex" label="性别" width="100" :formatter="formatSex"></el-table-column>
      <el-table-column prop="address" label="地址">
        <template slot-scope="scope">
          <i class="el-icon-place"></i>
          <span style="margin-left: 10px">{{ scope.row.address }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="200">
        <template slot-scope="scope">
          <el-button
            @click="handleClickEdit(scope.row)"
            type="primary"
            size="mini"
            icon="el-icon-edit"
          ></el-button>
          <el-button
            @click="handleClickDelete(scope.row)"
            type="primary"
            size="mini"
            icon="el-icon-delete"
          ></el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-row>
      <el-col :span="24">
        <div class="grid-content">
          <el-button
            type="primary"
            plain
            icon="el-icon-close"
            size="mini"
            @click="toggleSelection([tableData[1], tableData[2]])"
          >选择第一、三行</el-button>
          <el-button
            type="primary"
            icon="el-icon-close"
            size="mini"
            plain
            @click="toggleSelection()"
          >取消选择</el-button>
          <el-button
            type="primary"
            icon="el-icon-finished"
            size="mini"
            plain
            @click="getSelection()"
          >获取选择行</el-button>
        </div>
      </el-col>
    </el-row>

    <table id="bootstrap-table"></table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          sex: 1,
          address: "上海市普陀区金沙江路 1518 弄"
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          sex: 1,
          address: "上海市普陀区金沙江路 1517 弄"
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          sex: 1,
          address: "上海市普陀区金沙江路 1519 弄"
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          sex: 1,
          address: "上海市普陀区金沙江路 1516 弄"
        }
      ],
      multipleSelection: []
    };
  },
  methods: {
    formatSex(row, column, cellValue, index) {
      return row.sex == 1 ? "男" : row.sex == 0 ? "女" : "未知";
    },
    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
      console.log(val);
    },
    selectHandle(selection, row) {
      console.log("selectHandle");
      console.log(selection);
      console.log(row);
    },
    selectAllHandle(selection) {
      console.log("selectAllHandle");
      console.log(selection);
    },
    rowClickHandle(row, column, event) {
      console.log("rowClickHandle");
      console.log(row);
      console.log(column);
      console.log(event);
    },
    getSelection() {
      let selection = this.$refs.multipleTable.selection;
      console.log(selection);
    }
  },
  mounted() {
    $("#bootstrap-table")
      .bootstrapTable("destroy")
      .bootstrapTable({
        url: "",
        method: "GET",
        uniqueId: "id", // 绑定ID，不显示
        striped: false, //是否显示行间隔色
        cache: false, //是否使用缓存，默认为true，所以一般情况下需要设置一下这个属性（*）
        sortable: true, //是否启用排序
        sortOrder: "asc", //排序方式
        sidePagination: "client", //分页方式：client客户端分页，server服务端分页（*）
        undefinedText: "--",
        //singleSelect: true,                  // 单选checkbox，默认为复选
        showRefresh: true, // 显示刷新按钮
        showColumns: true, // 选择显示的列
        toolbar: "#item_info_toolbar", // 搜索框位置
        search: true, // 搜索开启,
        strictSearch: true,
        clickToSelect: true, // 点击选中行
        pagination: true, //是否显示分页
        pageNumber: 1, //初始化加载第一页，默认第一页,并记录
        pageSize: 5, //每页显示的数量
        pageList: [5, 10, 20, 50, 100], //设置每页显示的数量
        paginationPreText: "上一页",
        paginationNextText: "下一页",
        paginationLoop: false,
        //showToggle: true,                   //是否显示详细视图和列表视图的切换按钮
        //cardView: false,                    //是否显示详细视图
        //detailView: false,                  //是否显示父子表
        //showPaginationSwitch: true,
        //得到查询的参数
        queryParams: function(params) {
          //这里的键的名字和控制器的变量名必须一直，这边改动，控制器也需要改成一样的
          var temp = {
            rows: params.limit, //页面大小
            page: params.offset / params.limit + 1, //页码
            sort: params.sort, //排序列名
            sortOrder: params.order //排位命令（desc，asc）
          };
          return temp;
        },
        columns: [
          {
            checkbox: true
          },
          {
            field: "username",
            title: "用户名",
            valign: "middle",
            width: "16%",
            sortable: true
          },
          {
            field: "fullname",
            title: "姓名",
            width: "16%"
          },
          {
            field: "status",
            title: "密码认证",
            width: "16%"
          },
          {
            field: "availableSpace",
            title: "智能卡认证",
            valign: "middle",
            width: "16%"
          },
          {
            field: "totalSpace",
            title: "个人空间配额",
            width: "16%"
          },
          {
            field: "storageServer",
            title: "私密空间配额"
          }
        ],
        onLoadSuccess: function() {
          alert("表格加载成功！");
        },
        onLoadError: function() {
          showTips("数据加载失败！");
        },
        onDblClickRow: function(row, $element) {
          var id = row.ID;
          //EditViewById(id, 'view');
        },
        rowStyle: function(row, index) {
          //设置行的特殊样式
          //这里有5个取值颜色['active', 'success', 'info', 'warning', 'danger'];
          var strclass = "";
          if (index == 1) {
            strclass = "warning";
            console.log(row);
          }
          return { classes: strclass };
        }
      });
  }
};
</script>

<style scoped>
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
</style>
