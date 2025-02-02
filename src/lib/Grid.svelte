
{JSON.stringify(grid)}

    {#each grid as row}
    <div class="grid grid-cols-8 gap-4">
        {#each row as cell}
            <div class="col-span-1"> {cell?.name} </div>
        {/each}
    </div>
    {/each}

<script>
    // all resources do something when they are clicked
    // they will attermpt to consume other resources
    // and if the resources exist, they are deducted and 
    // all the generates are added to the resources
    // each has a time it takes to produce

    // all resources 'autoclick' by default, at the same rate
    // as their production time. when completed, it will begin 
    // a cooldown period before it can be auto clicked again.
    // autoclicking can be turned on or off using doublick

    // upgrades can be purchased to change the production rate of a resource
    // alter the things it produces and reduce its poroduction rate and autoclick cooldown

    let resources = [
        {
            name: "Seedling",
            description: "Produces trees at a slow rate",
            takes: 15,
            cooldown: 15,
            generates: [ {
                name: "Tree",
                count: 1,
                chance: 1
            }],
            consumes: [],
            position: "A1",
        },
        {
            name: "Tree",
            description: "Can be cut down to produce wood.",
            takes: 60,
            cooldown: 60,
            generates: [
                {
                    name: "Wood",
                    count: 5,
                    chance: 1
                }
            ],
            consumes: [
                {
                    name: "Tree",
                    count: 1,
                    chance: 1
                }
            ],
            position: "A2",
        },
        {
            name: "Wood",
            description: "Can be used to build structures, and broken down into Planks",
            takes: 3,
            cooldown: 20,
            generates: [
                {
                    name: "Plank",
                    count: 2,
                    chance: 1
                }
            ],
            consumes: [{
                name: "Wood",
                count: 1,
                chance: 1
            }],
            position: "A3",
        },
        {
            name: "Plank",
            description: "Used to build other things",
            generates: [],
            consumes: [],
            position: "A4",
        },
        {
            name: "Apple",
            description: "Consumed by humans for all sorts of things, or can be used to grow a tree",
            takes: 60,
            generates: [
                {
                    name: "Tree",
                    count: 1,
                    chance: 0.4
                }
            ],
            consumes: [
                {
                    name: "Apple",
                    count: 1,
                    chance: 1
                }
            ],
            position: "A5",
        }
    ]

const inventory = resources.map(resource => ({
    name: resource.name,
    count: 0
}))

const upgrades = [
    {
        name: "Harvest Seedlins",
        description: "Chopping down trees has a small chance to produce a Seedling",
        cost: [{
            name: "Plank",
            count: 10,
        }],
        effect: (resources) => {
            resources.find(resource => resource.name === "Tree").generates.push({
                name: "Seedling",
                count: 1,
                chance: 0.01
            })
        }
    },
    {
        name: "Falling Apples",
        description: "Chopping down trees will produce Fruit",
        cost: [{
            name: "Plank",
            count: 10,
        }],
        effect: (resources) => {
            resources.find(resource => resource.name === "Tree").generates.push({
                name: "Seedling",
                count: 1,
                chance: 0.01
            })
        }
    },

]

// construct an 8x8 grid of resources
// keyed by position A1 - H8
const grid = Array.from({length: 8}, (_, row) => Array.from({length: 8}, (_, col) => {
    return resources.find(resource => resource.position === `${String.fromCharCode(65 + col)}${row + 1}`)
}))
</script>
