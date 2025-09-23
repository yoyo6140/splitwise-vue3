<template>
  <div class="container">
    <h1 class="text-center my-3">Splitwise 分帳小幫手</h1>

    <!-- 成員列表卡片 -->
    <div class="row justify-content-center mb-3">
      <div class="col-10">
        <div class="card shadow-sm">
          <div class="card-body">
            <h5 class="card-title">分帳成員</h5>
            <member-list
              :members="members"
              @update-members="updateMembers"
              @delete-member-expenses="deleteMemberExpenses"
            />
          </div>
        </div>
      </div>
    </div>

    <!-- 新增支出卡片 -->
    <div class="row justify-content-center mb-3">
      <div class="col-10">
        <div class="card shadow-sm">
          <div class="card-body">
            <h5 class="card-title">新增支出款項</h5>
            <expense-input
              :members="members"
              @add-expense="addExpense"
            />
          </div>
        </div>
      </div>
    </div>

    <!-- 支出清單卡片 -->
    <div class="row justify-content-center mb-3">
      <div class="col-10">
        <div class="card shadow-sm">
          <div class="card-body">
            <h5 class="card-title">帳款清單</h5>
            <expense-list
              :members="members"
              :expenses="expenses"
              @delete-expense="deleteExpense"
            />
          </div>
        </div>
      </div>
    </div>

    <!-- 成員應付/應收卡片 -->
    <div class="row justify-content-center mb-3">
      <div class="col-10">
        <div class="card shadow-sm">
          <div class="card-body">
            <h5>成員應付/應收</h5>
            <balance-list
              :members="members"
              :expenses="expenses"
            />
          </div>
        </div>
      </div>
    </div>

    <!-- 清除全部資料按鈕 -->
    <div class="row justify-content-center">
      <div class="col-8 text-end">
        <button @click="clearAll" class="btn btn-danger mb-3">
          清除全部資料
        </button>
      </div>
    </div>
  </div>
</template>



<script>
import MemberList from './components/MemberList.vue';
import ExpenseInput from './components/ExpenseInput.vue';
import ExpenseList from './components/ExpenseList.vue';
import BalanceList from './components/BalanceList.vue';

export default {
  name: 'App',
  components:{MemberList,ExpenseInput,ExpenseList,BalanceList},
  data() {
    return {
      //讀取資料 如果沒有資料 則是 members:[] expenses:[]
      members: JSON.parse(localStorage.getItem('splitwise_members')) || [],
      expenses: JSON.parse(localStorage.getItem('splitwise_expenses')) || []
    }
  },
  methods: {
    //儲存函式 按下新增按鈕時 呼叫 同步將資料存入
    saveData() {
      localStorage.setItem('splitwise_members', JSON.stringify(this.members));
      localStorage.setItem('splitwise_expenses',JSON.stringify(this.expenses));
    },
    //監控members組件變化 如果有數據變化 立即渲染與更新
    updateMembers(newMembers) {
      this.members = newMembers
      this.saveData()
    },

    //新增支出
    addExpense(newExp) {
      this.expenses.push(newExp)
      this.saveData()
    },
    //刪除支出
    deleteExpense(expid) {
      this.expenses = this.expenses.filter(exp => exp.id !== expid)  // 在父組件刪掉
      this.saveData()
    },
    //因為刪除成員，同步刪除支出
    deleteMemberExpenses(id) {
      this.expenses = this.expenses.filter(expenses => expenses.paidBy !==id )
      this.saveData()

    },
    clearAll() {
      if (confirm("確定要清除所有成員與支出嗎？")) {
        this.members = []
        this.expenses = []
        localStorage.removeItem('splitwise_members')
        localStorage.removeItem('splitwise_expenses')
        
      }
    }
  }
  
}
</script>

<style>
body {
  font-family: 'Roboto', sans-serif;
  background: #f5f5f5;
  color: #333;
}
.container {
  max-width: 700px;
}
h1 {
  text-align: center;
  margin-bottom: 30px;
  color: #4c4c4c;
}
.card-title {
  border-bottom: 2px solid #eee;
  padding-bottom: 10px;
  margin-bottom: 15px;
  font-size: 1.2rem;
  color: #555;
}
.btn {
  font-weight: bold;
  font-size: 0.95rem;
  border-radius: 6px;
}
.btn-primary {
  background: #4c9aff;
  border-color: #4c9aff;
}
.btn-primary:hover {
  background: #357ae8;
  border-color: #357ae8;
}
.btn-danger {
  background: #f44336;
  border-color: #f44336;
}
.btn-danger:hover {
  background: #d32f2f;
  border-color: #d32f2f;
}


.btn-rounded {
  border-radius: 20px;
}
</style>
