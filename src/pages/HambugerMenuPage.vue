<script setup lang="ts">
import LinkWrapper from '../components/LinkWrapper.vue';
import MenuButton from '../components/MenuButton.vue';
import { ref } from 'vue';

const isMenuOpen = ref<boolean>(false);
const menuButtonRef = ref<any>();

interface MenuProps {
    label: string
    to?: string
}

const menu: MenuProps[] = [
    {
        label: "Posters"
    },
    {
        label: "Artwork Styles"
    },
    {
        label: "FAQ"
    }
];

const listItemRef = ref<HTMLUListElement[]>([]);


const index = ref(0)

const manageKeyboardAccessOnItem = (event: KeyboardEvent) => {


    if (!isMenuOpen.value) {
        return;
    }

    if (event.key === 'Escape') {
        menuButtonRef.value.closeMenu();
        isMenuOpen.value = false;
        menuButtonRef.value.$el.focus();
        return;
    }

    //const focusable = 'button, a:not(.skiplink), input, select, textarea, [tabindex]:not([tabindex="-1"])'

    if (event.key !== 'Tab') {
        return;
    }

    event.preventDefault();

    const focusableElements: (HTMLButtonElement | HTMLAnchorElement)[] = [
        menuButtonRef.value.$el as HTMLButtonElement,
        ...listItemRef.value.map((item) => item.querySelector('a') as HTMLAnchorElement)
    ];


    if (!event.shiftKey) {
        index.value++;

        if (index.value > focusableElements.length - 1) {
            index.value = 0;
        }
    }

    if (event.shiftKey) {
        index.value--;

        if (index.value < 0) {
            index.value = focusableElements.length - 1;
        }
    }

    focusableElements[index.value].focus();

}

</script>

<template>
    <div>
        <h1 class="font-extrabold text-4xl mb-8">
            Hamburger Menu Page
        </h1>

        <div
            class="max-w-lg border border-gray-800 h-[30rem] relative"
            :class="{
                'overflow-scroll': !isMenuOpen,
                'overflow-hidden': isMenuOpen,
            }"
        >
            <header
                class="bg-gray-800 p-6 flex justify-between"
                @keydown="manageKeyboardAccessOnItem"
            >
                <menu-button
                    @update:is-open="(e: boolean) => isMenuOpen = e"
                    class="z-50"
                    ref="menuButtonRef"
                />

                <button
                    class="block rounded-md bg-gray-500 px-3.5 py-2.5 text-center text-sm font-semibold text-white shadow-sm hover:bg-gray-600 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-gray-200"
                >
                    Button
                </button>

                <button
                    class="block rounded-md bg-gray-500 px-3.5 py-2.5 text-center text-sm font-semibold text-white shadow-sm hover:bg-gray-600 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-gray-200"
                >
                    Button
                </button>

                <nav
                    aria-label="Main"
                    :aria-hidden="!isMenuOpen"
                    class="aria-[hidden='true']:hidden absolute inset-0 p-6 m-2 rounded-lg bg-white shadow-lg"
                >
                    <ul class="list-none left-0 w-full space-y-4 mt-20">
                        <li
                            v-for="item in menu"
                            :key="item.label"
                            class="w-full "
                            ref="listItemRef"
                        >
                            <a
                                :href="item.to ?? '#'"
                                class="no-underline hover:underline px-3.5 py-2 inline-block w-full hover:bg-gray-100"
                            >
                                {{ item.label }}
                            </a>
                        </li>
                    </ul>
                </nav>
            </header>

            <main
                id="main"
                class="prose p-6"
            >
                <h2>Lorem ipsum dolor sit amet.</h2>

                <p>
                    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Porro dolorem libero amet iste tempora
                    deserunt
                    mollitia natus quidem ipsam! Nesciunt praesentium molestiae eum dolores reprehenderit sunt odio eos nam
                    enim, minus dolorum asperiores iste delectus reiciendis? Asperiores facere aliquid ex quae error iure,
                    aliquam harum cum! Itaque, ex! Nam, culpa.
                </p>

                <p>
                    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Porro dolorem libero amet iste tempora
                    deserunt
                    mollitia natus quidem ipsam! Nesciunt praesentium molestiae eum dolores reprehenderit sunt odio eos nam
                    enim, minus dolorum asperiores iste delectus reiciendis? Asperiores facere aliquid ex quae error iure,
                    aliquam harum cum! Itaque, ex! Nam, culpa.
                </p>

                <h2>Lorem ipsum dolor sit amet.</h2>

                <p>
                    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Porro dolorem libero amet iste tempora
                    deserunt
                    mollitia natus quidem ipsam! Nesciunt praesentium molestiae eum dolores reprehenderit sunt odio eos nam
                    enim, minus dolorum asperiores iste delectus reiciendis? Asperiores facere aliquid ex quae error iure,
                    aliquam harum cum! Itaque, ex! Nam, culpa.
                </p>

                <p>
                    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Porro dolorem libero amet iste tempora
                    deserunt
                    mollitia natus quidem ipsam! Nesciunt praesentium molestiae eum dolores reprehenderit sunt odio eos nam
                    enim, minus dolorum asperiores iste delectus reiciendis? Asperiores facere aliquid ex quae error iure,
                    aliquam harum cum! Itaque, ex! Nam, culpa.
                </p>

            </main>

        </div>

        <footer
            id="footer"
            class="mt-24"
        >
            <link-wrapper />
        </footer>
    </div>
</template>