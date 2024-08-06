<template>
  <UCard v-if="!success">
    <template #header>
      Sign-in to Finance Tracker
    </template>

    <form @submit.prevent="handleLogin">
      <UFormGroup label="Email" name="email" class="mb-4" :required="true">
        <UInput type="email" placeholder="Email" required v-model="email" />
      </UFormGroup>
      <UFormGroup label="Password" name="password" class="mb-4" :required="true">
        <UInput type="password" placeholder="Password" required v-model="password" />
      </UFormGroup>

      <UButton type="submit" variant="solid" color="black" :loading="pending" :disabled="pending">Sign-in</UButton>
    </form>
  </UCard>
  <UCard v-else>
    <template #header>
      Signing in...
    </template>
    <div class="text-center">
      <p class="mb-4">Wait a moment while we sign in...</p>
    </div>
  </UCard>
</template>

<script setup>
  const success = ref(false)
  const email = ref('')
  const password = ref('')
  const pending = ref(false)
  const { toastError } = useAppToast()
  const supabase = useSupabaseClient()
  const redirectUrl = useRuntimeConfig().public.baseUrl

  useRedirectIfAuthenticated()

  const handleLogin = async () => {
    pending.value = true

    try {
      const { error } = await supabase.auth.signInWithPassword({
        email: email.value,
        password: password.value,
        options: {
          emailRedirectTo: `${redirectUrl}/confirm`
        }
      })

      if(error) {
        toastError({
          title: 'Error authenticationg',
          description: error.message
        }) 
      } else {
        success.value = true
      }
    } finally {
      pending.value = false
    }
  }

</script>