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
                     let itemsInGrid = [ 
        {
            name: "header 2",
            height: 4,
            width: 16,
            start: { x: 13, y: 4 },
            end: { x: 29, y: 8 },
            type: "text",
            id: 2,
            fontSize: "75px",
            baseFontSize: "75px",
            changeFontManually: false,
            font: "Arial",
            color: "themeDefault",
            backgroundColor: "transparent",
            src: "",
            cropValues: {
                cropStart: { x: 39, y: 14 },
                cropEnd: { x: 0, y: 0 },
                baseWidth: 16,
                baseHeight: 4
            }
                },
 
        {
            name: "header 2",
            height: 4,
            width: 16,
            start: { x: 1, y: 11 },
            end: { x: 17, y: 15 },
            type: "text",
            id: 3,
            fontSize: "75px",
            baseFontSize: "75px",
            changeFontManually: false,
            font: "Arial",
            color: "themeDefault",
            backgroundColor: "transparent",
            src: "",
            cropValues: {
                cropStart: { x: 39, y: 14 },
                cropEnd: { x: 0, y: 0 },
                baseWidth: 16,
                baseHeight: 4
            }
                },
 
        {
            name: "header 2",
            height: 4,
            width: 16,
            start: { x: 23, y: 10 },
            end: { x: 39, y: 14 },
            type: "text",
            id: 4,
            fontSize: "75px",
            baseFontSize: "75px",
            changeFontManually: false,
            font: "Arial",
            color: "themeDefault",
            backgroundColor: "transparent",
            src: "",
            cropValues: {
                cropStart: { x: 39, y: 14 },
                cropEnd: { x: 0, y: 0 },
                baseWidth: 16,
                baseHeight: 4
            }
                }];
            let mainBackground = "transparent";
            let mainBackgroundImage = "none";
            $currentTheme = "light";
                
            
            
    
    //paste the text above this line   
    function updateCellSize(){
        let windowWidth = window.innerWidth;
        let currentCellsize = cellSize;
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
	onMount(() => {
        window.addEventListener("resize", (event) => {
            if (gridElement) {
                const rect = gridElement.getBoundingClientRect();
                gridStart = { x: rect.left, y: rect.top }; // Store the grid's top-left corner coordinates
            }
            updateCellSize();
        });
        if (gridElement) {//copilot suggested this
            const rect = gridElement.getBoundingClientRect();
            gridStart = { x: rect.left, y: rect.top }; // Store the grid's top-left corner coordinates
            gridElement = gridElement
        }
        updateCellSize();
	});
</script>
<main style="background-color: {mainBackground}; background-image: {mainBackgroundImage};">
	<article class = "grid" style="width: {gridSize.x * cellSize}px; height: {gridSize.y * cellSize}px;" bind:this={gridElement}>
	{#each itemsInGrid as item}
            <div class = "item" class:selected = {item == selectedItem} style="width:{item.width * cellSize}px; height:{item.height*cellSize}px; top: {(item.start.y*cellSize)+20}px; left: {(item.start.x *cellSize) + gridStart.x}px; background-color: {item.backgroundColor};">
                <input type="button" style= "top: {-5}px; left: {- 5}px; width:{item.width *cellSize+5}px; height:{item.height*cellSize+5}px;" class = "border" class:invisible = {item != selectedItem}/>
                {#if item.type == "text"}
                <p style="font-size: {item.fontSize}; font-family: {item.font};  color:{item.color};" 
                    class = "textInput" id = {String(item.id)}> {item.name} </p>
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
        height: 100dvh;
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