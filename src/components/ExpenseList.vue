<template>
  <section>
    <div v-if="expenses.length === 0"  class="text-muted">尚無支出紀錄</div>
    <div v-else class="list-group">
        <div v-for="exp in expenses" :key="exp.id"  class="list-group-item d-flex justify-content-between align-items-center">
            <span>{{ exp.payname }} - {{ exp.paymoney }}元 - {{ getPayerName(exp.paidBy) }}</span>
            <button @click.prevent="deleteItem(exp.id)" class="btn btn-sm btn-outline-danger">刪除</button>

        </div>
    </div>
  </section>
</template>

<script>
export default {
    name:'ExpenseList',
    props:['members', 'expenses'],
    methods: {
        getPayerName(id) {
            const m = this.members.find(m => m.id == id);
            return m ? m.name:'未知'
        },
        deleteItem(id) {
            this.$emit('delete-expense', id)  // 只傳單筆 id
        }
    }
}
</script>

<style>

</style>