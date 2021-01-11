<template>
	<div>
		<el-dialog title="添加处理人" :visible.sync="dialogVisible" width="700px" :close-on-click-modal="false" @click.native="closeDrop"
		 :close-on-press-escape="false" :show-close="false" @closed="$emit('close')" custom-class="attendance-dialog">
			<div class="w100" style="mini-height:300px;">
				<el-form label-width="80px" :model="formData">
					<el-form-item :label="'处理人'+(index+1)+':'" v-for="(item,index) in formData.selectedList" :key="index">
						<div class="ub ub-pj w100" style="padding-right:20px;box-sizing: border-box;">
							<div class="ub ub-f1">
								<div>
									<el-select size="small" v-model="item.dealUserType" placeholder="请选择">
										<el-option label="工单发起人" value="0"></el-option>
										<el-option label="数据相关人员" value="1"></el-option>
										<el-option label="人员,部门,角色,用户组" value="2"></el-option>
									</el-select>
								</div>
								<div v-if="item.dealUserType == 1" style="margin-left: 10px;width:300px;">
									<el-select style="width:100%;" size="small" v-model="item.type" placeholder="请选择">
										<el-option label="资产负责人" value="0"></el-option>
										<el-option label="告警负责人" value="1"></el-option>
									</el-select>
								</div>
								<div v-if="item.dealUserType == 2" style="width:300px;margin-left: 10px;">
									<personnel :index="index" ref="personRange" :usersSelected="item._users?item._users:[]" :usersList="allUserList"
									 :depList="allDepList" :rolesList="allRolesList" :dataList="dataList" :processList="processList" @confirmData="confirmData"></personnel>
								</div>
							</div>
							<div v-if="index!=0" class="close-btn" @click="del(index)">
								<i class="el-icon-circle-close"></i>
							</div>
						</div>
					</el-form-item>
				</el-form>
				<div style="padding-left:20px;margin-top: 10px;" v-if="formData.selectedList.length<3">
					<el-button style="color:#1cd7fa" type="text" size="mini" icon="el-icon-plus" @click="add">添加处理人</el-button>
				</div>
			</div>

			<span slot="footer" class="dialog-footer">
				<searchBtn title="确 定" @click="changeSet" />
				<div class="cancelBtn" style="margin-left: 10px;" @click="closeDialog">取 消</div>
			</span>
		</el-dialog>
	</div>
</template>

