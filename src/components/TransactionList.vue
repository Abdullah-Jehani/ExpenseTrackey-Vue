<template>
     <h3>History</h3>
      <ul id="list" class="list">

        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.cost < 0 ? 'minus' : 'plus'"> 
            {{transaction.text}} <span>$ {{transaction.cost}}</span><button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
        </li>
      
      </ul>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    transactions : {
        type: Array , 
        required: true
    }
})

const emit = defineEmits(['TransactionDeleted'])

const deleteTransaction = (id) => {
    emit('TransactionDeleted' , id)
    
}

</script> 