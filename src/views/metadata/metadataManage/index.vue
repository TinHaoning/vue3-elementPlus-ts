<template>
	<div class="table-demo-container layout-padding">
		<div class="table-demo-padding layout-border-card mb10">
			<TableSearch :search="state.tableData.search" @search="onSearch" />
		</div>
		<div class="table-demo-padding layout-padding-view layout-padding-auto">
			<el-container>
				<el-aside style="height: 680px" width="300px" class="left-container">
					<div class="left-header mb10">
						<el-text class="mx-1" size="large">系统分类</el-text>
						<el-button :icon="Plus" text circle @click="onOpenAddClass"></el-button>
					</div>
					<el-card class="box-card">
						<div v-for="(item, key) in 95" :key="key" class="text item">
							<div class="cellStyle mb15">
								<div class="titleName">生产子系统{{ key }}</div>
								<el-popover placement="bottom" :width="180" trigger="click">
									<template #reference>
										<el-button :icon="MoreFilled" circle />
									</template>
									<div class="popMenu">
										<el-button mini @click="onOpenEditClass('edit')">编辑</el-button>
										<el-button mini>删除</el-button>
									</div>
								</el-popover>
							</div>
						</div>
					</el-card>
				</el-aside>
				<el-main style="height: 680px">
					<Table
						style="height: 100%"
						ref="tableRef"
						v-bind="state.tableData"
						class="table-demo"
						@delRow="onTableDelRow"
						@pageChange="onTablePageChange"
						@sortHeader="onSortHeader"
						@addSource="toAddPage"
				/></el-main>
			</el-container>
		</div>
		<ClassDialog ref="classDialogRef" @refresh="getTableData()" />
	</div>
</template>

<script setup lang="ts" name="makeTableDemo">
import { defineAsyncComponent, reactive, ref, onMounted } from 'vue';
import { ElMessage } from 'element-plus';
import { MoreFilled, Plus } from '@element-plus/icons-vue';
import { useRouter } from 'vue-router';

// 引入组件
const Table = defineAsyncComponent(() => import('/@/components/table/rightTable.vue'));
const TableSearch = defineAsyncComponent(() => import('/@/views/dataSource/search.vue'));
const ClassDialog = defineAsyncComponent(() => import('/@/views/metadata/metadataManage/classDialog.vue'));

// 定义变量内容
const router = useRouter();
const tableRef = ref<RefType>();
const classDialogRef = ref();
const state = reactive<TableDemoState>({
	tableData: {
		// 列表数据（必传）
		data: [],

		// 表头内容（必传，注意格式）
		header: [
			{ key: 'type', colWidth: '', title: '类型', type: 'text', isCheck: true },
			{ key: 'name', colWidth: '', title: '名称', type: 'text', isCheck: true },
			{ key: 'levelStatus', colWidth: '', title: '所处阶段', type: 'text', isCheck: true },
			{ key: 'switch', colWidth: '', title: '同步状态', type: 'text', isCheck: true },
			{ key: 'updateTime', colWidth: '', title: '更新时间', type: 'text', isCheck: true },
		],

		// 配置项（必传）
		config: {
			total: 0, // 列表总数
			loading: true, // loading 加载
			isBorder: false, // 是否显示表格边框
			isSerialNo: true, // 是否显示表格序号
			isSelection: false, // 是否显示表格多选
			isOperate: true, // 是否显示表格操作栏
			isShowTitle: true, //列表头部标题显示与隐藏
			isSwitch: true, //显示与隐藏启用
		},

		headerMenu: {
			name: '基础列表',
		},

		button: [
			{ type: 'primary', text: '新增数据源', icon: 'Plus' },
			{ type: '', text: '导入', icon: '' },
			{ type: '', text: '导出', icon: '' },
		],

		// 搜索表单，动态生成（传空数组时，将不显示搜索，注意格式）
		search: [
			{ label: '元数据名称', prop: 'name', placeholder: '请输入数据源名称', required: false, type: 'input' },
			{
				label: '数据类型',
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
			{
				label: '所属系统',
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
			{
				label: '所处阶段',
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
			{
				label: '同步状态',
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
			{
				label: '采集状态',
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
	for (let i = 0; i < 50; i++) {
		state.tableData.data.push({
			type: `${i % 2 === 0 ? '新增' : '更新'}`,
			name: `人员定位${i + 1}`,
			levelStatus: `${i % 2 === 0 ? '采集阶段' : '准备阶段'}`,
			updateTime: `${new Date().toLocaleDateString()}`,
			switch: `${i % 3 === 0}`,
		});
	}
	// 数据总数（模拟，真实从接口取）
	state.tableData.config.total = state.tableData.data.length;
	setTimeout(() => {
		state.tableData.config.loading = false;
	}, 500);
};

// 打开新增菜单弹窗
const onOpenAddClass = (type: string) => {
	classDialogRef.value.openDialog(type);
};
// 打开编辑菜单弹窗
const onOpenEditClass = (type: string) => {
	classDialogRef.value.openDialog(type);
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

//跳转新增数据源页面
const toAddPage = () => {
	router.push({
		path: '/metadata/addMetadata',
	});
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
.left-container {
	padding-top: 15px;
	.left-header {
		height: 40px;
		display: flex;
		justify-content: space-between;
	}
	.box-card {
		height: calc(100% - 60px);
		overflow: auto;
	}
}
.cellStyle {
	display: flex;
	justify-content: space-between;
	align-items: center;
	.titleName {
		font-size: 13px;
		color: #000000;
	}
}
.popMenu {
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>
