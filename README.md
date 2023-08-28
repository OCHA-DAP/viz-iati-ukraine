# Ukraine IATI Dashboard

> We are no longer maintaining the Ukraine IATI Dashboard as of 14/08/2023
> 
> Contact us at [hdx@un.org](mailto:hdx@un.org) with questions or feedback.

A simple visualization using published IATI data on Ukraine.

Data was last generated 14/08/2023 by the [OCHA-DAP/hdx-scraper-iati-viz](https://github.com/ocha-dap/hdx-scraper-iati-viz) repository.

## Build Setup

``` bash
# install dependencies
$ npm i

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).

## Generate a Github Pages static site and push to Github

```bash
$ npm run gen-subfolder
$ npm run deploy
```

## Linting

Note that linting is enabled, and will automatically fix a number of issues in your code. However, sometimes this can feel a bit aggressive or annoying while you are working through something, so if you want to disable linting, you can comment out the 3 indicated lines in `nuxt.config.js`.

You can manage linting rules in `.eslintrc.js`. I disabled a few rules but you may have other preferences.

## Overview

There are three "tabs" in the visualization: Commmitments/Spending; Spending Flows; About. Each of these tabs is accessible from the top navigation bar. The navigation bar is found in `default.vue`.

### Commmitments/Spending (`pages/index.vue`)

The Commmitments/Spending tab contains data from IATI, and displays **components** in the following order:
* `Charts/DoughnutChart.vue`: a doughnut chart showing a side by side percentage breakdown of the top five categories by Commitments on the left and Spending on the right.
* `Charts/TimeseriesChart.vue`: a combination bar and line chart that shows monthly and cumulative Commitments and Spending totals by month.

### Spending Flows (`pages/spending_flows.vue`)

The Spending Flows tab contains data from IATI and displays the financial flows by reporting organization
* A Sankey chart (using the `Charts/FinancialSankey.vue` component) for the selected **organization**.

### About (`pages/about.vue`)

The About tab contains a description and background of the visualization. It describes data sources and provides some high-level technical details.

## Tooltips

Tooltips can be used throughout the site to provide more information where required.

* Tooltips should be saved as a CSV file, with a unique `key` column and a `tooltip` column. You might want to namespace the `key` column (e.g. prefix all existing tooltips with `activities_table__`)
* Tooltips are loaded when `activities.vue` loads and stored in the Vuex store. This means they are available to all other parts of the visualization through `this.$store.state.tooltips` - for the example below, the value of this is assigned to the computed property `tooltips`.
* You can add a tooltip wherever you want using something like the following code:
```
<b-badge
  v-b-tooltip.hover
  variant="secondary"
  pill
  :title="tooltips['theTooltipKey']">
  ?
</b-badge>
```

You can easily load tooltips in any tab by adding relevant functionality within the `loadData()` method of each tab. Alternatively, you could consider moving this retrieval to the Vuex store and call it from within the [nuxtServerInit](https://nuxtjs.org/docs/2.x/directory-structure/store#the-nuxtserverinit-action) action, though this is a bit fiddly.

