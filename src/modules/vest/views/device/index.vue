<template>
    <cl-crud ref="Crud">
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
    </cl-crud>
</template>

<script lang="tsx" name="k-product" setup>
import { useCrud, useUpsert, useTable, setFocus } from "@cool-vue/crud";
import { useCool } from "/@/cool";

// 基础
const { service } = useCool();

// crud
const Crud = useCrud(
    {service: service.base.v2.device}, (app) => {
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
            label: "玩家ID",
            prop: "playerId",
            minWidth: 150
        },
		{
			label: "渠道ID",
			prop: "channelIdName",
			minWidth: 140
		},
        {
            label: "渠道名称",
            prop: "channelName",
            minWidth: 140
        },
		{
			label: "版本号",
			prop: "version",
			minWidth: 140
		},
		{
			label: "系统语言",
			prop: "language",
			minWidth: 140
		},
		{
			label: "手机型号",
			prop: "deviceModel",
			minWidth: 140
		},
        {
            label: "创建时间",
            prop: "createTime",
            minWidth: 120
        },
        {
            label: "更新时间",
            prop: "updateTime",
            minWidth: 120
        }
    ]
});
</script>
