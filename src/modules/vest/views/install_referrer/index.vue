<template>
    <cl-crud ref="Crud">
<!--      <cl-row style="margin:10px 0px;">-->
<!--        <cl-search ref="Search" />-->
<!--      </cl-row>-->
        <cl-row>
            <!-- 刷新按钮 -->
            <cl-refresh-btn />
            <!-- 新增按钮 -->
            <cl-add-btn />
            <!-- 批量删除按钮 -->
            <cl-multi-delete-btn />
        </cl-row>
        <cl-row>
            <!-- 表格 -->
            <cl-table ref="Table"></cl-table>
        </cl-row>
        <cl-row>
            <cl-flex1 />
            <!-- 分页 -->
            <cl-pagination />
        </cl-row>
        <!-- 新增、编辑 -->
        <cl-upsert ref="Upsert" />
        <install-details :ref="setRefs('installlogs')" />
    </cl-crud>
</template>

<script lang="tsx" name="k-install-referrer" setup>
import {useCrud, useUpsert, useTable, setFocus, useSearch} from "@cool-vue/crud";
import { useCool } from "/@/cool";
import InstallDetails from '../../components/install_referrer_detail.vue'

// 基础
const { service,refs, setRefs } = useCool();

// crud
const Crud = useCrud(
    {service: service.base.v2.install_info}, (app) => {
        // Crud 加载完，默认刷新一次
        app.refresh();
    }
);

// 刷新列表，统一调用这个方法去刷新
function refresh(params?: any) {
    Crud.value?.refresh(params);
}

// 表格
const Table = useTable({
    columns: [
        {
            type: "selection",
            width: 60
        },
        {
          label: "安装来源",
          prop: "lastInstallReferrer",
          minWidth: 300,
          component: {
            name: "cl-code-json",
            props: {
              popover: true
            }
          }
        },
        {
          label: "所属渠道",
          prop: "channelName",
          minWidth: 150
        },
        {
            label: "IMEI",
            prop: "imei",
            minWidth: 150
        },
        {
            label: "最新版本号",
            prop: "lastVersion",
            minWidth: 140
        },
        {
            label: "最近登陆IP",
            prop: "lastIP",
            minWidth: 140
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
            label: "通过状态",
            prop: "state",
            minWidth: 120,
            component: {
              name: "cl-switch",
              props: {
                activeValue: true,
                inactiveValue: false,
              }
            }
        },
        {
            label: "操作",
            type: "op",
            buttons: [
              {
                label: "查看详情",
                onClick({ scope }) {
                  refs.installlogs.open(scope.row);
                  // refs.logs.open(scope.row);
                }
              }
            ]
        }
    ]
});
</script>
