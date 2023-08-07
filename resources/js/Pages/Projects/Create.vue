<template>
    <Head title="Skills Index"/>
    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">New Project</h2>
        </template>

        <div class="py-12">
            <div class="max-w-md mx-auto sm:px-6 lg:px-8 bg-white">
                <form class="p-4" @submit.prevent="submit">
                    <InputLabel for="skill_id" value="Skills"/>
                    <select v-model="form.skill_id" id="skill_id" name="skill_id"
                            class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md">
                        <option v-for="skill in skills" :key="skill.id" :value="skill.id">{{ skill.name }}</option>
                    </select>
                    <div>
                        <InputLabel for="name" value="Name"/>
                        <TextInput
                            id="name"
                            type="text"
                            class="mt-1 block w-full"
                            v-model="form.name"
                            autofocus
                            autocomplete="name"
                        />
                        <InputError class="mt-2" :message="form.errors.name"/>
                    </div>

                    <div>
                        <InputLabel for="project_url" value="URL"/>
                        <TextInput
                            id="project_url"
                            type="text"
                            class="mt-1 block w-full"
                            v-model="form.project_url"
                            autocomplete="userproject_url"
                        />
                        <InputError class="mt-2" :message="form.errors.name"/>
                    </div>

                    <div class="block mt-4">
                        <InputLabel for="image" value="Image"/>
                        <TextInput
                            id="image"
                            type="file"
                            @input="form.image = $event.target.files[0]"
                            class="mt-1 block w-full"
                            autofocus
                        />
                        <InputError class="mt-2" :message="form.errors.image"/>
                    </div>

                    <div class="flex items-center justify-end mt-4">
                        <PrimaryButton class="ml-4" :class="{ 'opacity-25': form.processing }"
                                       :disabled="form.processing">
                            Store
                        </PrimaryButton>
                    </div>
                </form>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import {Head, Link, useForm} from '@inertiajs/vue3';
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";
import InputError from "@/Components/InputError.vue";

defineProps({
    skills: Array,
});

const form = useForm({
    name: '',
    image: null,
    skill_id: '',
    project_url: '',
});
const submit = () => {
    form.post(route('projects.store'), {
        onFinish: () => form.reset('name', 'image'),
    });
};
</script>
