<script>
    import CellType from "./constants/cell-type";

    export let value;
    export let componentType = CellType.COMMON;
    export let alt = undefined;
</script>

<style>
    td {
        font-size: 14px;
        vertical-align: middle;
    }

    img {
        border-radius: 50%;
        margin: 10px 0;
    }

    a {
        color: skyblue;
        text-decoration: none;
    }

    address {
        padding-right: 48px;
    }
</style>

<td>
    {#if componentType === CellType.IMAGE}
        <img src={value} alt={alt} />
    {:else if componentType === CellType.ADDRESS}
        <address>{value}</address>
    {:else if componentType === CellType.EMAIL}
        <a href={`mailto:${value}`}>{value}</a>
    {:else}
        <span>{value}</span>
    {/if}
</td>