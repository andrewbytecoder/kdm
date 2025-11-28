<script setup lang="ts">
import { PhotoService } from '../../service/PhotoService';
import { ProductService } from '../../service/ProductService';
import { onMounted, ref, Ref } from 'vue';

interface Product {
    id: string;
    code: string;
    name: string;
    description: string;
    image: string;
    price: number;
    category: string;
    quantity: number;
    inventoryStatus: string;
    rating: number;
}

interface Image {
    itemImageSrc: string;
    thumbnailImageSrc: string;
    alt: string;
}

interface ResponsiveOptions {
    breakpoint: string;
    numVisible: number;
    numScroll?: number;
}

const products: Ref<Product[]> = ref([]);
const images: Ref<Image[]> = ref([]);
const galleriaResponsiveOptions: Ref<ResponsiveOptions[]> = ref([
    {
        breakpoint: '1024px',
        numVisible: 5
    },
    {
        breakpoint: '960px',
        numVisible: 4
    },
    {
        breakpoint: '768px',
        numVisible: 3
    },
    {
        breakpoint: '560px',
        numVisible: 1
    }
]);
const carouselResponsiveOptions: Ref<ResponsiveOptions[]> = ref([
    {
        breakpoint: '1024px',
        numVisible: 3,
        numScroll: 3
    },
    {
        breakpoint: '768px',
        numVisible: 2,
        numScroll: 2
    },
    {
        breakpoint: '560px',
        numVisible: 1,
        numScroll: 1
    }
]);

onMounted(() => {
    ProductService.getProductsSmall().then((data: Product[]) => (products.value = data));
    PhotoService.getImages().then((data: Image[]) => (images.value = data));
});

function getSeverity(status: string): string | null {
    switch (status) {
        case 'INSTOCK':
            return 'success';

        case 'LOWSTOCK':
            return 'warning';

        case 'OUTOFSTOCK':
            return 'danger';

        default:
            return null;
    }
}
</script>

// ... existing template code ...