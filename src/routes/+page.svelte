<script>
	import {onMount} from "svelte";
    import {currentTheme} from '$lib/theme-storage';
	let selectedItem = null;
    let cellSize = 20;
    let gridSize = {x: 60, y: 80};
    let gridStart = {x: 0, y: 0};
    let gridElement;
	let selectType = "none";
    //paste the text under this line


    
    //paste the text above this line   
    function updateCellSize(){
        let windowWidth = window.innerWidth;
        let windowHeight = document.documentElement.scrollHeight;
        let currentCellsize = cellSize;
        let main = document.getElementById("main");
        main.style.width = String(windowWidth) + "px";
        main.style.height = String(windowHeight) + "px";
        if (windowWidth < 400){
            cellSize = 4;
        }
        else if (windowWidth < 450){
            cellSize = 5;
        }
        else if(windowWidth < 500){
            cellSize = 6;
        }
        else if (windowWidth < 700){
            cellSize = 8;
        }
        else if (windowWidth < 800){
            cellSize = 10;
        }
        else if (windowWidth < 900){
            cellSize = 12;
        }
        else if (windowWidth < 1000){
            cellSize = 14;
        }
        else if (windowWidth < 1200){
            cellSize = 16;
        }
        else if (windowWidth < 1400){
            cellSize = 18;
        }
        else{
            cellSize = 20;
        }
        if(currentCellsize != cellSize){
            for (let i = 0; i < itemsInGrid.length; i++){
                itemsInGrid[i].fontSize = String((parseInt(itemsInGrid[i].baseFontSize)/20) * cellSize) + "px";
            }
            itemsInGrid = itemsInGrid
        }
}   
    function cropImage() {
        setTimeout(() => {
        for (let i = 0; i < itemsInGrid.length; i++){
            let item = itemsInGrid[i];
            console.log(item);
            if (item.type == "image"){
                let div = document.getElementById(String(-item.id))
                let img = document.getElementById(String(item.id))
                div.style.width = String((item.cropValues.baseWidth)* cellSize) + "px";
                div.style.height = String((item.cropValues.baseHeight) * cellSize) + "px";        
                div.style.top = String((-item.start.y + item.cropValues.cropStart.y)*cellSize) + "px";
                div.style.left = String((-item.start.x + item.cropValues.cropStart.x)*cellSize) + "px";
                img.style.top = String((item.start.y - item.cropValues.cropStart.y)*cellSize) + "px";
                img.style.left = String((item.start.x - item.cropValues.cropStart.x)*cellSize) + "px";
            }
        }

    }, 0);
    }
	onMount(() => {
        window.addEventListener("resize", (event) => {
            if (gridElement) {
                const rect = gridElement.getBoundingClientRect();
                gridStart = { x: rect.left, y: rect.top }; // Store the grid's top-left corner coordinates
            }
            updateCellSize();
            cropImage();
        });
        if (gridElement) {
            const rect = gridElement.getBoundingClientRect();
            gridStart = { x: rect.left, y: rect.top }; // Store the grid's top-left corner coordinates
            gridElement = gridElement
        }
        updateCellSize();
        cropImage();
    });
</script>
<main style="background-color: {mainBackground}; background-image: {mainBackgroundImage};" id = "main">
	<article class = "grid" style="width: {gridSize.x * cellSize}px; height: {gridSize.y * cellSize}px;" bind:this={gridElement}>
	{#each itemsInGrid as item}
            <div class = "item" class:selected = {item == selectedItem} style="width:{item.width * cellSize}px; height:{item.height*cellSize}px; top: {(item.start.y*cellSize)+20}px; left: {(item.start.x *cellSize) + gridStart.x}px; background-color: {item.backgroundColor};">
                <input type="button" style= "top: {-5}px; left: {- 5}px; width:{item.width *cellSize+5}px; height:{item.height*cellSize+5}px;" class = "border" class:invisible = {item != selectedItem}/>
                {#if item.type == "text"}
                <p style="font-size: {item.fontSize}; font-family: {item.font};  color:{item.color};" 
                    class = "textInput"> {item.name} </p>
                <div style="height: 100%; width: 100%; color: transparent; background-color: transparent; font-family: {item.font}" class = "textInput"> {item.name} </div>  <!--a div that changes font size with help of text fit and then applies that font size to the text area because of lack of support of text area-->    
                {/if}
                {#if item.type == "image"}
                <div id = {String(-item.id)} style="position: relative; overflow: hidden; width:100%; height:100%;" class:border2 = {selectType == "cropImage" && item == selectedItem}>
                <input type="button" id = {String(item.id)} style="height: {item.height*cellSize}px; width: {item.width*cellSize}px; position: absolute; background-image: url({item.src}); background-size: cover; background-repeat: no-repeat; background-size: 100% 100%;"/>  <!--https://stackoverflow.com/questions/71767782/make-image-fit-in-a-container-without-object-fit-contain-->
                </div>
                {/if}
			</div>
    {/each}
	</article>
</main>

<style>
    main {
        display: flex;
        height: 250dvh;
        width: 100dvw;
		justify-content: center;
        background-position: center;
        background-repeat: no-repeat;
        background-size: cover;
    }

    .invisible{
        display: none;
    }
    .selected{
        border: 2px solid rgb(0, 17, 255);
    }
    .textInput{
        width: 100%;
        height: 100%;
        display: inline-block;
        box-sizing: border-box;
        background-color: transparent;
        position: absolute;
        resize: none;
        font-size: inherit;
        text-overflow: ellipsis;
        line-height: 1.2;
        overflow-wrap: anywhere;
    }
    .item {
    position: absolute;
    background-color: transparent;
    }
    .border{
        position: absolute;
        background-color: transparent;
        border: 5px solid rgb(0, 110, 255);
        width: 100%;
        height: 100%;
    }
    .grid{
        flex-direction: column;
    }

</style>