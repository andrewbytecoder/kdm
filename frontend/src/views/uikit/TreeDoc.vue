<script setup lang="ts">
import { NodeService } from '../../service/NodeService';
import { onMounted, ref, Ref } from 'vue';

interface TreeNode {
    key: string;
    label: string;
    data?: string;
    icon?: string;
    children?: TreeNode[];
}

const treeValue: Ref<TreeNode[] | null> = ref(null);
const selectedTreeValue: Ref<Record<string, boolean> | null> = ref(null);
const treeTableValue: Ref<TreeNode[] | null> = ref(null);
const selectedTreeTableValue: Ref<Record<string, boolean> | null> = ref(null);

onMounted(() => {
    NodeService.getTreeNodes().then((data: TreeNode[]) => (treeValue.value = data));
    NodeService.getTreeTableNodes().then((data: TreeNode[]) => (treeTableValue.value = data));
});
</script>

<template>
    <div class="card">
        <div class="font-semibold text-xl">Tree</div>
        <Tree :value="treeValue" selectionMode="checkbox" v-model:selectionKeys="selectedTreeValue"></Tree>
    </div>

    <div class="card">
        <div class="font-semibold text-xl mb-4">TreeTable</div>
        <TreeTable :value="treeTableValue" selectionMode="checkbox" v-model:selectionKeys="selectedTreeTableValue">
            <Column field="name" header="Name" :expander="true"></Column>
            <Column field="size" header="Size"></Column>
            <Column field="type" header="Type"></Column>
        </TreeTable>
    </div>
</template>
