<script>
    import Box from "./Box.svelte";
    import EmployeesTable from "./EmployeesTable.svelte";
    import { onMount } from "svelte";
    import {
        Employee,
        EmployeeName,
        Location
    } from "./models";

    const total = 5000;

    const namesDB = [
        'Maksim',
        'Good Kat',
        'Patrick O\'Brian'
    ];

    let employeesResponse = [];

    let names = [ ...namesDB ];
    let acceptedNames = [];

    $: employees = employeesResponse;

    onMount(async function () {
        const response = await fetch(`https://randomuser.me/api/?results=${total}`, {
            headers: {
                "Content-Type": "application/json"
            }
        });
        const json = await response.json();

        employeesResponse = json.results.map(employee => new Employee({
            name: new EmployeeName(employee.name),
            email: employee.email,
            gender: employee.gender,
            location: new Location({
                city: employee.location.city,
                country: employee.location.country,
                postcode: String(employee.location.postcode),
                state: employee.location.state,
                street: employee.location.street.name
            }),
            thumbnail: employee.picture.thumbnail
        }));
    });

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

    .employees-table-container {
        display: flex;
        justify-content: center;
        margin-top: 30px;
        position: relative;
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

<div class="employees-table-container">
    <EmployeesTable employees={employees} />
</div>

