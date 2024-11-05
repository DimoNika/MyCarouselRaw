<template>
    
    
    
    
    <div style="display: flex; justify-content: center; width: 100%;">
        <div class="images-holder" id="images-holder" style="width: fit-content;">
            
        </div>
    </div>

    <div  style="width: 100%; text-align: center; margin: 10px 0;">
        <button :disabled="isButtonDisabled" @click="prevImg" style="margin: 8px;min-height: 30px; width: 30px; border-radius: 100%; background-color: darkgrey; text-align: center; display: inline-flex; align-items: center; justify-content: center;">
            <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" fill="currentColor" class="bi bi-arrow-left-short" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M12 8a.5.5 0 0 1-.5.5H5.707l2.147 2.146a.5.5 0 0 1-.708.708l-3-3a.5.5 0 0 1 0-.708l3-3a.5.5 0 1 1 .708.708L5.707 7.5H11.5a.5.5 0 0 1 .5.5"/>
            </svg>  
        </button>

        <button :disabled="isButtonDisabled" @click="nextImg" style="margin: 8px; min-height: 30px; width: 30px; border-radius: 100%; background-color: darkgrey; text-align: center; display: inline-flex; align-items: center; justify-content: center;">
            <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" fill="currentColor" class="bi bi-arrow-right-short" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M4 8a.5.5 0 0 1 .5-.5h5.793L8.146 5.354a.5.5 0 1 1 .708-.708l3 3a.5.5 0 0 1 0 .708l-3 3a.5.5 0 0 1-.708-.708L10.293 8.5H4.5A.5.5 0 0 1 4 8"/>
            </svg>
        </button>
        
    </div>
    <div id="links">
        
    </div>
    


</template>


