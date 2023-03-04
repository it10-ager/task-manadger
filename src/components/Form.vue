<template>
	<div class="container">
		<h1>{{ title }}</h1>
		<h3>Добавьте в список новые значения</h3>
		<form @submit.prevent="addTask" autocomplete="off">
			<input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Введите задачу"/> <!--при нажатии клавиши Enter задачи тоже записываются, удобно :)-->
			<button @click="showMess">Добавить дело</button>
		</form>
		<p class="alert-mess" :style="messStyle">{{ message }}</p> <!--вывод сообщения-->
		<ul>
			<li v-for="(el, index) in tasksArray" :key="index">	<!--Перебираем элементы массива по ключу index и удаляем выбранный-->
				<div class="list">
					<span> {{ el }} </span>	<!--задача-->
					<button v-if="trash.length < 5" @click="removeTask(index)">Удалить</button>
				</div>
			</li>
		</ul>
	</div>
	<div class="container-2">
		<div class="trash-num">
			<h2>{{ titleTrash }}</h2>
			<span :class="{error: fullTrash === 5}">({{ fullTrash }} / 5)</span>
		</div>
		<p class="trash-mess" :class="{error: fullTrash === 5}" v-if="trash.length > 4">
			{{ trashMess }}
			<span>(кнопка "Удалить" недоступна)</span>
		</p>
		<ul>
			<li v-for="(el, index) in trash" :key="index">
				<div class="trash-list">
					<span> {{ el }} </span><!--задача-->
					<button @click="restoreTask(index)">Восстановить</button>
				</div>
			</li>
		</ul>
		<p class="trash-alert-mess" v-if="trash.length === 0">Корзина пуста</p>
		<button class="clear-trash" v-if="trash.length > 0" @click="clearTrash">Очистить всё</button>
	</div>
</template>

<script>
	
	export default {
		name: "Form",
		data() {
			return {
				newTask: "",		//новая задача
				tasksArray: [],		//массив задач
				message: "",		//сообщения
				messStyle: {		//объект со стилями для сообщения
					color: '',
				},
				titleTrash: 'Корзина',
				trash: [],
				trashMess: 'Корзина заполнена!',
			};
		},

		props: {
			title: {
				type: String,
				require: true,
			},
		},

		methods: {
			addTask() {
				if (!this.newTask.trim()) 
					return;
				
				this.tasksArray.push(this.newTask.trim());	//добавляем этот элемент в массив .push()
				this.newTask = "";							//очищаем строку
			},

			deleteMess() {
				setTimeout(() => {
					this.message = '';
				}, 800);
			},

			showMess() {
				if (!this.newTask){
					this.message = 'Поле для ввода пустое!';	//задаем значение переменной
					this.messStyle.color = 'rgb(196, 8, 8)';	//меняем цвет сообщения
				} 
				else if(this.newTask){
					this.message = 'Задача успешно добавлена!';	//задаем значение переменной
					this.messStyle.color = 'rgb(48, 196, 11)';	//меняем цвет сообщения
				}
				else if(this.removeTask()){
					this.message = 'Задача помещена в корзину!';	//задаем значение переменной
					this.messStyle.color = '#ffa500';				//меняем цвет сообщения
				}
				this.deleteMess();
			},

			removeTask(index) {	
				this.message = 'Задача добавлена в корзину!';
     			this.messStyle.color = 'orange';
				this.trash.push(this.tasksArray[index]);	//перезаписываем элемент из задач в массив корзины
      			this.tasksArray.splice(index, 1);			//удаляем элемент из массива
				if(this.trash.length > 5){
					
				}
				this.deleteMess();
			},

			clearTrash() {
				this.trash = [];
			},

			restoreTask(index) {
				this.tasksArray.push(this.trash[index]);	//перезаписываем элемент из корзины в массив задач
				this.trash.splice(index, 1);				//убираем элемент из корзины
			}	
		},

		computed: {
			fullTrash(){
				return this.trash.length;
			}
		},	
	};
