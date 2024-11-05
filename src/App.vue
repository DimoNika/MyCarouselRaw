
<template>
    
    <div style="height: 200px;">
    </div>
    

    <div v-if="loading">Loading...</div>
    <div v-else>
        <MyCarousel :imagesArray=imagesArray> </MyCarousel>
    </div>
    

</template>

<script>
    import MyCarousel from '@/components/MyCarousel.vue'
    




export default {
    components: {
        MyCarousel
    },
    data() {
        return {
            imagesArray: [],
            loading: true
        }
    },
    mounted() {
        

        this.getImages()
        
    },
    methods: {
        test() {
            console.log(this.imagesArray)
            console.log(typeof(this.imagesArray))
        },
        getImages() {
            // fetch("https://picsum.photos/v2/list")
            fetch("https://picsum.photos/v2/list?page=4&limit=12")
            
            .then(response => {
                // console.log(response)
                return response.json()
            })
            .then(data => {
                // console.log(data)
                
                data.forEach(element => {
                    console.log(element)

                    let modifiedUrl = element.download_url.split('/').slice(0, -2).join('/') + '/400';
                    console.log(modifiedUrl)


                    this.imagesArray.push({"url": modifiedUrl, "isShowed": false, "chosen": false})
                    



                });
                this.loading = false;
                
            })
            .catch((test) => {
                console.log("error")
                console.log(test)
                
            })
            
            

        }
    },
}
</script>


<style scoped>


</style>
