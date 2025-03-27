<template>
	<div class="orders-view">
	  <h2>Список заказов</h2>
	  <div class="actions">
		<button @click="openModal">Фильтровать заказы</button>
		<button @click="fetchAllOrders" :disabled="!filtersApplied">
		  Показать все заказы
		</button>
	  </div>
  
	  <!-- Модалка -->
	  <ModalFilter
		:is-open="isModalOpen"
		@close="closeModal"
		@apply-filters="fetchFilteredOrders"
	  />
  
	  <!-- Уведомления -->
	  <Notification
		v-if="notification.message"
		:message="notification.message"
		:type="notification.type"
	  />
  
	  <!-- Список заказов -->
	  <div class="orders-list">
		<p v-if="orders.length === 0">Нет заказов для отображения.</p>
		<ul v-else>
		  <li v-for="order in orders" :key="order.id">
			{{ order.title }} (Статус: {{ order.status }})
		  </li>
		</ul>
	  </div>
  
	  <!-- Пагинация -->
	  <Pagination
		v-if="totalOrders > limit"
		:total="totalOrders"
		:limit="limit"
		:current-page.sync="currentPage"
		@update:currentPage="fetchOrdersWithCurrentPage"
	  />
	</div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import ModalFilter from '../components/ModalFilter.vue';
  import Notification from '../components/Notification.vue';
  import Pagination from '../components/Pagination.vue';
  
  export default {
	name: 'OrdersView',
	components: {
	  ModalFilter,
	  Notification,
	  Pagination
	},
	setup() {
	  const isModalOpen = ref(false);
	  const orders = ref([]);
	  const notification = ref({ message: '', type: 'success' });
	  const currentPage = ref(1);
	  const totalOrders = ref(0);
	  const limit = 5; // Ограничение на страницу
	  const selectedStatuses = ref([]); // Храним текущие фильтры
	  const filtersApplied = ref(false);
  
	  const showNotification = (message, type) => {
		notification.value = { message, type };
	  };
  
	  const openModal = () => {
		isModalOpen.value = true;
	  };
  
	  const closeModal = () => {
		isModalOpen.value = false;
	  };
  
	  const fetchOrders = async (statuses = [], page = 1) => {
		try {
		  const params = new URLSearchParams({
			page,
			limit,
			...(statuses.length > 0 && { statuses: statuses.join(',') })
		  });
		  const response = await fetch(`/orders?${params}`);
		  if (!response.ok) throw new Error('Ошибка сервера');
		  const data = await response.json();
		  orders.value = data.orders;
		  totalOrders.value = data.total;
		  currentPage.value = data.page;
		} catch (error) {
		  console.error('Ошибка при загрузке заказов:', error);
		  showNotification('Не удалось загрузить заказы. Попробуйте позже.', 'error');
		  orders.value = [];
		  totalOrders.value = 0;
		}
	  };
  
	  const fetchFilteredOrders = (statuses) => {
		selectedStatuses.value = statuses;
		filtersApplied.value = true;
		fetchOrders(statuses, 1);
		showNotification('Фильтры успешно применены!', 'success');
		closeModal();
	  };
  
	  const fetchAllOrders = () => {
		selectedStatuses.value = [];
		filtersApplied.value = false;
		fetchOrders([], 1);
		showNotification('Все заказы загружены!', 'success');
	  };
  
	  const fetchOrdersWithCurrentPage = (page) => {
		currentPage.value = page;
		fetchOrders(selectedStatuses.value, page);
	  };
  
	  // Загружаем заказы при загрузке страницы
	  fetchOrders();
  
	  return {
		isModalOpen,
		orders,
		notification,
		currentPage,
		totalOrders,
		limit,
		filtersApplied,
		openModal,
		closeModal,
		fetchFilteredOrders,
		fetchAllOrders,
		fetchOrdersWithCurrentPage
	  };
	}
  };
  </script>
  
  <style scoped lang="scss">
  .orders-view {
	padding: 40px;
	max-width: 900px;
	margin: 0 auto;
  }
  
  h2 {
	font-size: 28px;
	margin-bottom: 20px;
	color: #333;
  }
  
  .actions {
	display: flex;
	gap: 15px;
	margin-bottom: 20px;
  }
  
  button {
	padding: 10px 20px;
	border: none;
	border-radius: 6px;
	cursor: pointer;
	font-size: 14px;
  }
  
  button:first-of-type {
	background: #007bff;
	color: white;
  }
  
  button:last-of-type {
	background: #6c757d;
	color: white;
  }
  
  button:disabled {
	background: #b0c4de;
	cursor: not-allowed;
  }
  
  .orders-list {
	background: #fff;
	border-radius: 8px;
	box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
	padding: 20px;
  }
  
  ul {
	list-style: none;
	padding: 0;
  }
  
  li {
	padding: 15px;
	border-bottom: 1px solid #e9ecef;
	font-size: 16px;
	color: #444;
  }
  
  li:last-child {
	border-bottom: none;
  }
  
  p {
	color: #6c757d;
	font-style: italic;
	text-align: center;
	padding: 20px;
  }
  </style>