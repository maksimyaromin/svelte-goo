<script>
    import Box from "./Box.svelte";

    const namesDB = [
        'Maksim',
        'Good Kat',
        'Patrick O\'Brian'
    ];

    let names = [ ...namesDB ];
    let acceptedNames = [];

    function onDragStart(event) {
        if (event.target) {
            event.dataTransfer.setData(
                'text/plain',
                event.target.attributes.getNamedItem('data-name').value
            );
        }
    }

    function onDragOver(event) {
        event.preventDefault();
    }

    function onDrop(event) {
        const name = event.dataTransfer.getData('text');

        names = names.filter(na => na !== name);
        acceptedNames = [
            ...acceptedNames,
            name
        ];

        event.dataTransfer.clearData();
    }

    function onClick() {
        names = [ ...namesDB ];
        acceptedNames = [];
    }

</script>

<style>
    .container {
        display: flex;
        justify-content: space-between;
        margin: 20px auto;
        max-width: 798px;
    }

    .boxes {
        background-color: #E6E6E6;
        box-sizing: border-box;
        padding: 10px;
        width: 100px;
    }

    .reset-button {
        background: transparent;
        border: 0;
        border-radius: 0;
        box-shadow: 0 0 10px rgba(0, 0, 0, .2);
        cursor: pointer;
        font-weight: bold;
        outline: none!important;
        text-transform: uppercase;
    }
</style>

<div class="container">
    <div class="boxes boxes--source">
        {#each names as name}
            <Box name={name} isDraggable={true} onDragStart={onDragStart} />
        {/each}
    </div>

    <button class="reset-button" on:click={onClick}>Reset</button>

    <div
        class="boxes boxes--destination"
        on:dragover={onDragOver}
        on:drop={onDrop}
    >
        {#each acceptedNames as name}
            <Box name={name} isAccepted={true} />
        {/each}
    </div>
</div>

