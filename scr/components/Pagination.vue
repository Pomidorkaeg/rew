<template>
	<div class="pagination">
	  <button
		@click="changePage(currentPage - 1)"
		:disabled="currentPage === 1"
	  >
		Назад
	  </button>
	  <span>Страница {{ currentPage }} из {{ totalPages }}</span>
	  <button
		@click="changePage(currentPage + 1)"
		:disabled="currentPage === totalPages"
	  >
		Вперед
	  </button>
	</div>
  </template>
  
  <script>
  export default {
	name: 'Pagination',
	props: {
	  total: {
		type: Number,
		required: true
	  },
	  limit: {
		type: Number,
		required: true
	  },
	  currentPage: {
		type: Number,
		required: true
	  }
	},
	computed: {
	  totalPages() {
		return Math.ceil(this.total / this.limit);
	  }
	},
	methods: {
	  changePage(page) {
		if (page < 1 || page > this.totalPages) return;
		this.$emit('update:currentPage', page);
	  }
	}
  };
  </script>
  
  <style scoped lang="scss">
  .pagination {
	display: flex;
	align-items: center;
	gap: 15px;
	margin-top: 20px;
	justify-content: center;
  }
  
  button {
	padding: 8px 15px;
	border: none;
	border-radius: 5px;
	background: #007bff;
	color: white;
	cursor: pointer;
  }
  
  button:disabled {
	background: #b0c4de;
	cursor: not-allowed;
  }
  
  span {
	font-size: 14px;
	color: #666;
  }
  </style>