<script lang="ts" setup>
import { useBankAccountStore } from "./store/bankAccount";

const store = useBankAccountStore();

store.$onAction(({ name, store, after }) => {
    after((result) => {
        if (result && name === "charge") {
            store.processTransaction(result);
        }
    });
});
</script>

<template>
    <div>
        <HelloWorld />
        <p>Balance: {{ store.runningBalance }}</p>
        <p>Balance: {{ store.pendingAmount }}</p>

        <button @click="store.charge(5)">Buy Coffee $5</button>

        <h3>Pending:</h3>
        <ul>
            <li v-for="item in store.pendingTransactions" :key="item.id">${{ item.amount }}</li>
        </ul>

        <h3>Processed:</h3>
        <ul>
            <li v-for="item in store.processedTransactions" :key="item.id">${{ item.amount }}</li>
        </ul>
    </div>
</template>
