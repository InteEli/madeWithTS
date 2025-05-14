<script>
	let selectedItem = null;
	let selectType = "none";
	let itemsInGrid = [];      
	let gridStart = {x: 0, y: 0};
</script>
<main>
	{#each itemsInGrid as item}
            <div class = "item" class:selected = {item == selectedItem} style="width:{item.width *20}px; height:{item.height*20}px; top: {(item.start.y*20) + gridStart.y}px; left: {(item.start.x *20) + gridStart.x}px; background-color: {item.backgroundColor};">
                <input type="button" style= "top: {-5}px; left: {- 5}px; width:{item.width *20+5}px; height:{item.height*20+5}px;" class = "border" class:invisible = {item != selectedItem}/>
                {#if item.type == "text"}
                <textarea value={item.name} style="font-size: {item.fontSize}; font-family: {item.font};  color:{item.color};" 
                    class = "textInput" id = {String(item.id)}/>
                <div style="height: 100%; width: 100%; color: transparent; background-color: transparent; font-family: {item.font}" class = "textInput" id = {String(-item.id)}> {item.name} </div>  <!--a div that changes font size with help of text fit and then applies that font size to the text area because of lack of support of text area-->  
                <input type="button" on:mousedown={() => selectPlacedItem(item)} style="height: 100%; width: 100%; position: absolute;" on:dblclick={() => changeText(item)}/>  
                {/if}
                {#if item.type == "image"}
                <div id = {String(-item.id)} style="position: relative; overflow: hidden; width:100%; height:100%;" class:border2 = {selectType == "cropImage" && item == selectedItem}>
                <input type="button" id = {String(item.id)} style="height: {item.height*20}px; width: {item.width*20}px; position: absolute; background-image: url({item.src}); background-size: cover; background-repeat: no-repeat; background-size: 100% 100%;"/>  <!--https://stackoverflow.com/questions/71767782/make-image-fit-in-a-container-without-object-fit-contain-->
                </div>
                {/if}
			</div>
    {/each}
</main>

<style>
    main {
        display: grid;
        height: 100%;
        width: 100%;
        grid-template-columns: 200px 5fr;
    }
    .colorPicker{
        display: flex;
        position: absolute;
        flex-direction: column;
        padding: 10px;
        background-color: rgb(243, 243, 243);
        border-radius: 20px;
        height: 350px;
        width: 200px;
        color: black;
        font-family: 'Times New Roman', Times, serif;
        z-index: 100;
        margin: 20px;
    }
    .colorGrid {
    display: grid;
    grid-template-columns: repeat(5, 30px); 
    gap: 5px;
    padding: 10px;
}

.colorGrid button {
    width: 30px;
    height: 30px;
    border: none;
    cursor: pointer;
    border-radius: 5px; 
}

.colorGrid button:hover {
    border: 2px solid black; 
}
    .websiteGrid {
        display: grid;
        grid-template-columns: repeat(60, 20px);
        grid-template-rows: repeat(80, 20px);
        height: 100%;
        width: 100%;
        justify-content: center;
        align-items: center;
        position: relative;
    }

    .tools {
        display: flex;
        flex-direction: column;
        padding: 10px;
        background-color: rgb(236, 236, 236);
        height: 100%;
        width: 100%;
        color: black;
    }
    .settings {
        display: flex;
        flex-direction: row;
        padding: 10px;
        background-color: rgb(243, 243, 243);
        border-radius: 20px;
        height: 100%;
        width: 100%;
        color: black;
        font-family: 'Times New Roman', Times, serif;
        gap: 10px;
    }
    .sizeButton{
        position: absolute;
        background-color: rgb(0, 17, 255);
        border: 1px solid black;
        width: 15px;
        height: 15px;
    }
    .cell{
        border: 1px dashed black;
        width: 20px;
        height: 20px;
        z-index: 0;
    }
    .cell:hover{
        background-color: rgba(173, 216, 230, 0.258);
        border: 1px solid black;

    }
    .selectedItem{
        background-color: rgb(229, 229, 229);
        padding: 10px;
        display: flex;
        position: absolute;
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
        overflow: hidden;
        overflow-wrap: anywhere;
    }
    .item {
    position: absolute;
    background-color: transparent;
    }
    .higherZ{
        z-index: 2;
    }
    .border{
        position: absolute;
        background-color: transparent;
        border: 5px solid rgb(0, 110, 255);
        width: 100%;
        height: 100%;
    }
    .grid{
        display: grid;
        grid-template-rows: 50px 5fr;
        flex-direction: column;
    }

</style>