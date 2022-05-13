<template>
	<view>
		<view class="page-add">
			<input style="width: 70%;" focus placeholder="请输入要添加的事项" v-model="addText" />
			<button style="margin-left: 20px;" type="primary" size="mini" @click="addItem">添加</button>
		</view>
		<hr />
		
		<view class="page-list">
			<checkbox-group @change="checkboxChange">
				<label class="page-item" v-for="item in todoList" :key="item.id">
					<checkbox :value="item.value" :checked="item.checked" @click="checkItem(item)" />
					<view :class="{ underline: item.checked }">{{ item.value }}</view>
					<button style="margin-right: 20px;" type="warn" size="mini" @click="removeItem(item.id)">删</button>
				</label>
			</checkbox-group>
		</view>
		<hr />
		
		<view class="page-switch" @click="moveItem">
			<p>完成的移动到底部</p>
			<switch style="transform: scale(0.7);" />
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			todoList: [
				{
					id: 1,
					value: '上床',
					checked: false
				},
				{
					id: 2,
					value: '闭眼',
					checked: true
				},
				{
					id: 3,
					value: '睡觉',
					checked: false
				}
			],
			addText: '',
			checkedList: {},
			uncheckedList: {},
			moveFlag: false
		};
	},
	created() {
		this.$todoList = this.todoList;
	},
	methods: {
		checkboxChange: function(e) {
			var items = this.todoList,
				values = e.detail.value;
			for (var i = 0, lenI = items.length; i < lenI; ++i) {
				const item = items[i];
				if (values.includes(item.value)) {
					this.$set(item, 'checked', true);
				} else {
					this.$set(item, 'checked', false);
				}
			}
		},
		// 添加新的事项
		addItem() {
			var newTodo = {
				id: this.todoList.length + 1,
				value: this.addText
			};
			this.todoList.unshift(newTodo);
			this.addText = '';
			this.$todoList = this.todoList;
		},
		// 删除
		removeItem(idx) {
			this.todoList = this.todoList.filter(item => item.id != idx);
			this.$todoList = this.todoList;
		},
		// 完成的移动到最下面
		moveItem() {
			this.moveFlag = !this.moveFlag;
			if (this.moveFlag) {
				this.checkedList = this.todoList.filter(item => item.checked);
				this.uncheckedList = this.todoList.filter(item => !item.checked);
				this.todoList = [...this.uncheckedList, ...this.checkedList];
			} else {
				this.todoList = this.$todoList;
			}
		}
	}
};
</script>

<style>
.page-add {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin: 20px;
}

.page-list {
	margin-bottom: 15px;
	margin-top: 20px;
}

.page-item {
	margin-left: 20px;
	margin-bottom: 20px;
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.page-switch {
	display: flex;
	align-items: center;
	padding: 20px;
	float: right;
}

.underline {
	text-decoration: line-through;
}

hr {
}
</style>
