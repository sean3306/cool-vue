<template>
  <cl-dialog v-model="visible" :title="title" width="1200px">
    <cl-crud ref="Crud" padding="0">
      <cl-row>
        <!-- 刷新按钮 -->
        <cl-refresh-btn />
      </cl-row>

      <cl-row>
        <!-- 数据表格 -->
        <cl-table ref="Table" />
      </cl-row>

      <cl-row>
        <cl-flex1 />
        <!-- 分页控件 -->
        <cl-pagination />
      </cl-row>
    </cl-crud>
  </cl-dialog>
</template>

<script lang="ts" setup>
import { useCrud, useTable } from "@cool-vue/crud";
import { nextTick, ref } from "vue";
import { useCool } from "/@/cool";

const { service } = useCool();

// 是否可见
const visible = ref(false);

// 标题
const title = ref("");

// cl-table
const Table = useTable({
  autoHeight: false,
  columns: [
    {
      label: "#",
      type: "index"
    },
    {
      label: "安装来源",
      prop: "installReferrer",
      minWidth: 150,
      component: {
        name: "cl-code-json",
        props: {
          popover: true
        }
      }
    },
    {
      label: "登陆IP",
      prop: "ip",
      minWidth: 100
    },
    {
      label: "版本号",
      prop: "version",
      minWidth: 150,
      showOverflowTooltip: true
    },
    {
      label: "国家",
      prop: "country",
      minWidth: 140
    },
    {
      label: "国家IsoCode",
      prop: "country_code",
      minWidth: 140
    },
    {
      label: "运营商",
      prop: "isp",
      minWidth: 140
    },
    {
      label: "创建时间",
      prop: "createTime",
      minWidth: 160,
      sortable: "desc"
    }
  ]
});
console.log(service.base.v2)
// cl-crud
const Crud = useCrud({service: service.base.v2.install_info_Detail});

// 打开
function open(data: Eps.CloudFuncInfoEntity) {
  visible.value = true;
  title.value = `日志列表`;

  nextTick(() => {
    Crud.value?.refresh({
      page: 1,
      installId: data.id
    });
  });
}

// 关闭
function close() {
  visible.value = false;
}

defineExpose({
  open,
  close
});
</script>
