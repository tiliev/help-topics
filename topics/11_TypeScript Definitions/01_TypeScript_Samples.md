<!--
|metadata|
{
    "fileName": "typescript-samples",
    "controlName": [],
    "tags": []
}
|metadata|
-->

# TypeScript Samples

## Topic Overview
This topic covers samples with Ignite UI controls and TypeScript.

### In this topic

This topic contains the following sections:
-   [Requirements](#requirements)
-   [Tile Manager Sample](#tile_manager_sample)
    -   [Preview](#tile_manager_sample_preview)
    -   [Create the HTML](#tile_manager_steps_html)
    -   [Create the Data Source](#tile_manager_steps_ds)
    -   [Create the igTileManager](#tile_manager_steps_tm)
-   [Related Content](#related_content)

### <a id="requirements"></a>Requirements
In order to run these samples, you need to have:
-   The required Ignite UI JavaScript and CSS files
-   The required Ignite UI TypeScript definitions

### <a id="tile_manager_sample"></a>Tile Manager Sample​
This sample will demonstrate how we can use `igTileManager` with TypeScript.

#### <a id="tile_manager_sample_preview"></a>Preview
The following screenshot is a preview of the final result.

![](images/igTileManager_TypeScript.png)

<a id="tile_manager_steps_html"></a>Create the HTML - we are going to have three tabs with car brands and an `igTileManager` which will load photos of the selected car brand.

**In HTML:**
```html
<h1 class="hOne">Infragistics Car Dealership</h1>
<h3>Choose a car brand to browse</h3>
<div id="car-tabs">
    <ul>
        <li><a href="#magarcedesDashboard">Magarcedes</a></li>
        <li><a href="#hoidaDashboard">Hoida</a></li>
        <li><a href="#pausheDashboard">Paushe</a></li>
    </ul>

    <div id="magarcedesDashboard" class="dashboard"></div>
    <div id="hoidaDashboard" class="dashboard"></div>
    <div id="pausheDashboard" class="dashboard"></div>
</div>
```

​<a id="tile_manager_steps_ds"></a>Create the Data Source - we are adding the classes `CarData` and `Info`, and initialize the three car brands data. We are storing everything in the `Cars` array.

**In TypeScript:**
```typescript
/// <reference path="../../js/typings/jquery.d.ts" />
/// <reference path="../../js/typings/jqueryui.d.ts" />
/// <reference path="../../js/typings/igniteui.d.ts" />

class Info {
    description: string
    constructor(_description: string) {
        this.description = _description;
    }
}

class CarData {
    name: string;
    picture: string;
    priceRange: string;
    extras: Info[];
    constructor(_name: string, _picture: string, _priceRange: string, _extras: Info[]) {
        this.name = _name;
        this.picture = _picture;
        this.priceRange = _priceRange;
        this.extras = _extras;
    }

    addExtra(_extra) {
        this.extras.push(_extra);
    }
}

var Magarcedes: CarData[] = [];
Magarcedes.push(new CarData("2013 LSL AMG", "../../images/samples/tile-manager/car-dealership/shutterstock_139519967.jpg",
    "$199,500 - $206,000", [new Info("0-100 in 3.8 seconds"), new Info("Top speed: 317 km/h")]));
...

var Hoida: CarData[] = [];
Hoida.push(new CarData("2013 Candy", "../../images/samples/tile-manager/car-dealership/shutterstock_57034834.jpg",
    "$21,661 - $29,404", [new Info("Gas I4 2.5L engine"), new Info("Highway fuel efficiency 35 mpg")]));
...

var Paushe: CarData[] = [];
Paushe.push(new CarData("2013 CST", "../../images/samples/tile-manager/car-dealership/shutterstock_38288989.jpg",
    "$39,095 - $59,090", [new Info("Available All Wheel Drive"), new Info("Available touch-screen glide-up navigation with voice recognition"),
        new Info("Leather seating surfaces"), new Info("Adaptive Remote Start")]));
...

var Cars: CarData[][] = [];
Cars.push(Magarcedes);
Cars.push(Hoida);
Cars.push(Paushe);
```

​<a id="tile_manager_steps_tm"></a>Create the igTileManager - we are creating the `igTileManager` and the tabs. Then we preselect the first car brand and set the tabs to update the `igTileManager` data source when a tab is selected.

**In TypeScript:**
```typescript
$(function () {
    var activated: boolean[] = [false, false, false, false],
    options: IgTileManager = {
            columnWidth: 210,
            columnHeight: 210,
            marginLeft: 10,
            marginTop: 10,
            dataSource: Cars,
            items: [
                { rowIndex: 0, colIndex: 0, rowSpan: 2, colSpan: 2 },
                { rowIndex: 0, colIndex: 2, rowSpan: 1, colSpan: 1 },
                { rowIndex: 1, colIndex: 2, rowSpan: 1, colSpan: 1 },
                { rowIndex: 2, colIndex: 0, rowSpan: 1, colSpan: 1 },
                { rowIndex: 2, colIndex: 1, rowSpan: 1, colSpan: 1 },
                { rowIndex: 2, colIndex: 2, rowSpan: 1, colSpan: 1 }
            ],
            maximizedTileIndex: 0,
            maximizedState: '<figure><figcaption>${name}</figcaption><img src="${picture}" title="${Name}" alt="error" /></figure><ul><li>Price: ${priceRange}</li>' +
            '{{each ${extras} }}<li>${extras.description}</li>{{/each}}</ul>',
            minimizedState: '<figure><figcaption>${name}</figcaption><img src="${picture}" title="${Name}" alt="error" />'
        };

    // First time initialization of the tab that will be shown on page load
    options.dataSource = Cars[0];
    activated[0] = true;
    $('#magarcedesDashboard').igTileManager(options);

    var tabOptions: JQueryUI.TabsOptions = {
        activate: function (event, ui) {
            var index = ui.newTab.index();
            if (!activated[index]) {
                options.dataSource = Cars[index];
                ui.newPanel.igTileManager(options);
                activated[index] = true;
            } else {
                ui.newPanel.igTileManager('reflow');
            }
        }
    }

    $('#car-tabs').tabs(tabOptions);
});
```

### <a id="related_content"></a>Related Content
The following topic provides additional information related to this one:

[Using Ignite UI with TypeScript](Using-Ignite-UI-with-TypeScript.html) - This topic contains an overview for using the Ignite UI type definitions for TypeScript.