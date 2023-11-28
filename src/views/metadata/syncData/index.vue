<template>
	<div class="table-demo-container layout-padding">
		<div class="table-demo-padding layout-border-card mb10">
			<TableSearch :search="state.tableData.search" @search="onSearch" />
		</div>
		<div class="table-demo-padding layout-padding-view layout-padding-auto">
			<Table
				ref="tableRef"
				v-bind="state.tableData"
				class="table-demo"
				@delRow="onTableDelRow"
				@pageChange="onTablePageChange"
				@sortHeader="onSortHeader"
			/>
		</div>
	</div>
</template>

<script setup lang="ts" name="makeTableDemo">
import { defineAsyncComponent, reactive, ref, onMounted } from 'vue';
import { ElMessage } from 'element-plus';

// 引入组件
const Table = defineAsyncComponent(() => import('/@/components/table/syncSourceTable.vue'));
const TableSearch = defineAsyncComponent(() => import('/@/views/metadata/syncData/search.vue'));

// 定义变量内容
const tableRef = ref<RefType>();
const state = reactive<TableDemoState>({
	tableData: {
		// 列表数据（必传）
		data: [],
		// 表头内容（必传，注意格式）
		header: [
			{ key: 'name', colWidth: '', title: '名称', type: 'text', isCheck: true },
			{ key: 'type', colWidth: '', title: '数据源', type: 'text', isCheck: true },
			{ key: 'syncSwitch', colWidth: '', title: '同步状态', type: 'boolean', isCheck: true },
			{ key: 'createTime', colWidth: '', title: '最后执行时间', type: 'text', isCheck: true },
			{ key: 'updateTime', colWidth: '', title: '创建时间', type: 'text', isCheck: true },
			{ key: 'createUser', colWidth: '', title: '创建用户', type: 'text', isCheck: true },
		],
		// 配置项（必传）
		config: {
			total: 0, // 列表总数
			loading: true, // loading 加载
			isBorder: false, // 是否显示表格边框
			isSerialNo: true, // 是否显示表格序号
			isSelection: true, // 是否显示表格多选
			isOperate: true, // 是否显示表格操作栏
			isShowTitle: true, //列表头部标题显示与隐藏
			isSwitch: true, //显示与隐藏启用
		},
		headerMenu: {
			name: '数据源列表',
			btnList: [],
		},

		button: [
			{ type: 'primary', text: '新增数据源', icon: 'Plus' },
			{ type: '', text: '启用', icon: '' },
			{ type: '', text: '禁用', icon: '' },
		],

		// 搜索表单，动态生成（传空数组时，将不显示搜索，注意格式）
		search: [
			{ label: '任务名称', prop: 'name', placeholder: '请输入任务名称', required: false, type: 'input' },
			{
				label: '元数据类型',
				prop: 'isSupport',
				placeholder: '请选择',
				required: false,
				type: 'select',
				options: [
					{ label: '选项1', value: 1 },
					{ label: '选项2', value: 0 },
					{ label: '选项3', value: 0 },
					{ label: '选项4', value: 0 },
					{ label: '选项5', value: 0 },
					{ label: '选项6', value: 0 },
				],
			},
			{ label: '搜索条件', prop: 'name', placeholder: '请输入搜索条件', required: false, type: 'input' },
			{ label: '搜索条件', prop: 'name', placeholder: '请输入搜索条件', required: false, type: 'input' },
		],

		// 搜索参数（不用传，用于分页、搜索时传给后台的值，`getTableData` 中使用）
		param: {
			pageNum: 1,
			pageSize: 10,
		},
		// 打印标题
		printName: 'vueNextAdmin 表格打印演示',
	},
});

// 初始化列表数据
const getTableData = () => {
	state.tableData.config.loading = true;
	state.tableData.data = [];
	for (let i = 0; i < 100; i++) {
		state.tableData.data.push({
			name: `MySQL连接${i + 1}`,
			type: `MySQL${i + 1}`,
			createTime: `${new Date().toLocaleDateString()}`,
			createUser: `admin${i + 1}`,
			updateTime: `${new Date().toLocaleDateString()}`,
			syncSwitch: `${i % 3 === 0}`,
			image: `https://img2.baidu.com/it/u=417454395,2713356475&fm=253&fmt=auto?w=200&h=200`,
		});
	}
	// 数据总数（模拟，真实从接口取）
	state.tableData.config.total = state.tableData.data.length;
	setTimeout(() => {
		state.tableData.config.loading = false;
	}, 500);
};
// 搜索点击时表单回调
const onSearch = (data: EmptyObjectType) => {
	state.tableData.param = Object.assign({}, state.tableData.param, { ...data });
	tableRef.value.pageReset();
};
// 删除当前项回调
const onTableDelRow = (row: EmptyObjectType) => {
	ElMessage.success(`删除${row.name}成功！`);
	getTableData();
};
// 分页改变时回调
const onTablePageChange = (page: TableDemoPageType) => {
	state.tableData.param.pageNum = page.pageNum;
	state.tableData.param.pageSize = page.pageSize;
	getTableData();
};
// 拖动显示列排序回调
const onSortHeader = (data: TableHeaderType[]) => {
	state.tableData.header = data;
};
// 页面加载时
onMounted(() => {
	getTableData();
});
</script>

<style scoped lang="scss">
.table-demo-container {
	.table-demo-padding {
		padding: 15px;

		.table-demo {
			flex: 1;
			overflow: hidden;
		}
	}
}
.layout-border-card {
	background: var(--el-color-white);
	border-radius: 4px;
	border: 1px solid var(--el-border-color-light, #ebeef5);
	overflow: hidden;
}
</style>
