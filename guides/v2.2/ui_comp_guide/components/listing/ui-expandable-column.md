---
layout: default
group: UI_Components_guide
subgroup: components
title: Expandable Column сomponent
menu_title: Expandable Column component
version: 2.2
github_link: ui_comp_guide/components/listing/ui-expandable-column.md
---

## Overview
The Expandable Column UI component is an extension for [Column]({{page.baseurl}}ui_comp_guide/components/listing/ui-column.html). It alphabetically sorts the options associated with a record/row and renders several options (the number is defined in configuration) into a cell. The full list of options is displayed in a tooltip implemented by the Tooltip UI component `<Magento_Ui_module_dir>/view/base/web/js/lib/knockout/bindings/tooltip.js`.

The Expandable Column component can be used in Admin and the storefront.

## Structure

The Expandable Column UI component includes the following files:

- JS component: `<Magento_Ui_module_dir>/view/base/web/js/grid/columns/expandable.js`
- Template: `<Magento_Ui_module_dir>/view/base/web/templates/grid/cells/expandable.html`
- Template for tooltip content: `<Magento_Ui_module_dir>/view/base/web/templates/grid/cells/expandable/content.html`

## Configuration Settings

<table>
    <tr>
        <th>Option</th>
        <th>Description</th>
        <th>Type</th>
        <th>Default</th>
    </tr>
    <tr>
        <td><code>bodyTmpl</code></td>
        <td>Path to the template used for rendering the column's fields in the table's body.</td>
        <td>String</td>
        <td><code>ui/grid/cells/expandable</code></td>
    </tr>
    <tr>
        <td><code>tooltipTmpl</code></td>
        <td>Path to the template used for rendering the component's tooltip content template.</td>
        <td>String</td>
        <td><code>ui/grid/cells/expandable/content</code></td>
    </tr>
    <tr>
        <td><code>visibeItemsLimit</code></td>
        <td>A number of options to display in a cell.</td>
        <td>String</td>
        <td><code>'5'</code></td>
    </tr>
    <tr>
        <td><code>tooltipTitle</code></td>
        <td>A title for the tooltip.</td>
        <td>String</td>
        <td></td>
    </tr>
</table>

Component's options are set in the configuration `.xml` file as follows:

{% highlight xml %}
<column name="ids" class="Magento\Ui\Component\MassAction\Columns\Column">
    <argument name="data" xsi:type="array">
        <item name="options" xsi:type="object">Magento\Catalog\Model\Product\Attribute\Source\Status</item>
        <item name="config" xsi:type="array">
            <item name="component" xsi:type="string">Magento_Ui/js/grid/columns/expandable</item>
            <item name="tooltipTitle" xsi:type="string">Tooltip Title</item>
            <item name="visibeItemsLimit" xsi:type="number">5</item>
        </item>
    </argument>
</column>
{% endhighlight %}

## Dependencies on Other Components

This component has a dependency on the following component:

* Column: `<Magento_Ui_module_dir>/view/base/web/js/grid/columns/column.js`

## Methods and Events

The following API methods are available:

- `getFullLabel()`: gets a label from a full list of options.
- `getShortLabel()`: gets a label from a list of options limited by `visibeItemsLimit` value.
- `getLabelsArray()`: extracts an array of labels associated with provided values and sorts these labels alphabetically.
- `isExpandable()`: checks if the amount of options associated with a record is greater than a `visibeItemsLimit` value.
- `flatOptions()`: transforms the tree options structure to a linear array.