<script>
export default {
    props: {
        imagesArray: Object,
    },
    data() {
        return {
            imagesAmount: null,
            isButtonDisabled: false,
    
        }
    },
    mounted() {
        
     
        this.imagesAmountRegulator()
        window.addEventListener('resize', this.imagesAmountRegulator);
  
        this.initCaro()

    },
    
    methods: {
        initCaro() {
            const imagesHolder = document.getElementById("images-holder")

            console.log("check")
            for (let i = 0; i < this.imagesAmount; i++) {
                console.log("check")
                this.imagesArray[i].isShowed = true


                let holderDiv = document.createElement('div')
                holderDiv.classList.add('card')
                

                
                holderDiv.addEventListener('click', () => this.chooseImg(this.imagesArray[i], holderDiv));

                if (this.imagesArray[i].chosen) {
                    holderDiv.classList.add("chosen-card")
                }

                let img = document.createElement('img')
                img.src = this.imagesArray[i].url
                img.classList.add("image")


                holderDiv.appendChild(img)
                console.log(holderDiv)

                imagesHolder.appendChild(holderDiv)
                
            }  
        },
        nextImg() {
            
            console.log("next");
            
            this.isButtonDisabled = true;
            
            setTimeout(() => {
                this.isButtonDisabled = false;
            }, 1000);
            const imagesHolder = document.getElementById("images-holder")

            for (let i = 0; i < this.imagesArray.length; i++) {
                console.log(i);
                
                if (this.imagesArray[i].isShowed == false) {
                    this.imagesArray[i].isShowed = true
                    

                    let holderDiv = document.createElement('div')
                    holderDiv.classList.add('card')
                    if (this.imagesArray[i].chosen) {
                        holderDiv.classList.add("chosen-card")
                    }
                    holderDiv.addEventListener('click', () => this.chooseImg(this.imagesArray[i-1], holderDiv));
                    
                    holderDiv.classList.add('card-animate-in')
                    
                    

                    let img = document.createElement('img')
                    img.src = this.imagesArray[i].url
                    img.classList.add("image")


                    holderDiv.appendChild(img)
                    console.log(holderDiv)

                    imagesHolder.appendChild(holderDiv)

                    // remove first image

                    this.imagesArray[0].isShowed = false

                    
                    const firstElement = this.imagesArray.shift();
                    
                    
                    imagesHolder.children[0].classList.remove('card-animate-in');
                    this.imagesArray.push(firstElement);

                    imagesHolder.children[0].addEventListener('animationend', function() {
                        imagesHolder.children[0].remove();
                        console.log("remove")
                    }, { once: true });

                    
                    imagesHolder.children[0].classList.add("card-animate-out")
                    


                    break
                }
            }
        },
        prevImg() {
            this.isButtonDisabled = true;
            
            setTimeout(() => {
                this.isButtonDisabled = false; 
            }, 1000);
            console.log("prev");
            
            const imagesHolder = document.getElementById("images-holder")

            let lastElement = this.imagesArray.pop();
            lastElement.isShowed = true

            this.imagesArray.unshift(lastElement);


            let holderDiv = document.createElement('div')
            holderDiv.classList.add('card')
            holderDiv.addEventListener('click', () => this.chooseImg(this.imagesArray[0], holderDiv));

            if (this.imagesArray[0].chosen) {
                holderDiv.classList.add("chosen-card")
            }
            holderDiv.classList.add('card-animate-in')
            
            

            let img = document.createElement('img')
            img.src = this.imagesArray[0].url
            img.classList.add("image")


            holderDiv.appendChild(img)
            console.log(holderDiv)
            imagesHolder.insertBefore(holderDiv, imagesHolder.firstChild);

            for (let i = this.imagesArray.length - 1; i > 0; i--) {
                if (this.imagesArray[i].isShowed == true) {
                    this.imagesArray[i].isShowed = false
                    console.log(imagesHolder.lastChild)
                    
                    imagesHolder.lastChild.classList.remove('card-animate-in');

                    imagesHolder.lastChild.addEventListener('animationend', function() {
                        imagesHolder.lastChild.remove();
                        console.log("remove")
                    }, { once: true });
                    
                    imagesHolder.lastChild.classList.add("card-animate-out")

                    break
                }
            }
            

        },
        imagesAmountRegulator() {
            
            const imagesHolder = document.getElementById("images-holder")

            if (window.innerWidth < 865 ) {

                if (this.imagesAmount != 1) {
                    console.log('switch');
                    
                    this.imagesArray[1].isShowed = false
                    try {
                    imagesHolder.lastChild.remove()
                        
                    } catch (error) {
                        console.error(error);
                        
                    }

                }
                this.imagesAmount = 1
            } else if (window.innerWidth >= 865 && window.innerWidth < 1730) {
                if (this.imagesAmount == 1) {
                    this.imagesArray[1].isShowed = true

                    let holderDiv = document.createElement('div')
                    holderDiv.classList.add('card')
                    holderDiv.addEventListener('click', () => this.chooseImg(this.imagesArray[1], holderDiv));
                    if (this.imagesArray[1].chosen) {
                    holderDiv.classList.add("chosen-card")
                    }
                   
                    
                    

                    let img = document.createElement('img')
                    img.src = this.imagesArray[1].url
                    img.classList.add("image")


                    holderDiv.appendChild(img)
                    console.log(holderDiv)

                    imagesHolder.appendChild(holderDiv)
                } else if ((this.imagesAmount == 4)) {
                    console.log("4 to 2 ");
                    
                    [this.imagesArray[2], this.imagesArray[3]].forEach((element) => {
                        element.isShowed = false
                        try {
                            imagesHolder.lastChild.remove()
                        }
                        catch (error) {
                            console.error(error);
                            
                        }
                    })
                    
                }
                this.imagesAmount = 2
            } else {
                if ((this.imagesAmount == 2)) {
                    console.log("2 to 4");
                    
                    [this.imagesArray[2], this.imagesArray[3]].forEach((element) => {
                        element.isShowed = true
                        let holderDiv = document.createElement('div')
                        holderDiv.classList.add('card')
                        holderDiv.addEventListener('click', () => this.chooseImg(element, holderDiv));
                        if (element.chosen) {
                            holderDiv.classList.add("chosen-card")
                        }

                        
                        

                        let img = document.createElement('img')
                        img.src = element.url
                        img.classList.add("image")


                        holderDiv.appendChild(img)
                        console.log(holderDiv)

                        imagesHolder.appendChild(holderDiv)
                    })
                    
                }
                this.imagesAmount = 4
            }
        },
        chooseImg(card, target) {
            console.log(target);
            target.classList.add("chosen-card")
            console.log("choose");

            if (!card.chosen) {

                const linksHolder = document.getElementById("links")
                linksHolder
                let linkHolder = document.createElement("p")
                let newLink = document.createElement("a")
                linkHolder.appendChild(newLink)
                newLink.href = card.url
                newLink.innerHTML = card.url
                linksHolder.appendChild(linkHolder)
                card.chosen = true
            }


        },
        test() {
            console.log(this.imagesArray);
            

            
        }
        
      

    }
}
    

</script>

<style>

.image {
    width: 100%;
    height: 100%;
    border-radius: 10px;
    /* object-fit: cover; */
}




.images-holder {
    display: flex;
    flex-direction: row;
    /* max-width: 100%; */
    /* overflow: hidden; */
    
}



.card {
    padding: 0.5rem;
    
    align-items: center;
    height: 416px;
    width: 416px;
    
    border-radius: 10px;
   
}

.card:hover {
    cursor: pointer;
    background-color: rgba(0, 0, 0, 0.3);
}

.card-animate-out {
    animation: decreaseSize 1s ease;
    
}

.card-animate-in {
    animation: increaseWidth 1s ease;
    
}

.chosen-card {
    background-color: aquamarine !important;
}



@keyframes increaseWidth {
    from {
        width: 0; /* Конечная ширина */
        padding: 8px 0;
        
    }
}


@keyframes decreaseSize {
    to {
        width: 0; /* Конечная ширина */
        padding: 8px 0;
    }
}

button {
    all: unset;
    cursor: pointer;
}


</style>