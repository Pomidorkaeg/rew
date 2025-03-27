<template>
	<transition name="modal">
	  <div class="modal" v-if="isOpen" @click.self="closeModal">
		<div class="modal-content">
		  <h3>Фильтрация заказов</h3>
		  <p>Выберите статусы для фильтрации:</p>
  
		  <!-- Список статусов -->
		  <div class="checkbox-group">
			<label v-for="status in statuses" :key="status">
			  <input type="checkbox" :value="status" v-model="selectedStatuses" />
			  {{ status }}
			</label>
		  </div>
  
		  <!-- Кнопки -->
		  <div class="modal-actions">
			<button @click="applyFilters" :disabled="selectedStatuses.length === 0">
			  Применить
			</button>
			<button @click="resetFilters">Сбросить</button>
			<button @click="closeModal">Закрыть</button>
		  </div>
		</div>
	  </div>
	</transition>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
	name: 'ModalFilter',
	props: {
	  isOpen: {
		type: Boolean,
		default: false
	  }
	},
	setup(props, { emit }) {
	  // Список статусов
	  const statuses = [
		'НЕ ПОДХОДИТ ДАТА ИЛИ ВРЕМЯ',
		'НЕ ПОДХОДИТ ТОЧНАЯ ГРУЗА',
		'НЕ ПОДХОДИТ ОБЪЕМ ГРУЗА',
		'НЕ ПОДХОДИТ ТИП ГРУЗОРА',
		'НЕТ ПРОПУСКА В МКАД/ТТК',
		'ПОМОЧЬ МАЛЫЙ/АВАРИЯ',
		'ЗАБОЛЕН',
		'НЕТ САМОСВАРКИ',
		'НЕТ МЕЛ/КНЖКИ',
		'НЕТ ПРСОРТА',
		'НЕТ ЧАСТОТЫ',
		'НЕ УСТРАИВАЕТ СТАВКА ЗА РЕЙС',
		'НЕ УСТРАИВАЕТ МАРШРУТ',
		'ДРУГОЕ'
	  ];
  
	  const selectedStatuses = ref([]); // Выбранные статусы
  
	  const applyFilters = () => {
		if (selectedStatuses.value.length === 0) return;
		emit('apply-filters', selectedStatuses.value);
		emit('close');
	  };
  
	  const resetFilters = () => {
		selectedStatuses.value = [];
	  };
  
	  const closeModal = () => {
		emit('close');
	  };
  
	  return {
		statuses,
		selectedStatuses,
		applyFilters,
		resetFilters,
		closeModal
	  };
	}
  };
  </script>
  
  <style scoped lang="scss">
  .modal {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, 0.6);
	display: flex;
	justify-content: center;
	align-items: center;
	z-index: 999;
  }
  
  .modal-content {
	background: #fff;
	padding: 30px;
	border-radius: 12px;
	width: 500px;
	max-width: 90%;
	box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
  }
  
  h3 {
	font-size: 20px;
	margin-bottom: 10px;
	color: #333;
  }
  
  p {
	font-size: 14px;
	color: #666;
	margin-bottom: 20px;
  }
  
  .checkbox-group {
	display: flex;
	flex-direction: column;
	gap: 12px;
	margin: 20px 0;
	max-height: 300px;
	overflow-y: auto;
  }
  
  label {
	display: flex;
	align-items: center;
	gap: 10px;
	font-size: 14px;
	color: #444;
  }
  
  .modal-actions {
	display: flex;
	gap: 12px;
	justify-content: flex-end;
  }
  
  button {
	padding: 10px 20px;
	border: none;
	border-radius: 6px;
	cursor: pointer;
	font-size: 14px;
  }
  
  button:first-of-type {
	background: #28a745; // Зеленая для "Применить"
	color: white;
  }
  
  button:first-of-type:disabled {
	background: #b0c4de;
	cursor: not-allowed;
  }
  
  button:nth-of-type(2) {
	background: #f8f9fa;
	color: #333;
  }
  
  button:last-of-type {
	background: #dc3545;
	color: white;
  }
  
  /* Анимация модального окна */
  .modal-enter-active,
  .modal-leave-active {
	transition: opacity 0.3s ease;
  }
  
  .modal-enter-from,
  .modal-leave-to {
	opacity: 0;
  }
  
  .modal-content {
	transition: transform 0.3s ease;
  }
  
  .modal-enter-from .modal-content,
  .modal-leave-to .modal-content {
	transform: scale(0.95);
  }
  </style>