<script>
	import mixinPanel from '../../../common/mixinPanel'
	import dropdownBox from "../../../../pages/workFlow/workComponents/dropdownBox";
	// import {
	// 	getAllUsers,
	// 	getAllDep,
	// 	getAllRoles
	// } from '../../../../server/define.js'
	export default {
		mixins: [mixinPanel],
		components: {
			dropdownBox
		},
		props: {
			users: {
				type: Array,
				default: () => []
			}
		},
		data() {
			return {
				allUserList: [], // 所有人
				allDepList: [], // 所有部门
				allRolesList: [], // 所有角色
				dataList: [{
						name: '资产负责人',
						type: '3',
						id: 0
					},
					{
						name: '告警创建人',
						type: '3',
						id: 1
					}
				],
				processList: [{
					name: '流程相关',
					type: '4',
					id: 0
				}, ],
				dialogVisible: true,
				showParamDialog: false,
				nowIndex: null,
				formData: {
					selectedList: []
				},
				allList: [{
					id: 1,
					title: '测试1',
					isSelected: false,
				}, {
					id: 2,
					title: '测试2',
					isSelected: false,
				}, {
					id: 3,
					title: '测试3',
					isSelected: false,
				}, {
					id: 4,
					title: '测试4',
					isSelected: false,
				}, ],


			}
		},
		mounted() {
			this.formData.selectedList = this.$deepCopy(this.users.length == 0 ? [{
				dealUserType: '',
			}] : this.users);
			this.$nextTick(() => {
				this.init();
			})
		},
		methods: {
			closeDrop() {
				if (Array.isArray(this.$refs.personRange)) {
					this.$refs.personRange.forEach(item => {
						item.showMore = false;
					})
				}
			},
			del(index) {
				this.formData.selectedList.splice(index, 1);
			},
			add() {
				this.formData.selectedList.push({
					dealUserType: '',
				})
			},
			init() {
				this.get_dep();
				this.get_roles();
				this.get_users();
			},
			get_users() {
				setTimeout(() => {
					let res = [{
						"chineseName": "管理员",
						"id": "1"
					}, {
						"chineseName": "孙广",
						"id": "ff8080816b3f260e016b3f5389330045"
					}, {
						"chineseName": "张先生",
						"id": "402881f16bd05f8f016bd0601b680000"
					}, {
						"chineseName": "高雁",
						"id": "8a8be6ac711b790a01711c561968007f"
					}, {
						"chineseName": "徐东伟",
						"id": "8a8be6ac7139f45a01713a1852a10000"
					}, {
						"chineseName": "111",
						"id": "8a8be6ac73e5b22e01742df490410000"
					}, {
						"chineseName": "测试",
						"id": "8a8be6ac743369d40174348023810007"
					}, {
						"chineseName": "张飞",
						"id": "8a8be6ac743369d40174426238ef0008"
					}, {
						"chineseName": "诸葛亮",
						"id": "8a8be6ac746cf8be0174761c075300c9"
					}, {
						"chineseName": "小王",
						"id": "8a8be6ac7477bdea01747ad6fc170000"
					}, {
						"chineseName": "崔云",
						"id": "8a8be6ac747c7a2301749a1578f3019c"
					}, {
						"chineseName": "lijing",
						"id": "8a8be6ac749ff5ed0174b8b89cf30003"
					}, {
						"chineseName": "分析",
						"id": "8a8be6ac7698ed2d0176a750df9400ae"
					}, {
						"chineseName": "处置",
						"id": "8a8be6ac7698ed2d0176a75178a500af"
					}];
					res.forEach(item => {
						item.type = '0'
					})
					this.allUserList = res;
					this.$setsessionStorage('allUserList', res)
				}, 500)
			},
			get_roles() {
				setTimeout(() => {
					let res = [{
						"createTimeFormat": "2020-08-06 18:47:20",
						"createTime": "2020-08-06 18:47:20",
						"roleName": "test1",
						"id": "8a8be6ac73c356ef0173c361e82d0000",
						"status": 0
					}, {
						"createTimeFormat": "2019-12-10 14:34:16",
						"createTime": "2019-12-10 14:34:16",
						"roleName": "admin-L",
						"id": "8a8be68f6ed50ee8016eee83f70c00a7",
						"status": 0
					}, {
						"createTimeFormat": "2019-06-04 16:07:49",
						"createTime": "2019-06-04 16:07:49",
						"roleName": "裁判",
						"id": "4028a7d86b216c9e016b2187b1d70000",
						"status": 0
					}, {
						"createTimeFormat": "2019-05-24 13:12:52",
						"createTime": "2019-05-24 13:12:52",
						"roleName": "管理员",
						"id": "1",
						"status": 0
					}]
					console.log('所有角色', res)
					res.forEach(item => {
						item.type = '2'
					})
					this.allRolesList = res;
					this.$setsessionStorage('allRolesList', res)
				}, 500)
			},
			get_dep() {
				setTimeout(() => {
					let res = [{
						"name": "金融中心",
						"id": "8a8be68f6ed50ee8016eee8614680121"
					}, {
						"name": "网御星云",
						"id": "8a7481b56d474c38016d4759d7190000"
					}, {
						"name": "部门A",
						"id": "8a7481b56d474c38016d4759d71a0001"
					}, {
						"name": "部门B",
						"id": "4028a78c6b3f0002016b40f455b40000"
					}, {
						"name": "部门C",
						"id": "2"
					}, {
						"name": "部门D",
						"id": "1"
					}]
					console.log('所有部门', res)
					res.forEach(item => {
						item.type = '1'
					})
					this.allDepList = res;
					this.$setsessionStorage('allDepList', res)
				}, 500)
			},
			confirmData({
				val,
				index
			}) {
				console.log('val', val)
				console.log('index', index)
				val = val.length ? val : [];
				let type0 = [];
				let type1 = [];
				let type2 = [];
				let type3 = [],
					type4;
				this.formData.selectedList[index].users = [];
				this.formData.selectedList[index]._users = val;
				val.forEach(item => {
					if (item.type === '0') {
						type0.push(item.id)
					} else if (item.type === '1') {
						/*部门*/
						type1.push(item.id)
					} else if (item.type === '2') {
						type2.push(item.id)
					} else if (item.type === '3') {
						type3.push(item.id)
					} else if (item.type === '4') {
						type4 = true;
					}
				})
				type0.length && this.formData.selectedList[index].users.push({
					type: '0',
					ids: type0
				});
				type1.length && this.formData.selectedList[index].users.push({
					type: '1',
					ids: type1
				});
				type2.length && this.formData.selectedList[index].users.push({
					type: '2',
					ids: type2
				});
				type3.length && this.formData.selectedList[index].users.push({
					type: '3',
					ids: type3
				});
				type4 && this.formData.selectedList[index].users.push({
					type: '4'
				});
				console.log(this.formData.selectedList[index].users, '提醒人列表')
			},
			changeSet() {
				console.log('this.formData.selectedList2222222', this.formData.selectedList)
				//return
				let userInfo = this.$getsessionStorage('userInfo');
				this.closeDialog();
				let arr = [];
				this.formData.selectedList.map(item => {
					let obj = {};
					if (item.dealUserType == 0) {
						obj.dealUserType = item.dealUserType;
						obj.dealUserTypeNmae = '工单发起人';
						obj.userStr = userInfo.cnName;
					} else if (item.dealUserType == 1) {
						obj.dealUserTypeNmae = '数据相关人员';
						obj.dealUserType = item.dealUserType;
						if (item.type == '') {
							obj.users = [];
						} else {
							obj.users = [{
								id: item.type,
								name: item.type == 0 ? '资产负责人' : '告警创建人'
							}];
						}
						obj.userStr = obj.users.length > 0 ? obj.users[0].name : '';
					} else if (item.dealUserType == 2) {
						obj.dealUserTypeNmae = '人员,部门,角色,用户组';
						obj.dealUserType = item.dealUserType;
						obj._users = item._users.length ? item._users : [];
						obj.users = item.users.length ? item.users : [];
						obj.userStr = '';
						let names = [];

						if (obj._users.length > 0) {
							obj._users.map(tag => {
								if (tag.type === '0') {
									names.push(tag.chineseName)
								} else if (tag.type === '1' || tag.type === '3' || tag.type === '4') {
									names.push(tag.name)
								} else if (tag.type === '2') {
									names.push(tag.roleName)
								}
							})
							obj.userStr = names.join(',')
						}
					}
					arr.push(obj);
				})
				this.$emit('getUsersSet', {
					data1: this.formData.selectedList,
					data2: arr
				});

			},
			closeDialog() {
				//this.updateElement()
				this.dialogVisible = false
			}
		}
	}
</script>

<style lang="scss" scoped>
	.flow-containers .el-badge__content.is-fixed {
		top: 18px;
	}

	.close-btn {
		cursor: pointer;
		font-size: 16px;
		color: #ff6060;
	}
</style>
