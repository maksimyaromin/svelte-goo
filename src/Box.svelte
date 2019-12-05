<script>
    import classNames from "classnames";

    export let name;
    export let isDraggable = false;
    export let isAccepted = false;
    export let onDragStart = () => {};

    const classes = classNames({
        "box": true,
        "box--green": isAccepted
    });
</script>

<style>
    .box {
        align-items: center;
        background-color: blue;
        display: flex;
        justify-content: center;
        height: 80px;
        width: 80px;
    }

    .box.box--green {
        background-color: green;
    }

    .box {
        cursor: pointer;
    }

    .box:not(:last-child) {
        margin-bottom: 10px;
    }

    strong {
        color: white;
        text-align: center;
    }
</style>

<div data-name={name} class={classes} draggable={isDraggable} on:dragstart={onDragStart}>
    <strong>{name}</strong>
</div>