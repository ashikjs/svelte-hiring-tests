<script lang="ts">
  import {onMount} from "svelte";

  interface CarouselItem {
    id: string;
    src: string;
    alt: string;
  }

  export let items: CarouselItem[] = [
    {id: '1', src: 'https://picsum.photos/1200/680', alt: 'Slider 1'},
    {id: '2', src: 'https://picsum.photos/1200/680', alt: 'Slider 2'},
    {id: '3', src: 'https://picsum.photos/1200/680', alt: 'Slider 3'},
    {id: '4', src: 'https://picsum.photos/1200/680', alt: 'Slider 4'},
    {id: '5', src: 'https://picsum.photos/1200/680', alt: 'Slider 5'}
  ];

  // State variable
  let currentIndex = 0
  let totalItems = items.length
  let interval: NodeJS.Timer

  // All Controls function
  function nextSlider() {
    currentIndex = (currentIndex + 1 > totalItems - 1) ? 0 : currentIndex + 1
    clearInterval(interval)
  }

  function previousSlider() {
    currentIndex = (currentIndex - 1 < 0 ? totalItems - 1 : currentIndex - 1)
    clearInterval(interval)
  }

  function goToSlide(index: number) {
    currentIndex = index
    clearInterval(interval)
  }

  // Auto-slide
  onMount(() => {
    interval = setInterval(() => {
      currentIndex = (currentIndex + 1 > totalItems - 1) ? 0 : currentIndex + 1
    }, 2000)

    return () => clearInterval(interval)
  })

</script>

<div class="carousel">
    <!-- Carousel structure goes here -->
    <div class="carousel__container">
        <div class="carousel__track" style="transform: translateX(-{currentIndex * 100}%)">
            {#each items as {id, src, alt}}
                <div class="carousel__slide" key={id}>
                    <img class="carousel__image" src={src} alt={alt}>
                </div>
            {/each}
        </div>
    </div>
    <!-- Carousel arrow/navigation goes here -->
    <div class="carousel__arrow carousel__arrow--left" on:click={previousSlider}>
        &#x3C;
    </div>
    <div class="carousel__arrow carousel__arrow--right" on:click={nextSlider}>
        &#x3E;
    </div>

    <!-- Dot Indicators  -->
    <div class="carousel__dots">
        {#each Array(totalItems) as _, index}
            <button type="button"
                    class="carousel__dot {index === currentIndex ? 'carousel__dot--active' : ''}"
                    on:click={() => goToSlide(index)}></button>
        {/each}
    </div>
</div>

<style>
    .carousel {
        width: 100%;
        max-width: 800px;
        margin: 0 auto;
        position: relative;
    }

    .carousel__container {
        position: relative;
        overflow: hidden;
        aspect-ratio: 16/9;
    }

    .carousel__track {
        display: flex;
        width: 100%;
        transition: transform 0.3s ease-in-out;
    }

    .carousel__slide {
        flex: 0 0 100%;
        width: 100%;
    }

    .carousel__image {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .carousel__arrow {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        background: rgba(255, 255, 255, 0.7);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        cursor: pointer;
        font-size: 20px;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: background-color 0.3s;
        z-index: 1;
    }

    .carousel__arrow:hover {
        background: rgba(255, 255, 255, 0.9);
    }

    .carousel__arrow--left {
        left: 10px;
    }

    .carousel__arrow--right {
        right: 10px;
    }

    .carousel__dots {
        position: absolute;
        bottom: 15px;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        gap: 8px;
        padding: 5px 10px;
        background: rgba(0, 0, 0, 0.2);
        border-radius: 20px;
        z-index: 1;
    }

    .carousel__dot {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        border: none;
        background: rgba(255, 255, 255, 0.5);
        cursor: pointer;
        padding: 0;
        transition: background-color 0.3s;
    }

    .carousel__dot--active {
        background: white;
    }

    @media (max-width: 768px) {
        .carousel__arrow {
            width: 32px;
            height: 32px;
            font-size: 16px;
        }
    }
</style>
