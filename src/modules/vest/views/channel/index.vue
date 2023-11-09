<template>
    <cl-crud ref="Crud">
      <cl-row style="margin:10px 0px;">
        <cl-search ref="Search" />
      </cl-row>
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

<script lang="tsx" name="k-channel" setup>
import {useCrud, useUpsert, useTable, setFocus, useSearch} from "@cool-vue/crud";
import { useCool } from "/@/cool";
import ClSearch from "../../../../../packages/crud/src/components/search";

// 基础
const { service,refs, setRefs } = useCool();

// crud
const Crud = useCrud(
    {service: service.base.v2.channel}, (app) => {
        // Crud 加载完，默认刷新一次
        app.refresh();
    }
);

// 刷新列表，统一调用这个方法去刷新
function refresh(params?: any) {
    Crud.value?.refresh(params);
}

const Search = useSearch({
  resetBtn:true,

  items:[
    {
      label: "所属产品",
      prop: "productId",
      component: {
        name: "el-select",
        options: [],
        props: {
          multiple: false,
          clearable: true,
          onChange(status) {
            Crud.value?.refresh({ productId:status, page: 1 });
          }
        }
      }
    },
    {
      label: "渠道ID",
      prop: "channelId",
      span:12,
      component: {
        name: "el-input",
        props: {
          clearable: true
        }
      }
    },
    {
      label: "渠道名称",
      prop: "channelName",
      component: {
        name: "el-input",
        props: {
          clearable: true
        }
      }
    }
  ],
  async onLoad(){
    // 设置权限列表
    service.base.v2.product.list().then((res) => {
      Search.value?.setOptions(
          "productId",
          res.map((e) => {
            return {
              label: e.productName || "",
              value: e.id
            };
          })
      );
    })
  }
});

