<template>
    <n-card size="small" :bordered="true" class="nav-title-card">
        <template #header>
            <div class="navbar">
                <n-button
                    class="back-btn"
                    v-if="back"
                    @click="goBack"
                    quaternary
                    circle
                    size="small"
                >
                    <template #icon>
                        <n-icon><chevron-left-round /></n-icon>
                    </template>
                </n-button>

                {{ props.title }}

                <n-switch
                    :value="store.state.theme === 'dark'"
                    @update:value="switchTheme"
                    size="small"
                    class="theme-switch-wrap"
                >
                    <template #icon>
                        <dark-mode-outlined />
                    </template>
                </n-switch>
            </div>
        </template>
    </n-card>
</template>

<script setup lang="ts">
import { onMounted } from 'vue';
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';
import { useOsTheme } from 'naive-ui';
import { DarkModeOutlined, ChevronLeftRound } from '@vicons/material';

const store = useStore();
const router = useRouter();

const props = withDefaults(defineProps<{
    title: string,
    back: boolean,
}>(), {
    title: "",
    back: false
});
const switchTheme = (theme: boolean) => {
    if (theme) {
        localStorage.setItem('PAOPAO_THEME', 'dark');
        store.commit('triggerTheme', 'dark');
    } else {
        localStorage.setItem('PAOPAO_THEME', 'light');
        store.commit('triggerTheme', 'light');
    }
};
const goBack = () => {
    if (window.history.length <= 1) {
        router.push({
            path: '/',
        });
    } else {
        router.go(-1);
    }
};

onMounted(() => {
    if (!localStorage.getItem('PAOPAO_THEME')) {
        switchTheme((useOsTheme() as unknown as string) === 'dark');
    }
});
</script>

<style lang="less">
.nav-title-card {
    z-index: 99;
    width: 100%;
    top: 0;
    position: sticky;
    border-radius: 0;
    border-bottom: 0;
    background-color: rgba(255, 255, 255, 0.75);
    backdrop-filter: blur(12px);

    .navbar {
        height: 30px;
        position: relative;
        display: flex;
        align-items: center;

        .back-btn {
            margin-right: 8px;
        }

        .theme-switch-wrap {
            position: absolute;
            right: 0;
            top: calc(50% - 9px);
        }
    }
}
.dark {
    .nav-title-card {
        background-color: rgba(16, 16, 20, 0.75);
    }
}
</style>