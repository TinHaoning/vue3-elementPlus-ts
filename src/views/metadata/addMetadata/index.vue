<template>
	<div class="form-adapt-container layout-pd">
		<el-card shadow="hover" header="新建元数据">
			<el-form :model="state.form" size="default" label-width="100px" class="mt35 mb35">
				<el-row :gutter="35">
					<el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12" class="mb20">
						<el-form-item label="名称：">
							<el-input v-model="state.form.name" placeholder="请输入姓名" clearable></el-input>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="16" :lg="12" :xl="12" class="mb20">
						<el-form-item label="描述：">
							<el-input v-model="state.form.describe" placeholder="请输入描述" clearable></el-input>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="16" :lg="12" :xl="12" class="mb20">
						<el-form-item label="所属系统：" class="treeSelect">
							<el-tree-select v-model="state.form.affiliation" :data="data" filterable />
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="16" :lg="12" :xl="12" class="mb20">
						<el-form-item label="数据类型：">
							<el-select v-model="state.form.numType" placeholder="请选择职务" clearable class="w100">
								<el-option label="新增" value="1"></el-option>
								<el-option label="更新" value="2"></el-option>
								<el-option label="删除" value="3"></el-option>
								<el-option label="全量" value="4"></el-option>
							</el-select>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="16" :lg="12" :xl="12" class="mb20">
						<el-form-item label="阶段：">
							<el-select v-model="state.form.level" placeholder="请选择阶段" clearable class="w100">
								<el-option v-for="(item, key) in 20" :key="key" :label="'选项' + (key + 1)" :value="key"></el-option>
							</el-select>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="16" :lg="12" :xl="12" class="mb20">
						<el-form-item label="采集规则：">
							<el-select v-model="state.form.rule" placeholder="请选择规则" clearable class="w100">
								<el-option v-for="(item, key) in 20" :key="key" :label="'规则' + (key + 1)" :value="key"></el-option>
							</el-select>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="16" :lg="12" :xl="12" class="mb20">
						<el-form-item label="目标数据源：">
							<el-select v-model="state.form.source" placeholder="请选择数据源" clearable class="w100">
								<el-option v-for="(item, key) in 20" :key="key" :label="'数据源' + (key + 1)" :value="key"></el-option>
							</el-select>
						</el-form-item>
					</el-col>
					<el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
						<el-form-item>
							<div class="buttonBox mt30">
								<el-button type="primary"> 保存 </el-button>
								<el-button @click="cancel"> 取消 </el-button>
							</div>
						</el-form-item>
					</el-col>
				</el-row>
			</el-form>
		</el-card>
	</div>
</template>

<script setup lang="ts" name="pagesListAdapt">
import { ref, reactive } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import mittBus from '/@/utils/mitt';

// 定义变量内容
const router = useRouter();
const route = useRoute();
const state = reactive({
	form: {
		name: '',
		describe: '',
		affiliation: '',
		numType: '',
		level: '',
		rule: '',
		source: '',
	},
});
const sourceData = [
	{
		value: '1',
		label: 'Level one 1',
		children: [
			{
				value: '1-1',
				label: 'Level two 1-1',
				children: [
					{
						value: '1-1-1',
						label: 'Level three 1-1-1',
					},
				],
			},
		],
	},
	{
		value: '2',
		label: 'Level one 2',
		children: [
			{
				value: '2-1',
				label: 'Level two 2-1',
				children: [
					{
						value: '2-1-1',
						label: 'Level three 2-1-1',
					},
				],
			},
			{
				value: '2-2',
				label: 'Level two 2-2',
				children: [
					{
						value: '2-2-1',
						label: 'Level three 2-2-1',
					},
				],
			},
		],
	},
	{
		value: '3',
		label: 'Level one 3',
		children: [
			{
				value: '3-1',
				label: 'Level two 3-1',
				children: [
					{
						value: '3-1-1',
						label: 'Level three 3-1-1',
					},
				],
			},
			{
				value: '3-2',
				label: 'Level two 3-2',
				children: [
					{
						value: '3-2-1',
						label: 'Level three 3-2-1',
					},
				],
			},
		],
	},
];
const data = ref(sourceData);

// 取消返回管理页面
const cancel = () => {
	mittBus.emit('onCurrentContextmenuClick', Object.assign({}, { contextMenuClickId: 1, ...route }));
	router.back();
};
</script>
<style scoped lang="scss">
.treeSelect {
	::v-deep .el-select {
		width: 100%;
	}
}
.buttonBox {
	width: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>