// 表格
const Table = useTable({
    columns: [
        {
            type: "selection",
            width: 60
        },
		{
			label: "所属产品",
			prop: "productName",
			minWidth: 150
		},
        {
            label: "渠道ID",
            prop: "channelId",
            minWidth: 150
        },
        {
            label: "渠道名称",
            prop: "channelName",
            minWidth: 140
        },
        {
            label: "绑定域名",
            prop: "bindDomain",
            minWidth: 140
        },
		{
			label: "加密密钥",
			prop: "aesKey",
			minWidth: 140
		},
        {
            label: "IP验证",
            prop: "ipVerify",
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
            label: "安装来源验证",
            prop: "installReferrerVerify",
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
			label: "获取方式",
			prop: "installGetType",
			minWidth: 120,
			dict: [
				{
					label: "Body",
					value: 0,
					type: "success"
				},
				{
					label: "Query",
					value: 1,
					type: "success"
				},
				{
					label: "Header",
					value: 2,
					type: "success"
				},
				{
					label: "Header Auth",
					value: 3,
					type: "success"
				}
			]
		},
		{
			label: "允许安装来源",
			prop: "installReferrerArr",
			minWidth: 120,
			dict:[],
			formatter(row) {
				if(row.installReferrerArr.indexOf(",")> -1){
					let temp = row.installReferrerArr?.split(",");
					temp.pop();
					return temp;
				}else{
					return row.installReferrerArr;
				}

			}
		},
        {
            label: "混淆字段",
            prop: "confuseJson",
            minWidth: 120,
			component: {
				name: "cl-code-json",
				props: {
					popover: true
				}
			}
        },
		{
			label: "B包配置json",
			prop: "planBJson",
			minWidth: 120,
			component: {
				name: "cl-code-json",
				props: {
					popover: true
				}
			}
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
        width: "800px"
    },
    items: [
		{
			type:"tabs",
			props: {
				labels: [
					{
						label: "基础信息",
						value: "base"
					},
					{
						label: "混淆信息",
						value: "confuse"
					},
					{
						label: "配置信息",
						value: "planB"
					}
				]
			}
		},
		{
			prop: "channelId",
			label: "渠道ID",
			span: 24,
			required: true,
			group: "base",
			component: {
				name: "el-input",
				props: {
					placeholder: "请输入渠道ID"
				}
			}
		},
		{
			prop: "channelName",
			label: "渠道名称",
			span: 24,
			required: true,
			group: "base",
			component: {
				name: "el-input",
				props: {
					placeholder: "请输入渠道名称"
				}
			}
		},
		{
			prop: "bindDomain",
			label: "绑定域名",
			span: 24,
			required: true,
			group: "base",
			component: {
				name: "el-input",
				props: {
					placeholder: "请输入域名"
				}
			}
		},
		{
			label: "加密密钥",
			prop: "aesKey",
			span: 24,
			required: true,
			group: "base",
			component: {
				name: "el-input",
				props: {
					placeholder: "请输入密钥"
				}
			}
		},
		{
			prop: "productId",
			label: "所属产品",
			span: 24,
			required: true,
			group: "base",
			component: {
				name: "el-select",
				options: [],
				props: {
					multiple: false
				}
			}
		},
		{
			prop: "ipVerify",
			label: "IP验证",
			span: 24,
			required: true,
			group: "base",
			component: {
				name: "cl-switch",
				props: {
					activeValue: true,
					inactiveValue: false,
				}
			}
		},
		{
			prop: "ipRoleIdList",
			label: "地区配置",
			span: 24,
			required: true,
			group: "base",
			value:[],
			component: {
				name: "el-select",
				options: [],
				props: {
					multiple: true
				}
			}
		},
		{
			prop: "installGetType",
			label: "获取信息方式",
			value: 1,
			component: {
				name: "el-radio-group",
				options: [
					{
						label: "Body",
						value: 0
					},
					{
						label: "Query",
						value: 1
					},
					{
						label: "Header",
						value: 2
					},
					{
						label: "Header Auth",
						value: 3
					}
				]
			}
		},
		{
			prop: "installReferrerVerify",
			label: "来源认证",
			span: 4,
			required: true,
			group: "base",
			component: {
				name: "cl-switch",
				props: {
					activeValue: true,
					inactiveValue: false,
				}
			}
		},
		{
			prop: "installReferrerArr",
			label: "安装来源",
			span: 24,
			required: true,
            value:"facebook,",
			group: "base",
			component: {
				name: "el-input",
				props: {
					placeholder: "请输入来源信息"
				}
			}
		},
		{
			prop: "confuseJson",
			label: "混淆字段",
			span: 24,
			required: true,
			group: "confuse",
            value:"{}",
			component: {
				name: "cl-editor-monaco",
				props: {
					language: "json",
					options: {
						fontSize: "16px"
					}
				},
				ref: setRefs("confuseA")
			}
		},
		{
			prop: "planBJson",
			label: "B包配置json",
			span: 24,
			required: true,
            value:"{}",
			group: "planB",
			component: {
				name: "cl-editor-monaco",
				props: {
					language: "json",
					options: {
						fontSize: "16px"
					}
				},
				ref: setRefs("planB")
			}
		}
    ],
	async onOpen() {
		// 设置权限列表
		service.base.v2.product.list().then((res) => {
			Upsert.value?.setOptions(
				"productId",
				res.map((e) => {
					return {
						label: e.productName || "",
						value: e.id
					};
				})
			);
		});
		//查询地区
		service.base.v2.regional.list().then((res) => {
			Upsert.value?.setOptions(
				"ipRoleIdList",
				res.map((e) => {
					return {
						label: e.countryName || "",
						value: e.id
					};
				})
			);
		});
	},
    onOpened(data) {
        data[`data_${data.dataType}`] = data.data;
    },

    async onSubmit(data, { next }) {
		const planBJson = await refs.planB.formatCode();
		const confuseJson = await refs.confuseA.formatCode();
        next({
            ...data,
			planBJson,
			confuseJson
        });
    },
    plugins: [setFocus()]
});
</script>
