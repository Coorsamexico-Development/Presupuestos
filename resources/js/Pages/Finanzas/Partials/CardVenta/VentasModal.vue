<script setup>
import { ref } from 'vue';

import ButtonAdd from '@/Components/ButtonAdd.vue';
import DialogModal from '@/Components/DialogModal.vue';
import ItemVentaDatials from './ItemVentaDatials.vue';
import TableComponent from '@/Components/Table.vue';
import FormVentaModal from './FormVentaModal.vue';

const emit = defineEmits(["close",])
const props = defineProps({
    show: {
        type: Boolean,
        default: false,
    },
    ventas: {
        type: Object,
        required: true,
    }
});
const showingFormVenta = ref(false);
const venta = ref({});
const typeForm = ref("create");

// MODALS FUNCITON
const showFormVentas = (ventaSelect) => {
    if (ventaSelect === undefined) {
        typeForm.value = "create"
    } else {
        typeForm.value = "update"
        venta.value = ventaSelect
    }
    showingFormVenta.value = true
}
// EN MODALS FUNCTION

const close = () => {
    emit('close');
};

</script>
<template>
    <DialogModal :show="show" @close="close()">
        <template #title>
            <div class="flex flex-row">
                <div class="px-4 py-1 border-r-4 border-gray-600 basis-1/3">
                    <span class="block font-bold text-center text-white">
                        Ventas
                    </span>
                </div>
                <div class="flex-1 px-2 py-1">
                    <div class="flex justify-center">
                        <ButtonAdd @click="showFormVentas()" class="text-sm text-white h-7">
                            Agregar
                        </ButtonAdd>
                    </div>
                </div>
            </div>
        </template>
        <template #content>
            <TableComponent>
                <template #thead>
                    <tr>
                        <th>CLIENTE</th>
                        <th>IVA</th>
                        <th>TOTAL IVA</th>
                        <th>SUBTOTAL</th>
                        <th>TOTAL</th>
                        <th></th>
                    </tr>
                </template>
                <template #tbody>
                    <ItemVentaDatials v-for="(venta, index) in props.ventas" :key="venta.id + '' + index" :venta="venta"
                        @edit="showFormVentas($event)" />
                </template>
            </TableComponent>
            <!-- MODALS -->
            <FormVentaModal :show="showingFormVenta" :type-form="typeForm" :venta="venta"
                @close="showingFormVenta = false" />
            <!-- ENDS MODALS -->
        </template>
    </DialogModal>
</template>
