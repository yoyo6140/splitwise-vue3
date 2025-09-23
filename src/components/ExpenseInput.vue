<template>
  <section>
    <div class="mb-3">
        <label>款項名稱</label>
        <input type="text" v-model="payname" placeholder="請輸入款項名稱" class="form-control w-100"/>
    </div>

    <div class="mb-3">
        <label>款項金額</label>
        <input type="number" v-model.number="paymoney" min="0" class="form-control w-100"/>
    </div>
    <div class="mb-3">
        <label>由誰付款
            <select v-model="paidBy" class="form-select">
                <option value="">請選擇付款人</option>
                <option v-for="m in members" :key="m.id" :value="m.id">
                    {{ m.name }}
                </option>
            </select>
        </label>
        <button @click.prevent="addExpense" class="btn btn-success ms-2">增加支出</button>
    </div>
    
  </section>
</template>

<script>
export default {
    name:'ExpenseInput',
    props:['members'],
    data() {
        return {
            payname:'',
            paymoney:'',
            paidBy:''
        }
    },
    methods: {
        addExpense() {
            if (! this.payname.trim()) return alert('請輸入款項名稱');
            if (! this.paymoney || this.paymoney <= 0) return('請輸入正確金額');
            if (! this.paidBy) return alert('請選擇付款人');

            const newExp = {
                id:Date.now().toString(),
                payname:this.payname,
                paymoney:this.paymoney,
                paidBy:this.paidBy
            }
            this.$emit('add-expense', newExp)

            this.payname = ''
            this.paymoney = 0
            this.paidBy = ''
        }
    }
}
</script>

<style>

</style>