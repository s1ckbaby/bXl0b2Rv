<template>
	<view>
		<view class="page-top">
			<p style="font-size: 40px;">待办事项</p>
			<p style="font-size: 12px; padding-top: 3px; padding-bottom: 10px;">一次只做一件事</p>
		</view>

		<view class="page-list">
			<checkbox-group v-if="moveFlag == false" @change="checkboxChange">
				<label class="page-item" v-for="item in todoList" :key="item.id">
					<view :class="{ underline: item.checked }">{{ item.value }}</view>
					<view class="page-item-tool">
						<checkbox color="#ff6666" :value="item.value" :checked="item.checked" />
						<button class="xBtn" @click="removeItem(item.id)"><b>×</b></button>
					</view>
				</label>
			</checkbox-group>
			<checkbox-group v-else @change="checkboxChange">
				<label class="page-item" v-for="item in todoBox" :key="item.id">
					<view :class="{ underline: item.checked }">{{ item.value }}</view>
					<view class="page-item-tool">
						<checkbox color="#ff6666" :value="item.value" :checked="item.checked" />
						<button class="xBtn" @click="removeItem(item.id)"><b>×</b></button>
					</view>
				</label>
			</checkbox-group>
		</view>

		<view class="page-switch" @click="moveBtn">
			<p style="font-size: 12px;">完成的移动到底部</p>
			<switch color="#ffb4b4" style="transform: scale(0.7);" />
		</view>

		<view>
			<p style="margin-bottom: 7px;">添加待办事项</p>
			<view class="page-add">
				<input v-model="addText" />
				<button size="mini" @click="addItem">添加</button>
			</view>
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
			todoBox: [],
			addText: '',
			checkedList: [],
			uncheckedList: [],
			moveFlag: false
		};
	},
	created() {
		this.todoBox = this.todoList;
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
			this.moveItem();
		},
		// 添加新的事项
		addItem() {
			var newTodo = {
				id: this.todoList.length + 1,
				value: this.addText,
				checked: false
			};
			this.todoList.push(newTodo);
			this.moveItem();
			this.addText = '';
		},
		// 删除
		removeItem(idx) {
			this.todoList = this.todoList.filter(item => item.id != idx);
			this.todoList.forEach(function(item, i) {
				++i;
				item.id = i;
			});
			this.moveItem();
		},
		// 完成的移动到最下面
		moveBtn() {
			this.moveFlag = !this.moveFlag;
			this.moveItem();
		},
		moveItem() {
			if (this.moveFlag == true) {
				this.checkedList = this.todoList.filter(item => item.checked);
				this.uncheckedList = this.todoList.filter(item => !item.checked);
				this.todoBox = [...this.uncheckedList, ...this.checkedList];
			}
		}
	}
};
</script>

<style>
page {
	padding: 50px 70px;
	background: #ff6666;
	color: #fff;
}

.page-top {
	border-bottom: 1px solid rgba(255, 255, 255, 0.5);
}

.page-add {
	display: flex;
}

.page-list {
	margin-bottom: 15px;
	margin-top: 40px;
}

.page-item {
	background-color: rgba(255, 255, 255, 0.1);
	margin: 0 -100px 4px;
	padding: 20px 100px;
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.page-item-tool {
	display: flex;
	align-items: center;
}

.page-switch {
	margin-bottom: 10px;
	width: 100%;
	display: flex;
	align-items: center;
	justify-content: flex-end;
}

.underline {
	text-decoration: line-through;
	color: #aa5a5a;
}

.xBtn {
	width: 35px;
	background-color: rgba(255, 255, 255, 0);
	border: transparent;
	text-align: left;
	padding: 0px;
	margin: 0px;
}

.xBtn ::after {
	border-radius: 0;
	border: 1px solid rgba(0,0,0,0);
	outline: none;
}

input {
	border: none;
	background: #fff;
	width: 60%;
	color: #333333;
	height: 20px;
	padding: 10px;
}

button {
	line-height: 35px;
	color: white;
	background: #ff6666;
	border: 1px solid #FFFFFF;
	height: 40px;
	display: flex;
	align-items: center;
	justify-content: center;
}

button::after {
	 border: none;
}

b {
	font-size: 30px;
}
</style>