</script>

<style lang="css" scoped>
	.container{
		width: 30%;
		border-right: 2px solid #000;
		border-radius: 2px;
	}

	.container h1, h3{
		text-align: center;
	}

	.container h1{
		font-size: 30px;
		margin-bottom: 20px;
		font-weight: 700;
	}

	.container h3{
		font-size: 13px;
		color: rgb(63, 63, 63);
		margin-bottom: 10px;
		font-weight: 300;
	}

	.container form{
		margin: auto;
		margin-bottom: 3px;
		width: 70%;
		display: flex;
		justify-content: space-between;
	}

	.container form input{
		height: 40px;
		width: 240px;
		border-radius: 4px;
		border: 2px solid black;
		font-size: 15px;
		padding: 20px 12px;
		color: rgb(63, 63, 63);
		font-weight: 500;
	}

	.container form input::placeholder{
		font-size: 13px;
	}

	.container form button{
		border-radius: 4px;
		border: 2px solid rgb(69, 69, 69);
		width: 150px;
		cursor: pointer;
		background-color: #000;
		color: #fff;
		font-size: 14px;
		font-weight: 400;
	}

	.container form button:active,
	.container ul li button:hover, 
	.container-2 ul li .trash-list > button:hover,
	.container-2 .clear-trash:active{
		border: 2px solid black;
		background-color: #fff;
		color: #000;
		transition: all 100ms ease;
	}

	.container p.alert-mess{
		font-size: 10px;
		font-weight: 300;
		margin-left: 15%;
		margin-bottom: 20px;
	}

	.container ul{
		list-style: none;
		margin-left: 15%;
		font-size: 17px;
		font-weight: 500;
	}

	.container ul > li{
		margin-bottom: 5px;
	}

	.container ul li > .list{
		display: flex;
   		width: 80%;
    	justify-content: space-between;
		align-items: center;
	}

	.container ul li button{
		border-radius: 4px;
		border: 2px solid rgb(69, 69, 69);
		height: 40px;
		width: 120px;
		cursor: pointer;
		background-color: #000;
		color: #fff;
		font-size: 14px;
		font-weight: 400;
	}

	.container-2{
		width: 20%;
		text-align: left;
		margin-left: 15px;
	}

	.container-2 .trash-num > h2{
		font-size: 25px;
		margin-bottom: 1px;
		font-weight: 700;
	}

	.container-2 .trash-num > span{
		font-size: 14px;
		margin-left: 5px;
	}

	.container-2 > .trash-num{
		display: flex;
		align-items: baseline;
		margin-bottom: 5px;
	}

	.container-2 > .trash-mess{
		font-size: 13px;
		margin-bottom: 5px;
		margin-top: -5px;
	}

	.container-2 .trash-mess > span{
		font-size: 10px;
	}

	.container-2 .error{
		color: rgb(196, 8, 8);
	}

	.container-2 ul{
		list-style: none;
		font-size: 16px;
    	font-weight: 500;
	}

	.container-2 ul > li{
		margin-bottom: 5px;
	}

	.container-2 ul li > .trash-list{
		display: flex;
   		width: 100%;
    	justify-content: space-between;
		align-items: center;
	}

	.container-2 ul li .trash-list > button{
		border-radius: 4px;
		border: 2px solid rgb(69, 69, 69);
		height: 45px;
		width: 145px;
		cursor: pointer;
		background-color: #000;
		color: #fff;
		font-size: 13px;
		font-weight: 400;
	}

	.container-2 .clear-trash{
		border-radius: 4px;
		border: 2px solid rgb(69, 69, 69);
		height: 45px;
		width: 145px;
		cursor: pointer;
		background-color: #000;
		color: #fff;
		font-size: 14px;
		font-weight: 700;
		float: right;
	}
</style>