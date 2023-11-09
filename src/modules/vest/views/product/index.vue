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
    {service: service.base.v2.product}, (app) => {
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
            label: "产品ID",
            prop: "productId",
            minWidth: 150
        },
        {
            label: "产品名称",
            prop: "productName",
            minWidth: 140
        },
        {
            label: "产品密钥",
            prop: "productPassword",
            minWidth: 140
        },
        {
            label: "下载地址",
            prop: "productPlanBUrl",
            minWidth: 120
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
        },
        {
            label: "操作",
            type: "op"
        }
    ]
});

// cl-upsert
const Upsert = useUpsert({
    dialog: {
        width: "1000px"
    },
    items: [
        {
            prop: "productId",
            label: "产品ID",
            span: 24,
            required: true,
            component: {
                name: "el-input",
                props: {
                    placeholder: "请输入产品ID"
                }
            }
        },
        {
            prop: "productName",
            label: "产品名称",
            span: 24,
            required: true,
            component: {
                name: "el-input",
                props: {
                    placeholder: "请输入产品名称"
                }
            }
        },
        {
            prop: "productPassword",
            label: "产品密钥",
            span: 24,
            required: true,
            component: {
                name: "el-input",
                props: {
                    placeholder: "请输入产品密钥"
                }
            }
        },
        {
            prop: "productPlanBUrl",
            label: "下载地址",
            span: 24,
            required: true,
            component: {
                name: "el-input",
                props: {
                    placeholder: "请输入下载地址"
                }
            }
        }
    ],

    onOpened(data) {
        data[`data_${data.dataType}`] = data.data;
    },

    onSubmit(data, { next }) {
        next({
            ...data,
            data: data[`data_${data.dataType}`],
        });
    },
    plugins: [setFocus()]
});
</script>
