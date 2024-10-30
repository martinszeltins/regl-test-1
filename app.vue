<template>
    <div class="p-5 space-y-5">
        <h1 class="text-3xl">My Form</h1>

        <div><pre>$errors: {{ $errors }}, {{ typeof $errors }}</pre></div>

        <form @submit.prevent="submit">
            <div>
                <label for="name">Name</label>
                <input v-model="form.name" type="text" class="border border-gray-300 shadow ml-4">
                <ul>
                    <li v-for="error of $errors.name" :key="error">{{ error }}</li>
                </ul>
            </div>

            <div>
                <label for="email">Email</label>
                <input v-model="form.email" type="text" class="border border-gray-300 shadow ml-4">
                <ul>
                    <li v-for="error of $errors.email" :key="error">{{ error }}</li>
                </ul>
            </div>

            <button type="submit" class="shadow mt-4 bg-gray-200 px-5 py-2 rounded hover:bg-gray-300 hover:active:bg-gray-400">
                Submit
            </button>
        </form>
    </div>
</template>

<script setup lang="ts">
    import { useRegle } from '@regle/core'
    import { email, required, minLength, sameAs } from '@regle/validators'

    interface Skill {
        name: string
        level: number
    }

    interface Form {
        name: string
        email: string
        skills: Skill[]
    }

    const form = ref<Form>({
        name: '',
        email: '',
        skills: [],
    })

    const { $errors, validateForm } = useRegle(form, () => ({
        name: {
            required,
        },

        email: {
            email,
            required,
        },

        // TODO: Add validation for each skill
    }))

    const submit = async () => {
        const result = await validateForm()

        console.log(result)
    }
</script>
