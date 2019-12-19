<template>
    <v-carousel 
        cycle 
        interval="3000"
        show-arrows-on-hover
        hide-delimiters>
        <v-carousel-item
        v-for="(image, i) in images"
        :key="i"
        :src="image.link"
        ></v-carousel-item>
    </v-carousel>
</template>

<script>
export default {
    data: () => ({
        images: [
            { link: '/img/carousel/carousel1.png' },
            // { link: '/img/carousel/carousel2.jpg' },
            { link: '/img/carousel/carousel3.png' },
            // { link: '/img/carousel/carousel4.png' },
            { link: '/img/carousel/carousel5.png' },
            { link: '/img/carousel/carousel6.jpeg' },
            { link: '/img/carousel/carousel7.jpeg' },
            { link: '/img/carousel/carousel8.jpeg' },
            { link: '/img/carousel/carousel9.png' },
        ]
    }),
}
</script>