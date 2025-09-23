<template>
  <section>
    <div v-if="members.length === 0" class="text-muted">尚無新增支出</div>
    <table v-else class="table table-striped">
      <thead>
        <tr>
          <th>成員姓名</th>
          <th>應付/應收</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="m in members" :key="m.id">
          <td>{{ m.name }}</td>
          <td :class="getBalanceClass(m.id)">{{ formatBalance(m.id) }}</td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
export default {
    name: 'BalanceList',
    props: ['members', 'expenses'],
    computed: {
        balanceMap() {
            const map = {}
            this.members.forEach(m => map[m.id] = 0)
            
            const expenses = this.expenses || []
            const totalAmount = expenses.reduce((sum, e) => sum + e.paymoney, 0)
            const perPerson = this.members.length ? totalAmount / this.members.length : 0

            expenses.forEach(e => {
                if (map[e.paidBy] !== undefined) {
                map[e.paidBy] += e.paymoney
                }
            })

            const balanceMap = {}
            for (const id in map) {
                balanceMap[id] = map[id] - perPerson
            }
            return balanceMap
            }
    },
    methods: {
        formatBalance(id) {
            const balance = this.balanceMap[id]
            if (balance > 0) return `應收 ${balance.toFixed(2)} 元`
            if (balance < 0) return `應付 ${Math.abs(balance).toFixed(2)} 元`
            return '已結清'
        },
        getBalanceClass(id) {
            const balance = this.balanceMap[id]
            if (balance > 0) return 'table-success'
            if (balance < 0) return 'table-danger'
        return ''
        }
    }
}
</script>

<style scoped>
/* 如果還想要顏色，可以這樣加在 table class 上 */
.table-success { 
    color: green; font-weight: bold; 
}
.table-danger { 
    color: red; font-weight: bold; 
    }
</style>
