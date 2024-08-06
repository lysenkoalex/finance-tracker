<template>
    <UCard>
        <template #header>
            Signing in...
        </template>

        Wait a moment while we sign in...
    </UCard>
</template>

<script setup>
useRedirectIfAuthenticated()
</script>