<script>
    import CellType from "./constants/cell-type";
    import EmployeeTableCell from "./EmployeeTableCell.svelte";

    export let employees;
</script>

<style>
    th {
        color: grey;
        font-size: 12px;
        font-weight: 700;
        line-height: 48px;
        padding: 0;
        text-align: left;
        text-transform: uppercase;
    }

    th:not(:first-child) {
        padding-right: 48px;
    }
</style>

<table>
    <colgroup>
        <col style="width: 70px;" />
        <col />
        <col />
        <col />
        <col />
    </colgroup>
    <thead>
        <tr>
            <th></th>
            <th>Full Name</th>
            <th>Gender</th>
            <th>Address</th>
            <th>Email</th>
        </tr>
    </thead>
    <tbody>
        {#each employees as employee}
            <tr>
                <EmployeeTableCell
                    componentType={CellType.IMAGE}
                    value={employee.thumbnail}
                    alt={employee.fullName}
                />
                <EmployeeTableCell
                    value={employee.fullName}
                />
                <EmployeeTableCell
                    value={employee.gender}
                />
                <EmployeeTableCell
                    componentType={CellType.ADDRESS}
                    value={employee.address}
                />
                <EmployeeTableCell
                    componentType={CellType.EMAIL}
                    value={employee.email}
                />
            </tr>
        {/each}
    </tbody>
</table>