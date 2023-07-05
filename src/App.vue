<script setup lang="ts">
import { SpiffCommerceClient } from '@spiffcommerce/core';
import { SpiffCommerce3DPreviewService } from '@spiffcommerce/preview';
import { ref, onMounted } from 'vue'

const integrationProductId = 'b8b74dc9-655b-474e-b2b6-338db397e689';
const workflowId = '653b6369-89d1-4302-ba1e-ee50117e3c73';

const canvasRef = ref<HTMLCanvasElement | null>(null);
const client = new SpiffCommerceClient({});
const initialize = async () => {
    return await client.getWorkflowExperience(
        undefined,
        undefined,
        (workflow) => {
            return new SpiffCommerce3DPreviewService(workflow.globalPreviewConfig);
        },
        {
            type: 'integration',
            integrationProductId: integrationProductId,
            workflowId: workflowId,
        },
    );
};

onMounted(() => {
    initialize().then((experience) => {
        if(!canvasRef.value) return;
        experience.getWorkflowManager().getPreviewService().registerView(canvasRef.value);
    });
});
</script>

<template>
    <canvas id="preview" ref="canvasRef"></canvas>
</template>

<style scoped>
#preview {
    width: 100%;
    height: 100%;
    outline: none;
}
</style>
